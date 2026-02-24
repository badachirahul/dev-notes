
# 🔥 **Executors framework**

## 1️⃣ Basics

-   **What is Executor Framework?**
-   **Why it was introduced?**
-   **Difference between Thread vs Executor**

------------------------------------------------------------------------

## 2️⃣ Core Interfaces

-   **Executor**
-   **ExecutorService**
-   **ScheduledExecutorService**

------------------------------------------------------------------------

## 3️⃣ Thread Pools

-   **newFixedThreadPool()**
-   **newSingleThreadExecutor()**
-   **newCachedThreadPool()**
-   **newScheduledThreadPool()**

### 🔎 Understand:

-   How many threads created?
-   How tasks are queued?
-   When to use which?

------------------------------------------------------------------------

## 4️⃣ Task Submission Methods

-   **execute()**
-   **submit()**
-   Difference between them

------------------------------------------------------------------------

## 5️⃣ Callable & Future

-   Why Callable?
-   How Future works?
-   **get()**
-   Blocking behavior

------------------------------------------------------------------------

## 6️⃣ Shutdown Mechanism

-   **shutdown()**
-   **shutdownNow()**
-   **awaitTermination()**
-   **isShutdown()**
-   **isTerminated()**

------------------------------------------------------------------------

## 7️⃣ ThreadPool Internals (Important for Interviews)

-   Core pool size
-   Max pool size
-   Queue
-   Keep alive time
-   Rejection policy

*(Using ThreadPoolExecutor directly)*

------------------------------------------------------------------------

## 8️⃣ Scheduled Tasks

-   Run task after delay
-   Run task periodically

------------------------------------------------------------------------

<br>

## 1️⃣ **Basics**
* 
    ### 1. What is Executor Framework?
    * **Thread is low-level way to create threads manually, while `Executor Framework is a high-level API that manages threads efficiently using thread pools.`**

    ### 2. Why it was introduced?
        Problems with manual threads:

        ❌ Hard to manage many threads
        ❌ No thread reuse
        ❌ No thread pooling
        ❌ No return value support (Runnable)

    ### 3. What is Thread Pool?
    * `Thread pool is a pool of already created threads that are reused to execute tasks.`

    
## 2️⃣ **Core Interfaces**
* 
    ### **1. Executor**

    ``` java
    public interface Executor {
        void execute(Runnable command);
    }
    ```

    ✅ Key Points:
    - Only one method → execute()
    - Executes Runnable
    - Does NOT return result
    - No lifecycle control (shutdown etc.)

    👉 It is a simple task submission interface

    ------------------------------------------------------------------------

    ### **2. ExecutorService**

    - Extends `Executor`
    - **ExecutorService** → `Task + Lifecycle + Result`

    ``` java
    public interface ExecutorService extends Executor
    ```

    ✅ Adds:
    - `submit()` → **Takes Runnable or Callable Returns Future**
    - `shutdown()` → **Stops accepting new tasks, but finish already submitted tasks.**
        - Graceful termination
        - Running + queued tasks complete
    - `shutdownNow()` → **Stops accepting new tasks, interrupt running tasks, and remove waiting tasks.**
        - Returns list of tasks that never started
        - Does NOT forcibly kill threads (depends on interruption handling)
    - `invokeAll()` → **Executes a collection of tasks and waits until ALL complete.**
        - Returns List<Future>
        - Blocking method
        - Useful for batch parallel processing    
    - `invokeAny()` → **Executes a collection of tasks and returns result of FIRST completed task.**
        - Cancels remaining tasks
        - Blocking method
        - Useful when fastest result wins

    🔥 Important:
    - Supports Callable
    -  Returns result using Future
    - Can manage lifecycle

    ```java
    ExecutorService executor = Executors.newFixedThreadPool(5);
    Future<Integer> future = executor.submit(() -> 10);
    ```

    ------------------------------------------------------------------------

    ### **3. ScheduledExecutorService**

    - Extends `ExecutorService`
    - Used for `Time-based task execution`

    ``` java
    public interface ScheduledExecutorService extends ExecutorService
    ```

    ✅ Used when we need to:
    - Run task after delay
    - Run task repeatedly

    Example:

    ``` java
    ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);
    scheduler.schedule(task, 5, TimeUnit.SECONDS);
    ```

    ------------------------------------------------------------------------

    ## 🔥 Hierarchy

        Executor
        ↓
        ExecutorService
        ↓
        ScheduledExecutorService

<br>

## 3️⃣ **Types of Thread Pools**

1.  **newFixedThreadPool()**
2.  **newSingleThreadExecutor()**
3.  **newCachedThreadPool()**
4.  **newScheduledThreadPool()**
* 
    ## **1. newFixedThreadPool(n)**

    ``` java
    ExecutorService service = Executors.newFixedThreadPool(n);
    ```

    ### 🔹 Threads Created

    -   Exactly n threads
    -   Created when needed
    -   Reused for new tasks

    ### 🔹 Queue Type

    -   Uses unbounded queue (LinkedBlockingQueue)
    -   If all threads busy → tasks wait in queue

    ### 🔹 Behavior

        If 2 threads:

            T1 → Task1  
            T2 → Task2  
            Task3, Task4 → Waiting in queue

    ### 🔹 When To Use

    -   ✔ Limited number of threads
    -   ✔ Control CPU usage
    -   ✔ Most common choice

    ------------------------------------------------------------------------

    ## **2. newSingleThreadExecutor()**

    ``` java
    ExecutorService service = Executors.newSingleThreadExecutor();
    ```

    ### 🔹 Threads Created

    -   Only 1 thread

    ### 🔹 Queue Type

    -   Unbounded queue

    ### 🔹 Behavior

    -   Tasks execute one by one
    -   Order guaranteed (FIFO)

        ```
        Task1 → Task2 → Task3 → Task4
        ```

    ### 🔹 When To Use

    -   ✔ Sequential execution
    -   ✔ Logging systems
    -   ✔ File writing
    -   ✔ Order matters

    ------------------------------------------------------------------------

    ## **3. newCachedThreadPool()**

    ``` java
    ExecutorService service = Executors.newCachedThreadPool();
    ```

    ### 🔹 Threads Created

    -   Creates new thread for each task if needed
    -   Reuses idle threads
    -   No fixed limit

    ### 🔹 Queue Type

    -   No real queue
    -   Uses SynchronousQueue
    -   If no idle thread → new thread created

    ### 🔹 Behavior

    - If 100 tasks: 
        - 👉 May create 100 threads

    ### 🔹 When To Use

    -   ✔ Many short-lived tasks
    -   ✔ Asynchronous tasks
    -   ❌ Not for heavy CPU tasks
    -   ❌ Can cause memory issues

    ------------------------------------------------------------------------

    ## **4. newScheduledThreadPool(n)**

    ``` java
    ScheduledExecutorService service = Executors.newScheduledThreadPool(n);
    ```

    ### 🔹 Threads Created

    -   n threads

    ### 🔹 Queue Type

    -   Delay-based queue

    ### 🔹 Behavior
    - Used for: 
        - Time-based task execution.
        - Periodic tasks

    Example:

    ``` java
    service.schedule(task, 5, TimeUnit.SECONDS);
    ```

    ### 🔹 When To Use

    -   ✔ Timers
    -   ✔ Periodic jobs
    -   ✔ Background maintenance

    ------------------------------------------------------------------------

    ## 🔥 Comparison Table

    | Pool Type | Threads     | Queue             | Best For           |
    |-----------|-------------|-------------------|--------------------|
    | Fixed     | Fixed n     | Unbounded queue   | General purpose    |
    | Single    | 1           | Unbounded queue   | Sequential tasks   |
    | Cached    | Unlimited   | No queue          | Many short tasks   |
    | Scheduled | Fixed n     | Delay queue       | Timed tasks        |

<br>

## 4️⃣ Task Submission Methods
-   **execute()**
-   **submit()**

    | execute()                            | submit()                                    |
    |--------------------------------------|---------------------------------------------|
    | Returns `void`                       | Returns `Future`                            |
    | Accepts only `Runnable`              | Accepts `Runnable` and `Callable`           |
    | Cannot get result                    | Can get result using `get()`                |
    | Cannot cancel task                   | Can cancel task                             |
    | It is a Method of Executor interface | It is a method of ExecutorService interface |
    | Exception printed immediately        | Exception thrown when calling `get()`       |

* Use `execute()` for:
    - Simple tasks
    - No object creation
    - No tracking

* Use `submit()` for:
    - Need to track the result
    - Need cancellation support