## WITH

### 0. What is sub query?
- A sub query is a query **nested inside another query**.
- The outer query is called the **main query**, and the inner one is called the **sub query**.
- The subquery provides a data set to the main query.

### 1. Scalar Subquery
- A sub query that **returns a single value** (1 row, 1 column).
- Commonly used in `SELECT`, `WHERE` and `HAVING`
``` mysql
SELECT *
FROM employee_table
WHERE salary > (SELECT AVG(salary) AS average
                FROM employee_table)
```
- Main query of statement below is
> ```mysql
> SELECT *
> FROM employee_table
> WHERE salary >
> ```
- Sub query of statement below is
> ``` mysql
> (SELECT AVG(salary)
> AS average FROM employee_table)
> ```

### 2. Multi-row Subquery
- A sub query that **returns more than 2 rows**.
- Commonly used with `IN`, `ANY`, `ALL` and `EXISTS`
- This let us make flexible comparison statement
``` mysql
SELECT *
FROM employee_table
WHERE department_id IN (SELECT department_id
                        FROM department
                        WHERE location = "Seoul")
```
- Main query of statement below is

> ```mysql
> SELECT *
> FROM employee_table
> WHERE department_id IN
> ```
- Sub query of statement below is
> ``` mysql
> (SELECT department_id
>  FROM department
>  WHERE location = "Seoul")
> ```
### 3. Multi-column Subquery

### 4. Correlated Subquery

### 5. Inline View or Derived Table
