# Multithreading

## Basics:

* **Program**: A program is a set of instruction.

* **Process**: A program in execution is called process.

* **Thread**: A thread is the smallest unit of execution within a process.

* **Multi-tasking**: Executing multiple tasks similtaneously at OS level.

### 1. What is Multithreading? 
* **Executing multiple threads concurrently within a single process.**

### 2. Why to use Multithreading:
* Better CPU Utilization
* Faster response
* Develop video games.

#### 🔥 Simple Rule
* 1 Core → Concurrent (time slicing)
* Multiple Cores → Concurrent + Possibly Parallel


# Lock
- A lock is a synchronization mechanism that allows only one thread to access a shared resource at a time.

### Types of Lock
1. **Intrinsic Locks**
2. **Explicit Locks** (from java.util.concurrent.locks)


### **1. Intrinsic Locks:**
* These are the locks automatically provided by Java’s `synchronized` keyword.
* Below are the types:

    #### i.  Instance synchronization: 
        public synchronized void method() { 

        }
    * 🔐 Lock is on:
        - 👉 Object (this)
    * 🧠 Meaning:
        - Each object has its own lock. 
        - If 5 objects exist → 5 locks exist.
    * Threads using different objects do NOT block each other.

    #### ii. Static synchronization:
        public static synchronized void method() { 

        }
    * 🔐 Lock is on:
        - Class (ClassName.class)
    * 🧠 Meaning:
        - Only ONE class lock exists.
        - Even if 100 objects exist → still 1 lock.
    * Only ONE thread can enter at a time (for that class).

    #### iii. Synchronized block:
        public void method() {
            synchronized (lockObject) {
                // critical section
            }
        }

    * 🔐 Lock is on:
        - 👉 The object inside brackets
            lockObject

    * It can be:
        - this
        - Any custom object
        - ClassName.class

    ### 🔥 Comparison of Synchronization Types
    | Feature          | Instance Synchronization  | Static Synchronization     | Synchronization Block     |
    |------------------|---------------------------|----------------------------|---------------------------|
    | Lock Type        | Object (`this`)           | Class (`ClassName.class`)  | Any object you choose     |
    | Lock Scope       | Per object                | Per class                  | Depends on object used    |
    | Lock Area        | Entire method             | Entire method              | Only selected block       |
    | Number of Locks  | One per object            | One per class              | Depends on lock object    |
    | Flexibility      | Low                       | Low                        | Very High                 |
    | Use Case         | Protect object data       | Protect class-level data   | Protect critical section  |



### **2. Explicit Locks**

- Explicit lock is a manually controlled lock mechanism provided by `java.util.concurrent.locks`, mainly using `ReentrantLock`.


    ### 🔹 Basic Structure

    ``` java
    import java.util.concurrent.locks.ReentrantLock;

    ReentrantLock lock = new ReentrantLock();

    lock.lock();
    try {
        // critical section
    } finally {
        lock.unlock();
    }
    ```

    ### 🔹 Why `finally` Block?
        To ensure:

        - Lock is always released
        - Even if an exception occurs

        If you forget `unlock()` → Deadlock risk 🚨
 
    ### 🔹 Instance-Level Explicit Lock

    ``` java
    private final Lock lock = new ReentrantLock();
    ```

    -   Each object has its own lock.
    -   Equivalent to → `instance synchronized`.

    ### 🔹 Class-Level Explicit Lock

    ``` java
    private static final Lock lock = new ReentrantLock();
    ```

    -   All objects share the same lock.
    -   Equivalent to → `static synchronized`.