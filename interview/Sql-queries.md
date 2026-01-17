<!-- TOC -->
* [Q-1 WAQ to select the 2nd highest salary from the employee table](#q-1-waq-to-select-the-2nd-highest-salary-from-the-employee-table)
* [Q-2 WAQ to select the highest salary in each department](#q-2-waq-to-select-the-highest-salary-in-each-department)
* [Q-3 WAQ to display alternate records in a table](#q-3-waq-to-display-alternate-records-in-a-table)
* [Q-4 WAQ to find duplicated values and their frequency in the department column](#q-4-waq-to-find-duplicated-values-and-their-frequency-in-the-department-column)
* [Q-5 WAQ to display employee names that start with a given character.](#q-5-waq-to-display-employee-names-that-start-with-a-given-character)
* [Q-6 WAQ to display employee names that end with a given character.](#q-6-waq-to-display-employee-names-that-end-with-a-given-character)
* [Q-7 WAQ to display employee names that contain a given character anywhere.](#q-7-waq-to-display-employee-names-that-contain-a-given-character-anywhere)
* [Q-8 WAQ to display employee names that don't contain a given character.](#q-8-waq-to-display-employee-names-that-dont-contain-a-given-character)
* [Q-9 WAQ to display employee names and hire dates for employees who joined in December.](#q-9-waq-to-display-employee-names-and-hire-dates-for-employees-who-joined-in-december)
* [Q-10 WAQ to select employees whose names contain exactly two 'L' characters.](#q-10-waq-to-select-employees-whose-names-contain-exactly-two-l-characters)
* [Q-11 WAQ to find the nth row of an employee table.](#q-11-waq-to-find-the-nth-row-of-an-employee-table)
  * [Problem with This Query](#problem-with-this-query)
    * [1. No Defined Order](#1-no-defined-order)
    * [2. Nondeterministic Result](#2-nondeterministic-result)
  * [Correct Solution (With ORDER BY)](#correct-solution-with-order-by)
* [Q-12 Explain the difference b/w UNION and UNION ALL](#q-12-explain-the-difference-bw-union-and-union-all)
  * [Example](#example)
  * [Important Rules (Interview-Relevant)](#important-rules-interview-relevant)
<!-- TOC -->

# Q-1 WAQ to select the 2nd highest salary from the employee table

| emp_id | emp_name | department | salary |
|--------|----------|------------|--------|
| 1      | Alice    | HR         | 60000  |
| 2      | Bob      | HR         | 75000  |
| 3      | Carol    | IT         | 90000  |
| 4      | Dave     | IT         | 120000 |
| 5      | Eve      | Finance    | 85000  |
| 6      | Frank    | Finance    | 95000  |

```sql
select max(salary)
from emp
where salary not in (select max(salary) from emp)
```


# Q-2 WAQ to select the highest salary in each department

| emp_id | emp_name | department | salary |
|--------|----------|------------|--------|
| 1      | Alice    | HR         | 60000  |
| 2      | Bob      | HR         | 75000  |
| 3      | Carol    | IT         | 90000  |
| 4      | Dave     | IT         | 120000 |
| 5      | Eve      | Finance    | 85000  |
| 6      | Frank    | Finance    | 95000  |

```sql
select max(salary),
       department
from emp
group by department
```


# Q-3 WAQ to display alternate records in a table

| emp_id | emp_name | department | salary |
|--------|----------|------------|--------|
| 1      | Alice    | HR         | 60000  |
| 2      | Bob      | HR         | 75000  |
| 3      | Carol    | IT         | 90000  |
| 4      | Dave     | IT         | 120000 |
| 5      | Eve      | Finance    | 85000  |
| 6      | Frank    | Finance    | 95000  |

At first glance, the following query might appear to give the correct answer.

```sql
select *
from emp
where emp_id % 2 = 0;
```

But what if `emp_id`'s are `1, 4, 7, 10`. This would break `% 2` logic. 
That's why preferred answer is to use the window function.

```sql
select *
from (select *,
             row_number() over(order by emp_id) as rn
      from emp) t
where rn % 2 = 1
```



# Q-4 WAQ to find duplicated values and their frequency in the department column

| emp_id | emp_name | department |
|-------:|----------|------------|
|      1 | Alice    | IT         |
|      2 | Bob      | HR         |
|      3 | Carol    | IT         |
|      4 | Dave     | Finance    |
|      5 | Eve      | IT         |
|      6 | Frank    | HR         |
|      7 | Grace    | Finance    |
|      8 | Heidi    | HR         |
|      9 | Ivan     | IT         |
|     10 | Judy     | Finance    |

```sql
SELECT *,
       COUNT(*) OVER (PARTITION BY department) AS dept_count
FROM emp;
```



# Q-5 WAQ to display employee names that start with a given character.

| emp_id | emp_name | department |
|-------:|----------|------------|
|      1 | Alice    | IT         |
|      2 | Bob      | HR         |
|      3 | Carol    | IT         |
|      4 | Dave     | Finance    |
|      5 | Eve      | IT         |
|      6 | Frank    | HR         |
|      7 | Grace    | Finance    |
|      8 | Heidi    | HR         |
|      9 | Ivan     | IT         |
|     10 | Judy     | Finance    |

```sql
select *
from emp
where emp_name like 'A%'  -- emp_name starts with 'A'
```



# Q-6 WAQ to display employee names that end with a given character.

| emp_id | emp_name | department |
|-------:|----------|------------|
|      1 | Alice    | IT         |
|      2 | Bob      | HR         |
|      3 | Carol    | IT         |
|      4 | Dave     | Finance    |
|      5 | Eve      | IT         |
|      6 | Frank    | HR         |
|      7 | Grace    | Finance    |
|      8 | Heidi    | HR         |
|      9 | Ivan     | IT         |
|     10 | Judy     | Finance    |

```sql
select *
from emp
where emp_name like '%A'  -- emp_name ends with 'A'
```



# Q-7 WAQ to display employee names that contain a given character anywhere.

| emp_id | emp_name | department |
|-------:|----------|------------|
|      1 | Alice    | IT         |
|      2 | Bob      | HR         |
|      3 | Carol    | IT         |
|      4 | Dave     | Finance    |
|      5 | Eve      | IT         |
|      6 | Frank    | HR         |
|      7 | Grace    | Finance    |
|      8 | Heidi    | HR         |
|      9 | Ivan     | IT         |
|     10 | Judy     | Finance    |

```sql
select *
from emp
where emp_name like '%A%'  -- emp_name contains 'A' anywhere
```



# Q-8 WAQ to display employee names that don't contain a given character.

| emp_id | emp_name | department |
|-------:|----------|------------|
|      1 | Alice    | IT         |
|      2 | Bob      | HR         |
|      3 | Carol    | IT         |
|      4 | Dave     | Finance    |
|      5 | Eve      | IT         |
|      6 | Frank    | HR         |
|      7 | Grace    | Finance    |
|      8 | Heidi    | HR         |
|      9 | Ivan     | IT         |
|     10 | Judy     | Finance    |

```sql
select *
from emp
where emp_name not like '%A%'  -- emp_name don't contain 'A'
```



# Q-9 WAQ to display employee names and hire dates for employees who joined in December.


```sql
select name
     , hire_date
from employee
where date_part('month', hire_date) = 12
  and date_part('year', hire_date) = 2025
```

While correct, this approach applies functions to the column and can prevent index usage. 
An index-friendly alternative is a date range filter.

```sql
select name
     , hire_date
from employee
where hire_date between '2025-12-01' and '2025-12-31'
```



# Q-10 WAQ to select employees whose names contain exactly two 'L' characters.

```sql
select *
from employee
where name like '%L%L%'
```

This query checks for at least two `L`s, not exactly two. To get the exactly 2 `L`s use the following:

```sql
SELECT *
FROM employee
WHERE LENGTH(name) - LENGTH(REPLACE(name, 'L', '')) = 2;
```



# Q-11 WAQ to find the nth row of an employee table.

| employee_id | name  | department | salary | hire_date  |
|------------:|-------|------------|-------:|------------|
|           1 | Alice | IT         |   9500 | 2019-01-10 |
|           2 | Bob   | HR         |   7200 | 2018-03-15 |
|           3 | Carol | IT         |   8800 | 2020-06-20 |
|           4 | David | Finance    |   6100 | 2017-11-01 |
|           5 | Eve   | HR         |   7400 | 2021-09-12 |
|           6 | Frank | IT         |   8800 | 2019-12-05 |
|           7 | Grace | Finance    |   6900 | 2022-02-18 |
|           8 | Heidi | IT         |  10200 | 2016-07-30 |
|           9 | Ivan  | HR         |   6600 | 2020-04-25 |
|          10 | Judy  | Finance    |   8300 | 2018-08-14 |
|          11 | Kevin | IT         |   7800 | 2021-01-22 |
|          12 | Laura | HR         |   7100 | 2019-05-03 |


```sql
select *
from (select *,
             row_number() over() as rk
      from employee) t
where
    rk = 2
```

## Problem with This Query

### 1. No Defined Order

* `ROW_NUMBER()` assigns numbers based on row order
* SQL tables do not have an inherent order
* Since no `ORDER BY` is specified, the database can assign row numbers in any order

### 2. Nondeterministic Result

* The row returned as `rk = 2`:
    * may change between executions
    * may change after data reloads
    * may change with different execution plans

So this query does not reliably return the "2nd row".

## Correct Solution (With ORDER BY)

You must define what "nth row" means.

Example: 2nd Row by Salary (Descending)

```sql
SELECT *
FROM (
    SELECT *,
           ROW_NUMBER() OVER (ORDER BY salary DESC) AS rk
    FROM employee
) t
WHERE rk = 2;
```

Key Interview Takeaway

> `ROW_NUMBER()` must always be used with `ORDER BY` when selecting a nth row.
>



# Q-12 Explain the difference b/w UNION and UNION ALL

| Aspect         | UNION                              | UNION ALL                  |
|----------------|------------------------------------|----------------------------|
| Duplicate rows | Removes duplicates                 | Keeps duplicates           |
| Operation      | Performs deduplication             | Simple concatenation       |
| Performance    | Slower                             | Faster                     |
| Sorting        | Implicit sort/hash for dedup       | No sort required           |
| Use case       | When unique result set is required | When all rows are required |


## Example

**Table A**

| id |
|----|
| 1  |
| 2  |
| 3  |

**Table B**

| id |
|----|
| 2  |
| 3  |
| 4  |

**Using `UNION`**

```sql
SELECT id FROM A
UNION
SELECT id FROM B;
```

Result:

```text
1
2
3
4
```

Duplicates removed.

**Using `UNION ALL`**

```sql
SELECT id FROM A
UNION ALL
SELECT id FROM B;
```

Result:

```text
1
2
2
3
3
4
```

Duplicates preserved.

## Important Rules (Interview-Relevant)

* Both queries must return:
    * Same number of columns
    * Compatible data types

