# Hibernate / JPA Learning

## 1. ORM Fundamentals

-   What is ORM (Object Relational Mapping)
-   Problems with JDBC
-   Benefits of ORM
-   ORM tools overview
* **Entity** - `An Entity is a Java class that represents a table in the database.`
* **EntityManager** - `Is a JPA interface which manages the entities created by the developer and handles all database operations on them — like saving, finding, updating and deleting.`



    ### 1️⃣ What is ORM (Object Relational Mapping)?
    * `ORM is a concept/technique, that maps Java Class  to a  Database Table.`

    ### 2️⃣ What is JPA (Java Persistence API)?
    * `JPA is a specification that defines rules and provides interfaces.`
    * **specification** - A set of rules and interfaces that define how something should work, but do not provide the actual implementation.
    * **But JPA does NOT perform ORM itself. It only defines how ORM should work.**

    * **Some JPA Implementations:** 
        | Implementation  | Maintained By      | Popularity | Used In               |
        | --------------- | ------------------ | ---------- | --------------------- |
        | **Hibernate**   | Red Hat            | ⭐⭐⭐⭐⭐      | Spring Boot           |
        | **EclipseLink** | Eclipse Foundation | ⭐⭐⭐        | Jakarta EE            |
        | **OpenJPA**     | Apache             | ⭐⭐         | WebSphere             |
        | **DataNucleus** | DataNucleus team   | ⭐          | Specialized platforms |



    ### 3️⃣ What is Hibernate?
    * `Hibernate is a framework that implements JPA and performs the ORM.`

##

<br>

## Spring Data JPA - (Java Persistence Query Language)

## 1. Important Rule

`@Query` uses **JPQL by default**, not SQL.

JPQL works with: - **Entity classes** - **Entity fields**

It does **not** use table names or column names.

------------------------------------------------------------------------

## 2. Wrong Example (SQL used in JPQL)

``` java
@Query("select * from student where firstname = ?1")
List<Student> findByFirstName(String firstName);
```

Why this is wrong: - `student` is a **table name** - `select *` is SQL
syntax - JPQL requires **entity names and fields**

------------------------------------------------------------------------

## 3. Correct JPQL Example

``` java
@Query("SELECT s FROM Student s WHERE s.firstName = ?1")
List<Student> findByFirstName(String firstName);
```

Explanation:

  Part            Meaning
  --------------- --------------
  `Student`       Entity class
  `s`             Alias
  `s.firstName`   Entity field

JPQL mapping:

    Entity class  -> Table
    Object        -> Row
    Field         -> Column

------------------------------------------------------------------------

## 4. Native SQL Query

If you want to use **real SQL**, you must enable `nativeQuery = true`.

``` java
@Query(value = "SELECT * FROM student WHERE firstname = ?1", nativeQuery = true)
List<Student> findByFirstName(String firstName);
```

Now the query uses: - Table name - Column name - SQL syntax

------------------------------------------------------------------------

## 5. Best Spring Data JPA Approach

Spring Data can generate queries automatically from method names.

Example:

``` java
List<Student> findByFirstName(String firstName);
```

Spring automatically generates the JPQL query internally.

`JPA uses Domain Specific Language (DSL)`
------------------------------------------------------------------------

## 6. Summary

  Type          | Example
  --------------| --------------------------------------------------
  JPQL          | `SELECT s FROM Student s WHERE s.firstName = ?1`
  Native SQL    | `SELECT * FROM student WHERE firstname = ?1`
  Method Query  | `findByFirstName(String firstName)`

------------------------------------------------------------------------

## 7. Key Concept

    JPQL → uses Entity and fields
    SQL  → uses Tables and columns