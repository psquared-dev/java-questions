# Q-1 You have 500 MB of memory, but the input data size is 2 GB. How would you sort this data and print it line by line in sorted order?

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

--

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

---

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

---

# Q-Class Loaders

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

---

