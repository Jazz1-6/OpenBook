# Example — CSE2006 Java (Units 3 & 4)

Real OpenBook output. Model quality, density, and formatting after this.

---

# CSE2006 — OpenBook Reference Pack
Units: 3 & 4
Exam: CAT-2 (Open Book) | Page budget: 10 | Estimated: 9.8

---

## Table of Contents

- §3.1 Exceptions: Concept, Hierarchy, Types
- §3.2 try / catch / Multiple catch
- §3.3 throw, throws, finally
- §3.4 Built-in & Custom Exceptions
- §3.5 Annotations (Overview)
- §3.6 Multithreading: Intro & Thread Creation
- §3.7 Thread Life Cycle
- §3.8 Synchronization
- §3.9 User-defined Packages
- §3.10 HOTS — Unit 3
- §4.1 String Class, Methods & Operations
- §4.2 1-D, 2-D and Jagged Arrays
- §4.3 Collections Framework Overview
- §4.4 Collection & List Interface
- §4.5 ArrayList, Vector, Stack
- §4.6 Byte vs Character Streams
- §4.7 Java I/O Streams & Reader/Writer
- §4.8 HOTS — Unit 4
- Quick Revision Sheet
- Index

---

## Unit 3 — Exception Handling & Multithreading

### §3.1 Exceptions: Concept, Hierarchy, Types  [S1]

An exception is an event that disrupts the normal flow of a program's instructions during execution. When it occurs, the runtime creates an exception object (type, message, call stack) and hands control to the exception-handling mechanism.

**Error vs Exception**

| Aspect | Error | Exception |
|---|---|---|
| Meaning | Serious JVM/environment problem | Recoverable condition |
| Examples | `OutOfMemoryError`, `StackOverflowError` | `IOException`, `ArithmeticException` |
| Should catch? | No | Yes |

**Hierarchy**

```
Throwable
├── Error
│   ├── OutOfMemoryError
│   └── StackOverflowError
└── Exception
    ├── Checked (compile-time)
    │   ├── IOException
    │   ├── SQLException
    │   └── ClassNotFoundException
    └── RuntimeException (unchecked)
        ├── ArithmeticException
        ├── ArrayIndexOutOfBoundsException
        ├── NullPointerException
        ├── NumberFormatException
        └── ClassCastException
```

- **Checked** exceptions: must be caught or declared with `throws` — compiler enforces.
- **Unchecked** exceptions: subclasses of `RuntimeException`, not compiler-checked; usually programming bugs.

> Exam relevance: Almost always asked as "differentiate Error vs Exception" or "checked vs unchecked".

---

### §3.2 try / catch / Multiple catch  [S1]

Code that may throw goes in `try`. On exception, control jumps to the first matching `catch`; remaining `try` statements are skipped.

**Code — basic try/catch**

```java
public class Divide {
    public static void main(String[] args) {
        int a = 10, b = 0;
        try {
            int result = a / b; // throws ArithmeticException
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            System.out.println("Cannot divide by zero: " + e.getMessage());
        }
        System.out.println("Program continues...");
    }
}
```

**Multiple catch — order matters**

```java
try {
    int[] arr = new int[5];
    arr[10] = 50 / 0; // two possible faults
} catch (ArithmeticException e) {
    System.out.println("Arithmetic problem: " + e);
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Array index problem: " + e);
} catch (Exception e) { // generic catch — MUST be last
    System.out.println("Some other exception: " + e);
}
```

**Rules**

- Java checks catch blocks top-to-bottom; first match wins.
- Subclasses must come **before** superclasses — else "unreachable code" compile error.
- Java 7+: multi-catch with pipe — `catch (ArithmeticException | ArrayIndexOutOfBoundsException e)`.

**Gotcha:** placing `catch (Exception e)` first makes all later catches unreachable → compile error.

---

### §3.3 throw, throws, finally  [S1]

| Keyword | Purpose | Used in |
|---|---|---|
| `throw` | Explicitly raises an exception instance | Inside a method body |
| `throws` | Declares a method may propagate an exception | Method signature |
| `finally` | Block that always executes (cleanup) | After try/catch |

**Code — all three together**

```java
static void checkAge(int age) throws IllegalArgumentException {
    if (age < 18) {
        throw new IllegalArgumentException("Age must be 18 or above");
    }
    System.out.println("Age accepted: " + age);
}

public static void main(String[] args) {
    try {
        checkAge(15);
    } catch (IllegalArgumentException e) {
        System.out.println("Caught: " + e.getMessage());
    } finally {
        System.out.println("Finally block always runs - cleanup here.");
    }
}
```

**Key facts**

- `finally` executes whether or not an exception was thrown — even if `try` contains `return`.
- Standard place to release resources (files, DB connections, streams).
- If `finally` itself has a `return`, it overrides any pending return from `try`/`catch`.

> Exam relevance: "Explain throw vs throws" and "what does finally return" are both recurring.

---

### §3.4 Built-in & Custom Exceptions  [S1]

Java ships ready-made exception classes (`ArithmeticException`, `NullPointerException`, `NumberFormatException`, `ClassCastException`).

Custom exception = extend `Exception` (checked) or `RuntimeException` (unchecked).

**Code — custom checked exception**

```java
class InsufficientBalanceException extends Exception {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}

class BankAccount {
    private double balance = 1000;
    void withdraw(double amount) throws InsufficientBalanceException {
        if (amount > balance) {
            throw new InsufficientBalanceException("Insufficient funds for this withdrawal");
        }
        balance -= amount;
        System.out.println("Withdrawal successful. Balance: " + balance);
    }
}

public class Bank {
    public static void main(String[] args) {
        BankAccount acc = new BankAccount();
        try {
            acc.withdraw(5000);
        } catch (InsufficientBalanceException e) {
            System.out.println("Transaction failed: " + e.getMessage());
        }
    }
}
```

**When to extend which**

| Extend | Result | Use when |
|---|---|---|
| `Exception` | Checked | Business rule caller must handle |
| `RuntimeException` | Unchecked | Programming error |

---

### §3.5 Annotations (Overview)  [S1]

Annotations are metadata tags (prefixed with `@`). They don't change logic directly but can be read by compiler or runtime.

| Annotation | Purpose |
|---|---|
| `@Override` | Verifies method correctly overrides a superclass method |
| `@Deprecated` | Marks element as obsolete |
| `@SuppressWarnings` | Tells compiler to ignore specific warnings |

---

### §3.6 Multithreading: Intro & Thread Creation  [S1]

A thread is the smallest unit of execution within a process. Multithreading lets one program run multiple tasks concurrently, sharing process memory.

**Two ways to create a thread**

```java
// Method 1: extend Thread
class MyThread extends Thread {
    public void run() {
        for (int i = 1; i <= 3; i++)
            System.out.println(Thread.currentThread().getName() + " -> " + i);
    }
}

// Method 2: implement Runnable (preferred)
class MyRunnable implements Runnable {
    public void run() {
        for (int i = 1; i <= 3; i++)
            System.out.println(Thread.currentThread().getName() + " -> " + i);
    }
}

public class ThreadDemo {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start();
        Thread t2 = new Thread(new MyRunnable());
        t2.start();
    }
}
```

**Why prefer Runnable**

- Java allows only single inheritance for classes.
- `extends Thread` blocks extending any other class.
- `implements Runnable` keeps the inheritance slot free and decouples task from thread.

**Gotcha:** calling `run()` directly executes it as a normal method on the current thread — no new thread. Only `start()` creates a new call stack.

---

### §3.7 Thread Life Cycle  [S1]

| State | Description |
|---|---|
| New | Thread created, not started |
| Runnable | Ready to run, waiting for CPU |
| Running | Scheduler allocated CPU |
| Blocked / Waiting | Waiting for lock, I/O, or another thread |
| Terminated | `run()` completed or exception |

**Transition control:** move from Runnable → Running is decided by the JVM/OS thread scheduler — not the programmer. Programmer can only hint via thread priority.

---

### §3.8 Synchronization  [S1]

**Code — synchronized method**

```java
class Counter {
    private int count = 0;
    public synchronized void increment() { count++; }
    public int getCount() { return count; }
}

public class SyncDemo {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) counter.increment();
        };
        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        t1.start(); t2.start();
        t1.join(); t2.join();
        System.out.println("Final count: " + counter.getCount()); // reliably 2000
    }
}
```

- Every Java object has an intrinsic monitor lock.
- `synchronized` method/block: thread must acquire the monitor before entering — only one at a time.
- Race condition = correctness depends on unpredictable thread interleaving (`count++` is not atomic).
- Also possible: `synchronized(this) { /* critical section */ }` — finer-grained locking.

---

### §3.9 User-defined Packages  [S1]

A package groups related classes/interfaces, controls naming collisions and access.

**Code — package declaration + import**

```java
// File: mypack/Calculator.java
package mypack;
public class Calculator {
    public int add(int a, int b) { return a + b; }
}

// File: Test.java
import mypack.Calculator;
public class Test {
    public static void main(String[] args) {
        Calculator c = new Calculator();
        System.out.println(c.add(4, 5));
    }
}
```

Compile: `javac -d . mypack/Calculator.java Test.java`
Run: `java Test`

---

### §3.10 HOTS — Unit 3  [S1]

**Q1 [Conceptual]** Differentiate between an Error and an Exception. Why does Java's design discourage catching Errors in application code?

**A:** An `Error` (e.g., `OutOfMemoryError`, `StackOverflowError`) signals a serious problem in the JVM or environment that application code generally cannot fix — recommended response is to let the program terminate. An `Exception` signals a condition the program logic can reasonably anticipate and recover from. Catching Errors is discouraged because it can hide a corrupted JVM state and lead to unpredictable failures instead of a clean shutdown.

**Q4 [Trace/Output]** Given a `try` block with `return 1` and a `finally` block with `return 2`, what value is returned?

**A:**

```
Step 1: try block prepares return value 1
Step 2: finally block runs before control leaves the method
Step 3: finally returns 2, overriding the pending return
```

Final answer: `test()` returns `2`. This is poor practice — it silently discards the try block's result.

**Q11 [Application]** Two threads increment a shared counter 1000 times each without synchronization. Explain why the final count may not be 2000, then rewrite using `synchronized`.

**A:** Without synchronization, `count++` is not atomic — it is a read, increment, write sequence that can interleave between threads (both read the same value before either writes back), causing lost updates. Marking `increment()` as `synchronized` forces one thread to complete the read-modify-write before the other can begin, guaranteeing 2000. See §3.8 for the fixed code.

---

## Unit 4 — Strings, Arrays, Collections & I/O Streams

### §4.1 String Class, Methods & Operations  [S2]

Strings in Java are **immutable** — once created, content cannot change. Every operation that appears to modify returns a new String.

**Why immutability matters**

- Safe to share across threads
- Safe as HashMap keys (hash code never changes)
- String literals are pooled and reused

**Code — literal vs new**

```java
String s1 = "hello";              // pooled
String s2 = "hello";              // same pooled reference
String s3 = new String("hello");  // new heap object

System.out.println(s1 == s2);      // true  (same pooled reference)
System.out.println(s1 == s3);      // false (different objects)
System.out.println(s1.equals(s3)); // true  (same content)
```

**Gotcha:** `==` compares references, `.equals()` compares content.

---

### §4.2 1-D, 2-D and Jagged Arrays  [S2]

An array is a fixed-size, indexed collection of same-type elements in contiguous memory. Arrays are objects; length via `.length` field (not a method).

**Code — 1-D, 2-D, jagged**

```java
// 1-D
int[] marks = {90, 85, 76, 60};
for (int m : marks) System.out.println(m);

// 2-D rectangular
int[][] matrix = new int[3][3];
for (int i = 0; i < 3; i++)
    for (int j = 0; j < 3; j++)
        matrix[i][j] = i * 3 + j;

// Jagged — rows of different lengths
int[][] jagged = new int[3][];
jagged[0] = new int[]{1};
jagged[1] = new int[]{1, 2};
jagged[2] = new int[]{1, 2, 3};
```

A **jagged array** is an array of arrays where each sub-array can have a different length — useful for variable-length rows (triangular data, students with different subject counts).

---

### §4.3 Collections Framework Overview  [S2]

A unified architecture for storing and manipulating groups of objects.

```
Collection (interface)
├── List  → ArrayList, LinkedList, Vector, Stack   (ordered, duplicates OK)
├── Set   → HashSet, LinkedHashSet, TreeSet        (no duplicates)
└── Queue → LinkedList, PriorityQueue              (FIFO / priority)

Map (separate hierarchy, key-value pairs)
└── HashMap, TreeMap, LinkedHashMap
```

---

### §4.4 Collection & List Interface  [S2]

**Collection** defines baseline ops: `add()`, `remove()`, `size()`, `contains()`, `iterator()`, bulk ops `addAll()`/`removeAll()`.

**List extends Collection** and adds index-based access: `get(i)`, `set(i, e)` — plus guarantees insertion order + duplicate elements.

> Note: `Collection` has no `get(int index)` method. Declare reference as `List` if you need indexed access.

---

### §4.5 ArrayList, Vector, Stack  [S2]

| Class | Backing | Thread-safe? | Growth | Use |
|---|---|---|---|---|
| ArrayList | Resizable array | No | 1.5× | General-purpose list, fast random access |
| Vector | Resizable array | Yes (synchronized) | 2× (legacy) | Legacy thread-safe list |
| Stack | Extends Vector | Yes (inherited) | Same as Vector | LIFO: push/pop/peek |

**Code — usage**

```java
ArrayList<String> list = new ArrayList<>();
list.add("Java");
list.add("Python");
list.add("Java"); // duplicates allowed

Vector<Integer> vec = new Vector<>();
vec.add(10); vec.add(20);

Stack<Integer> stack = new Stack<>();
stack.push(1); stack.push(2); stack.push(3);
System.out.println(stack.pop());  // 3 (LIFO)
System.out.println(stack.peek()); // 2 (top, not removed)
```

---

### §4.6 Byte vs Character Streams  [S2]

Java I/O has two parallel class hierarchies.

| Aspect | Byte Streams | Character Streams |
|---|---|---|
| Base classes | `InputStream` / `OutputStream` | `Reader` / `Writer` |
| Data unit | 8-bit byte | 16-bit Unicode char |
| Handles encoding | No | Yes (automatic) |
| Use for | Images, audio, binary | Text files |

**Gotcha:** reading a JPEG with `FileReader` corrupts binary data — use `FileInputStream`.

---

### §4.7 Java I/O Streams & Reader/Writer  [S2]

**Code — byte + character I/O with try-with-resources**

```java
import java.io.*;

public class FileCopyDemo {
    public static void main(String[] args) throws IOException {
        // Byte stream copy — any file type
        try (FileInputStream fis = new FileInputStream("source.dat");
             FileOutputStream fos = new FileOutputStream("dest.dat")) {
            int data;
            while ((data = fis.read()) != -1) fos.write(data);
        }

        // Character stream — read text line by line
        try (BufferedReader br = new BufferedReader(new FileReader("notes.txt"))) {
            String line;
            while ((line = br.readLine()) != null) System.out.println(line);
        }

        // Character stream — write text
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"))) {
            bw.write("Hello, Java I/O!");
            bw.newLine();
        }
    }
}
```

`try-with-resources` auto-closes each stream (they implement `AutoCloseable`), preventing leaks even if an exception is thrown.

---

### §4.8 HOTS — Unit 4  [S2]

**Q1 [Conceptual]** Why are String objects immutable in Java? What practical problems would arise if Strings were mutable?

**A:** Strings are immutable so they can be safely shared: the String pool reuses identical literals without risk of corruption, Strings work as HashMap keys (hash code must never change), and immutable objects are inherently thread-safe. If mutable: pooled literals would corrupt each other, HashMap lookups would break, and thread-safety would require synchronization everywhere.

**Q4 [Design]** Compare `StringBuilder` and `StringBuffer`. In a single-threaded file-processing utility that concatenates thousands of lines, which would you choose and why?

**A:** `StringBuffer` methods are synchronized (thread-safe, locking overhead); `StringBuilder` methods are not (faster in single-threaded contexts). For single-threaded file processing, choose `StringBuilder` — synchronization is not needed and would only waste CPU cycles.

**Q9 [Trace/Output]** Given a Vector and an ArrayList populated identically and accessed by two threads, which is safe from data corruption?

**A:**

```
Step 1: Vector's methods are synchronized on the Vector's monitor
Step 2: concurrent access is serialized — no low-level data corruption
Step 3: ArrayList is not synchronized — concurrent modification may corrupt state
Step 4: even Vector's per-method sync does NOT protect compound operations
```

Final answer: Vector is safe for single-method calls; ArrayList is not. For compound ops, use external synchronization on either.

**Q13 [Conceptual]** Why is `try-with-resources` preferred over manually closing streams in `finally`? What interface must a class implement?

**A:** Manual `finally` closing needs null-checks and a nested try-catch (since `close()` can throw), and is easy to get wrong. `try-with-resources` auto-calls `close()` on each declared resource on block exit — cleaner and correctly suppresses/chains close exceptions. A class must implement **`AutoCloseable`** to be usable in try-with-resources.

---

## Quick Revision Sheet

### Unit 3

- Throwable → Error / Exception; Exception → Checked / RuntimeException (unchecked).
- Catch blocks: most specific → most general, else unreachable-code error.
- `finally` always runs, even after `return` in try/catch — used for cleanup.
- `throw` raises an exception instance; `throws` declares possible exceptions in signature.
- `start()` creates a new thread and calls `run()` on it; calling `run()` directly does NOT start a new thread.
- Thread life cycle: New → Runnable → Running → Blocked/Waiting → Terminated.
- `synchronized` prevents race conditions — one thread into a critical section at a time.
- Prefer `Runnable` over `Thread` (keeps single-inheritance slot free).

### Unit 4

- Strings are immutable; use `StringBuilder`/`StringBuffer` for heavy editing.
- String literals are pooled; `new String()` forces a separate heap object.
- Jagged array = array of arrays with rows of different lengths.
- Collection → List (ordered, duplicates OK), Set (unique), Queue (FIFO); Map separate.
- ArrayList fast, NOT synchronized; Vector synchronized (slower); Stack extends Vector (LIFO).
- Byte streams = binary; Character streams = text.
- `BufferedReader`/`BufferedWriter` wrap raw streams for faster line-based I/O.
- `try-with-resources` auto-closes any `AutoCloseable` stream.

---

## Index / Cross-References

| Concept | Section |
|---|---|
| Error vs Exception | §3.1 |
| Checked vs unchecked | §3.1 |
| Multiple catch order | §3.2 |
| throw / throws / finally | §3.3 |
| Custom exceptions | §3.4 |
| Annotations | §3.5 |
| Thread creation | §3.6 |
| Runnable vs Thread | §3.6 |
| Thread life cycle | §3.7 |
| Race condition | §3.8 |
| synchronized | §3.8 |
| Packages | §3.9 |
| String immutability | §4.1 |
| String pool vs new | §4.1 |
| Jagged array | §4.2 |
| Collections hierarchy | §4.3 |
| List vs Collection | §4.4 |
| ArrayList vs Vector | §4.5 |
| Stack LIFO | §4.5 |
| Byte vs char streams | §4.6 |
| try-with-resources | §4.7 |
| AutoCloseable | §4.7 |