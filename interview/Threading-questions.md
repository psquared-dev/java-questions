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
<!-- TOC -->

# Q-1 What is the difference between wait() and sleep() in Java?

### wait()

* Puts the thread into `WAITING` (or `TIMED_WAITING`) state
* Releases the lock immediately
* Thread stays waiting until `notify()` / `notifyAll()` is called
* After notify:
    * Thread moves to `BLOCKED` (to re-acquire the lock)
    * Then to RUNNABLE

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
    * Moves from WAITING → BLOCKED
    * Competes to re-acquire the lock


### notifyAll()

* Wakes all threads waiting on the same object's monitor
* All woken threads:
    * Move to BLOCKED
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

A notified thread does not run immediately. It first moves to the BLOCKED state and must re-acquire 
the monitor lock before continuing execution.

### Full lifecycle (clean mental model)

When a thread is notified:

1. WAITING
    * Thread was sleeping via `wait()`

2. `notify()` / `notifyAll()` called
    * Thread is moved to BLOCKED
    * It does not run yet

3. BLOCKED
    * Waiting to re-acquire the same monitor lock

4. RUNNABLE

    * Once it gets the lock
    * Execution resumes after `wait()`

This is why:
* Notification ≠ immediate execution
* Lock ownership still matters


# Q-6 What is the difference between BLOCKED and WAITING thread states?

### WAITING state

A thread is in WAITING when:

* It has **explicitly decided to pause**
* It is waiting for a **signal**, not a lock

Caused by:

* wait()
* join()
* park()

How it exits:

* `notify()` / `notifyAll()`
* Target thread finishes (for `join()`)

### BLOCKED state

A thread is in BLOCKED when:

* It wants to enter a synchronized block
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

The `volatile` keyword establishes a happens-before relationship.

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



