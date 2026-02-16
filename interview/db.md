<!-- TOC -->
* [Q-1 What is 1NF, 2NF and 3NF](#q-1-what-is-1nf-2nf-and-3nf)
  * [1NF](#1nf)
  * [2NF](#2nf)
  * [3NF](#3nf)
* [Q-2 Types of Index (postgres)](#q-2-types-of-index-postgres)
  * [B-Tree Index](#b-tree-index)
  * [Hash Index](#hash-index)
  * [GIN (Generalized Inverted Index)](#gin-generalized-inverted-index)
* [Q-3 What is selectivity and cardinality](#q-3-what-is-selectivity-and-cardinality)
    * [Cardinality](#cardinality-)
* [Q-4 What is isolation level](#q-4-what-is-isolation-level)
    * [Read Uncommitted (lowest isolation)](#read-uncommitted-lowest-isolation)
    * [Read Committed (PostgreSQL default)](#read-committed-postgresql-default)
    * [Repeatable Read](#repeatable-read)
    * [Serializable (highest isolation)](#serializable-highest-isolation)
* [Q-5 What's the difference between Dirty Read, Non-Repeatable Read, Phantom Read](#q-5-whats-the-difference-between-dirty-read-non-repeatable-read-phantom-read)
* [Q-6 What is ACID](#q-6-what-is-acid)
  * [Atomicity](#atomicity)
  * [Consistency](#consistency)
  * [Isolation](#isolation)
  * [Durability](#durability)
* [Q-7 Database locks](#q-7-database-locks)
* [Q-8 Types of cache](#q-8-types-of-cache)
  * [Write through cache](#write-through-cache)
  * [Write around cache](#write-around-cache)
  * [Write back cache](#write-back-cache)
<!-- TOC -->

# Q-1 What is 1NF, 2NF and 3NF

## 1NF

1. Each column must store a single, indivisible value (no lists, arrays, or comma-separated values).
1. No repeating groups / columns
1. Each record is unique. No two rows should be completely identical.

## 2NF

Any non-key field should be dependent on the entire primary key.

Example:

| Course | Date      | CourseTitle      | Room | Capacity | Available |
|--------|-----------|------------------|------|----------|-----------|
| SQL101 | 3/1/2013  | SQL Fundamentals | 4A   | 12       | 4         |
| DB202  | 3/1/2013  | Database Design  | 7B   | 14       | 7         |
| SQL101 | 4/14/2013 | SQL Fundamentals | 7B   | 14       | 10        |
| SQL101 | 5/28/2013 | SQL Fundamentals | 12A  | 8        | 8         |
| CS200  | 4/15/2012 | C Programming    | 4A   | 12       | 11        |

Assume we have composite key on `Course` and `Date` column. The `CourseTitle` can be determined just 
from `Course` column that's why It's not in 2NF.

**Note:** 2NF is only applicable when we have composite key


## 3NF

Non-key field should not be dependent on any other non-key field.

Example:


| Course | Date      | CourseTitle      | Room | Capacity | Available |
|--------|-----------|------------------|------|----------|-----------|
| SQL101 | 3/1/2013  | SQL Fundamentals | 4A   | 12       | 4         |
| DB202  | 3/1/2013  | Database Design  | 7B   | 14       | 7         |
| SQL101 | 4/14/2013 | SQL Fundamentals | 7B   | 14       | 10        |
| SQL101 | 5/28/2013 | SQL Fundamentals | 12A  | 8        | 8         |
| CS200  | 4/15/2012 | C Programming    | 4A   | 12       | 11        |


Again assume we have composite key on `Course` and `Date` column. We can find the `Capacity` just by the `Room`. 
Hence, It's not in 3NF.


# Q-2 Types of Index (postgres)

## B-Tree Index

Think of it as: A sorted phonebook.
* Structure: Balanced tree (root → branches → leaves).
* What’s stored in leaves: The indexed key + CTID (page + row in the heap table).
* How it works:
    *   Keys are kept in sorted order.
    *   Binary search down the tree → find the key → get CTIDs → fetch rows.

* Best for:
    *   Equality (=) and range (<, >, BETWEEN) queries.
    *   ORDER BY and comparisons.

Example:

```sql
CREATE INDEX idx_users_name ON users(name);
SELECT * FROM users WHERE name > 'John';
```

## Hash Index

Think of it as: A cabinet with labeled buckets, where the label is a hash of your key.

* Structure:
    * A hash function maps a key → bucket number.
    * Each bucket holds one or more CTIDs (linked list style if collisions happen).

* What’s stored: Just enough to check equality: hash value + CTIDs.
* How it works:
    * Hash the search key → jump to the correct bucket → scan CTIDs inside.

* Best for:
    * Equality lookups only (=).
    * When sorting or ranges don’t matter.

```sql
CREATE INDEX idx_users_email_hash ON users USING hash(email);
SELECT * FROM users WHERE email = 'abc@example.com';
```

* Extra: Since PostgreSQL 10+, hash indexes are WAL-logged (safe for production).   
* Weakness: No range scans, can’t use for >, <.

## GIN (Generalized Inverted Index)

**Think of it as:** A giant dictionary → for each word (or value), it lists all the rows where it appears.

* Structure:
    * Keys (tokens/values) → Posting list (small) or Posting tree (large).
    * Posting list/tree contains CTIDs of matching rows.

* What’s stored: Token → list/tree of CTIDs.
* How it works:
    * Break the column value into tokens (e.g., words in text, elements in an array, keys in JSONB).
    * For each token, store where it appears.
    * Searching for multiple tokens merges the CTID lists.

* Best for:
    * Full-text search (tsvector), array columns, JSONB.
    * Queries like "find all rows containing both 'cat' and 'dog'".

```sql
CREATE INDEX idx_articles_tsv ON articles USING gin(to_tsvector('english', content));
SELECT * FROM articles WHERE to_tsvector('english', content) @@ to_tsquery('postgres & index');
```

# Q-3 What is selectivity and cardinality

### Cardinality 
The number of unique values in a column.

* High cardinality → many unique values. Example: email column in a user table — almost every row has a different value.

* Low cardinality → few unique values. Example: gender column — only "M" and "F" (or a few options).

In query optimization:

* High cardinality columns tend to benefit more from indexes, because they help filter down to very few rows.
* Low cardinality columns (like is_active = true/false) are often poor candidates for indexing because queries still return a large portion of the table.

# Q-4 What is isolation level

When many transactions run at the same time, they might interfere with each other.
Isolation level tells the database how much one transaction is allowed to see the effects of others before they are committed.

It's basically a trade-off between:

* Correctness (safety)
* Performance (speed & concurrency)


### Read Uncommitted (lowest isolation)

* Transactions can see uncommitted changes from others.
* PostgreSQL actually treats this the same as Read Committed (so you don’t get "dirty reads").
* Problems: Dirty reads, non-repeatable reads, phantom reads.

Example:
* Transaction A updates a row but doesn’t commit.
* Transaction B can already see this change → but if A later rolls back, B saw something that "never really happened."


### Read Committed (PostgreSQL default)

* Each query inside your transaction sees a snapshot of the database at the start of that query.
* You won’t see uncommitted data, but if another transaction commits between your queries, you can see its changes.
* Problems: Non-repeatable reads, phantom reads.

Example:

* Transaction A reads a row (price = 100).
* Transaction B updates that row to price = 200 and commits.
* If Transaction A queries again, it now sees 200 → inconsistent within one transaction.


### Repeatable Read

* Each query in your transaction sees the same snapshot of data (from when the transaction started).
* Even if others commit, you won’t see their changes until you commit and start a new transaction.
* Problems: Phantom reads (new rows appearing).

Example:

* Transaction A queries: "SELECT all users where age > 20" → gets 5 rows.
* Transaction B inserts a new user with age = 25 and commits.
* If A runs the query again, it still sees only 5 rows (no inconsistency in rows you already saw),
but if it tries to insert/update in the same range, it may get conflicts.


### Serializable (highest isolation)

* Transactions behave as if they ran one after another, never at the same time.
* The database may block or even abort some transactions to keep this illusion.
* No anomalies: no dirty reads, no non-repeatable reads, no phantom reads.
* But it’s the slowest and may cause more rollbacks.

# Q-5 What's the difference between Dirty Read, Non-Repeatable Read, Phantom Read

* **Dirty Read:** You read uncommitted data written by another transaction.
* **Non-Repeatable Read:** You read the same row twice, but it has changed because another transaction committed an update.
* **Phantom Read:** You read a set of rows twice with the same condition, but the number of rows changes because 
  another transaction inserted/deleted.

# Q-6 What is ACID

## Atomicity

"All or nothing."

* A transaction’s operations either all succeed or all fail.
* If any step fails, the whole transaction is rolled back.

Example:

* Bank transfer: deduct ₹100 from Account A and add ₹100 to Account B.
* If the deposit fails, the deduction must be undone.

## Consistency

"Valid state → valid state."

* A transaction brings the database from one valid state to another, following all rules (constraints, triggers, cascades).
* Example:
    * If an account balance cannot be negative, a transaction that would violate this must be rejected.

## Isolation

"Transactions don't interfere."

* Concurrent transactions appear as if they were executed serially (one after another), even if they actually run in parallel.
* Prevents anomalies like dirty reads, non-repeatable reads, phantom reads (controlled by isolation levels).
* Example:
    * Two people booking the same seat → isolation ensures only one succeeds.


## Durability

"Once committed, always committed."

* After a transaction commits, its changes are permanent, even if there’s a crash or power failure.
* Achieved by writing to logs (WAL – Write-Ahead Log) or persistent storage before acknowledging commit.
* Example:
    * Once you get a "Payment Successful" message, the transaction won’t vanish even if the server crashes immediately.


# Q-7 Database locks

# Q-8 Types of cache

## Write through cache

Every write goes to both the cache and the underlying storage (memory/disk) at the same time.

## Write around cache

Writes go directly to the storage (skipping the cache). Cache is only updated on a read miss later.

## Write back cache

Writes go to the cache only at first, and are marked as "dirty". Later, the dirty data is flushed (written back) to 
storage asynchronously.










