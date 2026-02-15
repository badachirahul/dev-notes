# PostgreSQL

* ### **SQL → Standard language (like a contract)**
* ### **MySQL / PostgreSQL → Software that implements SQL + adds extra features**

## **1. Constraints:**
* **Constraints are rules applied to columns.**
* Types of constraints:
    1. Not-Null
    2. Unique
    3. Primary Keys
    4. Foreign Keys
    5. Check

## **2. Main Clauses in a SELECT Query**
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

<br>

## **3. Aggregate functions**
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

<br>

## **4. String Functions:**
* 
    ### 1. CONCAT()

    -   Combines multiple strings into one.
    -   Example: `SELECT CONCAT(first_name, ' ', last_name) FROM employees;`
    -   Output: Combined full name (e.g., `Rahul Sharma`)

    ------------------------------------------------------------------------

    ### 2. CONCAT_WS()

    -   Combines strings with a separator.
    -   We can convert into **.csv** file format also.
    -   Example: `SELECT CONCAT_WS(',', 'Rahul', 'Badachi', '052');`
    -   Output: `Rahul,Badachi,052`

    ------------------------------------------------------------------------

    ### 3. SUBSTR()

    -   Extracts part of a string.
    -   Example: `SELECT SUBSTR('Hello World', 1, 5);`
    -   Output: `Hello`

    ------------------------------------------------------------------------

    ### 4. REPLACE()

    -   Replaces part of a string with another value.
    -   Example: `SELECT REPLACE('rahul@gmail.com', 'gmail', 'yahoo');`
    -   Output: `rahul@yahoo.com`

    ------------------------------------------------------------------------

    ### 5. REVERSE()

    -   Reverses the string.
    -   Example: `SELECT REVERSE('abc');`
    -   Output: `cba`

    ------------------------------------------------------------------------

    ### 6. LENGTH()

    -   Returns number of characters.
    -   Example: `SELECT LENGTH('hello');`
    -   Output: `5`

    ------------------------------------------------------------------------

    ### 7. LOWER()

    -   Converts text to lowercase.
    -   Example: `SELECT LOWER('RAHUL');`
    -   Output: `rahul`

    ------------------------------------------------------------------------

    ### 8. UPPER()

    -   Converts text to uppercase.
    -   Example: `SELECT UPPER('rahul');`
    -   Output: `RAHUL`

    ------------------------------------------------------------------------

    ### 9. LEFT()

    -   Returns leftmost characters.
    -   Example: `SELECT LEFT('PostgreSQL', 4);`
    -   Output: `Post`

    ------------------------------------------------------------------------

    ### 10. RIGHT()

    -   Returns rightmost characters.
    -   Example: `SELECT RIGHT('PostgreSQL', 3);`
    -   Output: `SQL`

    ------------------------------------------------------------------------

    ### 11. TRIM()

    -   Removes spaces from both sides.
    -   Example: `SELECT TRIM('  rahul  ');`
    -   Output: `rahul`

    ------------------------------------------------------------------------

    ### 12. SPLIT_PART()

    -   Splits a string using delimiter and returns specified part.
    -   Example: `SELECT SPLIT_PART('rahul@gmail.com', '@', 2);`
    -   Output: `gmail.com`
        
        ### Working:

        Original string:

            rahul@gmail.com

        Split using delimiter: `@`

        After splitting:

            1 → rahul
            2 → gmail.com

        We requested part number **2**.

        ------------------------------------------------------------------------

        ### Output

            gmail.com

        ------------------------------------------------------------------------

        ### Important Notes

        -   Position starts from **1**, not 0.
        -   If the requested part does not exist, PostgreSQL returns an **empty
            string**.
        -   The original string is not modified.

<br>

## **5. SQL SELECT Execution Order:**
* 
    ### 1️⃣ FROM

    - First it finds the table.

        ``` sql
        FROM employees
        ```

    - 👉 Loads all rows from `employees`.

    ------------------------------------------------------------------------

    ### 2️⃣ WHERE

    - Filters rows.

        ``` sql
        WHERE salary > 50000
        ```

    - 👉 Keeps only rows where salary is greater than 50000.

    ------------------------------------------------------------------------

    ### 3️⃣ GROUP BY

    - Groups similar values.

        ``` sql
        GROUP BY department
        ```

    - 👉 Groups rows based on department.

    ------------------------------------------------------------------------

    ### 4️⃣ HAVING

    - Filters grouped data.

        ``` sql
        HAVING COUNT(*) > 5
        ```

    - 👉 Keeps only groups having more than 5 rows.

    ------------------------------------------------------------------------

    ### 5️⃣ SELECT

    - Chooses what to display.

        ``` sql
        SELECT name, salary
        ```

    - 👉 Displays only name and salary columns.

    ------------------------------------------------------------------------

    ### 6️⃣ ORDER BY

    - Sorts the result.

        ``` sql
        ORDER BY salary DESC
        ```

    - 👉 Sorts rows by salary in descending order.

    ------------------------------------------------------------------------

    ### 7️⃣ LIMIT

    - Restricts number of rows.

        ``` sql
        LIMIT 10
        ```

    - 👉 Returns only first 10 rows.