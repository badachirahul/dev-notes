# Generics

## Slide 1: Why Generics introduced?
* Let's understand with example.

* Ex: 
    ```java
    “Okay, first let’s understand why generics were introduced using an example. Let’s go back before Java 5.

    Suppose we are getting data from the frontend, and the data type is names. So I create an ArrayList.”

    List names = new ArrayList();
    names.add("Doremon");
    names.add("Nobita");
    names.add("Shizuka");
    names.add("Suneo");
    names.add("Gian");

    “Now, by mistake, someone adds a number into the list.”

    names.add(52);

    “Later, while retrieving data, we assume everything is a String.”

    String name = (String) names.get(5);

    “At runtime, this throws a ClassCastException.”
    ```
## Slide 2: Generics were introduced for:
    - Type safety
    - Remove manual casting
    - Avoid runtime errors (like ClassCastException)
    - Code reuse
    - Better code readability


## Slide 3: Let's understand how 'Generics' work?
* `The compiler first checks the entire code for type safety. If everything is correct, it then performs type erasure and generates bytecode.`

    - Compiler checks type safety
    - If no errors → applies Type Erasure
    - Then generates bytecode (.class)

* Step by step working during Compile Time:
    - Step 1: Compiler checks type correctness
        ```
        
        List<String> list = new ArrayList<>();

        If you try:

        list.add(10); // ❌ compile-time error
        ```

    - Step 2: Type Erasure
        ```
        Compiler removes generic type information

        👉 Converted to:

        List list = new ArrayList();
        ```
    
    - Step 3: Code converted to byte code

    - Step 4: Runtime
        ```
        JVM sees only raw types
        No <String> exists
        ```

2. **Why Type Erasure removes Generics at Compile Time?**
    - `Backward Compatibility` 
        - Before Java 5:
            ```
            List list = new ArrayList();
            ```

        - After Java 5:
            ```
            List<String> list = new ArrayList<>();

            👉 Old code must still work with new JVM ❗
            ```

        - So Java people decided:
            ```
            Don’t change JVM
            Remove generics before runtime
            ```

## Slide 4: Type Parameter Naming Conventions

### 💡 What it means

When we write generics like:

```java
class Box<T> {}
```

👉 `T` is just a **name**, but Java follows **standard conventions** for readability.

---

## 📌 Common Conventions

- **T** → **Type** (general purpose)
- **E** → **Element** (used in collections like List)
- **K** → **Key** (used in Map)
- **V** → **Value** (used in Map)
- **N** → **Number**



## Slide 5: Generic Class
* 👉 Generic classes are used to:
    - ✔ Make custom classes type-safe
    - ✔ Make them reusable for different data types
* Syntax: 
    ```Java
    class Box<T> {
        T value;

        void set(T value) {
            this.value = value;
        }

        T get() {
            return value;
        }
    }	

    public class Generics {
        public static void main(String[] args) {
            
            Box<String> b = new Box();
            b.set("Rahul");
            
            String s = b.get();
        }
    }
    ```


## Slide 6: Generic Interface
* `An interface that works with different data types.`

* Syntax: 
    ```Java
    interface Container<T> {
        void add(T item);
        T get();
    }

    class StringContainer implements Container<String> {
        String value;

        public void add(String item) {
            value = item;
        }

        public String get() {
            return value;
        }
    }

    public class Main {
        public static void main(String[] args) {
            Container<String> c = new StringContainer();

            c.add("Hello");

            String s = c.get(); // no cast needed

            System.out.println(s);
        }
    }

    ```

## Slide 7: Bounded Type Parameters
- 🎤 Script:
    ```
    Till now, we saw that generics can accept any type.
    But sometimes, we want to restrict the type.

    This is where bounded type parameters come into picture.

    Using the extends keyword, we can limit the type to a specific class and its subclasses.”
    ```
- Syntax: 
    ```java
    <T extends Number>
    ```

- Example: 

    ```java

    class Box<T extends Number> {
        T value;
    }
    ```

- 👉 In normal Java:
    - Class → extends
    - Interface → implements

- 👉 BUT in generics:
    - extends is used for both class and interface ✅


## Slide 8: Generic Constructor
- 🎤 Script:
    ```
    A generic constructor works independently of the class type and allows us to pass different types of data while creating objects.
    ```

-   Syntax: 
     ```java
    class Box {
        <T> Box(T value) {
            System.out.println(value);
        }
    }
    ```

## Slide 9: Generic Methods
- 🎤 Script:
    ```
    A generic method declares its own type parameter before the return type and allows us to reuse the same method for multiple data types.
    ```

-   Syntax: 
    ```java
    class Demo {
        public <T> void print(T data) {
            System.out.println(data);
        }
    }
    ```

## Slide 10: Wildcard In Generics

🔥 Types of Wildcards

---

### 1️⃣ Unbounded Wildcard

```java
List<?> list;
```

👉 Accepts any type ✔  
👉 Read-only mostly

---

### 2️⃣ Upper Bounded Wildcard

```java
List<? extends Number>
```
* 👉 Can refer to:
    ```
    List<Integer> ✔
    List<Double> ✔
    ```
* ✔ Can read data safely
* ❌ Cannot add elements

---

### 3️⃣ Lower Bounded Wildcard

```java
List<? super Integer>
```

* 👉  Can refer to:
    ```
    List<Integer> ✔
    List<Number> ✔
    List<Object> ✔
    ```
* ✔ Can add elements
* ❌ Limited reading

    - Example:
        ```java
        import java.util.*;

        class Demo {

            // Producer → extends (READ)
            public static void printNumbers(List<? extends Number> list) {
                for (Number n : list) {
                    System.out.println(n);
                }

                // list.add(10); ❌ not allowed
            }

            // Consumer → super (WRITE)
            public static void addNumbers(List<? super Integer> list) {
                list.add(10);
                list.add(20);

                // Integer i = list.get(0); ❌ not safe
            }

            public static void main(String[] args) {

                List<Integer> intList = new ArrayList<>();
                List<Number> numList = new ArrayList<>();
                List<Object> objList = new ArrayList<>();

                // 🔹 Producer (extends)
                printNumbers(intList);  // ✔
                printNumbers(numList);  // ✔
                // printNumbers(objList); ❌

                // 🔹 Consumer (super)
                addNumbers(intList);   // ✔
                addNumbers(numList);   // ✔
                addNumbers(objList);   // ✔

                System.out.println(intList);
                System.out.println(numList);
                System.out.println(objList);
            }
        }
        ```