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

