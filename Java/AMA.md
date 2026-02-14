### 1. Why Static Method Cannot Access Non-Static Method?
* #### Static methods cannot access non-static methods directly because non-static methods require an object reference, and static methods do not have an object context.

### 2. Can we give synchronized for a constructor? and if yes/no why?
* #### No, we cannot declare a constructor as synchronized.

    #### 🔥 Why Not Allowed?
    * Because synchronization works on an already existing object’s monitor.
    * But during constructor execution:
        * The object is still being created.
        * The reference is not yet safely shared with other threads.
    * So constructor execution is already effectively thread-safe.

    #### ✅ But we can use synchronized Block Inside Constructor


### 3. What is Functional Interface?
* #### An interface having only one abstract method is called Functional Interface.

### 4. Can a functional interface have default methods & static methods?
* #### Yes — a functional interface can have:
    * ✔ default methods
    * ✔ static methods
* Condition: 👉 It must have only one abstract method.