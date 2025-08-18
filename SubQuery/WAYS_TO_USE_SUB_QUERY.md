## SUB_QUERY

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
WHERE department_id IN (
      SELECT department_id
      FROM department
      WHERE location = "Seoul"
)
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
- A subquery that **returns more than one column**.
- The main query compares multiple columns with the data returned by the subquery at the same time.
- **The number of columns in the comparison must exactly match** the number of columns returned by the subquery.
- Can often be replaced with a `JOIN` or `EXISTS` approach.
> ``` mysql
> -- EXISTS
> SELECT *
> FROM employees e
> WHERE EXISTS (
>     SELECT 1
>     FROM departments d
>     WHERE e.dept_id = d.dept_id
>           AND e.job_id = d.manager_job_id
> );
> 
> -- Multi-column Subquery
> SELECT *
> FROM employees
> WHERE (dept_id, job_id) IN (
>     SELECT dept_id, manager_job_id
>     FROM departments
> );
>```
### 4. Correlated Subquery
- A subquery that **references values from the main query**.
- The subquery is executed once for each row of the main query -> may cause performance issues.
- Can often be replaced with a `JOIN` or a window function.
> ``` mysql
> SELECT emp_id, name, salary, dept_id
> FROM employees e
> WHERE salary = (
>     SELECT MAX(salary)
>     FROM employees
>     WHERE dept_id = e.dept_id
> );
> ```
### 5. Inline View or Derived Table
