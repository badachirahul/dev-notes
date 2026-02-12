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


## Synchronization
### 1. What is Synchronization?
- Synchronization is a mechanism that allows only one thread at a time to access a shared resource.
### 2. Difference between instance synchronized and static synchronized?
#### i.  Instance synchronized: 
* **public synchronized void method() { }**
* 🔐 Lock is on:
    - 👉 Object (this)
* 🧠 Meaning:
    - Each object has its own lock. 
    - If 5 objects exist → 5 locks exist.
* Threads using different objects do NOT block each other.


#### ii. Static synchronized:
* **public static synchronized void method() { }**
* 🔐 Lock is on:
    - Class (ClassName.class)
* 🧠 Meaning:
    - Only ONE class lock exists.
    - Even if 100 objects exist → still 1 lock.
* Only ONE thread can enter at a time (for that class).
