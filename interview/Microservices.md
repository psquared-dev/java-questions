## Q-1 Types of caches

### Write through cache

Every write goes to both the cache and the underlying storage (memory/disk) at the same time.

### Write around cache

Writes go directly to the storage (skipping the cache). Cache is only updated on a read miss later.

### Write back cache

Writes go to the cache only at first, and are marked as "dirty". Later, the dirty data is flushed (written back) to storage asynchronously.



## Q-2 What are common microserivces design pattern?

