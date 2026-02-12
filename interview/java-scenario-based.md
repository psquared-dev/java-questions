<!-- TOC -->
* [Q - You have 500 MB of memory, but the input data size is 2 GB. How would you sort this data and print it line by line in sorted order?](#q---you-have-500-mb-of-memory-but-the-input-data-size-is-2-gb-how-would-you-sort-this-data-and-print-it-line-by-line-in-sorted-order)
  * [Phase 1: The "Divide and Sort" (Creating Runs)](#phase-1-the-divide-and-sort-creating-runs)
  * [Phase 2: The "K-Way Merge" (The Tricky Part)](#phase-2-the-k-way-merge-the-tricky-part)
    * [Why this works](#why-this-works)
    * [Java Implementation Keywords (For the Interview)](#java-implementation-keywords-for-the-interview)
* [Q - Java 8 Streams (Lazy Evaluation)](#q---java-8-streams-lazy-evaluation)
  * [The "Vertical" Execution Flow](#the-vertical-execution-flow)
* [Q- Map vs. FlatMap](#q--map-vs-flatmap)
* [Q - Class Loaders](#q---class-loaders)
  * [1. The Hierarchy (The Chain of Command)](#1-the-hierarchy-the-chain-of-command)
  * [2. The Security Twist (The "Sandboxing" Exception)](#2-the-security-twist-the-sandboxing-exception)
* [Q - In the following single line of code, exactly how many String objects are created in memory?](#q---in-the-following-single-line-of-code-exactly-how-many-string-objects-are-created-in-memory)
  * [1. The Literal (`"abc"`) — Object #1](#1-the-literal-abc--object-1)
  * [2. The Constructor (`new String(...)`) — Object #2](#2-the-constructor-new-string--object-2)
  * [Visual Representation](#visual-representation)
* [Q - You have an ExecutorService configured with a fixed thread pool of 10 threads and a bounded queue of size 100.](#q---you-have-an-executorservice-configured-with-a-fixed-thread-pool-of-10-threads-and-a-bounded-queue-of-size-100)
  * [The Specific Exception: `RejectedExecutionException`](#the-specific-exception-rejectedexecutionexception)
  * [The "Silent Killer" (Default Policy)](#the-silent-killer-default-policy)
  * [Senior Dev Follow-Up: "How do we fix this?"](#senior-dev-follow-up-how-do-we-fix-this)
* [Q - Have you used multithreading? If yes, where have you used it?](#q---have-you-used-multithreading-if-yes-where-have-you-used-it)
  * [The "Whiteboard" Answer](#the-whiteboard-answer)
  * [1. The Architecture (Producer-Consumer)](#1-the-architecture-producer-consumer)
  * [2. The Critical Challenge (The "Bulkhead")](#2-the-critical-challenge-the-bulkhead)
  * [3. The Result](#3-the-result)
<!-- TOC -->

# Q - You have 500 MB of memory, but the input data size is 2 GB. How would you sort this data and print it line by line in sorted order?

This is a classic system design and algorithms interview question. 
The standard solution is called **External Merge Sort**.

Since you cannot load all 2GB into your 500MB RAM, you must break the 
problem down into two phases: **Sort Small Chunks** and then **Merge Them**.

Here is the step-by-step breakdown.

## Phase 1: The "Divide and Sort" (Creating Runs)

We cannot read the whole file, but we *can* read parts of it.

1. **Chunking:** Read the 2GB file in blocks that fit comfortably in memory. 
    To be safe (leaving room for JVM overhead), let's say we read **100MB chunks**.
2. **In-Memory Sort:**
    * Load the first 100MB into a standard list (`ArrayList<String>`).
    * Sort it using standard Java sorting (`Collections.sort()`).
    * Write this sorted data to a temporary file on disk (e.g., `temp_chunk_1.txt`).

3. **Repeat:** Do this for the entire 2GB file.
    * You will end up with **20 temporary files**, where each individual file is perfectly sorted.


## Phase 2: The "K-Way Merge" (The Tricky Part)

Now you have 20 sorted files. You can't just concatenate them 
because `File 1` might have "Apple" and `File 2` might have `"Banana"` and `"Zebra"`. 
You need to merge them in order.

We use a **Min-Heap (Priority Queue)** to solve this.

1. **Open Streams:** Open a `BufferedReader` for all 20 temporary files simultaneously.
2. **Initial Load:** Read just the **first line** from each of the 20 files. Put these 20 lines into a **Min-Heap**.
    * *The Heap ensures the smallest string (alphabetically) is always at the top.*

3. **The Loop:**
    * **Pop:** Extract the smallest item from the Heap.
    * **Write:** Write it to your final `sorted_output.txt`.
    * **Refill:** Check which file that item came from (e.g., File 4). Read the *next* line from 
      File 4 and push it into the Heap.

4. **Repeat:** Continue until the Heap is empty and all files are exhausted.

### Why this works

* **Memory Usage:**
    * In Phase 1, you only hold 100MB at a time.
    * In Phase 2, you only hold **20 strings** (one from each file) in memory at any given second. This is tiny!

* **Result:** The final output file is 2GB and perfectly sorted.

### Java Implementation Keywords (For the Interview)

To impress the interviewer, mention the specific Java classes you would use:

* **`BufferedReader`:** For reading lines efficiently from disk without loading the whole file.
* **`PriorityQueue`:** The Java implementation of a Min-Heap.
* **`Comparable` Wrapper:** You'll need a small wrapper class (e.g., `FileEntry`) that 
  stores the `String line` and the `BufferedReader reader` so you know which file to 
  read from next when you pop an item.


------------


# Q - Java 8 Streams (Lazy Evaluation)

Scenario: You have a list of 1,000,000 integers.

```java
List<Integer> numbers = // ... 1 million numbers ...

Integer result = numbers.stream()
    .map(n -> { 
        System.out.println("Mapping: " + n); 
        return n * 2; 
    })
    .filter(n -> { 
        System.out.println("Filtering: " + n); 
        return n > 10; 
    })
    .findFirst()
    .orElse(null);
```

The Question: How many times will "Mapping: ..." be printed to the console?

1. 1,000,000 times (All mapped first, then filtered).
2. Just enough times until we find a match (Short-circuiting).
3. Something else?

Answer is 2.

Unlike a traditional `for` loop that might process the entire collection horizontally (Row by Row),
Streams process **Vertically** (Element by Element).

## The "Vertical" Execution Flow

1. **Element 1:** Go through `map`  Go through `filter`  Check `findFirst`. (Fail? Next).
2. **Element 2:** Go through `map`  Go through `filter`  Check `findFirst`. (Fail? Next).
3. **Element 3:** Go through `map`  Go through `filter`  Check `findFirst`. (**Success!**  **STOP everything**).

Even if you have 1,000,000 elements, if the *3rd* one matches, the stream pipeline **terminates immediately**.
The other 999,997 elements are never even touched.


------------


# Q- Map vs. FlatMap

You have a `List<Order>` where each Order contains a `List<LineItem>`

```java
List<Order> orders = database.getOrders();

// 1. orders.stream().map(order -> order.getLineItems()) ...
// 2. orders.stream().flatMap(order -> order.getLineItems().stream()) ...
```

What is the specific difference in the return type (Structure) between using `.map()` and `.flatMap()` here?


1. `Stream<List<LineItem>>`
2. `Stream<LineItem>`


------------


# Q - Class Loaders

Scenario: You create a class in your own project with the exact same name and package
as a core Java class: `package java.lang; public class String { ... }`.

The Question: 1. When you run your application and use String, which class gets loaded?

* A) Your custom `java.lang.String`.
* B) The official JDK `java.lang.String`.
* C) The JVM crashes with a security error.

2\. Why? (Name the specific mechanism that enforces this decision).

(Hint: Think about the hierarchy of ClassLoaders: Bootstrap → Extension → Application).

Answer is B

## 1. The Hierarchy (The Chain of Command)

Java ClassLoaders are hierarchical. When you ask for a class, the request goes **UP**, not down.

1. **Application ClassLoader:** "I need `java.lang.String`." (Delegates to parent).
2. **Platform (Extension) ClassLoader:** "I need `java.lang.String`." (Delegates to parent).
3. **Bootstrap ClassLoader:** "I found it in the core JDK modules (java.base)!"
    * **Loads the real String class.**
    * **Returns it down the chain.**

Your custom `java.lang.String` sitting in your classpath is effectively **invisible**.
The Application ClassLoader never even gets a chance to look for it because the parent already found it.

## 2. The Security Twist (The "Sandboxing" Exception)

What if you try to create a *new* class in that package, like `java.lang.MyString`?

* The Bootstrap ClassLoader says: "I don't have this."
* The Application ClassLoader tries to load it from your code.
* **CRASH:** `java.lang.SecurityException: Prohibited package name: java.lang`.

**Why?**
The JVM protects the core `java.*` packages. If it didn't, you could write a
class called `java.lang.Integer` that steals data or breaks memory safety, and trick other 
parts of the system into using it.


------------


# Q - In the following single line of code, exactly how many String objects are created in memory?

```java
String s = new String("abc");
```

The Answer is 2.

Here is exactly why:

## 1. The Literal (`"abc"`) — Object #1

The moment the JVM sees the string literal `"abc"` in your code, it
checks the **String Constant Pool** (a special area in the Heap).

* **If "abc" is not there:** It creates a new String object with the value "abc" and places it in the Pool.
* **If "abc" is there:** It just returns a reference to the existing one.
* **In this case (first time):** It creates **Object #1** in the Pool.

## 2. The Constructor (`new String(...)`) — Object #2

The keyword `new` **always** forces the creation of a new object in
the main **Heap** memory (outside the Pool).

* It takes the value "abc" from the Pool object.
* It creates a *copy* of that data into a brand new memory location.
* **In this case:** It creates **Object #2** in the Heap.

## Visual Representation

```text
Heap Memory
 ├── String Constant Pool
 │    └── "abc"  (Object #1: The Literal)
 │
 └── Main Heap Area
      └── String @Address100  (Object #2: The 'new' Object)
           └── value: "abc"

```

So, the variable `s` points to **Object #2**.


------------


# Q - You have an ExecutorService configured with a fixed thread pool of 10 threads and a bounded queue of size 100.

```java
new ThreadPoolExecutor(10, 10, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue<Runnable>(100));
```

Scenario:

1. Traffic spikes.
2. 10 tasks are running (Threads are busy).
3. 100 tasks are waiting (Queue is full).
4. Task #111 arrives.

What happens to Task #111? Does the application crash? Does it hang? Or does something else happen?
Please name the specific concept/mechanism involved.

You are correct! By default, the application does **not** crash or hang - it throws a runtime exception.

## The Specific Exception: `RejectedExecutionException`

Here is the flow:

1. **Core Threads (10):** Busy.
2. **Queue (100):** Full.
3. **Task #111:** The Executor says, "I have no threads and no space."
4. **Action:** It triggers the **Rejection Policy**.

## The "Silent Killer" (Default Policy)

The default policy is **`AbortPolicy`**.

* **Behavior:** It throws `RejectedExecutionException`.
* **Impact:** If you don't catch this exception in your code, that specific
  task (Task #111) is **lost forever**. The user gets a 500 error, and the request is dropped.

## Senior Dev Follow-Up: "How do we fix this?"

In a production system, you almost never want to just crash on overload. You change the policy:

* **`CallerRunsPolicy` (The Throttle):**
* **Behavior:** The thread that *submitted* the task (usually the main HTTP thread) is forced
  to execute the task itself.
* **Result:** This slows down the input rate naturally because the submitter is busy working.
  It prevents data loss and provides automatic "backpressure."


---------------


# Q - Have you used multithreading? If yes, where have you used it?

Here is the consolidated, "Senior Engineer" level answer.

This answer works because it doesn't just say "I used a thread pool." It explains **why** you 
used it (throughput) and **how you controlled it** (stability).

## The "Whiteboard" Answer

"**Yes, absolutely.** The most critical use case was in our **Identity Provider (IDP)** system.

I designed a background pipeline to decommission **2 million legacy users** who were still using 
insecure 'Security Question' (SQA) authentication. The challenge was deleting this massive amount 
of data without locking the database and blocking active user logins."

## 1. The Architecture (Producer-Consumer)

"I implemented a **Producer-Consumer** pattern to decouple the scanning from the deletion logic:

* **The Producer:** A single thread that scanned our Read-Replica database for 
   inactive accounts (5+ years dormant) and pushed their IDs into a bounded `BlockingQueue` of size 500. 
   This created natural **backpressure** to prevent memory overflows.
* **The Consumers:** I used a `ThreadPoolExecutor` with **10 worker threads** to process the queue.

## 2. The Critical Challenge (The "Bulkhead")

"The real problem was that if all 10 threads hit the primary database with `DELETE` queries 
simultaneously, we would exhaust the **Database Connection Pool** and cause timeouts for 
the live `auth/login` service.

To solve this, I used a **Semaphore** with exactly **3 permits** inside the worker threads.

* **The Logic:** This acted as a **Bulkhead**. The 10 threads could concurrently handle CPU-heavy tasks like
   audit logging or token invalidation, but they had to 'line up' to acquire a permit before touching the database.
* **The Safety:** This guaranteed that our background job never held more than 3 active DB connections 
   at once, leaving the rest of the pool free for live customer traffic."

## 3. The Result

"This architecture allowed us to purge all 2 million insecure identities in a 
single 4-hour maintenance window. We reduced our security attack surface by 40% while 
maintaining **zero downtime** and keeping login latency under 50ms."


---------------