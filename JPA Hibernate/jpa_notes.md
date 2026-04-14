# Hibernate / JPA Learning

* **Entity -** `An Entity is a Java class that represents a table in the database.`
* **EntityManager -** 
    - `Is a JPA interface which manages the entities created by the developer and handles all database operations on them like saving, finding, updating and deleting.`

## 1. @Entity
* `👉 Marks a class as a database table.`
* **⚠️ Rules:**
    - Must have default constructor
    - Should have @Id
    - Class should be non-final

## 2. @Id
* `👉 Marks a field as Primary Key.`

## 3. @Table
* `👉 Used to customize table name.`

## 4. @Column
* `👉 Used to customize column properties.`
* **🧠 You can control:**
    - name
    - nullable
    - length
    - unique

## 5. @GeneratedValue
* `👉 Used to auto-generate ID.`
* **🧠 Strategies:**
    - **IDENTITY →**
        - 👉 `Insert first → DB gives ID back`
        - 👉 ❌ No batching
        - 👉 DB controls everything
    
    - **SEQUENCE →**
        - 👉 `Get IDs first → then insert`
        - 👉 ✅ Batching possible
        - 👉 Hibernate manages IDs (in memory)
         
        ```
        @Id
        @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "post_seq")
        @SequenceGenerator(
            name = "post_seq",
            sequenceName = "post_sequence",
            allocationSize = 50
        )
        private Long id;
        ```

    - **AUTO →**
        -  👉 “Let JPA decide (based on DB)”
        - 👉 🤷 Not predictable
        - 👉 Internally becomes IDENTITY or SEQUENCE
    
    - **TABLE  →**
        - 👉 “Use a separate table to generate IDs”
        - 👉 ❌ Slow (extra DB calls)
        - 👉 Rarely used
   
* **🚀 How to enable batching (Hibernate):**
    ```
    In application.properties:

    spring.jpa.properties.hibernate.jdbc.batch_size=50
    spring.jpa.properties.hibernate.order_inserts=true
    spring.jpa.properties.hibernate.order_updates=true
    ```

<br>

## **Hibernate - Entity Lifecycle**
* **Dirty checking:** `👉 Hibernate automatically detects changes when the entity is in persistant state and updates to DB when transaction is flushed/committed this is dirty checking.`

* 👉 **Flush:** 
    - SQL executed in DB but still part of transaction 
    - Can rollback
* 👉 **Commit:**
    - transaction completed, changes are permanent
    - Cannot rollback.

    | Stage        | User A sees | User B sees |
    | ------------ | ----------- | ----------- |
    | Before flush | Old         | Old         |
    | After flush  | New         | Old ❗       |
    | After commit | New         | New ✅       |

<br>

## **Entity Relationships**

1. `OneToOne`
2. `OneToMany or ManyToOne`
3. `ManyToMany`

| Concept     | Meaning           |
| ----------- | ----------------- |
| Owning side | FK holder         |
| Child       | FK holder         |
| Parent      | Referenced entity |


### 2. `OneToMany` or `ManyToOne`

* 🔥 Rule (never forget)
    - `@ManyToOne` → `Owning side` → `@JsonBackReference`
    - `@OneToMany` → `Inverse side` → `@JsonManagedReference` <br><br>
🧠 Why always ManyToOne owns?

* Because: 👉 Foreign key is always on “many” side (comment table has post_id)