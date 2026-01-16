<!-- TOC -->
* [Q-1 WAQ to select the 2nd highest salary from the employee table](#q-1-waq-to-select-the-2nd-highest-salary-from-the-employee-table)
* [Q-2 WAQ to select the highest salary in each department](#q-2-waq-to-select-the-highest-salary-in-each-department)
* [Q-3 WAQ to display alternate records in a table](#q-3-waq-to-display-alternate-records-in-a-table)
* [Q-4 WAQ to find duplicated values and their frequency in the department column](#q-4-waq-to-find-duplicated-values-and-their-frequency-in-the-department-column)
* [Q-5 WAQ to display employee names that start with a given character.](#q-5-waq-to-display-employee-names-that-start-with-a-given-character)
* [Q-6 WAQ to display employee names that end with a given character.](#q-6-waq-to-display-employee-names-that-end-with-a-given-character)
* [Q-7 WAQ to display employee names that contain a given character anywhere.](#q-7-waq-to-display-employee-names-that-contain-a-given-character-anywhere)
* [Q-8 WAQ to display employee names that don't contain a given character.](#q-8-waq-to-display-employee-names-that-dont-contain-a-given-character)
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

But what if `emp_id`'s are `1, 4, 7, 10`. This would break `% 2` logic. That's why preferred answer is to use the 
window function.

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