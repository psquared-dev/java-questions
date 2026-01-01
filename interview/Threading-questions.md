<!-- TOC -->
* [Q-1 What is the difference between wait() and sleep() in Java?](#q-1-what-is-the-difference-between-wait-and-sleep-in-java)
    * [wait()](#wait)
    * [sleep()](#sleep)
    * [Quick state summary (very useful)](#quick-state-summary-very-useful)
* [Q-2 What happens if notify() is called before wait()? Does the waiting thread get notified later? Why or why not?](#q-2-what-happens-if-notify-is-called-before-wait-does-the-waiting-thread-get-notified-later-why-or-why-not)
* [Q-3 Why should wait() always be called inside a while loop and not an if statement?](#q-3-why-should-wait-always-be-called-inside-a-while-loop-and-not-an-if-statement)
    * [Why re-checking is necessary](#why-re-checking-is-necessary)
    * [What while guarantees](#what-while-guarantees)
    * [Why if is dangerous](#why-if-is-dangerous)
* [Q-4: Difference between notify() and notifyAll()](#q-4-difference-between-notify-and-notifyall)
    * [notify()](#notify)
    * [notifyAll()](#notifyall)
    * [Why notify() is dangerous](#why-notify-is-dangerous)
    * [Why notifyAll() is safer](#why-notifyall-is-safer)
* [Q5-Why does a thread wake up from wait() and still not run immediately? What happens after it is notified?](#q5-why-does-a-thread-wake-up-from-wait-and-still-not-run-immediately-what-happens-after-it-is-notified)
    * [Full lifecycle (clean mental model)](#full-lifecycle-clean-mental-model)
* [Q-6 What is the difference between BLOCKED and WAITING thread states?](#q-6-what-is-the-difference-between-blocked-and-waiting-thread-states)
    * [WAITING state](#waiting-state)
    * [BLOCKED state](#blocked-state)
* [Q-7 Why does wait() release the lock but sleep() does not?](#q-7-why-does-wait-release-the-lock-but-sleep-does-not)
    * [Why wait() releases the lock but sleep() does not](#why-wait-releases-the-lock-but-sleep-does-not)
    * [wait() — coordination mechanism](#wait--coordination-mechanism)
    * [sleep() — time-based pause](#sleep--time-based-pause)
* [Q-8 What problem does volatile solve, and what problem does it NOT solve?](#q-8-what-problem-does-volatile-solve-and-what-problem-does-it-not-solve)
    * [Memory visibility](#memory-visibility)
    * [Correct ordering of instructions across threads](#correct-ordering-of-instructions-across-threads)
    * [The One Rule to Remember (Perfect)](#the-one-rule-to-remember-perfect)
    * [What volatile does NOT solve](#what-volatile-does-not-solve)
* [Q-9 Why is volatile sufficient for a stop flag but not for a counter?](#q-9-why-is-volatile-sufficient-for-a-stop-flag-but-not-for-a-counter)
    * [Why this distinction matters](#why-this-distinction-matters)
* [Q-10  What is a deadlock? Can you name the four necessary conditions for deadlock?](#q-10--what-is-a-deadlock-can-you-name-the-four-necessary-conditions-for-deadlock)
    * [The 4 Necessary Conditions (Coffman Conditions)](#the-4-necessary-conditions-coffman-conditions)
    * [Example of Deadlock](#example-of-deadlock)
    * [Mapping the Code to the 4 Conditions](#mapping-the-code-to-the-4-conditions)
    * [How to prevent the deadlock?](#how-to-prevent-the-deadlock)
* [Q-11 Why Thread.stop() is not recommended to stop the thread?](#q-11-why-threadstop-is-not-recommended-to-stop-the-thread)
    * [Example](#example)
* [Q-12: What is the correct way to stop a thread in Java?](#q-12-what-is-the-correct-way-to-stop-a-thread-in-java)
    * [The Code Example:](#the-code-example)
* [Q-13 How does the `Thread.interrupt()` mechanism work conceptually?](#q-13-how-does-the-threadinterrupt-mechanism-work-conceptually)
* [Q-14 How do you handle interruption if the thread is actively working (Awake)?](#q-14-how-do-you-handle-interruption-if-the-thread-is-actively-working-awake)
    * [The Code Example:](#the-code-example-1)
* [Q-15 How do you handle interruption if the thread is Sleeping or Waiting?](#q-15-how-do-you-handle-interruption-if-the-thread-is-sleeping-or-waiting)
    * [The Code Example:](#the-code-example-2)
* [Q-16 What is the "Flag Clearing" trap when InterruptedException is thrown?](#q-16-what-is-the-flag-clearing-trap-when-interruptedexception-is-thrown)
    * [The Code Example (The "Zombie" Thread Bug):](#the-code-example-the-zombie-thread-bug)
* [Q-17 Does interrupt() wake up a thread waiting for a Lock (BLOCKED)?](#q-17-does-interrupt-wake-up-a-thread-waiting-for-a-lock-blocked)
    * [The Code Example:](#the-code-example-3)
* [Q-18 What is ReentrantLock?](#q-18-what-is-reentrantlock)
  * [Why does it exist when we already have synchronized?](#why-does-it-exist-when-we-already-have-synchronized)
  * [First: What does "Reentrant" mean?](#first-what-does-reentrant-mean)
  * [Basic usage of ReentrantLock](#basic-usage-of-reentrantlock)
  * [Key capabilities of ReentrantLock (with examples)](#key-capabilities-of-reentrantlock-with-examples)
    * [Explicit lock control (manual)](#explicit-lock-control-manual)
    * [Try acquiring a lock (non-blocking)](#try-acquiring-a-lock-non-blocking)
    * [Interruptible lock acquisition](#interruptible-lock-acquisition)
    * [Fairness (very important interview point)](#fairness-very-important-interview-point)
    * [Condition variables (replacement for wait/notify)](#condition-variables-replacement-for-waitnotify)
  * [Why ReentrantLock is NOT a replacement for synchronized](#why-reentrantlock-is-not-a-replacement-for-synchronized)
  * [When SHOULD you use ReentrantLock?](#when-should-you-use-reentrantlock)
  * [When should you NOT use it?](#when-should-you-not-use-it)
* [Q-19 What is a race condition?](#q-19-what-is-a-race-condition)
* [Q-20 What is atomicity, and how is it different from visibility?](#q-20-what-is-atomicity-and-how-is-it-different-from-visibility)
  * [Atomicity](#atomicity)
  * [Visibility](#visibility)
* [Q-21 Why was ExecutorService introduced? What problem does it solve compared to creating threads manually?](#q-21-why-was-executorservice-introduced-what-problem-does-it-solve-compared-to-creating-threads-manually)
  * [Problems with creating threads manually](#problems-with-creating-threads-manually)
    * [1 - Thread creation is expensive](#1---thread-creation-is-expensive)
    * [2 - No control over number of threads](#2---no-control-over-number-of-threads)
    * [3 - No lifecycle management](#3---no-lifecycle-management)
    * [4 - No result handling](#4---no-result-handling)
* [Q-22 What is an Executor interface?](#q-22-what-is-an-executor-interface)
  * [1. Why the Executor interface exists](#1-why-the-executor-interface-exists)
  * [2. What exactly is Executor?](#2-what-exactly-is-executor)
    * [3. Conceptual model](#3-conceptual-model)
    * [4. Where real power comes from (Executor implementations)](#4-where-real-power-comes-from-executor-implementations)
    * [Error handling behavior](#error-handling-behavior)
  * [One-line summary](#one-line-summary)
* [Q-23 What is ExecutorService interface?](#q-23-what-is-executorservice-interface)
  * [Step 1 — Where ExecutorService sits in the story](#step-1--where-executorservice-sits-in-the-story)
  * [Step 2 — What ExecutorService actually is](#step-2--what-executorservice-actually-is)
  * [Step 3 — The core responsibility of ExecutorService](#step-3--the-core-responsibility-of-executorservice)
  * [Step 4 — Submitting work (execute vs submit)](#step-4--submitting-work-execute-vs-submit)
  * [Step 5 — Why submit() exists at all](#step-5--why-submit-exists-at-all)
  * [Step 6 — Understanding Future](#step-6--understanding-future)
  * [Step 7 — Callable vs Runnable (why both exist)](#step-7--callable-vs-runnable-why-both-exist)
  * [Step 8 — Managing executor lifecycle (very important)](#step-8--managing-executor-lifecycle-very-important)
  * [Step 9 — Waiting for termination](#step-9--waiting-for-termination)
  * [Step 10 — What ExecutorService deliberately does NOT decide](#step-10--what-executorservice-deliberately-does-not-decide)
* [Q-24 What's the differences b/w ForkJoinPool and ThreadPoolExecutor?](#q-24-whats-the-differences-bw-forkjoinpool-and-threadpoolexecutor)
  * [The Classic: ThreadPoolExecutor](#the-classic-threadpoolexecutor)
  * [The Specialist: ForkJoinPool (Java 7+)](#the-specialist-forkjoinpool-java-7)
  * [Key Differences](#key-differences)
  * [Deep Dive: Why LIFO in ForkJoinPool?](#deep-dive-why-lifo-in-forkjoinpool)
  * [When to use which?](#when-to-use-which)
* [Q-25 Explain some types of ExecutorService?](#q-25-explain-some-types-of-executorservice)
  * [SingleThreadExecutor](#singlethreadexecutor)
  * [FixedThreadPool](#fixedthreadpool)
  * [CachedThreadPool](#cachedthreadpool)
  * [ScheduledThreadPoolExecutor](#scheduledthreadpoolexecutor)
  * [WorkStealingPool (ForkJoinPool)](#workstealingpool-forkjoinpool)
* [Q-26 Explain ForkJoinPool with an example](#q-26-explain-forkjoinpool-with-an-example)
  * [Rules of engagement](#rules-of-engagement)
  * [Problem](#problem)
  * [GLOBAL VIEW 1 — Task tree (structure only)](#global-view-1--task-tree-structure-only)
  * [GLOBAL VIEW 2 — Initial state](#global-view-2--initial-state)
  * [STEP 1 — Root starts on W1](#step-1--root-starts-on-w1)
  * [STEP 2 — Stealing happens](#step-2--stealing-happens)
  * [STEP 3 — W1 splits (5..8)](#step-3--w1-splits-58)
  * [STEP 4 — W2 splits (1..4)](#step-4--w2-splits-14)
  * [STEP 5 — More stealing (4 workers active)](#step-5--more-stealing-4-workers-active)
  * [STEP 6 — Leaf computations (base case)](#step-6--leaf-computations-base-case)
  * [STEP 7 — join() points (waiting is visible)](#step-7--join-points-waiting-is-visible)
  * [STEP 8 — Result build-up (bottom → top)](#step-8--result-build-up-bottom--top)
  * [STEP 9 — Final join at root](#step-9--final-join-at-root)
  * [FINAL GLOBAL VIEW — Everything together](#final-global-view--everything-together)
* [Q-27 How ForkJoinPool() is different from Executors.newWorkStealingPool()](#q-27-how-forkjoinpool-is-different-from-executorsnewworkstealingpool)
  * [1. The Return Type (API vs Implementation)](#1-the-return-type-api-vs-implementation)
  * [2. The Hidden Difference: "Async Mode"](#2-the-hidden-difference-async-mode)
* [Q-28 What is CompletableFuture?](#q-28-what-is-completablefuture)
  * [Step 1 — Why CompletableFuture was needed](#step-1--why-completablefuture-was-needed)
  * [Step 2 — What CompletableFuture actually represents](#step-2--what-completablefuture-actually-represents)
  * [Step 3 — How CompletableFuture is different from Future](#step-3--how-completablefuture-is-different-from-future)
  * [Step 4 — Creating a CompletableFuture](#step-4--creating-a-completablefuture)
  * [Step 5 — Non-blocking result handling (core idea)](#step-5--non-blocking-result-handling-core-idea)
  * [Step 6 — Chaining (this is the superpower)](#step-6--chaining-this-is-the-superpower)
  * [Step 7 — Async vs non-async stages](#step-7--async-vs-non-async-stages)
  * [Step 8 — Combining multiple futures](#step-8--combining-multiple-futures)
  * [Step 9 — Error handling (major improvement over Future)](#step-9--error-handling-major-improvement-over-future)
  * [Step 10 — Manual completion (why it’s called Completable)](#step-10--manual-completion-why-its-called-completable)
  * [Step 11 — Blocking is still possible (but optional)](#step-11--blocking-is-still-possible-but-optional)
  * [Step 12 — Execution model (important)](#step-12--execution-model-important)
* [Q-30 What is Virtual Thread?](#q-30-what-is-virtual-thread)
  * [The Analogy](#the-analogy)
    * [The Old Way: Platform Threads (The "Personal Butler" Model)](#the-old-way-platform-threads-the-personal-butler-model)
    * [The New Way: Virtual Threads (The "Order Pad" Model)](#the-new-way-virtual-threads-the-order-pad-model)
    * [The Technical Translation](#the-technical-translation)
    * [Why is this huge?](#why-is-this-huge)
  * [Example of Virtual Thread](#example-of-virtual-thread)
* [Q-31 What is Thread Local?](#q-31-what-is-thread-local)
  * [The Purpose](#the-purpose)
  * [Code Example: The "Context Holder" Pattern](#code-example-the-context-holder-pattern)
  * [The Danger: Memory Leaks (The "Dirty Thread" Problem)](#the-danger-memory-leaks-the-dirty-thread-problem)
* [Q-32 What is CountDownLatch vs CyclicBarrier?](#q-32-what-is-countdownlatch-vs-cyclicbarrier)
* [Q-33 What is Semaphore?](#q-33-what-is-semaphore)
* [Q-34 BlockingQueue (why introduced)](#q-34-blockingqueue-why-introduced)
* [Q-35 ConcurrentHashMap (how it avoids full locking)](#q-35-concurrenthashmap-how-it-avoids-full-locking)
* [Q-36 What is ReentrantReadWriteLock?](#q-36-what-is-reentrantreadwritelock)
  * [The Purpose: Performance](#the-purpose-performance)
  * [Code Example: A Thread-Safe Cache](#code-example-a-thread-safe-cache)
    * [Visualizing the difference](#visualizing-the-difference)
  * [Critical "Senior Dev" Warning](#critical-senior-dev-warning)
* [Q-37 What is Monitor object?](#q-37-what-is-monitor-object)
  * [The Mental Model: "The Secure Room"](#the-mental-model-the-secure-room)
  * [How it maps to Code](#how-it-maps-to-code)
    * [Example 1: The Simplest Example (Mutual Exclusion)](#example-1-the-simplest-example-mutual-exclusion)
    * [Example 2: The Classic "Wait/Notify" Example (Coordination)](#example-2-the-classic-waitnotify-example-coordination)
    * [Example 3: The "Modern" Explicit Monitor (ReentrantLock)](#example-3-the-modern-explicit-monitor-reentrantlock)
* [Q-38 Which object shouldn't be used as a Monitor object?](#q-38-which-object-shouldnt-be-used-as-a-monitor-object)
  * [What SHOULD be used instead](#what-should-be-used-instead)
* [Q-39 Is it valid to use a synchronized block inside a Lambda expression?](#q-39-is-it-valid-to-use-a-synchronized-block-inside-a-lambda-expression)
  * [The Code Example](#the-code-example-4)
  * [The "Gotcha" (Scope of this)](#the-gotcha-scope-of-this)
  * [Example](#example-1)
* [Q-40 Does thread release the lock after OS preemption?](#q-40-does-thread-release-the-lock-after-os-preemption)
* [Q-41 Is it possible for JVM to re-order statements inside synchronized block?](#q-41-is-it-possible-for-jvm-to-re-order-statements-inside-synchronized-block)
* [Q-42 What is AtomicReference?](#q-42-what-is-atomicreference)
* [Q-43 When would you use AtomicReference instead of synchronized?](#q-43-when-would-you-use-atomicreference-instead-of-synchronized)
* [Q-44 What is Cache-Coherence?](#q-44-what-is-cache-coherence)
* [Q-45 What is False Sharing?](#q-45-what-is-false-sharing)
* [Q-46 What is Cache Affinity?](#q-46-what-is-cache-affinity)
* [Q-47 Why False Sharing is more likely happen with ExecutorService?](#q-47-why-false-sharing-is-more-likely-happen-with-executorservice)
<!-- TOC -->

# Q-1 What is the difference between wait() and sleep() in Java?

### wait()

* Puts the thread into `WAITING` (or `TIMED_WAITING`) state
* Releases the lock immediately
* Thread stays waiting until `notify()` / `notifyAll()` is called
* After notify:
    * Thread moves to `BLOCKED` (to re-acquire the lock)
    * Then to `RUNNABLE`
* Belongs to: `Object` class
* Used for: inter-thread communication

### sleep()

* Puts the thread into `TIMED_WAITING`
* Does NOT release the lock
* Automatically wakes up after time expires
* Then moves to `RUNNABLE`
* Belongs to: `Thread` class
* Used for: pausing execution

### Quick state summary (very useful)

```text
wait()   → WAITING → BLOCKED → RUNNABLE
sleep()  → TIMED_WAITING → RUNNABLE
```

# Q-2 What happens if notify() is called before wait()? Does the waiting thread get notified later? Why or why not?

If `notify()` is called before a thread calls `wait()`, the notification is lost. 
Java does not queue notifications, so a thread that starts waiting later will wait indefinitely 
unless another notification occurs.

# Q-3 Why should wait() always be called inside a while loop and not an if statement?

`while` loop is used so the condition is re-checked every time the thread wakes up, because waking up 
does not guarantee the condition is `true`.

### Why re-checking is necessary

A thread can wake up when:

* It was notified
* It was notified but another thread consumed the resource first
* It woke up spuriously (no notify at all)

So waking up only means:
> "Something might have changed"

Not:
> You can proceed safely

### What while guarantees

```java
while (!condition) {
    wait();
}
```

This guarantees:

* The thread proceeds only when the condition is actually true
* State corruption is avoided
* Code is safe under:
    * Multiple threads
    * Spurious wakeups
    * Lost notifications


### Why if is dangerous

```java
if (!condition) {
    wait();
}
```

This checks the condition only once. If the condition changes again before the thread runs:

* The thread blindly proceeds
* Bugs happen

# Q-4: Difference between notify() and notifyAll()

**Explain the following:**
* What each method does.
* Why `notify()` is considered dangerous (Lost Wakeup problem).
* Why `notifyAll()` is safer.


### notify()

* Wakes one arbitrary thread waiting on the same object's monitor
* JVM chooses which thread
* That thread:
    * Moves from `WAITING` → `BLOCKED`
    * Competes to re-acquire the lock


### notifyAll()

* Wakes all threads waiting on the same object's monitor
* All woken threads:
    * Move to `BLOCKED`
    * Compete for the lock
    * Re-check the condition in a while loop

### Why notify() is dangerous

**Problem scenario**

* Multiple threads waiting for **different conditions**
* `notify()` may wake the **wrong thread**
* The woken thread:
    * Finds condition still false
    * Goes back to waiting
* Correct thread remains asleep → **deadlock or starvation**

**Example**

* Multiple consumers + producers
* One consumer gets notified
* Another consumer needed the signal
* System stalls

### Why notifyAll() is safer

* Wakes everyone
* Each thread re-checks its condition
* Only the thread whose condition is true proceeds
* Others go back to waiting

This avoids:

* Missed signals
* Deadlocks caused by wrong thread selection

# Q5-Why does a thread wake up from wait() and still not run immediately? What happens after it is notified?

A notified thread does not run immediately. It first moves to the `BLOCKED` state and must re-acquire 
the monitor lock before continuing execution.

### Full lifecycle (clean mental model)

When a thread is notified:

1. `WAITING`
    * Thread was sleeping via `wait()`

2. `notify()` / `notifyAll()` called
    * Thread is moved to `BLOCKED`
    * It does not run yet

3. `BLOCKED`
    * Waiting to re-acquire the same monitor lock

4. `RUNNABLE`

    * Once it gets the lock
    * Execution resumes after `wait()`

This is why:
* Notification ≠ immediate execution
* Lock ownership still matters


# Q-6 What is the difference between BLOCKED and WAITING thread states?

### WAITING state

A thread is in `WAITING` when:

* It has **explicitly decided to pause**
* It is waiting for a **signal**, not a lock

Caused by:

* `wait()`
* `join()`
* `park()`

How it exits:

* `notify()` / `notifyAll()`
* Target thread finishes (for `join()`)

### BLOCKED state

A thread is in `BLOCKED` when:

* It wants to enter a `synchronized` block
* But another thread already holds the lock

Caused by:
* Contention for a monitor lock

How it exits:
* Lock becomes available

# Q-7 Why does wait() release the lock but sleep() does not?

`wait()` releases the lock because it is used for inter-thread coordination and allows other 
threads to modify shared state, whereas `sleep()` is only a time delay and therefore does not release any locks.

### Why wait() releases the lock but sleep() does not

Because `wait()` is designed for coordination between threads, while `sleep()` is only for pausing execution.

### wait() — coordination mechanism

* Used for inter-thread communication
* The thread is saying:
    > "I can't proceed until some condition changes"
* To allow other threads to change that condition, it must release the lock

If `wait()` did NOT release the lock:

* No other thread could enter the synchronized block
* No one could call `notify()`
* **Deadlock would occur**

So:

```text
wait() → releases lock → allows other threads to run
```

### sleep() — time-based pause

* Used only to pause execution
* Thread does NOT depend on any shared condition
* There is no reason to release the lock

So:

```text
sleep() → keeps lock → resumes after time
```

# Q-8 What problem does volatile solve, and what problem does it NOT solve?

In Java, threads may:

* Cache variables locally
* Reorder instructions for performance

This can cause **visibility bugs** and **out-of-order execution** across threads.

The `volatile` keyword establishes a **happens-before** relationship.

Meaning:
> If Thread A writes to a `volatile` variable, and Thread B later reads that same variable,
then **everything Thread A did before the write is visible to Thread B after the read**.
>

This gives two guarantees:

* Memory visibility
* Correct ordering of instructions across threads


### Memory visibility

Changes made by one thread are immediately visible to other threads.

```java
volatile boolean ready = false;
int data;

Thread A:
data = 42;
ready = true;   // volatile write

Thread B:
if (ready) {    // volatile read
    System.out.println(data); // guaranteed to print 42
}
```

Because of volatile, Thread B cannot see stale values.

### Correct ordering of instructions across threads

This is where the two important statements apply:

**"Writes before volatile write happen first"**

* All writes before a volatile write
* Are flushed to main memory
* And cannot be reordered after the volatile write

In other words:

```java
x = 10;
ready = true; // volatile write
```

The JVM is **not allowed** to reorder this as:

```java
ready = true;
x = 10;   // ❌ forbidden
```

**"Reads after volatile read see them"**

* All reads after a volatile read
* Will see the latest values written before the volatile write

So once a thread reads a volatile variable:

```java
if (ready) {   // volatile read
    // all previous writes are visible here
}
```

It is guaranteed to see everything published before that volatile write.

### The One Rule to Remember (Perfect)

> If Thread A writes to a volatile variable, and Thread B later reads that same variable, then 
> everything Thread A did before the write is visible to Thread B after the read.

✅ This single rule fully captures:

* Memory visibility
* Correct instruction ordering across threads
* The meaning of happens-before

### What volatile does NOT solve

❌ Mutual exclusion (atomicity)

`volatile` does NOT:

* Lock anything
* Prevent race conditions
* Make compound operations atomic

Example:

```java
volatile int count = 0;

count++; // NOT atomic
```

This involves:

* Read
* Increment
* Write

Multiple threads can interleave these steps and corrupt the value.

To solve this, you need:

* `synchronized`
* `Lock`
* `AtomicInteger`


# Q-9 Why is volatile sufficient for a stop flag but not for a counter?

`volatile` is sufficient for a stop flag because it guarantees visibility — when one thread updates the flag, other 
threads immediately see the change. However, it is not sufficient for a counter because incrementing a counter 
is not an atomic operation and requires mutual exclusion, which `volatile` does not provide.

### Why this distinction matters

✅ Stop flag — works with `volatile`

```java
volatile boolean stop = false;

while (!stop) {
    // do work
}
```

* Only reads and writes
* No compound operation
* Visibility is enough

❌ Counter — does NOT work with `volatile`

```java
volatile int count = 0;
count++; // read → increment → write (3 steps)
```

Two threads can:

* Read same value
* Increment independently
* Overwrite each other

Result → **lost updates**



# Q-10  What is a deadlock? Can you name the four necessary conditions for deadlock?

A deadlock is a situation in concurrent programming where two or more threads are blocked forever, waiting for 
each other to release a resource.

The "Two-Key" Analogy: Imagine two people, Alice and Bob, and two locked doors, Door A and Door B.

* Alice holds the key to Door A but needs the key to Door B to proceed.
* Bob holds the key to Door B but needs the key to Door A to proceed.
* Neither is willing (or able) to hand over their key until they get the other one.
* Result: They both stand there forever.


### The 4 Necessary Conditions (Coffman Conditions)

For a deadlock to occur, ALL FOUR of these conditions must be true at the same time. 
If you break even one of them, the deadlock is impossible.

1. Mutual Exclusion: 
    * The resource can only be held by one thread at a time. (e.g., A printer or a synchronized block). 
Shared resources (like read-only files) don't cause deadlocks.

2. Hold and Wait:
    * A thread is holding onto one resource (e.g., Lock A) and is waiting to acquire another 
resource (e.g., Lock B) without releasing the first one.

3. No Preemption:
    * A resource cannot be forcibly taken away from a thread. It must be released voluntarily by the thread
holding it. (e.g., You can't just steal the lock from a running thread).

4. Circular Wait:
    * A closed chain of threads exists, where each thread holds a resource needed by the next thread in the chain.
    * Thread A waits for Thread B -> Thread B waits for Thread A.

### Example of Deadlock

Here is the classic "Bank Transfer" deadlock scenario.

Imagine two bank accounts. 
Thread-1 tries to transfer money from Alice to Bob. 
Thread-2 tries to transfer from Bob to Alice.


```java
public class DeadlockDemo {
    // These are the two resources (The "Keys")
    private static final Object lockAlice = new Object();
    private static final Object lockBob = new Object();

    public static void main(String[] args) {
        
        // Thread 1: Alice -> Bob
        Thread t1 = new Thread(() -> {
            synchronized (lockAlice) { // 1. Acquire Lock A
                System.out.println("Thread 1: Holding Alice...");

                try { Thread.sleep(100); } catch (InterruptedException e) {}

                System.out.println("Thread 1: Waiting for Bob...");
                synchronized (lockBob) { // 2. Try to Acquire Lock B
                    System.out.println("Thread 1: Success!");
                }
            }
        });

        // Thread 2: Bob -> Alice
        Thread t2 = new Thread(() -> {
            synchronized (lockBob) { // 1. Acquire Lock B
                System.out.println("Thread 2: Holding Bob...");

                try { Thread.sleep(100); } catch (InterruptedException e) {}

                System.out.println("Thread 2: Waiting for Alice...");
                synchronized (lockAlice) { // 2. Try to Acquire Lock A
                    System.out.println("Thread 2: Success!");
                }
            }
        });

        t1.start();
        t2.start();
    }
}
```

**Output:**

```text
Thread 1: Holding Alice...
Thread 2: Holding Bob...
Thread 2: Waiting for Alice...
Thread 1: Waiting for Bob...
```

### Mapping the Code to the 4 Conditions

| Condition               | Where it is in the code                                                                                                                     |
|:------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------|
| **1. Mutual Exclusion** | The `synchronized` keyword ensures only one thread can hold `lockAlice` at a time.                                                          |
| **2. Hold and Wait**    | Look at Thread 1: It is **holding** `lockAlice` (line 10) and **waiting** for `lockBob` (line 16). It refuses to let go of Alice.           |
| **3. No Preemption**    | Java cannot force Thread 1 to release `lockAlice`. Thread 2 cannot say "Give me the lock now!". Thread 1 must finish the block voluntarily. |
| **4. Circular Wait**    | Thread 1 waits for Bob. Thread 2 waits for Alice. It is a perfect circle.                                                                   |


### How to prevent the deadlock?

The easiest way to fix a deadlock is to destroy the Circular Wait condition.

**The Rule:** Always acquire locks in the same order. If both threads try to get Alice first, then Bob second, 
a deadlock is impossible.

```java
// FIXED LOGIC for Thread 2
// Instead of grabbing Bob first, we grab Alice first (just like Thread 1)
synchronized (lockAlice) { 
    synchronized (lockBob) {
         // Transfer logic...
    }
}
```


# Q-11 Why Thread.stop() is not recommended to stop the thread?

The single most important reason `Thread.stop()` is deprecated is Data Corruption.

It forces a thread to unlock its locks immediately, even if it was in the middle of a critical operation.

Here is exactly how `Thread.stop()` breaks your specific code example.

### Example

You have a critical section that does two things. They must happen together (Atomicity).

```java
synchronized void update() {
    // STEP 1: Move the money
    balance -= amount;

    // <--- CRITICAL MOMENT: stop() is called HERE

    // STEP 2: Record the transaction
    auditLog.add(entry);
}
```

**The Scenario: The Invisible Theft**

1. Thread A acquires the lock on the object.
2. Thread A executes `balance -= amount`. The money is deducted.
3. `Thread.stop()` hits Thread A.
    * Thread A immediately dies.
    * CRITICAL: The JVM releases the synchronized lock instantly.
    * Thread A never executes line 2 (`auditLog.add`).

**The Aftermath (Why it is a disaster**)

The lock is now open. Thread B comes in and looks at the data.

* Balance: Reduced. (Money is gone).
* Audit Log: Empty. (No record of where it went).

Your system is now in a corrupted state. You have missing money and no logs. 
Because the lock was released, Thread B assumes everything is fine and proceeds to process more transactions 
on top of this broken data, making the problem impossible to trace.


# Q-12: What is the correct way to stop a thread in Java?

You should never force a thread to stop (e.g., `stop()`) because it can leave shared data in a broken state. 
Instead, you "ask" the thread to stop using `interrupt()`, and the thread must voluntarily agree to shut down.

### The Code Example:

```java
public class CorrectStopDemo {
    public static void main(String[] args) throws InterruptedException {
        Thread worker = new Thread(() -> {
            System.out.println("Worker: I am running...");
            
            // COOPERATIVE CHECK:
            // "If no one asked me to stop, I keep going."
            while (!Thread.currentThread().isInterrupted()) {
                // Do work...
                Math.sin(0.5); 
            }
            
            System.out.println("Worker: I received the signal. Stopping gracefully.");
        });

        worker.start();
        Thread.sleep(100);
        
        System.out.println("Main: Asking worker to stop...");
        worker.interrupt(); // The polite signal
    }
}
```

# Q-13 How does the `Thread.interrupt()` mechanism work conceptually?

Think of the Interrupt as a simple internal `boolean` flag (`interrupt` status) inside the `Thread` object.

* Calling `worker.interrupt()` sets this flag to `true`.
* It does not kill the thread. It just flips a switch.
* If the thread never checks this switch, it will run forever.

The Code Example (The Ignorant Thread): This example proves that `interrupt()` does nothing if the worker ignores it.

```java
public class IgnorantThread {
    public static void main(String[] args) throws InterruptedException {
        Thread worker = new Thread(() -> {
            // BUG: We are using 'true' instead of checking isInterrupted()
            while (true) {
                // I am ignoring the flag completely!
                Math.random(); 
            }
        });

        worker.start();
        Thread.sleep(100);
        
        worker.interrupt(); 
        System.out.println("Main: I called interrupt, but the worker is still running forever!");
        // The program will never terminate.
    }
}
```

# Q-14 How do you handle interruption if the thread is actively working (Awake)?

If the thread is CPU-busy (calculating, processing), it acts as a "Gatekeeper". It must explicitly check 
the flag using `isInterrupted()` before starting the next chunk of work.

### The Code Example:

```java
public class AwakeInterruption {
    public static void main(String[] args) throws InterruptedException {
        Thread worker = new Thread(() -> {
            long count = 0;
            
            // GATEKEEPER: Check the flag before every iteration
            while (!Thread.currentThread().isInterrupted()) {
                count++; // The "Meat" (Work)
            }
            
            System.out.println("Worker: Stopped after counting to " + count);
        });

        worker.start();
        Thread.sleep(10); // Let it run for 10ms
        
        worker.interrupt(); // Set the flag
    }
}
```

# Q-15 How do you handle interruption if the thread is Sleeping or Waiting?

If the thread is paused (sleeping), it cannot check the while loop. The JVM handles this by waking 
the thread up and throwing an `InterruptedException`. This is the "Emergency Alarm."

### The Code Example:

```java
public class SleepInterruption {
    public static void main(String[] args) throws InterruptedException {
        Thread worker = new Thread(() -> {
            try {
                System.out.println("Worker: Going to sleep for 10 years...");
                
                // BLOCKED STATE
                Thread.sleep(1000 * 60 * 60 * 24 * 365 * 10); 
                
            } catch (InterruptedException e) {
                // The JVM wakes us up here!
                System.out.println("Worker: Ouch! I was woken up explicitly!");
            }
        });

        worker.start();
        Thread.sleep(1000);
        
        System.out.println("Main: Waking up the worker...");
        worker.interrupt(); // Triggers the Exception
    }
}
```

# Q-16 What is the "Flag Clearing" trap when InterruptedException is thrown?

When `InterruptedException` is thrown, the JVM clears the `interrupt` flag (resets it to `false`).

* **The Trap:** If you catch the exception and don't fix the flag, your while loop will think everything is fine
and keep running.
* **The Fix:** Call `Thread.currentThread().interrupt()` inside the catch block to put the flag back to `true`.

### The Code Example (The "Zombie" Thread Bug):

```java
public class FlagClearingTrap {
    public static void main(String[] args) throws InterruptedException {
        Thread worker = new Thread(() -> {
            // 1. The loop checks the flag
            while (!Thread.currentThread().isInterrupted()) {
                try {
                    System.out.println("Worker: Working...");
                    Thread.sleep(1000); 
                } catch (InterruptedException e) {
                    System.out.println("Worker: Exception caught! (Flag is now CLEARED by JVM)");
                    
                    // BUG: We swallowed the exception and didn't restore the flag.
                    // The loop condition !isInterrupted() is now TRUE again!
                    // The thread will NOT stop. It acts like a Zombie.
                }
            }
        });

        worker.start();
        Thread.sleep(2500);
        
        System.out.println("Main: FIRE INTERRUPT!");
        worker.interrupt();
    }
}
```

**The Correct Fix:** Inside the `catch` block, add this line:


```java
} catch (InterruptedException e) {
    System.out.println("Worker: Interrupted!");
    // RESTORE THE FLAG
    Thread.currentThread().interrupt(); 
}
```

# Q-17 Does interrupt() wake up a thread waiting for a Lock (BLOCKED)?

No. A thread waiting for a synchronized lock is BLOCKED, not WAITING. `interrupt()` has no effect on it. 
It will sit there frozen until it gets the lock.

### The Code Example:

```java
public class BlockedInterruption {
    public static void main(String[] args) throws InterruptedException {
        Object lock = new Object();

        // Thread-1: Grabs the lock and holds it forever
        Thread greedyThread = new Thread(() -> {
            synchronized (lock) {
                try { Thread.sleep(999999); } catch (InterruptedException e) { }
            }
        });
        greedyThread.start();
        Thread.sleep(100); // Ensure greedyThread has the lock

        // Thread-2: Tries to enter the lock (Will get BLOCKED)
        Thread blockedThread = new Thread(() -> {
            System.out.println("BlockedThread: Trying to get lock...");
            synchronized (lock) {
                System.out.println("BlockedThread: I got the lock! (Unreachable)");
            }
        });
        blockedThread.start();
        Thread.sleep(1000);

        System.out.println("Main: Interrupting the BlockedThread...");
        blockedThread.interrupt(); 
        
        System.out.println("Main: Interrupt sent. Observe that BlockedThread DOES NOT wake up.");
        // The program hangs here. blockedThread ignores the interrupt.
    }
}
```


# Q-18 What is ReentrantLock?

`ReentrantLock` is a lock implementation provided by Java in `java.util.concurrent.locks`.

It is an **explicit locking mechanism**, meaning:

* You **manually acquire** the lock
* You **manually release** the lock
* You get **more control** than `synchronized`

## Why does it exist when we already have synchronized?

`synchronized` is:

* Simple
* Safe
* Easy to use

But it is also:

* Rigid
* Limited
* Hard to control in advanced concurrency scenarios

`ReentrantLock` was introduced to solve **real-world concurrency limitations** of `synchronized`.

## First: What does "Reentrant" mean?

Reentrant = same thread can acquire the same lock multiple times

This applies to both:

* `synchronized`
* `ReentrantLock`

Example (important):

```java
class A {
    synchronized void m1() {
        m2();   // same thread enters again
    }

    synchronized void m2() {
        System.out.println("Inside m2");
    }
}
```

This works because Java locks are **reentrant**.

If locks were not reentrant → this would deadlock.

So the name `ReentrantLock` emphasizes this behavior explicitly.

## Basic usage of ReentrantLock

**Simple example**

```java
ReentrantLock lock = new ReentrantLock();

lock.lock();      // acquire lock
try {
    // critical section
    System.out.println("Inside critical section");
} finally {
    lock.unlock();   // MUST be called
}
```

⚠️ If you forget `unlock()` → **deadlock risk**

This is why `synchronized` is safer for simple cases.

## Key capabilities of ReentrantLock (with examples)

Now let’s build intuition feature by feature.

### Explicit lock control (manual)

**synchronized**

```java
synchronized (lock) {
        // lock acquired automatically
        }
// lock released automatically
```

**ReentrantLock**

```java
lock.lock();
try {
    // work
} finally {
    lock.unlock();
}
```

👉 More control, but more responsibility.

### Try acquiring a lock (non-blocking)

❌ Not possible with synchronized

With synchronized, if lock is taken:
* Thread blocks forever

✅ Possible with ReentrantLock

```java
if (lock.tryLock()) {
    try {
        // got the lock
    } finally {
        lock.unlock();
    }
} else {
    // lock not available — do something else
}
```

Use case:

* Avoid blocking UI thread
* Skip optional work if resource is busy

### Interruptible lock acquisition

❌ synchronized

If a thread is blocked waiting for a monitor lock:
* interrupt() does NOTHING
* Thread stays blocked

✅ ReentrantLock

```java
lock.lockInterruptibly();
```

Now:
* Thread can be interrupted
* Useful during shutdown or cancellation

### Fairness (very important interview point)

**synchronized**

* No fairness guarantee
* JVM chooses next thread arbitrarily

**ReentrantLock**

```java
ReentrantLock lock = new ReentrantLock(true); // fair lock
```

Fair lock:
* Longest-waiting thread gets lock first

Tradeoff:

* Fair → lower throughput
* Unfair (default) → faster

### Condition variables (replacement for wait/notify)

With `synchronized`:

* Only one wait set per lock
* Uses `wait()`, `notify()`, `notifyAll()`

With `ReentrantLock`:

* Multiple conditions per lock
* Much cleaner and safer

Example:

```java
ReentrantLock lock = new ReentrantLock();
Condition notEmpty = lock.newCondition();
Condition notFull = lock.newCondition();
```

```java
lock.lock();
try {
    while (empty) {
        notEmpty.await();
    }
    // consume
    notFull.signal();
} finally {
    lock.unlock();
}
```

This solves many classic `notify()` bugs.

## Why ReentrantLock is NOT a replacement for synchronized

Important interview nuance:
> `ReentrantLock` is not better, it is more powerful.

Use:

* `synchronized` → simple, safe, low-risk
* `ReentrantLock` → complex, performance-sensitive, advanced control

## When SHOULD you use ReentrantLock?

* ✔️ You need tryLock()
* ✔️ You need interruptible locking
* ✔️ You need fairness
* ✔️ You need multiple condition queues
* ✔️ You’re building concurrency primitives or frameworks

## When should you NOT use it?

* ❌ Simple synchronization
* ❌ Low contention
* ❌ When correctness > flexibility


# Q-19 What is a race condition?

A race condition occurs when multiple threads access shared mutable data concurrently and the result depends 
on execution order, often leading to incorrect outcomes.


# Q-20 What is atomicity, and how is it different from visibility?

## Atomicity

Atomicity means an operation is indivisible — it either happens completely or not at all, 
and no other thread can observe it in an intermediate state.

## Visibility

Visibility ensures that when one thread updates a variable, other threads see the updated value 
instead of a stale cached value.


# Q-21 Why was ExecutorService introduced? What problem does it solve compared to creating threads manually?


Before `ExecutorService`, developers created threads manually using the `Thread` class. 
This approach worked for small programs but caused serious problems in real-world applications.

## Problems with creating threads manually

### 1 - Thread creation is expensive

* Creating a new thread allocates stack memory and OS resources
* Frequent thread creation leads to performance overhead

```java
new Thread(task).start();  // expensive if done repeatedly
```

### 2 - No control over number of threads

* Unbounded thread creation can:
    * Exhaust CPU
    * Exhaust memory
    * Crash the application

Example:

```java
for (int i = 0; i < 10000; i++) {
    new Thread(task).start(); // dangerous
}
```

### 3 - No lifecycle management

* No standard way to:
    * Reuse threads
    * Shut them down gracefully
    * Wait for all tasks to finish


### 4 - No result handling

* Threads cannot return values
* Handling results required shared mutable state

# Q-22 What is an Executor interface?

## 1. Why the Executor interface exists

Before Java 5, concurrency typically looked like this:

```java
new Thread(() -> doWork()).start();
```

This approach has structural problems:

1. You cannot control how many threads are created.
    * You cannot control how many threads are created.
2. No reuse
    * Threads are expensive; creating them repeatedly is wasteful.
3. No lifecycle management
    * No standard way to shut down, monitor, or throttle execution.
4. Business logic tightly coupled with threading logic
    * Your application logic decides how execution happens.

Java introduced `Executor` to separate concerns:
> What to execute vs How to execute


## 2. What exactly is Executor?

```java
public interface Executor {
    void execute(Runnable command);
}
```

That is the entire interface.

**Key observations**

* Executor:
    * Does not create threads
    * Does not define scheduling
    * Does not return results
* It is a **task execution abstraction**, nothing more.

### 3. Conceptual model

Think of Executor as a **task sink**.

You submit work; something else decides:

* Which thread runs it
* When it runs
* Whether it runs immediately, later, or never

### 4. Where real power comes from (Executor implementations)

Executor is a **foundation interface**.

More powerful abstractions build on it:

```text
Executor
   |
   +-- ExecutorService
           |
           +-- ThreadPoolExecutor
           +-- ScheduledThreadPoolExecutor
```

Executor alone:
* Fire-and-forget
* No result
* No cancellation
* No lifecycle control

### Error handling behavior

If a `Runnable` throws an exception:

```java
executor.execute(() -> {
    throw new RuntimeException("Boom");
});
```

* Exception is thrown **inside worker thread**
* Caller never sees it
* Thread may die or be replaced (implementation-dependent)

This is a major reason why higher-level interfaces exist.

## One-line summary

The `Executor` interface is used to submit tasks, but it is the concrete 
implementation (like `ThreadPoolExecutor` or `ForkJoinPool`) that decides how to execute them.

# Q-23 What is ExecutorService interface?

## Step 1 — Where ExecutorService sits in the story

Before `ExecutorService`, we already have:

* A task → `Runnable` or `Callable`
* Someone who can run tasks → `Executor`

But `Executor` only says:
> "I can execute a task."
>

It does not say:

* when it will finish
* how many tasks it can handle
* how to stop it
* how to observe results

Real systems cannot live with that uncertainty.

This is why `ExecutorService` exists.

## Step 2 — What ExecutorService actually is

Formally:

```java
public interface ExecutorService extends Executor
```

This tells us two things:

1. Every `ExecutorService` is an `Executor`
2. It adds management and control

Think of it as:
> Executor + lifecycle + results + control


## Step 3 — The core responsibility of ExecutorService

`ExecutorService` answers five critical questions that `Executor` cannot:

1. How do I submit work?
2. How do I get results?
3. How do I wait for completion?
4. How do I cancel work?
5. How do I shut down cleanly?

Everything in `ExecutorService` exists to answer one of these.

## Step 4 — Submitting work (execute vs submit)

**execute(Runnable)** 

Inherited from `Executor`.

```java
executorService.execute(() -> doWork());
```

Characteristics:

* Fire-and-forget
* No result
* No visibility
* Exceptions go to thread’s uncaught handler

Use case:

* Logging
* Metrics
* Side effects


**submit(...)**

This is new in `ExecutorService`.

```java
Future<Integer> future =
        executorService.submit(() -> 42);
```

Key difference:
* You get a `Future`

This is extremely important.

## Step 5 — Why submit() exists at all

Real applications often need:

* Results
* Error handling
* Coordination between tasks

`submit()` solves this by returning a handle to the task.

That handle is `Future`.

## Step 6 — Understanding Future

A `Future` represents the lifecycle of a task.

Think of it as a box that may be:

* empty (task running)
* filled (task completed)
* broken (task failed)
* cancelled

Key methods:

```java
future.get();        // blocks until done
future.isDone();     // non-blocking check
future.cancel(true); // attempt cancellation
```

Why this matters:

* Without `Future`, async code becomes guesswork
* With `Future`, async code becomes controllable

This is a major reason `ExecutorService` exists.

## Step 7 — Callable vs Runnable (why both exist)

**Runnable**

```java
Runnable r = () -> doWork();
```

* No return value
* No checked exception

**Callable**

```java
Callable<Integer> c = () -> 42;
```

* Returns a value
* Can throw checked exceptions


`ExecutorService` supports both because:

* Some tasks only do
* Some tasks compute

This is another layer of realism added by `ExecutorService`.

## Step 8 — Managing executor lifecycle (very important)

Threads are resources. Resources must be released.

So `ExecutorService` introduces lifecycle methods.

**shutdown()**

```java
executorService.shutdown();
```

What it means:

* Stop accepting new tasks
* Let existing tasks finish

This is a **graceful shutdown**.

**shutdownNow()**

```java
executorService.shutdownNow();
```

What it means:

* Attempt to interrupt running tasks
* Return tasks that never started

This is best-effort, not guaranteed.


## Step 9 — Waiting for termination

Sometimes you need to wait:

```java
executorService.awaitTermination(10, TimeUnit.SECONDS);
```

Why this exists:

* Coordinated shutdown
* Service stop hooks
* Application exits

This is essential for:

* Servers
* Batch jobs
* Graceful redeployments

## Step 10 — What ExecutorService deliberately does NOT decide

Notice what `ExecutorService` does not specify:

* How many threads
* Which queue
* Thread reuse strategy
* Scheduling policy

That is intentional.

Those decisions belong to implementations like:

* ThreadPoolExecutor
* ForkJoinPool

`ExecutorService` focuses on control, not mechanics.


# Q-24 What's the differences b/w ForkJoinPool and ThreadPoolExecutor?

To understand the difference, we first need to understand the "why". Both of these are implementations of 
the `ExecutorService` interface, designed to solve the same fundamental problem: **creating a Thread is expensive**.

Instead of creating a new OS thread for every task (which burns memory and CPU cycles), we create a "pool" of 
threads once and reuse them.

Here is the breakdown from the ground up.

## The Classic: ThreadPoolExecutor

Think of this as a standard Bank Counter System.

* **The Structure:** There is one central queue (ticket machine) and a fixed number of tellers (Threads).
* **The Process:**
    * You (the main program) submit a task.
    * The task goes into the central `BlockingQueue`.
    * All threads compete to grab the next task from this single queue.
    * Once a thread grabs a task, it processes it to completion, then comes back to the queue for another.

* **The Philosophy:** "Fairness and Order." Tasks are generally processed in the
order they arrive (depending on the queue type).
* **The Limitation:** If one thread gets a huge task, it is stuck. If the queue is locked by 
one thread taking a task, others have to wait (contention).

## The Specialist: ForkJoinPool (Java 7+)

Think of this as a Restaurant Kitchen preparing a massive banquet.

* **The Structure:** There is no single central line. Every chef (Thread) has their **own personal desk (Deque)** of tasks.
* **The Philosophy:** "Divide and Conquer" (Recursive processing).
* **The Process:**
    * **Fork:** A big task (e.g., "Prep 1000 steaks") arrives. One chef takes it.
    * **Split:** That chef realizes it's too big, splits it into two tasks of 500, keeps one, and pushes the other 
to the top of their own pile.
    * **Join:** They keep splitting until the tasks are small enough to just cook.
    * **Work Stealing (The Magic):** If another chef finishes their work early and has nothing 
to do, they don't wait. They look at your pile, sneak to the back (the tail), and "steal" one of 
your unprocessed sub-tasks to help you out.

## Key Differences

Here is how they compare fundamentally:


| Feature                | ThreadPoolExecutor (TPE)                                                     | ForkJoinPool (FJP)                                                                                          |
|:-----------------------|:-----------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------|
| **Work Queue**         | **Single Central Queue.** All threads contend for the same lock to get work. | **Multiple Local Queues.** Each thread has its own Deque. Low contention.                                   |
| **Task Relationship**  | Best for **Independent tasks** (e.g., User A login, User B upload).          | Best for **Recursive tasks** (e.g., Sorting an array, Matrix multiplication).                               |
| **Algorithm**          | Standard Producer-Consumer.                                                  | **Work-Stealing Algorithm.**                                                                                |
| **The "Stuck" Factor** | If a thread waits for a result (e.g., IO), it does nothing (blocks).         | If a thread waits for a sub-task (`join`), it effectively puts that task aside and works on something else. |
| **Order of Execution** | Usually **FIFO** (First-In-First-Out).                                       | **LIFO** (Last-In-First-Out) for the owner thread; **FIFO** for the stealer.                                |

## Deep Dive: Why LIFO in ForkJoinPool?

This is a brilliant optimization for **CPU Cache Locality**.

* When a thread splits a task, it pushes the new sub-task to the head of its deque.
* It immediately pops the head again to work on it.
* Since this data was just created, it is likely still hot in the CPU's L1/L2 Cache.
* Standard TPE often processes "older" tasks first, meaning the data might be cold (flushed from cache to RAM), 
causing cache misses.


## When to use which?

1\. Use `ThreadPoolExecutor` when:

* You have blocking IO tasks (Database calls, API requests).
* The tasks are unrelated (Task A doesn't care if Task B finishes).
* You need strict control over priority or timing (e.g., `ScheduledThreadPoolExecutor`).

2\. Use `ForkJoinPool` when:

* You have computational heavy tasks (Number crunching, Image processing).
* The tasks can be broken down recursively (The "Divide and Conquer" pattern).
* You are using Java Streams (`.parallelStream()`), which uses the common FJP under the hood.


# Q-25 Explain some types of ExecutorService?

1. SingleThreadExecutor
2. FixedThreadPool
3. CachedThreadPool
4. ScheduledThreadPoolExecutor
5. WorkStealingPool (ForkJoinPool)

## SingleThreadExecutor

```java
ExecutorService executor = Executors.newSingleThreadExecutor();
```

**Behavior**

* Uses exactly one thread
* Tasks execute sequentially
* Tasks are queued if the thread is busy
* Thread is reused

**Realistic use case**

* Audit logging
* Event processing
* Writing to a file (order matters)

**Why it exists**

* Removes need for `synchronized`
* Guarantees order
* Simplifies single-threaded background work

## FixedThreadPool

```java
ExecutorService executor = Executors.newFixedThreadPool(4);
```

**Behavior**

* Fixed number of threads
* At most N tasks run concurrently
* Remaining tasks wait in a queue

**Realistic use case**

* Handling HTTP requests
* Processing jobs from a queue
* CPU-bound tasks

**Why it exists**

* Prevents thread explosion
* Gives predictable resource usage


## CachedThreadPool

```java
ExecutorService executor = Executors.newCachedThreadPool();
```

**Behavior**

* Creates threads as needed
* Reuses idle threads
* No upper limit on threads

**Realistic use case**

* Short-lived I/O tasks
* Async callbacks
* Network operations

**Why it exists**

* Fast response under burst load
* Avoids queuing delay

⚠️ Danger: can create too many threads if tasks block


## ScheduledThreadPoolExecutor

```java
ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(2);
```

**Behavior**

* Executes tasks:
    * After a delay
    * Periodically
* Supports fixed-rate and fixed-delay scheduling

**Realistic use case**

* Cron jobs
* Heartbeats
* Cleanup tasks
* Monitoring

**Why it exists**

* Safe replacement for Timer
* Handles exceptions properly


## WorkStealingPool (ForkJoinPool)

```java
ExecutorService executor = Executors.newWorkStealingPool();
```

**Behavior**

* Uses ForkJoinPool
* Threads steal work from each other
* Optimized for CPU-bound tasks

**Realistic use case**

* Parallel data processing
* Recursive algorithms
* Parallel streams

**Why it exists**

* Maximizes CPU utilization
* Reduces idle threads

⚠️ Not suitable for blocking I/O

# Q-26 Explain ForkJoinPool with an example

```java
public class WorkStealingDemo {
    static class SumTask extends RecursiveTask<Integer> {
        private final int from;
        private final int to;

        SumTask(int from, int to) {
            this.from = from;
            this.to = to;
        }

        @Override
        protected Integer compute() {
            if (to - from <= 1) {
                int sum = 0;

                for (int i = from; i <= to; i++) {
                    sum += i;
                }

                System.out.println(
                        Thread.currentThread().getName() +
                                " computing Sum(" + from + ".." + to + ") = " + sum
                );

                return sum;
            }

            int mid = (from + to) / 2;

            SumTask leftTask = new   SumTask(from, mid);
            SumTask rightTask = new SumTask(mid + 1, to);

            leftTask.fork();
            int rightResult = rightTask.compute();
            int leftResult = leftTask.join();

            return leftResult + rightResult;
        }
    }

    public static void main(String[] args) throws Exception {
        ForkJoinPool pool = new ForkJoinPool(4);
        SumTask sumTask = new SumTask(1, 8);
        int result = pool.invoke(sumTask);
        System.out.println("Result: " + result);
        pool.shutdown();
    }
}
```

## Rules of engagement

**Deque orientation (horizontal)**

```text
[ FRONT | ........ | BOTTOM ]
```

* BOTTOM → owner pushes & pops
* FRONT → thieves steal
  
**Method semantics**

* `fork()` → pushes task to BOTTOM of current worker deque
* `compute()` → normal method call, runs immediately
* `join()` → wait point
    * if result ready → return
    * if not → worker waits (may steal)


## Problem

Compute:

```text
Sum(1..8)
```

Using 4 workers: `W1, W2, W3, W4`

## GLOBAL VIEW 1 — Task tree (structure only)

This is the entire logical tree that will exist by the end.

```text
                        (1..8)
                       /      \
                 (1..4)        (5..8)
                /      \       /      \
           (1..2)   (3..4) (5..6)   (7..8)
```

Nothing here talks about **workers yet**.

Now we will **attach workers + deques + waiting states**.

## GLOBAL VIEW 2 — Initial state

```text
Workers:
W1, W2, W3, W4

Deques:
W1: [ FRONT |  | BOTTOM ]
W2: [ FRONT |  | BOTTOM ]
W3: [ FRONT |  | BOTTOM ]
W4: [ FRONT |  | BOTTOM ]
```

## STEP 1 — Root starts on W1

```text
W1 executes compute(1..8)
```

Split happens:

```text
left.fork();      // (1..4)
right.compute();  // (5..8)
```

**State now**

```text
TREE:
                        (1..8) [W1 running]
                       /      \
                (1..4) [enqueued]   (5..8) [W1 running]

DEQUES:
W1: [ FRONT |  | (1..4) | BOTTOM ]
W2: [ FRONT |  | BOTTOM ]
W3: [ FRONT |  | BOTTOM ]
W4: [ FRONT |  | BOTTOM ]
```

## STEP 2 — Stealing happens

W2 is idle → steals from **FRONT of W1**.

```text
W2 steals (1..4)
```

**State**

```text
TREE:
                        (1..8) [W1 running]
                       /      \
              (1..4) [W2 running]   (5..8) [W1 running]

DEQUES:
W1: [ FRONT |  | BOTTOM ]
W2: [ FRONT |  | BOTTOM ]
W3: [ FRONT |  | BOTTOM ]
W4: [ FRONT |  | BOTTOM ]
```

## STEP 3 — W1 splits (5..8)

```java
left.fork();      // (5..6)
right.compute();  // (7..8)
```

```text
TREE:
                        (1..8) [W1 running]
                       /      \
              (1..4) [W2]        (5..8) [W1 running]
                                   /      \
                        (5..6) [enq]   (7..8) [W1 running]

DEQUES:
W1: [ FRONT |  | (5..6) | BOTTOM ]
W2: [ FRONT |  | BOTTOM ]
W3: [ FRONT |  | BOTTOM ]
W4: [ FRONT |  | BOTTOM ]
```

## STEP 4 — W2 splits (1..4)

```java
left.fork();      // (1..2)
right.compute();  // (3..4)
```

```text
TREE:
                        (1..8) [W1]
                       /      \
              (1..4) [W2 running]     (5..8) [W1]
               /      \                  /      \
      (1..2) [enq]  (3..4) [W2 running] (5..6) [enq] (7..8) [W1]

DEQUES:
W1: [ FRONT |  | (5..6) | BOTTOM ]
W2: [ FRONT |  | (1..2) | BOTTOM ]
W3: [ FRONT |  | BOTTOM ]
W4: [ FRONT |  | BOTTOM ]
```

## STEP 5 — More stealing (4 workers active)

* W3 steals from W1 → (5..6)
* W4 steals from W2 → (1..2)

```text
TREE:
                        (1..8) [W1]
                       /      \
              (1..4) [W2]            (5..8) [W1]
               /      \                /      \
      (1..2) [W4 run] (3..4) [W2 run] (5..6) [W3 run] (7..8) [W1 run]

DEQUES:
W1: [ FRONT |  | BOTTOM ]
W2: [ FRONT |  | BOTTOM ]
W3: [ FRONT |  | BOTTOM ]
W4: [ FRONT |  | BOTTOM ]
```

## STEP 6 — Leaf computations (base case)

All workers now compute actual values:

```text
W1: (7..8) = 15
W2: (3..4) = 7
W3: (5..6) = 11
W4: (1..2) = 3
```

## STEP 7 — join() points (waiting is visible)

**W1 reaches:**

```java
left.join(); // waiting for (5..6)
```

But `(5..6)` already completed by W3.

→ No waiting, immediate return.

**W2 reaches:**

```java
left.join(); // waiting for (1..2)
```

But `(1..2)` already completed by W4.

→ No waiting, immediate return.

(This is important: join() is special, but does not always block.)

## STEP 8 — Result build-up (bottom → top)

Now results combine **upward in the tree**.

```text
W3 returns: (5..6) = 11
W1 computes: (5..8) = 11 + 15 = 26

W4 returns: (1..2) = 3
W2 computes: (1..4) = 3 + 7 = 10
```

## STEP 9 — Final join at root

W1 now does:

```java
join(1..4)
```

* `(1..4)` already completed by W2
* Immediate return

Final result:

```text
(1..8) = 26 + 10 = 36    
```

## FINAL GLOBAL VIEW — Everything together

```text
                        (1..8) = 36 [W1]
                       /                  \
          (1..4) = 10 [W2]              (5..8) = 26 [W1]
             /        \                    /          \
   (1..2)=3 [W4]  (3..4)=7 [W2]   (5..6)=11 [W3]  (7..8)=15 [W1]
```

# Q-27 How ForkJoinPool() is different from Executors.newWorkStealingPool()

There are two key differences: one is about **Type** (what you get), and one is about **Algorithm** (how it works).

## 1. The Return Type (API vs Implementation)

* `new ForkJoinPool()` returns the concrete `ForkJoinPool` class.
    * You get full access to specific methods like `.invoke()`, `.fork()`, `.join()`, and `.getStealCount()`.

* `Executors.newWorkStealingPool()` returns the `ExecutorService` interface.
    * It hides the implementation. You only get standard methods like `.submit()` and `.shutdown()`. 
  You cannot call specific ForkJoin methods without casting.

## 2. The Hidden Difference: "Async Mode"

This is the critical performance difference.

* `new ForkJoinPool()` defaults to **Async Mode = false** (LIFO / Stack).
    *  **Behavior:** When a thread adds a task, it processes the **most recently added** task next.
    * **Why:** This optimizes for **CPU Cache** (Locality). The data for the newest sub-task is likely still hot in the CPU cache.
    * **Best For: Recursive Tasks** (Divide and conquer, sorting, matrix math).

* `Executors.newWorkStealingPool()` sets **Async Mode = true** (FIFO / Queue).
    * **Behavior:** When a thread adds a task, it processes the oldest task next.
    * **Why:** This optimizes for **Fairness**. It processes tasks in the order they arrived.
    * **Best For: Event Handling / Message Processing** (Processing independent requests).

# Q-28 What is CompletableFuture?

One-line definition (memorize this)
> `CompletableFuture` is a Java class that represents an asynchronous computation which can be explicitly completed 
> and allows non-blocking, functional composition of dependent tasks.


## Step 1 — Why CompletableFuture was needed

Before `CompletableFuture`, Java had `Future`.

**Problem with Future**

```java
Future<Integer> f = executor.submit(task);
Integer result = f.get();   // BLOCKS
```

Issues:

* `get()` blocks the thread
* No way to chain tasks
* No clean way to handle errors
* Hard to express async pipelines

So Java needed:

* Non-blocking async
* Chaining
* Error handling
* Composition

This led to `CompletableFuture` (Java 8).

## Step 2 — What CompletableFuture actually represents

A `CompletableFuture<T>` represents:
> "A value of type T that will be available in the future, and on which more work can be attached."

It is both:
* a promise (someone completes it)
* a pipeline (actions run when it completes)

## Step 3 — How CompletableFuture is different from Future

| Feature                | Future | CompletableFuture |
|------------------------|--------|-------------------|
| Blocking get           | Yes    | Optional          |
| Chaining               | No     | Yes               |
| Non-blocking callbacks | No     | Yes               |
| Manual completion      | No     | Yes               |
| Error handling         | Poor   | Rich              |
| Functional style       | No     | Yes               |

## Step 4 — Creating a CompletableFuture

**Asynchronous computation**

```java
CompletableFuture<Integer> cf =
    CompletableFuture.supplyAsync(() -> 10);
```

Meaning:
* Task runs asynchronously
* Result will be available later

**Void task**

```java
CompletableFuture<Void> cf =
    CompletableFuture.runAsync(() -> doWork());
```


## Step 5 — Non-blocking result handling (core idea)

Instead of blocking:

```java
Integer result = cf.get();  // blocking
```

You attach **callbacks**:

```java
cf.thenAccept(result -> {
    System.out.println(result);
});
```

Key idea:
> Threads do not wait — work happens when the result arrives


## Step 6 — Chaining (this is the superpower)

```java
CompletableFuture<Integer> cf =
    CompletableFuture.supplyAsync(() -> 10)
        .thenApply(x -> x * 2)
        .thenApply(x -> x + 5);
```

Execution flow:

Each step:

* Runs after the previous completes
* Does not block

## Step 7 — Async vs non-async stages

```java
thenApply(...)        // may run in same thread
thenApplyAsync(...)   // always runs asynchronously
```

Rule:

* Async variants may use a different thread
* You can also supply your own executor

```java
thenApplyAsync(fn, executor)
```

## Step 8 — Combining multiple futures

**Combine two independent tasks**

```java
CompletableFuture<Integer> f1 = ...
CompletableFuture<Integer> f2 = ...

CompletableFuture<Integer> result =
    f1.thenCombine(f2, (a, b) -> a + b);
```

Meaning:

* Wait for both
* Combine results

**Wait for all**

```java
CompletableFuture.allOf(f1, f2, f3);
```

**First one wins**

```java
CompletableFuture.anyOf(f1, f2);
```

## Step 9 — Error handling (major improvement over Future)

**Handle errors**

```java
cf.exceptionally(ex -> {
    return -1;
});
```

**Handle success + failure**

```java
cf.handle((result, ex) -> {
    if (ex != null) return -1;
    return result;
});
```

Errors are treated as **data**, not crashes.

## Step 10 — Manual completion (why it’s called Completable)

```java
CompletableFuture<Integer> cf = new CompletableFuture<>();

// later
cf.complete(42);
```

Or on failure:

```java
cf.completeExceptionally(new RuntimeException());
```

This enables:

* Bridging callbacks → futures
* Adapting legacy async APIs

## Step 11 — Blocking is still possible (but optional)

```java
cf.join();  // unchecked exception
cf.get();   // checked exception
```

Blocking is **allowed**, but not the design goal.

## Step 12 — Execution model (important)

* If no executor is provided:
    * Uses ForkJoinPool.commonPool
* Async stages may execute on:
    * same thread
    * common pool
    * custom executor

# Q-30 What is Virtual Thread?

## The Analogy

Here is the simplest explanation using a Restaurant Analogy.

### The Old Way: Platform Threads (The "Personal Butler" Model)

Imagine a restaurant (Your Server) with 100 tables (Tasks). In the old version of Java (Platform Threads), you hired 
**one Butler for every table**.

1. A Customer sits down.
2. A Butler runs over.
3. The Customer says: "Let me think about what I want to order..." (This is like a database call or waiting for a file).
4. **The Problem:** The Butler just **stands there waiting**. He cannot help anyone else. He is blocked.
5. **The Limit:** You can only hire 1,000 Butlers because they are expensive (RAM). If 1,001 customers come, the new guy waits outside.

### The New Way: Virtual Threads (The "Order Pad" Model)

In Java 21+ (Virtual Threads), we change the rules.

1. A Customer sits down.
2. A Waiter runs over.
3. The Customer says: "Let me think..."
4. **The Magic:** The Waiter **leaves immediately**. He writes "Table 5 is thinking" on a sticky note (The Virtual Thread) 
and sticks it on the table.
5. The Waiter runs to help Table 6.
6. When Table 5 is ready, **any available Waiter** sees the sticky note, runs over, and continues the service.

**Result:** You only need **5 Waiters** (Carrier Threads) to serve **1,000,000 Tables** (Virtual Threads).

### The Technical Translation

1. The Waiter (Carrier Thread): This is the expensive OS Thread (Platform Thread).
We only have a few of these (usually equal to your CPU cores).
2. The Sticky Note (Virtual Thread): This is the Virtual Thread. It is just a tiny piece of memory in Java. 
It is not "real" to the Operating System.
3. "Let me think" (Blocking I/O): When your code pauses (waiting for a Database or API), 
Java unmounts the Virtual Thread (puts the sticky note down) and frees up the Carrier Thread to do other work.

### Why is this huge?

* Before: You could handle ~5,000 simultaneous users.
* Now: You can handle ~1,000,000 simultaneous users on the same hardware.

You don't need to change your coding style. You write code that looks like it blocks (wait for DB), 
but under the hood, it's non-blocking and superfast.


## Example of Virtual Thread

Here is an example of Virtual Thread:

```java
import java.time.Duration;

public class VirtualThreadExample {

    public static void main(String[] args) throws Exception {

        Runnable task = () -> {
            String name = Thread.currentThread().toString();
            System.out.println("Started " + name);

            try {
                // Simulate a blocking API / DB / network call
                Thread.sleep(Duration.ofSeconds(2));
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            System.out.println("Finished " + name);
        };

        // Create MANY virtual threads
        for (int i = 0; i < 10_000; i++) {
            Thread.startVirtualThread(task);
        }

        Thread.sleep(5000);
        System.out.println("Main done");
    }
}
```

# Q-31 What is Thread Local?

`ThreadLocal` is a Java class that lets you create variables that can only be read and written by the same thread.

Think of it as a **"Global Map"** where the **Key** is the **Thread itself**. Even though you define the `ThreadLocal` variable 
as `static` (global), when Thread A reads it, it gets Thread A's value. When Thread B reads it, it gets Thread B's value. 
They never interfere with each other.

## The Purpose

1. **Carrying Context (The "Invisible Backpack"):**
    * Instead of passing parameters (like `UserContext`, `TransactionID`, or `DatabaseConnection`) through every 
   single method in your call stack (`Controller` -> `Service` -> `Repository` -> `Helper`), you put it in a `ThreadLocal` at 
   the start.
    * Any method downstream can reach into the "backpack" and grab it.
    * Real-world use: Spring Security (`SecurityContextHolder`), Log4j MDC (Mapped Diagnostic Context), 
   Database Transaction Managers.

2. **Thread Safety for "Unsafe" Objects:**
    * Some older classes (like `SimpleDateFormat`) are **not** thread-safe. If you share one instance across 
    threads, it crashes or gives wrong dates.
    * Instead of using `synchronized` (which is slow), you give each thread its own private instance using `ThreadLocal`.


## Code Example: The "Context Holder" Pattern

This is the most common pattern you will see in Enterprise Java (Spring, Hibernate, etc.).

**Scenario:** We want to trace a transactionId across multiple service calls without passing it as an argument.

```java
public class ThreadLocalDemo {

    // 1. Create the ThreadLocal
    // usage: "static final" is best practice for the key itself
    public static final ThreadLocal<String> transactionIdHolder = new ThreadLocal<>();

    public static void main(String[] args) {
        
        // Thread 1: Sets its own ID
        Thread t1 = new Thread(() -> {
            transactionIdHolder.set("TX-123"); // Put value in backpack
            processRequest();
            // IMPORTANT: Cleanup is crucial (explained below)
            transactionIdHolder.remove(); 
        }, "Thread-1");

        // Thread 2: Sets a DIFFERENT ID
        Thread t2 = new Thread(() -> {
            transactionIdHolder.set("TX-456"); // Different value!
            processRequest();
            transactionIdHolder.remove();
        }, "Thread-2");

        t1.start();
        t2.start();
    }

    // A method deep in the code that needs the ID
    public static void processRequest() {
        // It magically grabs the correct value for THIS thread
        String id = transactionIdHolder.get();
        System.out.println(Thread.currentThread().getName() + " is processing " + id);
    }
}
```

**Output:**

```text
Thread-1 is processing TX-123
Thread-2 is processing TX-456
```

Thread 1 never sees "TX-456", and Thread 2 never sees "TX-123".

## The Danger: Memory Leaks (The "Dirty Thread" Problem)

This is a favorite interview topic for Senior Engineers.

**The Setup:** You are using a **Thread Pool** (like Tomcat or `ExecutorService`).

**The Problem:**

1. Request 1 comes in. Borrow **Thread-1** from the pool.
2. You set `ThreadLocal.set("User: Alice")`.
3. The request finishes, **BUT you forget to call** `.remove()`.
4. `Thread-1` goes back to the pool. It is not destroyed; it just sleeps. The "Alice" data is **still inside it**.
5. **Request 2** comes in. It borrows `Thread-1` (reuse).
6. The code calls `ThreadLocal.get()`.
7. Bug: It finds "User: Alice" from the previous request! Now Request 2 thinks it is Alice. 
This is a massive security risk and memory leak.

**The Fix:** Always use a `try-finally` block to ensure cleanup.

```java
try {
    userContext.set(currentUser);
    chain.doFilter(request, response);
} finally {
    // MUST DO THIS to prevent memory leaks and data bleeding
    userContext.remove();
}
```

# Q-32 What is CountDownLatch vs CyclicBarrier?

# Q-33 What is Semaphore?

# Q-34 BlockingQueue (why introduced)

# Q-35 ConcurrentHashMap (how it avoids full locking)

# Q-36 What is ReentrantReadWriteLock?

A `ReentrantReadWriteLock` is a more advanced lock that separates access into two different modes: **Read** and **Write**.

Unlike a standard `ReentrantLock` (or `synchronized`) which is **Exclusive** (only one thread enters, period),
a `ReadWriteLock` **allows multiple threads** to read data simultaneously, as long as no one is writing.

## The Purpose: Performance

The main purpose is to boost concurrency in scenarios where you have many readers 
but few writers (e.g., a Cache, a Configuration map, or a Product Catalog).

* **Standard Lock:** If 10 threads want to read a value, they must form a single-file line. 
Thread 1 reads, then Thread 2, etc. (Slow).
* **ReadWriteLock:** All 10 threads can grab the "Read Lock" and read at the exact same time. 
The "Write Lock" is only needed when data changes.

| Current Holder | Thread Wants READ  | Thread Wants WRITE                              |
|:---------------|:-------------------|:------------------------------------------------|
| **None**       | ✅ Allowed          | ✅ Allowed                                       |
| **Reader(s)**  | ✅ Allowed (Shared) | ❌ Blocked (Must wait for all readers to finish) |
| **Writer**     | ❌ Blocked          | ❌ Blocked                                       |


## Code Example: A Thread-Safe Cache

Here is a cache where `get()` is fast and parallel, but `put()` is exclusive and safe.

```java
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.ReentrantReadWriteLock;
import java.util.concurrent.locks.Lock;

public class ReadWriteCache<K, V> {
    
    private final Map<K, V> map = new HashMap<>();
    private final ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();
    
    // Extract the two separate locks
    private final Lock readLock = rwLock.readLock();
    private final Lock writeLock = rwLock.writeLock();

    // WRITER: Exclusive access
    public void put(K key, V value) {
        writeLock.lock(); // Only ONE thread can be here
        try {
            System.out.println(Thread.currentThread().getName() + " is writing " + key);
            Thread.sleep(1000); // Simulate slow write
            map.put(key, value);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            writeLock.unlock();
        }
    }

    // READER: Shared access
    public V get(K key) {
        readLock.lock(); // MANY threads can be here at once
        try {
            System.out.println(Thread.currentThread().getName() + " is reading " + key);
            return map.get(key);
        } finally {
            readLock.unlock();
        }
    }
}
```

### Visualizing the difference

If you run get() from 5 threads:
* With ReentrantLock:
    * Thread 1 enters... exits.
    * Thread 2 enters... exits.
    * (Serial execution)
* With ReentrantReadWriteLock:
    * Thread 1, 2, 3, 4, 5 enter simultaneously.
    * (Parallel execution)

## Critical "Senior Dev" Warning

Don't blindly use this everywhere. `ReentrantReadWriteLock` has overhead.

* It is more complex to manage than a standard lock.
* If you have **mostly writes** (or equal reads/writes), it is actually **slower** than a standard `ReentrantLock` 
because of the extra logic to track readers.
* Modern Alternative: Java 8 introduced `StampedLock`, which is faster and supports "Optimistic Reads," often 
replacing `ReentrantReadWriteLock` in high-performance code.


# Q-37 What is Monitor object?

In Java, a Monitor is the internal synchronization mechanism used to handle concurrency. 
It is the theoretical concept behind the `synchronized` keyword and `wait()`/`notify()`.

Every object in Java is associated with a Monitor. You don't see it explicitly in code, but the JVM creates 
it when you use synchronization.

## The Mental Model: "The Secure Room"

Imagine the Monitor as a special building with three distinct areas:

1\. **The Entry Set (The Hallway):** Where threads wait before they can enter the synchronized block. 
They are fighting to get in.

2\. **The Owner (The Room):** The critical section. **Only one thread** can be here at a time. It holds the "Key" (Lock).

3\. **The Wait Set (The Waiting Room):** A separate room where threads go if they voluntarily give up the key
(`via wait()`) because they are waiting for a condition to change.


## How it maps to Code

1\. Entering (`synchronized`):

* Thread tries to enter **The Owner** area.
* If occupied, it goes to the **Entry Set** and becomes `BLOCKED`.
* Senior Detail: The JVM decides who gets in next (usually not FIFO/Fair).

2\. Working (Inside the block):

* The thread is the **Owner**. It holds the monitor lock. No one else can enter.

3\. Pausing (`wait()`):

* The thread realizes it can't proceed (e.g., the queue is empty).
* It releases the lock **immediately** and moves to the **Wait Set**.
* State changes from `RUNNABLE` → `WAITING`.

4\. Resuming (`notify()`):

* Another thread (current Owner) calls `notify()`.
* It picks a thread from the **Wait Set** and moves it to the **Entry Set**.
* Crucial Detail: The woken thread does not run immediately. It must wait in the Entry Set until the current Owner
releases the lock, then it fights to acquire the lock again.

Here are some examples of monitor object.

### Example 1: The Simplest Example (Mutual Exclusion)

This uses the Monitor solely for its Mutex (Locking) capability. 
This is the most common use case: **protecting shared state**.

```java
public class SharedCounter {
    // Every instance of SharedCounter is a Monitor Object
    private int count = 0;

    // The 'synchronized' keyword acquires the Monitor Lock of 'this' instance
    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}
```

* **The Monitor:** The `SharedCounter` instance itself.
* **The Action:** Threads fight for the lock in the "Entry Set." No coordination (`wait`/`notify`) is needed here, 
just exclusion.

### Example 2: The Classic "Wait/Notify" Example (Coordination)

This utilizes the full power of the Monitor: **Mutex + Wait Set**. 
This is the textbook definition of a Monitor (handling condition variables).

**Scenario:** A specialized "Blocking Queue" where producers must wait if full, and consumers must wait if empty.

```java
public class SimpleBlockingQueue<T> {
    private final Queue<T> queue = new LinkedList<>();
    private final int limit;
    
    // We use a dedicated object as the Monitor (Best Practice)
    // instead of 'this' to avoid external code locking on our instance.
    private final Object monitor = new Object();

    public SimpleBlockingQueue(int limit) {
        this.limit = limit;
    }

    public void put(T item) throws InterruptedException {
        synchronized (monitor) {
            // 1. Guard Condition: While full, go to Wait Set
            while (queue.size() == limit) {
                monitor.wait(); // Releases lock, thread sleeps in Wait Set
            }
            
            // 2. Critical Section: Modify State
            queue.add(item);
            
            // 3. Notification: Wake up waiting threads (Consumers)
            monitor.notifyAll(); // Moves threads from Wait Set -> Entry Set
        }
    }

    public T take() throws InterruptedException {
        synchronized (monitor) {
            // 1. Guard Condition: While empty, go to Wait Set
            while (queue.isEmpty()) {
                monitor.wait();
            }
            
            // 2. Critical Section
            T item = queue.remove();
            
            // 3. Notification: Wake up waiting threads (Producers)
            monitor.notifyAll();
            return item;
        }
    }
}
```

### Example 3: The "Modern" Explicit Monitor (ReentrantLock)

In modern Java (JDK 5+), we often implement the Monitor pattern explicitly using `ReentrantLock` and `Condition`. 
This is functionally identical but offers more control (e.g., multiple wait sets).

```java
public class ExplicitMonitor {
    private final Lock lock = new ReentrantLock();
    // A specific 'Wait Set' for a specific condition
    private final Condition notEmpty = lock.newCondition(); 

    public void doWork() throws InterruptedException {  
        lock.lock(); // Enter the Monitor
        try {
            while (isEmpty()) {
                notEmpty.await(); // Go to 'Wait Set' (Releases lock)
            }
            consume();
        } finally {
            lock.unlock(); // Exit the Monitor
        }
    }
}
```

Note that with `ReentrantLock`, **entry set** is still linked to lock, but **wait set** is part of `Condition`.

# Q-38 Which object shouldn't be used as a Monitor object?

Objects that are publicly accessible, mutable, or shared unintentionally should not be used as monitor objects.

1\. String objects

* Strings are **interned and shared**
* Different code may unknowingly synchronize on the same `String`
* Can cause accidental deadlocks

❌ Bad:
```java
synchronized ("LOCK") { }
```

2\. Wrapper objects (Integer, Long, etc.)

* Immutable but cached and reused
* Auto-boxing may return the same instance

❌ Bad:
```java
Integer lock = 1;
synchronized (lock) { }
```

3\. Class objects (SomeClass.class)

* Globally accessible
* Any code can synchronize on it
* Creates global contention

❌ Bad:
```java
synchronized (MyService.class) { }
```

4\. this (in public classes)

* Exposes your lock to external callers
* External code can block your internals

❌ Risky:

```java
synchronized (this) { }
```

5\. Mutable objects used for other purposes

* If the reference changes, locking breaks
* Monitor identity must be stable

❌ Bad:
```java
lock = new Object(); // breaks synchronization
```

## What SHOULD be used instead

✔ A private, final lock object

```java
private final Object lock = new Object();

synchronized (lock) {
    // safe
}
```

Final rule (lock this in):

Monitor object must be:
* ✔ private
* ✔ final
* ✔ dedicated only for locking


# Q-39 Is it valid to use a synchronized block inside a Lambda expression?

Yes, absolutely. A lambda expression is just a shorthand for an implementation of a functional interface. 
You can write any valid Java code inside the curly braces `{ ... }`, including a synchronized block.

However, there is a **critical scope rule** you must know.

## The Code Example

```java
public class LambdaSync {
    private final Object lock = new Object();
    private int count = 0;

    public void startTask() {
        Runnable task = () -> {
            // YES: This is valid
            synchronized (lock) {
                count++;
                System.out.println(Thread.currentThread().getName() + ": " + count);
            }
        };

        new Thread(task).start();
    }
}
```

## The "Gotcha" (Scope of this)

If you write `synchronized(this)` inside an anonymous inner class vs. a lambda, the meaning of `this` changes.

* **In an Anonymous Inner Class:** this refers to the inner class instance (the Runnable itself).
* **In a Lambda:** `this` refers to the enclosing class instance (e.g., `LambdaSync`). Lambdas do not introduce 
a new scope for `this`.

```java
public void demonstration() {
    // ANONYMOUS CLASS
    Runnable r1 = new Runnable() {
        @Override
        public void run() {
            synchronized(this) { 
                // Locks on the 'r1' object itself!
            }
        }
    };

    // LAMBDA
    Runnable r2 = () -> {
        synchronized(this) { 
            // Locks on the 'LambdaSync' (enclosing) instance!
        }
    };
}
```

## Example

```java
public void demonstration() {
    // ANONYMOUS CLASS
    Runnable r1 = new Runnable() {
        @Override
        public void run() {
            synchronized(this) { 
                // Locks on the 'r1' object itself!
            }
        }
    };

    // LAMBDA
    Runnable r2 = () -> {
        synchronized(this) { 
            // Locks on the 'LambdaSync' (enclosing) instance!
        }
    };
}
```

You can synchronize inside a lambda.

* **Best Practice:** Lock on a specific, private final object (like `lock` in the first example) rather than `this` to 
avoid confusion about lexical scoping.
* **Constraint:** Any local variable you lock on (captured from outside) must be **effectively final**.

# Q-40 Does thread release the lock after OS preemption?

# Q-41 Is it possible for JVM to re-order statements inside synchronized block?
AtomicReference

# Q-42 What is AtomicReference?

# Q-43 When would you use AtomicReference instead of synchronized?

Atomic references are ideal for atomic replacement of immutable objects, while synchronized blocks remain
the right choice for protecting multi-step operations and invariants.

# Q-44 What is Cache-Coherence?

# Q-45 What is False Sharing?

# Q-46 What is Cache Affinity?

# Q-47 Why False Sharing is more likely happen with ExecutorService?


