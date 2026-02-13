# Multithreading

## Basics:

* **Program**: A program is a set of instruction.

* **Process**: A program under execution is called process.

* **Thread**: A thread is the smallest unit of execution within a process.

* **Multi-tasking**: Executing multiple tasks similtaneously at OS level.

### 1. What is Multithreading? 
* **Executing multiple threads concurrently within a single process.**

### 2. Why to use Multithreading:
* To utilize CPU time efficiently
* Faster response
* Develop video games.

#### 🔥 Simple Rule
* 1 Core → Concurrent (time slicing)
* Multiple Cores → Concurrent + Possibly Parallel

<br>

# **Lock**
- A lock is a synchronization mechanism that allows only one thread to access a shared resource at a time.

### Types of Lock
1. **Intrinsic Locks**
2. **Explicit Locks** (from java.util.concurrent.locks)


## **1. Intrinsic Locks:**
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

    ### Disadvantages of Synchronized (Intrinsic Lock)
    *   No fairness policy
    *   No try-lock support (only blocking)
    *   No interruptible locking
    *   No read/write lock support    

## **2. Explicit Locks**

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

    ## 🔐 Important Methods of Lock (ReentrantLock)

    ### 🔹 1. lock()

    -   Acquires the lock.
    -   If lock is not available, the thread waits.

    ``` java
    lock.lock();
    ```

    ------------------------------------------------------------------------

    ### 🔹 2. unlock()

    -   Releases the lock.
    -   Must be called after `lock()`.

    ``` java
    lock.unlock();
    ```

    ------------------------------------------------------------------------

    ### 🔹 3. tryLock()

    -   Attempts to acquire the lock.
    -   Does NOT wait.
    -   Returns `true` if successful, otherwise `false`.

    ``` java
    if (lock.tryLock()) {
        try {
            // critical section
        } finally {
            lock.unlock();
        }
    }
    ```

    ------------------------------------------------------------------------

    ### 🔹 4. tryLock(long time, TimeUnit unit)

    -   Waits for a specific time to acquire the lock.
    -   Returns `true` if lock is acquired within the time limit.

    ``` java
    lock.tryLock(5, TimeUnit.SECONDS);
    ```

    ------------------------------------------------------------------------

    ### 🔹 5. lockInterruptibly()

    -   Acquires the lock but allows interruption while waiting.

    ``` java
    lock.lockInterruptibly();
    ```

    ------------------------------------------------------------------------

    ### 🔹 6. newCondition()

    -   Creates a `Condition` object.
    -   Used for advanced thread communication (alternative to wait/notify).

    ``` java
    Condition condition = lock.newCondition();
    ```
    <br>

    ### ⚠️ Starvation
    - Starvation occurs when a thread does not get a chance to access a required resource for a long time.
        - It’s not just CPU execution:
        - It can be: 
            - Not getting lock
            - Not getting CPU time
            - Not getting resource
    - Happens when other threads continuously get priority.
    - `synchronized` may cause starvation due to lack of fairness policy.
    
    - To overcome this, we use **Fairness policy**


    ### 🔥 Fairness Policy
    * Fair lock: Threads acquire the lock in the order they requested it (first-come-first-serve).
    ``` java
    ReentrantLock fairLock = new ReentrantLock(true); // Fair lock
    ```
    <br>

    ## 🔐 Read/Write Lock (ReentrantReadWriteLock)

    ### 🔹 What is Read/Write Lock?

    A **ReadWriteLock** allows:

    -   Multiple threads to **read** at the same time ✅
    -   Only one thread to **write** at a time ❌

    ### Rules:

    -   When writing → no one can read
    -   When reading → no one can write

    ------------------------------------------------------------------------

    ## 🧠 Why Do We Need It?

    With normal `synchronized`:

    -   Only one thread is allowed at a time (even for reading)
    -   Reduces performance in read-heavy systems

    ReadWriteLock improves performance when:

    -   Many read operations
    -   Few write operations

    <br>

    ## 🔒 Thread Blocking Methods
    ### 🧵 From Thread class
    * Thread.sleep()
    * Thread.join()

    ### 🧵 From Object class
    * wait()

    ### 🔐 From Lock interface
    * lock()
    * lockInterruptibly()
    * tryLock(long time, TimeUnit unit)

    ### 🟢 Interruptible Blocking Methods (Throws InterruptedException)
    * sleep()
    * join()
    * wait()
    * lockInterruptibly()
    * tryLock(timeout)
    * BlockingQueue.take()
    * These throw InterruptedException.

<br>

## **Daemon Threads**
#### -> Daemon threads are background threads that support user threads and cannot prevent the JVM from exiting.

### 🔹Important Rules – Daemon Thread (Java)
- ✔ Must call `setDaemon(true)` **before** `start()`
- ❌ If you call `setDaemon(true)` after `start()` → `IllegalThreadStateException`
- ✔ Use `isDaemon()` to check whether a thread is daemon or not

    #### 👉 Default Behavior
    - Main thread → ❌ Not a daemon thread
    - Threads created by main → ❌ Not daemon (unless explicitly set)

### 🔹Daemon Thread Inheritance Rule (Java):
- 👉 A new thread **inherits the daemon status of its parent thread**.
- If parent is **non-daemon (like main thread)** → new thread is **non-daemon**
- If parent is **daemon** → new thread is **daemon**

### ⚠️ Never use daemon thread for:
- Critical business logic
- Database updates
- File saving
- Important transactions

### 🟢 Daemon threads are for:
- Background tasks
- Monitoring
- Logging
- Garbage collection
