# PostgreSQL

## 1. Constraints:
* **Constraints are rules applied to columns.**
* Types of constraints:
    1. Not-Null
    2. Unique
    3. Primary Keys
    4. Foreign Keys
    5. Check Constraints

## 2. Main Clauses in a SELECT Query
* General order:
    1. SELECT
    2. FROM
    3. JOIN
    4. WHERE
    5. GROUP BY
    6. HAVING
    7. ORDER BY
    8. LIMIT
    9. OFFSET

## 3. Aggregate functions
* 
    ### 1. COUNT()
    * Counts the number of rows.
    * Example: `SELECT COUNT(*) FROM employees;`

    ------------------------------------------------------------------------

    ### 2. SUM()
    * Adds numeric column values.
    * Example: `SELECT SUM(salary) FROM employees;`

    ------------------------------------------------------------------------

    ### 3. AVG()
    * Calculates the average of numeric values.
    * Example: `SELECT AVG(salary) FROM employees;`

    ------------------------------------------------------------------------

    ### 4. MIN()
    * Returns the smallest value.
    * Example: `SELECT MIN(salary) FROM employees;`

    ------------------------------------------------------------------------

    ### 5. MAX()
    * Returns the largest value.
    * Example: `SELECT MAX(salary) FROM employees;`