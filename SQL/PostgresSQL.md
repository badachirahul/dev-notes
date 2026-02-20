# PostgreSQL

* ### **SQL → Standard language (like a contract)**
* ### **MySQL / PostgreSQL → Software that implements SQL + adds extra features**
* ### **Caching: Caching is storing frequently accessed data in fast memory to reduce load and improve performance.**

## Topics
* Joins
* Aggregations
* Normalization
* Indexes
* Transactions
* ACID
* Isolation Levels
* Locking
* CAP Theorem
* Triggers


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

    ### 📘 GROUP BY Rule
    * **If you use an aggregate function** `(COUNT, SUM, AVG, MIN, MAX)`
    * **Every non-aggregated column in** `SELECT Must be written in GROUP BY`

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

    ### 13. STRING_AGG()
    -   Combines multiple row values into a single string using a separator.
    -   Example: `SELECT STRING_AGG(fname, ', ') FROM employees;`
    -   Output: `Rahul, Amit, Sneha`

<br>

## **5. SQL SELECT Execution Order:**
* 
    ### 1️⃣ FROM 

    - First it finds the table.

        ``` sql
        FROM employees
        ```

    - 👉 Loads all rows from `employees`.
    - **After 'FROM' immediately `JOINS` get executed.**

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


## **6. JOINS:**
- **A JOIN combines rows from two tables based on a related column.**
- **Types**
    ```
    1. Inner Join
    2. Left Join
    3. Right Join
    4. Full Join
    5. Self Join
    ```

    ### 👨‍💼 employees:
    | emp_id | name   | email                                       | salary | dept_id | hire_date  |
    | ------ | ------ | ------------------------------------------- | ------ | ------- | ---------- |
    | 1      | Rahul  | [rahul@gmail.com](mailto:rahul@gmail.com)   | 50000  | 101     | 2020-01-10 |
    | 2      | Suman  | [suman@gmail.com](mailto:suman@gmail.com)   | 47000  | 102     | 2019-03-15 |
    | 3      | Arjun  | [arjun@gmail.com](mailto:arjun@gmail.com)   | 55000  | 101     | 2021-06-01 |
    | 4      | Kavita | [kavita@gmail.com](mailto:kavita@gmail.com) | 52000  | 103     | 2022-02-20 |


    ### 🏢 departments:
    | dept_id | dept_name | location  | budget  | manager_name |
    | ------- | --------- | --------- | ------- | ------------ |
    | 101     | IT        | Bangalore | 1000000 | Ramesh       |
    | 102     | HR        | Mumbai    | 400000  | Anita        |
    | 104     | Finance   | Delhi     | 800000  | Mehta        |


    ### **1. INNER JOIN -**
    * **Only matching rows from both tables.**
    * **For INNER JOIN, order does NOT change result. These two are same:**<br>
    `FROM employees e INNER JOIN departments d`<br>
    `FROM departments d INNER JOIN employees e`
    * ### ✅ Query + Result:
        ```sql
        SELECT e.emp_id, e.name, e.salary,
               d.dept_name, d.location
        FROM employees e
        INNER JOIN departments d
        ON e.dept_id = d.dept_id;
        ```
        | emp_id | name  | salary | dept_name | location  |
        | ------ | ----- | ------ | --------- | --------- |
        | 1      | Rahul | 50000  | IT        | Bangalore |
        | 3      | Arjun | 55000  | IT        | Bangalore |
        | 2      | Suman | 47000  | HR        | Mumbai    |


    ### **2. LEFT JOIN -**
    * **Keep ALL rows from the left table and only matching rows from the right table. If no match → NULL.**
    * **Order `MATTERS` in LEFT JOIN.**
    * ### ✅ Query + Result:
        ```sql
        SELECT e.emp_id, e.name, e.salary,
               d.dept_name, d.location
        FROM employees e
        LEFT JOIN departments d
        ON e.dept_id = d.dept_id;
        ```
        | emp_id | name   | salary | dept_name | location  |
        | ------ | ------ | ------ | --------- | --------- |
        | 1      | Rahul  | 50000  | IT        | Bangalore |
        | 3      | Arjun  | 55000  | IT        | Bangalore |
        | 2      | Suman  | 47000  | HR        | Mumbai    |
        | 4      | Kavita | 52000  | NULL      | NULL      |


     ### **3. RIGHT JOIN -**
    * **Keep ALL rows from the right table and only matching rows from the left table. If no match → NULL.**
    * **Order `MATTERS` in RIGHT JOIN.**
    * ### ✅ Query + Result:
        ```sql
        SELECT e.emp_id, e.name,  
               d.dept_name, d.location
        FROM employees e
        RIGHT JOIN departments d
        ON e.dept_id = d.dept_id;
        ```
        | emp_id | name  | dept_name | location  |
        | ------ | ----- | --------- | --------- |
        | 1      | Rahul | IT        | Bangalore |
        | 3      | Arjun | IT        | Bangalore |
        | 2      | Suman | HR        | Mumbai    |
        | NULL   | NULL  | Finance   | Delhi     |


    ### **4. FULL JOIN -**
    * **FULL JOIN returns all rows from both tables. If there is no match → the missing side becomes NULL.**
    * ### ✅ Query + Result:
        ```sql
        SELECT e.emp_id, e.name,
               d.dept_name
        FROM employees e
        FULL JOIN departments d
        ON e.dept_id = d.dept_id;
        ```
        | emp_id | name   | dept_name |
        | ------ | ------ | --------- |
        | 1      | Rahul  | IT        |
        | 3      | Arjun  | IT        |
        | 2      | Suman  | HR        |
        | 4      | Kavita | NULL      |
        | NULL   | NULL   | Finance   |

    ### **5. Self JOIN -**
    * **SELF JOIN is when a table is joined with itself.**
    * Used when rows in the same table are related to other rows in that same table.
    * **Aliases** are compulsory to differentiate the two copies.
    * ### ✅ Query + Result:
        ```sql
        select distinct m1.firstname, m1.surname
        from cd.members m1, cd.members m2
        where m1.memid = m2.recommendedby
        order by m1.surname, m1.firstname
        ```
    *   `m1` → Represents the person who recommended someone
    *   `m2` → Represents the person who was recommended
        | memid | name  | recommendedby |
        | ----- | ----- | ------------- |
        | 1     | Rahul | NULL          |
        | 2     | Suman | 1             |
        | 3     | Arjun | 1             |

<br>

## **7. SQL DDL and DML Syntax Reference**

## 🔹 DDL (Data Definition Language)
*   
    ### 1️⃣ CREATE - Creates new database objects (table, database, etc.).

    #### Create Table - 

    ``` sql
    CREATE TABLE table_name (
        column1 datatype constraints,
        column2 datatype,
        column3 datatype
    );
    ```

    #### Create Database

    ``` sql
    CREATE DATABASE database_name;
    ```

------------------------------------------------------------------------
* 
    ### 2️⃣ ALTER - Changes the structure of an existing table.

    #### Add Column

    ``` sql
    ALTER TABLE table_name
    ADD COLUMN column_name datatype;
    ```

    #### Modify Column

    ``` sql
    ALTER TABLE table_name
    ALTER COLUMN column_name TYPE new_datatype;
    ```

    #### Drop Column

    ``` sql
    ALTER TABLE table_name
    DROP COLUMN column_name;
    ```

------------------------------------------------------------------------

* 
    ### 3️⃣ DROP - Permanently removes the object and its data.

    #### Drop Table

    ``` sql
    DROP TABLE table_name;
    ```

    #### Drop Database

    ``` sql
    DROP DATABASE database_name;
    ```

------------------------------------------------------------------------
* 
    ### 4️⃣ TRUNCATE - Removes all rows quickly but keeps the table structure.

    ``` sql
    TRUNCATE TABLE table_name;
    ```

    Removes all rows but keeps table structure.

------------------------------------------------------------------------

## 🔹 DML (Data Manipulation Language)
* 
    ### 1️⃣ INSERT - Adds new data into a table.

    ``` sql
    INSERT INTO table_name (column1, column2)
    VALUES (value1, value2);
    ```

    Insert all columns:

    ``` sql
    INSERT INTO table_name
    VALUES (value1, value2, value3);
    ```

------------------------------------------------------------------------
* 
    ### 2️⃣ UPDATE - Modifies existing data.

    ``` sql
    UPDATE table_name
    SET column1 = value1,
        column2 = value2
    WHERE condition;
    ```

------------------------------------------------------------------------
* 
    ### 3️⃣ DELETE - Removes specific data from a table.

    ``` sql
    DELETE FROM table_name
    WHERE condition;
    ```

    Delete all rows:

    ``` sql
    DELETE FROM table_name;
    ```

------------------------------------------------------------------------
* 
    ## 🎯 Quick Comparison
    ```
    Command  |  Removes Data | Removes Structure
    ---------| --------------| -------------------
    DELETE   |  Yes          |  No
    TRUNCATE |  Yes (all)    |  No
    DROP     |  Yes          |  Yes
    ```

<br>

# Normalization
### ✅ organizing tables to remove redundancy and avoid anomalies.

### Types: 
1. **1NF**
2. **2NF**
3. **3NF**

    ### 1. 1NF 
    * **👉 Rules:** 
        - **Every column should have a single value.**
        - **Each row should be unique, either through a single or multiple columns. Not mandatory to have primary key.**

    ### 2. 2NF
    * **👉 Rules:**
        - **Must be in 1NF**
        -  **No partial dependency** `(Every non-key column must depend on the full composite key)`

            | Dependency Type | Non-key column Depends On       | 2NF?   |
            | --------------- | ------------------------------- | ------ |
            | Partial         |      A only or B only           | ❌ No  |
            | Full            |      A + B together             | ✅ Yes |
    
    * **composite key**: A primary key made using two or more columns together.


    ### 3. 3NF
    * **👉 Rules:**
        - **Must be in 2NF**
        -  **No transitive dependency** `(Non-key column should NOT depend on another non-key column)`
