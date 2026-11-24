<!-- TOC -->
* [Q - What is the difference between wait() and sleep() in Java?](#q---what-is-the-difference-between-wait-and-sleep-in-java)
* [Q - What happens if notify() is called before wait()? Does the waiting thread get notified later? Why or why not?](#q---what-happens-if-notify-is-called-before-wait-does-the-waiting-thread-get-notified-later-why-or-why-not)
* [Q - Why should wait() always be called inside a while loop and not an if statement?](#q---why-should-wait-always-be-called-inside-a-while-loop-and-not-an-if-statement)
* [Q - Difference between notify() and notifyAll()](#q---difference-between-notify-and-notifyall)
* [Q - Why does a thread wake up from wait() and still not run immediately? What happens after it is notified?](#q---why-does-a-thread-wake-up-from-wait-and-still-not-run-immediately-what-happens-after-it-is-notified)
* [Q - What is the difference between BLOCKED and WAITING thread states?](#q---what-is-the-difference-between-blocked-and-waiting-thread-states)
* [Q - Why does wait() release the lock but sleep() does not?](#q---why-does-wait-release-the-lock-but-sleep-does-not)
* [Q - What problem does volatile solve, and what problem does it NOT solve?](#q---what-problem-does-volatile-solve-and-what-problem-does-it-not-solve)
* [Q - Why is volatile sufficient for a stop flag but not for a counter?](#q---why-is-volatile-sufficient-for-a-stop-flag-but-not-for-a-counter)
* [Q - What is a deadlock? Can you name the four necessary conditions for deadlock?](#q---what-is-a-deadlock-can-you-name-the-four-necessary-conditions-for-deadlock)
* [Q - Why Thread.stop() is not recommended to stop the thread?](#q---why-threadstop-is-not-recommended-to-stop-the-thread)
* [Q - What is the correct way to stop a thread in Java?](#q---what-is-the-correct-way-to-stop-a-thread-in-java)
* [Q - How does the `Thread.interrupt()` mechanism work conceptually?](#q---how-does-the-threadinterrupt-mechanism-work-conceptually)
* [Q - How do you handle interruption if the thread is actively working (Awake)?](#q---how-do-you-handle-interruption-if-the-thread-is-actively-working-awake)
* [Q - How do you handle interruption if the thread is Sleeping or Waiting?](#q---how-do-you-handle-interruption-if-the-thread-is-sleeping-or-waiting)
* [Q - What is the "Flag Clearing" trap when InterruptedException is thrown?](#q---what-is-the-flag-clearing-trap-when-interruptedexception-is-thrown)
* [Q - Does interrupt() wake up a thread waiting for a Lock (BLOCKED)?](#q---does-interrupt-wake-up-a-thread-waiting-for-a-lock-blocked)
* [Q - What is ReentrantLock?](#q---what-is-reentrantlock)
* [Q - What is a race condition?](#q---what-is-a-race-condition)
* [Q - What is atomicity, and how is it different from visibility?](#q---what-is-atomicity-and-how-is-it-different-from-visibility)
* [Q - Why was ExecutorService introduced? What problem does it solve compared to creating threads manually?](#q---why-was-executorservice-introduced-what-problem-does-it-solve-compared-to-creating-threads-manually)
* [Q - What is an Executor interface?](#q---what-is-an-executor-interface)
* [Q - What is ExecutorService interface?](#q---what-is-executorservice-interface)
* [Q - What is ThreadPoolExecutor?](#q---what-is-threadpoolexecutor)
* [Q - Explain some types of ExecutorService?](#q---explain-some-types-of-executorservice)
* [Q - Explain ForkJoinPool with an example](#q---explain-forkjoinpool-with-an-example)
* [Q - What's the differences b/w ForkJoinPool and ThreadPoolExecutor?](#q---whats-the-differences-bw-forkjoinpool-and-threadpoolexecutor)
* [Q - How ForkJoinPool() is different from Executors.newWorkStealingPool()](#q---how-forkjoinpool-is-different-from-executorsnewworkstealingpool)
* [Q - What is CompletableFuture?](#q---what-is-completablefuture)
* [Q - What is Virtual Thread?](#q---what-is-virtual-thread)
* [Q - What is Thread Local?](#q---what-is-thread-local)
* [Q - What is CountDownLatch](#q---what-is-countdownlatch)
* [Q - What is CyclicBarrier?](#q---what-is-cyclicbarrier)
* [Q - What is Semaphore?](#q---what-is-semaphore)
* [Q - BlockingQueue (why introduced)](#q---blockingqueue-why-introduced)
* [Q - ConcurrentHashMap (how it avoids full locking)](#q---concurrenthashmap-how-it-avoids-full-locking)
* [Q - What is ReentrantReadWriteLock?](#q---what-is-reentrantreadwritelock)
* [Q - What is Monitor object?](#q---what-is-monitor-object)
* [Q - Which object shouldn't be used as a Monitor object?](#q---which-object-shouldnt-be-used-as-a-monitor-object)
* [Q - Is it valid to use a synchronized block inside a Lambda expression?](#q---is-it-valid-to-use-a-synchronized-block-inside-a-lambda-expression)
* [Q - Does thread release the lock after OS preemption?](#q---does-thread-release-the-lock-after-os-preemption)
* [Q - What is the as-if-serial rule in Java, and what does it allow the JVM to do?](#q---what-is-the-as-if-serial-rule-in-java-and-what-does-it-allow-the-jvm-to-do)
* [Q - Is it possible for JVM to re-order statements inside a synchronized block?](#q---is-it-possible-for-jvm-to-re-order-statements-inside-a-synchronized-block)
* [Q - What is AtomicReference?](#q---what-is-atomicreference)
* [Q - When would you use AtomicReference instead of synchronized?](#q---when-would-you-use-atomicreference-instead-of-synchronized)
* [Q - What is Cache-Coherence?](#q---what-is-cache-coherence)
* [Q - What is False Sharing?](#q---what-is-false-sharing)
* [Q - What is Cache Affinity?](#q---what-is-cache-affinity)
* [Q - Why False Sharing is more likely happen with ExecutorService?](#q---why-false-sharing-is-more-likely-happen-with-executorservice)
* [Q - How to provide initial value when using ThreadLocal?](#q---how-to-provide-initial-value-when-using-threadlocal)
* [Q - What is InheritableThreadLocal?](#q---what-is-inheritablethreadlocal)
* [Q - What is ThreadLocalMap?](#q---what-is-threadlocalmap)
<!-- TOC -->

# Q - What is the difference between wait() and sleep() in Java?

<h3> wait() </h3>

* Puts the thread into `WAITING` (or `TIMED_WAITING`) state
* Releases the lock immediately
* Thread stays waiting until `notify()` / `notifyAll()` is called
* After notify:
    * Thread moves to `BLOCKED` (to re-acquire the lock)
    * Then to `RUNNABLE`
* Belongs to: `Object` class
* Used for: inter-thread communication

<h3> sleep() </h3>

* Puts the thread into `TIMED_WAITING`
* Does NOT release the lock
* Automatically wakes up after time expires
* Then moves to `RUNNABLE`
* Belongs to: `Thread` class
* Used for: pausing execution

<h3> Quick state summary (very useful) </h3>

```text
wait()   → WAITING → BLOCKED → RUNNABLE
sleep()  → TIMED_WAITING → RUNNABLE
```


----------------------


# Q - What happens if notify() is called before wait()? Does the waiting thread get notified later? Why or why not?

No. If `notify()` is called before wait(), the notification is completely lost, and a thread 
that calls `wait()` later will not receive it. It will block indefinitely unless another notification 
occurs later.


<h3> Step-by-Step Verification </h3>

1. **How `wait()` behaves:** A thread is **only** added to the Monitor's Wait Set at the exact 
   moment it executes `wait()`. Prior to calling `wait()`, the thread is not in the Wait Set at all.
2. **How `notify()` behaves:** Calling `notify()` inspects the Wait Set *at that exact instant*. 
   It attempts to wake up one thread currently sitting in that set. If the Wait Set is 
   empty when `notify()` is called, **the notification does nothing and disappears forever**. 
   Java keeps no memory, flag, or "counter" of past `notify()` calls.
3. **The Sequence:** If `notify()` executes first, it finds an empty Wait Set and exits. 
   When `wait()` executes later, the thread enters the Wait Set and goes to sleep, waiting for a signal 
   that has already passed. It will remain stuck in `WAITING` indefinitely unless another thread 
   calls `notify()` in the future.


----------------------



# Q - Why should wait() always be called inside a while loop and not an if statement?

`while` loop is used so the condition is re-checked every time the thread wakes up, because waking up 
does not guarantee the condition is `true`.

<h3> Why re-checking is necessary </h3>

A thread can wake up when:

* It was notified
* It was notified but another thread consumed the resource first
* It woke up spuriously (no notify at all)

So waking up only means:
> "Something might have changed"

Not:
> You can proceed safely

<h3> What while guarantees </h3>

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


<h3> Why if is dangerous </h3>

```java
if (!condition) {
    wait();
}
```

This checks the condition only once. If the condition changes again before the thread runs:

* The thread blindly proceeds
* Bugs happen

# Q - Difference between notify() and notifyAll()

**Explain the following:**
* What each method does.
* Why `notify()` is considered dangerous (Lost Wakeup problem).
* Why `notifyAll()` is safer.


<h3> notify() </h3>

* Wakes one arbitrary thread waiting on the same object's monitor
* JVM chooses which thread
* That thread:
    * Moves from `WAITING` → `BLOCKED`
    * Competes to re-acquire the lock


<h3> notifyAll() </h3>

* Wakes all threads waiting on the same object's monitor
* All woken threads:
    * Move to `BLOCKED`
    * Compete for the lock
    * Re-check the condition in a while loop

<h3> Why notify() is dangerous </h3>

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

<h3> Why notifyAll() is safer </h3>

* Wakes everyone
* Each thread re-checks its condition
* Only the thread whose condition is true proceeds
* Others go back to waiting

This avoids:

* Missed signals
* Deadlocks caused by wrong thread selection

# Q - Why does a thread wake up from wait() and still not run immediately? What happens after it is notified?

A notified thread does not run immediately. It first moves to the `BLOCKED` state and must re-acquire 
the monitor lock before continuing execution.

<h3> Full lifecycle (clean mental model) </h3>

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


# Q - What is the difference between BLOCKED and WAITING thread states?

<h3> WAITING state </h3>

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

<h3> BLOCKED state </h3>

A thread is in `BLOCKED` when:

* It wants to enter a `synchronized` block
* But another thread already holds the lock

Caused by:
* Contention for a monitor lock

How it exits:
* Lock becomes available

# Q - Why does wait() release the lock but sleep() does not?

`wait()` releases the lock because it is used for inter-thread coordination and allows other 
threads to modify shared state, whereas `sleep()` is only a time delay and therefore does not release any locks.

<h3> Why wait() releases the lock but sleep() does not </h3>

Because `wait()` is designed for coordination between threads, while `sleep()` is only for pausing execution.

<h3> wait() — coordination mechanism </h3>

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

<h3> sleep() — time-based pause </h3>

* Used only to pause execution
* Thread does NOT depend on any shared condition
* There is no reason to release the lock

So:

```text
sleep() → keeps lock → resumes after time
```

# Q - What problem does volatile solve, and what problem does it NOT solve?

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


<h3> Memory visibility </h3>

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

<h3> Correct ordering of instructions across threads </h3>

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

<h3> The One Rule to Remember (Perfect) </h3>

> If Thread A writes to a volatile variable, and Thread B later reads that same variable, then 
> everything Thread A did before the write is visible to Thread B after the read.

✅ This single rule fully captures:

* Memory visibility
* Correct instruction ordering across threads
* The meaning of happens-before

<h3> What volatile does NOT solve </h3>

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


# Q - Why is volatile sufficient for a stop flag but not for a counter?

`volatile` is sufficient for a stop flag because it guarantees visibility — when one thread updates the flag, other 
threads immediately see the change. However, it is not sufficient for a counter because incrementing a counter 
is not an atomic operation and requires mutual exclusion, which `volatile` does not provide.

<h3> Why this distinction matters </h3>

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



# Q - What is a deadlock? Can you name the four necessary conditions for deadlock?

A deadlock is a situation in concurrent programming where two or more threads are blocked forever, waiting for 
each other to release a resource.

The "Two-Key" Analogy: Imagine two people, Alice and Bob, and two locked doors, Door A and Door B.

* Alice holds the key to Door A but needs the key to Door B to proceed.
* Bob holds the key to Door B but needs the key to Door A to proceed.
* Neither is willing (or able) to hand over their key until they get the other one.
* Result: They both stand there forever.


<h3> The 4 Necessary Conditions (Coffman Conditions) </h3>

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

<h3> Example of Deadlock </h3>

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

<h3> Mapping the Code to the 4 Conditions </h3>

| Condition               | Where it is in the code                                                                                                                     |
|:------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------|
| **1. Mutual Exclusion** | The `synchronized` keyword ensures only one thread can hold `lockAlice` at a time.                                                          |
| **2. Hold and Wait**    | Look at Thread 1: It is **holding** `lockAlice` (line 10) and **waiting** for `lockBob` (line 16). It refuses to let go of Alice.           |
| **3. No Preemption**    | Java cannot force Thread 1 to release `lockAlice`. Thread 2 cannot say "Give me the lock now!". Thread 1 must finish the block voluntarily. |
| **4. Circular Wait**    | Thread 1 waits for Bob. Thread 2 waits for Alice. It is a perfect circle.                                                                   |


<h3> How to prevent the deadlock? </h3>

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


# Q - Why Thread.stop() is not recommended to stop the thread?

The single most important reason `Thread.stop()` is deprecated is Data Corruption.

It forces a thread to release its locks immediately, even if it was in the middle of a critical operation.

Here is exactly how `Thread.stop()` breaks your specific code example.

<h3> Example </h3>

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


# Q - What is the correct way to stop a thread in Java?

You should never force a thread to stop (e.g., `stop()`) because it can leave shared data in a broken state. 
Instead, you "ask" the thread to stop using `interrupt()`, and the thread must voluntarily agree to shut down.

<h3> The Code Example: </h3>

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

# Q - How does the `Thread.interrupt()` mechanism work conceptually?

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

# Q - How do you handle interruption if the thread is actively working (Awake)?

If the thread is CPU-busy (calculating, processing), it acts as a "Gatekeeper". It must explicitly check 
the flag using `isInterrupted()` before starting the next chunk of work.

<h3> The Code Example: </h3>

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

# Q - How do you handle interruption if the thread is Sleeping or Waiting?

If the thread is paused (sleeping), it cannot check the while loop. The JVM handles this by waking 
the thread up and throwing an `InterruptedException`. This is the "Emergency Alarm."

<h3> The Code Example: </h3>

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

# Q - What is the "Flag Clearing" trap when InterruptedException is thrown?

When `InterruptedException` is thrown, the JVM clears the `interrupt` flag (resets it to `false`).

* **The Trap:** If you catch the exception and don't fix the flag, your while loop will think everything is fine
and keep running.
* **The Fix:** Call `Thread.currentThread().interrupt()` inside the catch block to put the flag back to `true`.

<h3> The Code Example (The "Zombie" Thread Bug): </h3>

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

# Q - Does interrupt() wake up a thread waiting for a Lock (BLOCKED)?

No. A thread waiting for a lock is `BLOCKED`, not `WAITING`. `interrupt()` has no effect on it. 
It will sit there frozen until it gets the lock.

<h3> The Code Example: </h3>

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


# Q - What is ReentrantLock?

**What does reentrant mean?**

It means:

> A thread that already owns a lock can acquire the same lock again without blocking.

* `synchronized`
* ReentrantLock

Both are reentrant locks.

Now coming back to `ReentrantLock` class. 

`ReentrantLock` is a lock implementation provided by Java in `java.util.concurrent.locks`.

It is an **explicit locking mechanism**, meaning:

* You **manually acquire** the lock
* You **manually release** the lock
* You get **more control** than `synchronized`

<h2> Why does it exist when we already have synchronized? </h2>

`synchronized` is:

* Simple
* Safe
* Easy to use

But it is also:

* Rigid
* Limited
* Hard to control in advanced concurrency scenarios

`ReentrantLock` was introduced to solve **real-world concurrency limitations** of `synchronized`.

<h2> First: What does "Reentrant" mean? </h2>

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

<h2> Basic usage of ReentrantLock </h2>

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

<h2> Key capabilities of ReentrantLock (with examples) </h2>

Now let’s build intuition feature by feature.

<h3> Explicit lock control (manual) </h3>

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

<h3> Try acquiring a lock (non-blocking) </h3>

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

<h3> Interruptible lock acquisition </h3>

❌ synchronized

If a thread is in `BLOCKED` state waiting for a monitor lock:
* `interrupt()` does NOTHING
* Thread stays blocked

✅ ReentrantLock

```java
lock.lockInterruptibly();
```

Now:
* Thread can be interrupted
* Useful during shutdown or cancellation

<h3> Fairness (very important interview point) </h3>

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

<h3> Condition variables (replacement for wait/notify) </h3>

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

<h2> Why ReentrantLock is NOT a replacement for synchronized </h2>

Important interview nuance:
> `ReentrantLock` is not better, it is more powerful.

Use:

* `synchronized` → simple, safe, low-risk
* `ReentrantLock` → complex, performance-sensitive, advanced control

<h2> When SHOULD you use ReentrantLock? </h2>

* ✔️ You need `tryLock()`
* ✔️ You need interruptible locking
* ✔️ You need fairness
* ✔️ You need multiple condition queues
* ✔️ You’re building concurrency primitives or frameworks

<h2> When should you NOT use it? </h2>

* ❌ Simple synchronization
* ❌ Low contention
* ❌ When correctness > flexibility


# Q - What is a race condition?

A race condition occurs when multiple threads perform unsynchronized operations on 
shared mutable state, and the final result is unpredictable and depends on the order of execution of threads


# Q - What is atomicity, and how is it different from visibility?

<h2> Atomicity </h2>

Atomicity means an operation is indivisible — it either happens completely or not at all, 
and no other thread can observe it in an intermediate state.

<h2> Visibility </h2>

Visibility ensures that when one thread updates a variable, other threads see the updated value 
instead of a stale cached value.


# Q - Why was ExecutorService introduced? What problem does it solve compared to creating threads manually?


Before `ExecutorService`, developers created threads manually using the `Thread` class. 
This approach worked for small programs but caused serious problems in real-world applications.

<h2> Problems with creating threads manually </h2>

**`ExecutorService`** decouples **task submission** 
from **thread execution, pooling, and lifecycle management**, replacing 
manual `new Thread(...).start()` calls with a managed pool of worker threads.

* **High creation cost:** Spawning OS/JVM threads repeatedly wastes CPU and 
    stack memory (`new Thread(task).start()`).
* **Unbounded thread count:** Spawning threads per task (e.g., inside a `10,000`-item loop) 
    can exhaust CPU, heap/stack memory, and crash the JVM.
* **Weak lifecycle control:** No built-in way to reuse idle threads, shut down 
    gracefully (`shutdown` / `awaitTermination`), or track completion cleanly.
* **Clunky result/error handling:** `Runnable` returns nothing, forcing shared mutable state 
    or manual coordination instead of `Future` / `Callable`.


# Q - What is an Executor interface?

<h2> 1. Why the Executor interface exists </h2>

Before Java 5, concurrency typically looked like this:

```java
new Thread(() -> doWork()).start();
```

This approach has structural problems:

1. Task submission = thread creation.
    * You cannot control how many threads are created.
2. No reuse
    * Threads are expensive; creating them repeatedly is wasteful.
3. No lifecycle management
    * No standard way to shut down, monitor, or throttle execution.
4. Business logic tightly coupled with threading logic
    * Your application logic decides how execution happens.

Java introduced `Executor` to separate concerns:
> What to execute vs How to execute


<h2> 2. What exactly is Executor? </h2>

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

<h3> 3. Conceptual model </h3>

Think of Executor as a **task sink**.

You submit work; something else decides:

* Which thread runs it
* When it runs
* Whether it runs immediately, later, or never

<h3> 4. Where real power comes from (Executor implementations) </h3>

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

<h3> Error handling behavior </h3>

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

<h2> One-line summary </h2>

The `Executor` interface is used to submit tasks, but it is the concrete 
implementation (like `ThreadPoolExecutor` or `ForkJoinPool`) that decides how to execute them.

# Q - What is ExecutorService interface?

<h2> Step 1 — Where ExecutorService sits in the story </h2>

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

<h2> Step 2 — What ExecutorService actually is </h2>

Formally:

```java
public interface ExecutorService extends Executor
```

This tells us two things:

1. Every `ExecutorService` is an `Executor`
2. It adds management and control

Think of it as:
> Executor + lifecycle + results + control


<h2> Step 3 — The core responsibility of ExecutorService </h2>

`ExecutorService` answers five critical questions that `Executor` cannot:

1. How do I submit work?
2. How do I get results?
3. How do I wait for completion?
4. How do I cancel work?
5. How do I shut down cleanly?

Everything in `ExecutorService` exists to answer one of these.

<h2> Step 4 — Submitting work (execute vs submit) </h2>

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

<h2> Step 5 — Why submit() exists at all </h2>

Real applications often need:

* Results
* Error handling
* Coordination between tasks

`submit()` solves this by returning a handle to the task.

That handle is `Future`.

<h2> Step 6 — Understanding Future </h2>

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

<h2> Step 7 — Callable vs Runnable (why both exist) </h2>

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

<h2> Step 8 — Managing executor lifecycle (very important) </h2>

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


<h2> Step 9 — Waiting for termination </h2>

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

<h2> Step 10 — What ExecutorService deliberately does NOT decide </h2>

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


# Q - What is ThreadPoolExecutor?

`ThreadPoolExecutor` is the primary implementation of the `ExecutorService` interface.

Instead of creating a new thread for every task, `ThreadPoolExecutor` creates a pool of 
worker threads that are reused to execute multiple tasks throughout their lifetime. 
This significantly reduces the overhead associated with thread creation and destruction, leading 
to better performance and resource utilization.

---

<h2> ThreadPoolExecutor Constructor Parameters </h2>

```java
ThreadPoolExecutor(
        int corePoolSize,
        int maximumPoolSize,
        long keepAliveTime,
        TimeUnit unit,
        BlockingQueue<Runnable> workQueue,
        ThreadFactory threadFactory,
        RejectedExecutionHandler handler
)
```

These parameters define the thread creation policy, task scheduling policy, 
thread reuse strategy, and rejection policy of the executor.

| Parameter                      | Meaning                                                                                                                                                                                                                                    |
|--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`corePoolSize`**             | The minimum number of worker threads to keep alive. When a new task is submitted and the current worker count is less than `corePoolSize`, a new worker thread is created to execute the task immediately, even if other workers are idle. |
| **`maximumPoolSize`**          | The maximum number of worker threads that can exist in the pool. Additional threads beyond the core pool are created only when the work queue is full.                                                                                     |
| **`keepAliveTime`**            | The amount of time that idle threads above the `corePoolSize` are allowed to remain idle before being terminated.                                                                                                                          |
| **`TimeUnit`**                 | Specifies the unit of time for `keepAliveTime` (e.g., seconds, milliseconds, minutes).                                                                                                                                                     |
| **`workQueue`**                | The `BlockingQueue` used to hold tasks waiting for execution when all core worker threads are busy. The queue implementation significantly affects the executor's scheduling behavior.                                                     |
| **`ThreadFactory`**            | Responsible for creating new worker threads. It can be customized to set thread names, priorities, daemon status, or uncaught exception handlers.                                                                                          |
| **`RejectedExecutionHandler`** | Defines the policy to apply when a task cannot be accepted because both the work queue is full and the pool has reached `maximumPoolSize`.                                                                                                 |

---

<h2> The Lifecycle of a Task </h2>

When a task is submitted, the executor decides what to do in this strict order:

1. If `poolSize < corePoolSize` then create a new thread (even if other threads are idle)
2. Else, try to enqueue the task into the blocking queue (`queue.offer(task)`)
3. If the queue is full and `poolSize < maxPoolSize` then create a non-core thread.
4. If the queue is full and `poolSize == maxPoolSize` then reject the task (via `RejectedExecutionHandler`)

> **The Golden Rule of Scaling:** The pool will **never** grow past the `corePoolSize` until 
> the `workQueue` is 100% full.

---

<h2> A Common Trait That Surprises People </h2>

Because of that golden rule, look at this incredibly common mistake:

```java
// A queue that can hold infinite tasks
BlockingQueue<Runnable> queue = new LinkedBlockingQueue<>(); 

ThreadPoolExecutor executor = new ThreadPoolExecutor(
    5,    // corePoolSize
    10,   // maximumPoolSize
    60, TimeUnit.SECONDS, 
    queue
);

```

In this setup, **your pool will never, ever create more than 5 threads**. Why? Because a 
default `LinkedBlockingQueue` is unbounded—it can hold an infinite number of tasks. Since the 
queue can never fill up, the executor will never trigger step 3 to hire your "on-call" threads. 
Your `maximumPoolSize` of 10 is completely useless here.

<h2> Types of queues </h2>

When you configure a `ThreadPoolExecutor`, you have to choose a **`BlockingQueue`**. 
This queue acts as the temporary warehouse where tasks sit when your worker threads are too 
busy to process them immediately.

There are **three primary categories** of queues you can use. Choosing one completely 
changes the rules of how your thread pool scales, handles traffic spikes, and 
preserves (or destroys) order.

---

<h3> 1. Bounded Queues (`ArrayBlockingQueue`) </h3>

A bounded queue has a **strict, fixed capacity limit** (a hard ceiling) that you must 
define upfront when you build it.

* **The Blueprint:** `new ArrayBlockingQueue<>(100)` (Holds exactly 100 tasks).

* **How it works with the Executor:**
  1. Tasks go to your `corePoolSize` threads first.
  2. If those core threads are busy, incoming tasks accumulate inside the queue line.
  3. If the queue hits its max limit (e.g., all 100 slots are full), *only then* does the executor spin up extra threads up to your `maximumPoolSize`.
  4. If the max threads are busy AND the queue is full, it triggers your **Rejection Policy**.


* **The Execution Order:** The queue itself hands out tasks in strict First-In, First-Out (**FIFO**) order. However, if your pool has more than 1 thread active, those threads process tasks concurrently on different CPU cores, meaning tasks will still finish out of order. Furthermore, if the queue fills up, new tasks will bypass the queue entirely to run on the newly spawned max threads, scrambling submission order.
* **Best Used For:** Enterprise applications where resource protection is paramount. It guarantees your application will never crash from running out of memory (OOM) because there is a strict ceiling on both threads and tasks.

---

<h3> 2. Unbounded Queues (`LinkedBlockingQueue`, `PriorityBlockingQueue`) </h3>

An unbounded queue has a **practically infinite capacity** (set by default to over 2 billion items). 
It will grow seamlessly to accommodate whatever you throw at it.

* **The Blueprint:** `new LinkedBlockingQueue()` or `new PriorityBlockingQueue()`.

* **How it works with the Executor:**
  1. Tasks are handed to your `corePoolSize` threads.
  2. If they are busy, the tasks flow into the queue.
  3. Because the queue is infinite, it **never fills up**. Therefore, the 
     executor **never creates extra threads** past the `corePoolSize`. 
     Your `maximumPoolSize` setting is completely ignored, and tasks are never rejected.


* **The Execution Order:**
  * `LinkedBlockingQueue`: Handed off in strict **FIFO** order to waiting threads.
  * `PriorityBlockingQueue`: Discards arrival order entirely. It continuously reshuffles itself based on a comparison score you 
     define, forcing **highest-priority tasks to cut to the absolute front of the line**.


* **Best Used For:**
  * `Linked`: Smooth, predictable workloads where you want tasks processed in the sequence they arrived and are 100% certain your core threads can keep up with demand.
  * `Priority`: Background job engines (like processing VIP user requests ahead of standard system cleanups).


* **The Massive Risk:** If tasks arrive faster than your core threads can finish them, the queue will swell endlessly, swallow 
  your system's RAM, and crash your application with an `OutOfMemoryError`.

---

<h3> 3. Direct Hand-off Queues (`SynchronousQueue`) </h3>

This is the anomaly. A direct hand-off queue has a capacity of **exactly zero**. 
It does not act like a bucket; it acts like a face-to-face hand-off between threads.

* **The Blueprint:** `new SynchronousQueue()`.

* **How it works with the Executor:**
  1. When a task is submitted, the queue instantly says, *"I have no storage space to hold this."*
  2. This immediate failure forces the executor to look for an idle thread. 
     If none are idle, it **instantly spawns a new thread** up to your `maximumPoolSize`.
  3. If it hits the maximum pool size, it immediately rejects the task.


* **The Execution Order:** It destroys FIFO. Because it stores no tasks, it 
  stores **sleeping threads** inside an internal memory structure. By default, it 
  operates as a **LIFO (Last-In, First-Out) stack** for those threads. It lets the newest, freshest thread 
  cut to the front of the line to catch the incoming task, which optimizes CPU cache performance 
  but obliterates sequential task ordering.

* **Best Used For:** Maximum throughput and rapid response times under erratic workloads. 
  This is the structural foundation of `Executors.newCachedThreadPool()`, allowing it to 
  dynamically spawn hundreds of threads for sudden traffic spikes and shut them down immediately 
  when the rush ends.

---

<h3> Master Cheat Sheet </h3>

| Queue Type                  | Structural Storage            | Uses `maximumPoolSize`? | Primary Operational Threat             | Execution Sort Order     |
|-----------------------------|-------------------------------|-------------------------|----------------------------------------|--------------------------|
| **`ArrayBlockingQueue`**    | Fixed-size Array Bucket       | **Yes**                 | Task Rejection (`Exception`)           | Strict FIFO              |
| **`LinkedBlockingQueue`**   | Infinite Node Chain Bucket    | **No**                  | `OutOfMemoryError` (RAM Crash)         | Strict FIFO              |
| **`PriorityBlockingQueue`** | Infinite Heap Array Bucket    | **No**                  | Task Starvation (Low priority ignored) | Sorted by Priority Value |
| **`SynchronousQueue`**      | **Zero Storage** (Rendezvous) | **Yes**                 | Massive Thread Spikes                  | LIFO Stack (for threads) |

---

<h2> Under the Hood: Locking Architecture and GC Performance </h2>

<h3> 1. The Two-Lock Optimization (Linked vs. Array) </h3>

This is a massive structural difference in how these queues handle highly concurrent applications.

<h4> Bounded Queues (`ArrayBlockingQueue`) </h4>

An `ArrayBlockingQueue` uses **one single lock** for everything.

* Under the hood, it has one `ReentrantLock`. Both the producers (threads calling `put()`) and 
  the consumers (worker threads calling `take()`) must fight for this exact same lock.
* **The Performance Bottleneck:** If a producer is trying to add a task to the queue at the exact 
  same microsecond a worker thread is trying to pull a task out, **they block each other**. 
  They cannot operate simultaneously.

<h4> Unbounded Queues (`LinkedBlockingQueue`) </h4>

A `LinkedBlockingQueue` uses a brilliant **"Two-Lock Queue" algorithm** (originally designed 
by researchers Michael Scott and John Mellor-Crummey). It splits the synchronization into two 
independent locks:

1. `takeLock`: Handled exclusively by consumer threads pulling from the head.
2. `putLock`: Handled exclusively by producer threads inserting at the tail.

```text
         ┌───────────────┐                  ┌───────────────┐
         │   takeLock    │                  │   putLock     │
         └───────┬───────┘                  └───────┬───────┘
                 ▼                                  ▼
   Head ──► [Node] ──► [Node] ──► [Node] ──► [Node] ──► Tail

```

Because the head and the tail of a linked list are physically separate objects in memory, **a producer thread can insert a task at the exact same moment a worker thread is pulling a task out.** They do not contend for the same lock, which drastically increases throughput under heavy multi-threaded traffic.

---

<h3> 2. Memory Allocation & Garbage Collection (GC) Pressure </h3>

This is the hidden operational cost that bites teams when they scale up their applications.

<h4> Bounded Queues (`ArrayBlockingQueue`) — Allocated Upfront </h4>

Because an `ArrayBlockingQueue` uses a fixed-size Java array under the hood (`Object[] items`), **all the memory for the queue slots is allocated at the exact moment you instantiate it.**

* If you write `new ArrayBlockingQueue<>(100_000)`, Java immediately claims a contiguous block of memory big enough to hold 100,000 object references.
* As tasks flow into and out of the array, the array indices simply change. No new internal wrapper objects are created or destroyed. Memory footprint is flat, predictable, and causes **virtually zero Garbage Collection pressure**.

<h4> Unbounded Queues (`LinkedBlockingQueue`) — Swift Memory Spikes & GC Stress </h4>

A `LinkedBlockingQueue` allocates memory **dynamically on demand**. Every single time your application submits a task, the queue has to use the `new` keyword to create a brand new internal `Node` object to wrap your task and link it to the chain.

* **The GC Nightmare:** If your application experiences a massive traffic spike and drops 100,000 tasks into an unbounded queue, Java instantly allocates 100,000 fresh `Node` objects.
* Once your worker threads quickly process those 100,000 tasks, all 100,000 of those temporary `Node` objects instantly become garbage.
* This floods Java's young memory generation (Eden space), forcing the Garbage Collector to run aggressively to clean up the mess. If the spike is severe enough, the GC pauses can freeze your entire application.

---

<h3> Updated Cheat Sheet Comparison </h3>

| Feature                | `ArrayBlockingQueue`                                     | `LinkedBlockingQueue`                                                |
|------------------------|----------------------------------------------------------|----------------------------------------------------------------------|
| **Locking Strategy**   | **Single Lock** (Producers & Consumers block each other) | **Two Locks** (Producers & Consumers run concurrently)               |
| **Memory Allocation**  | **Upfront static allocation** (Flat footprint)           | **Dynamic on-demand allocation** (Can spike swiftly)                 |
| **Garbage Collection** | **Extremely Low** (Reuses fixed array slots)             | **High GC Pressure** (Constant creation/destruction of Node objects) |

Thank you for bringing those up—those two mechanics bridge the gap between how a queue works in a textbook versus how it actually behaves under a massive production load.

------------

# Q - Explain some types of ExecutorService?

1. newSingleThreadExecutor
2. newFixedThreadPool
3. newCachedThreadPool
4. newScheduledThreadPool
5. newWorkStealingPool (ForkJoinPool)

<h2> SingleThreadExecutor </h2>

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

<h2> FixedThreadPool </h2>

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


<h2> CachedThreadPool </h2>

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


<h2> ScheduledThreadPoolExecutor </h2>

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


<h2> WorkStealingPool (ForkJoinPool) </h2>

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


--------------


# Q - Explain ForkJoinPool with an example

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

<h2> Rules of engagement </h2>

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


<h2> Problem </h2>

Compute:

```text
Sum(1..8)
```

Using 4 workers: `W1, W2, W3, W4`

<h2> GLOBAL VIEW 1 — Task tree (structure only) </h2>

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

<h2> GLOBAL VIEW 2 — Initial state </h2>

```text
Workers:
W1, W2, W3, W4

Deques:
W1: [ FRONT |  | BOTTOM ]
W2: [ FRONT |  | BOTTOM ]
W3: [ FRONT |  | BOTTOM ]
W4: [ FRONT |  | BOTTOM ]
```

<h2> STEP 1 — Root starts on W1 </h2>

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

<h2> STEP 2 — Stealing happens </h2>

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

<h2> STEP 3 — W1 splits (5..8) </h2>

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

<h2> STEP 4 — W2 splits (1..4) </h2>

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

<h2> STEP 5 — More stealing (4 workers active) </h2>

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

<h2> STEP 6 — Leaf computations (base case) </h2>

All workers now compute actual values:

```text
W1: (7..8) = 15
W2: (3..4) = 7
W3: (5..6) = 11
W4: (1..2) = 3
```

<h2> STEP 7 — join() points (waiting is visible) </h2>

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

<h2> STEP 8 — Result build-up (bottom → top) </h2>

Now results combine **upward in the tree**.

```text
W3 returns: (5..6) = 11
W1 computes: (5..8) = 11 + 15 = 26

W4 returns: (1..2) = 3
W2 computes: (1..4) = 3 + 7 = 10
```

<h2> STEP 9 — Final join at root </h2>

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

<h2> FINAL GLOBAL VIEW — Everything together </h2>

```text
                        (1..8) = 36 [W1]
                       /                  \
          (1..4) = 10 [W2]              (5..8) = 26 [W1]
             /        \                    /          \
   (1..2)=3 [W4]  (3..4)=7 [W2]   (5..6)=11 [W3]  (7..8)=15 [W1]
```


------------


# Q - What's the differences b/w ForkJoinPool and ThreadPoolExecutor?

This is a very common interview question. The key difference is **the execution model**, not just the API.

| Feature             | `ThreadPoolExecutor`                   | `ForkJoinPool`                                               |
|---------------------|----------------------------------------|--------------------------------------------------------------|
| Primary purpose     | Execute independent tasks              | Execute recursive divide-and-conquer tasks                   |
| Queue               | Usually one shared blocking queue      | One deque (double-ended queue) per worker thread             |
| Load balancing      | Workers pull from shared queue         | Workers steal work from each other                           |
| Best for            | Web servers, I/O tasks, business logic | Parallel algorithms, recursive computations                  |
| Task type           | `Runnable`, `Callable`                 | `RecursiveTask`, `RecursiveAction`, also supports `Runnable` |
| Scheduling          | FIFO (typically)                       | LIFO for own tasks, FIFO when stealing                       |
| Work stealing       | ❌ No                                   | ✅ Yes                                                        |
| Recursive tasks     | Poor fit                               | Excellent                                                    |
| Blocking operations | Handles reasonably                     | Discouraged unless using `ManagedBlocker`                    |


------------


# Q - How ForkJoinPool() is different from Executors.newWorkStealingPool()

There are two key differences: one is about **Type** (what you get), and one is about **Algorithm** (how it works).

<h2> 1. The Return Type (API vs Implementation) </h2>

* `new ForkJoinPool()` returns the concrete `ForkJoinPool` class.
    * You get full access to specific methods like `.invoke()`, `.fork()`, `.join()`, and `.getStealCount()`.

* `Executors.newWorkStealingPool()` returns the `ExecutorService` interface.
    * It hides the implementation. You only get standard methods like `.submit()` and `.shutdown()`. 
  You cannot call specific ForkJoin methods without casting.

<h2> 2. The Hidden Difference: "Async Mode" </h2>

This is the critical performance difference.

* `new ForkJoinPool()` defaults to **Async Mode = false** (LIFO / Stack).
    *  **Behavior:** When a thread adds a task, it processes the **most recently added** task next.
    * **Why:** This optimizes for **CPU Cache** (Locality). The data for the newest sub-task is likely still hot in the CPU cache.
    * **Best For: Recursive Tasks** (Divide and conquer, sorting, matrix math).

* `Executors.newWorkStealingPool()` sets **Async Mode = true** (FIFO / Queue).
    * **Behavior:** When a thread adds a task, it processes the oldest task next.
    * **Why:** This optimizes for **Fairness**. It processes tasks in the order they arrived.
    * **Best For: Event Handling / Message Processing** (Processing independent requests).


---------


# Q - What is CompletableFuture?

`CompletableFuture` is an implementation of the `Future` interface that represents 
the result of an asynchronous computation and provides a rich API for 
composing, chaining, and coordinating asynchronous tasks.

Here is the consolidated breakdown of the four major drawbacks of the 
traditional `Future` interface, paired directly with clear, interview-ready code examples.

---

<h2> The Core Drawbacks of Traditional `Future` </h2>

<h3> 1. The Blocking Trap (`.get()`) </h3>

* **The Issue:** Traditional `Future` lacks a non-blocking method to fetch results. 
  Calling `.get()` entirely stalls the calling thread until the asynchronous worker thread 
  finishes its execution. This creates severe performance bottlenecks and undermines the purpose 
  of asynchronous design.

```java
ExecutorService executor = Executors.newSingleThreadExecutor();
Future<String> future = executor.submit(() -> {
    Thread.sleep(2000); // Simulating a long-running network operation
    return "Data Fetched";
});

System.out.println("Processing other tasks on the main thread...");

// The Blocking Trap: This completely halts the main thread for up to 2 seconds
String result = future.get(); 

System.out.println("Result received: " + result); // Execution only reaches here AFTER the block
executor.shutdown();
```

---

<h3> 2. No Native Callback Support (Polling with `.isDone()`) </h3>

* **The Issue:** You cannot register a listener or callback function to automatically trigger 
  the moment a task finishes. Instead, you are forced to write a "busy-wait" loop to repeatedly 
  poll the `Future` using `.isDone()`, which wastes valuable CPU cycles.

```java
ExecutorService executor = Executors.newSingleThreadExecutor();
Future<String> future = executor.submit(() -> {
    Thread.sleep(1500);
    return "Task Complete";
});

// Wasting CPU cycles pulling status manually because we can't say "call me when done"
while (!future.isDone()) {
    System.out.println("Task still running... checking again in 100ms");
    Thread.sleep(100); 
}

// Even after the loop breaks, we are still forced to call the blocking .get()
String result = future.get(); 
System.out.println(result);
executor.shutdown();
```

---

<h3> 3. The "Async Pipeline" Nightmare (Nested `.get()` Dependencies) </h3>

* **The Issue:** Traditional `Future` objects cannot be chained or composed together fluidly. 
  If Task B depends on the output of Task A, and Task C depends on Task B, the only way to link
  them is to manually call blocking `.get()` methods inside the sequence. This destroys concurrency 
  by forcing worker threads to wait idly on each other.

```java
ExecutorService executor = Executors.newFixedThreadPool(3);

// Task A: Fetch User
Future<User> userFuture = executor.submit(() -> fetchUser(userId));

// Task B: Depends on User data. Must block to get User first.
Future<Order> orderFuture = executor.submit(() -> {
    User user = userFuture.get(); // Blocks worker thread 2 waiting on thread 1!
    return fetchLatestOrder(user);
});

// Task C: Depends on Order data. Must block to get Order.
Future<Receipt> receiptFuture = executor.submit(() -> {
    Order order = orderFuture.get(); // Blocks worker thread 3 waiting on thread 2!
    return generateReceipt(order);
});

// The main thread must block yet again to extract the final result
Receipt finalReceipt = receiptFuture.get(); 
executor.shutdown();
```

---

<h3> 4. Brittle Exception Handling </h3>

* **The Issue:** If a background worker thread crashes, the exception is swallowed and 
  wrapped in a generic `ExecutionException`. This exception can only be caught at the 
  very end of the line during the `.get()` invocation, leading to messy, localized `try-catch` 
  structures with almost no opportunity to supply clean recovery paths or fallback data.

```java
ExecutorService executor = Executors.newSingleThreadExecutor();
Future<String> future = executor.submit(() -> {
    if (true) {
        throw new RuntimeException("Database connection failed!");
    }
    return "Success";
});

try {
    // Exception handling is strictly tied to the blocking retrieval call
    String result = future.get(); 
} catch (InterruptedException e) {
    Thread.currentThread().interrupt(); // Clean thread hygiene
} catch (ExecutionException e) {
    // Bulky handling; hard to cleanly recover or inject an elegant fallback value here
    System.err.println("Worker thread failed: " + e.getCause().getMessage());
} finally {
    executor.shutdown();
}
```

---

<h2>  The Resolution </h2>

`CompletableFuture` provides a rich API that allows you to start, transform, and combine 
asynchronous operations without nesting or blocking.
---

Here is the comprehensive handbook explaining all 10 
core `CompletableFuture` methods:

---

<h2> 🏗️ Group 1: Initiating Asynchronous Tasks </h2>

<h3> 1. `supplyAsync()` </h3>

* 📌 **Role & Signature:** Static factory method used to start an asynchronous 
computation that **computes and returns a result**.
    * **Input:** `Supplier<T>` (Signature: `T get()` — takes no args, returns value of type `T`).
    * **Returns:** `CompletableFuture<T>`.
    * **Execution:** Executed on `ForkJoinPool.commonPool()` by default unless a custom `Executor` is passed.

* **Practical Code Example:**

    ```java
    // Fires a background thread to fetch a user from DB
    CompletableFuture<User> userFuture = CompletableFuture.supplyAsync(() -> {
        return userRepository.findById("USER-101"); // Returns User object
    });
    ```


* 💡 **Senior Interview Takeaway:** *"Use `supplyAsync()` when your background operation computes 
or fetches data that downstream steps in your pipeline depend on."*


---

<h3> 2. `runAsync()` </h3>

* 📌 **Role & Signature:** Static factory method used to start an asynchronous task that runs 
purely for its **side effects and returns no value** (`void`).
    * **Input:** `Runnable` (Signature: `void run()` — takes no args, returns `void`).
    * **Returns:** `CompletableFuture<Void>`.

* 💻 **Practical Code Example:**

    ```java
    // Fire-and-forget log flushing or audit metrics recording
    CompletableFuture<Void> logFuture = CompletableFuture.runAsync(() -> {
        auditLogger.logEvent("User logged in at " + LocalDateTime.now());
    });
    ```


* 💡 **Senior Interview Takeaway:** *"Use `runAsync()` for fire-and-forget workloads like 
sending emails, logging metrics, or clearing caches where no data payload flows back into your application."*


---

<h2> ⚡ Group 2: Transforming & Chaining (Single Pipeline) </h2>

<h3> 3. `thenApply()` </h3>

* 📌 **Role & Signature:** Performs an **in-memory, synchronous transformation** on the result of the 
  previous stage. Acts as the **`map()`** operator for futures.
    * **Input:** `Function<T, U>` (Takes result `T` from previous step, returns new value `U`).
    * **Returns:** `CompletableFuture<U>`.

* 💻 **Practical Code Example:**

    ```java
    CompletableFuture<String> upperNameFuture = CompletableFuture.supplyAsync(() -> new User("alice"))
        // Takes User object, synchronously transforms it into an uppercase String
        .thenApply(user -> user.getName().toUpperCase()); // Returns "ALICE"
    ```

* 💡 **Senior Interview Takeaway:** *"Use `thenApply()` for fast, in-memory synchronous 
transformations (formatting strings, calculating mathematical totals, or mapping DTOs) that do not perform further async I/O."*


---

<h3> 4. `thenCompose()` </h3>

* 📌 **Role & Signature:** Chains two **dependent asynchronous tasks** sequentially where the second 
task uses the first task's result to launch another `CompletableFuture`. Acts as the **`flatMap()`** operator, preventing 
nested futures (`CompletableFuture<CompletableFuture<U>>`).

    * **Input:** `Function<T, CompletionStage<U>>` (Takes result `T`, returns a new `CompletableFuture<U>`).
    * **Returns:** `CompletableFuture<U>` (flattened single level).

* 💻 **Practical Code Example:**

    ```java
    CompletableFuture<Order> orderFuture = CompletableFuture.supplyAsync(() -> userService.getUser("101"))
        // Takes User, fires a SECOND async request (returns CompletableFuture<Order>)
        .thenCompose(user -> orderService.fetchLatestOrderAsync(user));
    ```


* 💡 **Senior Interview Takeaway:** *"Use `thenCompose()` whenever your downstream step is another 
asynchronous call (like firing a second DB or HTTP request) to flatten the pipeline into a single `CompletableFuture`."*


---

<h3> 5. `thenAccept()` </h3>

* 📌 **Role & Signature:** Consumes the final output of an upstream stage without returning any result. 
Serves as a **terminal consumer** at the end of a transformation chain.
    * **Input:** `Consumer<T>` (Signature: `void accept(T t)` — consumes `T`, returns `void`).
    * **Returns:** `CompletableFuture<Void>`.

* 💻 **Practical Code Example:**

    ```java
    CompletableFuture.supplyAsync(() -> userRepository.findById("101"))
        .thenApply(User::getEmail)
        // Consumes the email String and prints it; pipeline terminates with Void
        .thenAccept(email -> System.out.println("Notification sent to: " + email));
    ```


* 💡 **Senior Interview Takeaway:** *"Use `thenAccept()` as a non-blocking terminal callback when you want 
to consume the pipeline's result (e.g., render UI, write to HTTP response) without producing further data."*

---

<h2> 🔀 Group 3: Combining Multiple Futures </h2>

<h3> 6. `thenCombine()` </h3>

* 📌 **Role & Signature:** Executes two **independent futures concurrently** in parallel and merges their 
results using a `BiFunction` once **BOTH** finish.
    * **Input:** Another `CompletableFuture<U>` + `BiFunction<T, U, V>`.
    * **Returns:** `CompletableFuture<V>`.

* 💻 **Practical Code Example:**

    ```java
    CompletableFuture<User> userFuture = CompletableFuture.supplyAsync(() -> userService.getUser("101"));
    CompletableFuture<Cart> cartFuture = CompletableFuture.supplyAsync(() -> cartService.getCart("101"));
  
    // Executes in parallel; merges when BOTH complete
    CompletableFuture<CheckoutSummary> summaryFuture = userFuture.thenCombine(
        cartFuture, 
        (user, cart) -> new CheckoutSummary(user, cart) // BiFunction
    );
    ```


* ⚙️ **Visual Flow:**

    ```text
    userFuture (Stream A) ──┐
                         ├─► [ Both Done? ] ──► BiFunction(user, cart) ──► CompletableFuture<CheckoutSummary>
    cartFuture (Stream B) ──┘
    ```


* 💡 **Senior Interview Takeaway:** *"Use `thenCombine()` to merge the outcomes of two independent, parallel asynchronous
tasks, optimizing overall response time to `max(timeA, timeB)` rather than sequential `timeA + timeB`."*

---

<h3> 7. `allOf()` </h3>

* 📌 **Role & Signature:** Static combinator method that acts as a **synchronization barrier** for a batch of
futures. Completes only when **EVERY** input future completes.

    * **Input:** `CompletableFuture<?>... cfs` (varargs array of futures).
    * **Returns:** `CompletableFuture<Void>` (does **not** carry individual task results).

* 💻 **Practical Code Example:**

    ```java
    CompletableFuture<String> f1 = CompletableFuture.supplyAsync(() -> "Image 1");
    CompletableFuture<String> f2 = CompletableFuture.supplyAsync(() -> "Image 2");
    CompletableFuture<String> f3 = CompletableFuture.supplyAsync(() -> "Image 3");
  
    // Barrier completes when f1, f2, AND f3 finish
    CompletableFuture<Void> allBatch = CompletableFuture.allOf(f1, f2, f3);
  
    // Extract all results together non-blockingly:
    CompletableFuture<List<String>> resultsFuture = allBatch.thenApply(v -> 
        List.of(f1.join(), f2.join(), f3.join()) // .join() is non-blocking here because allOf guaranteed completion!
    );
    ```


* ⚙️ **Visual Flow:**

    ```text
    f1 ──┐
    f2 ──┼─► CompletableFuture.allOf() ──► [ Void Barrier Done ] ──► thenApply() ──► CompletableFuture<List<String>>
    f3 ──┘    
    ```


* 💡 **Senior Interview Takeaway:** *"Because `allOf()` returns `CompletableFuture<Void>`, attach 
a `.thenApply()` block using `.join()` to extract all completed payload values into a single `List` non-blockingly."*


---

<h3> 8. `anyOf()` </h3>

* 📌 **Role & Signature:** Static combinator method that implements a **"fastest-wins" race condition**. Completes 
as soon as **ANY ONE** of the input futures completes.
    * **Input:** `CompletableFuture<?>... cfs` (varargs array).
    * **Returns:** `CompletableFuture<Object>` (returns `Object` because inputs can be heterogeneous types).


* 💻 **Practical Code Example:**

    ```java
    CompletableFuture<String> server1 = CompletableFuture.supplyAsync(() -> fetchFromMirror1());
    CompletableFuture<String> server2 = CompletableFuture.supplyAsync(() -> fetchFromMirror2());
    
    // Returns whichever server responds first
    CompletableFuture<Object> fastestFuture = CompletableFuture.anyOf(server1, server2);
    
    // Cast back to domain model safely
    CompletableFuture<String> result = fastestFuture.thenApply(res -> (String) res);
    ```


* ⚙️ **Visual Flow:**

    ```text
    server1 (Slow: 300ms) ──┐
                            ├─► [ First Finish Wins! ] ──► Returns Server 2 Result ──► CompletableFuture<Object>
    server2 (Fast: 50ms)  ──┘
    ```


* 💡 **Senior Interview Takeaway:** *"Use `anyOf()` for redundant API/DNS calls or fetching data from the 
fastest server mirror. It returns `CompletableFuture<Object>` because Java resolves input types at compile-time to `Object`."*


---

<h2> 🚨 Group 4: Exception Handling & Recovery </h2>

<h3> 9. `exceptionally()` </h3>

* 📌 **Role & Signature:** Acts as an **asynchronous `catch` block** in the reactive pipeline. Intercepts any 
exception thrown upstream and returns a safe fallback value.

    * **Input:** `Function<Throwable, T>` (Receives the exception, returns fallback of type `T`).
    * **Returns:** `CompletableFuture<T>`.

* 💻 **Practical Code Example:**

    ```java
    CompletableFuture<Integer> balanceFuture = CompletableFuture.supplyAsync(() -> {
        if (true) throw new RuntimeException("DB Connection Timeout!");
        return 1000;
    })
    // Intercepts error and returns fallback value 0 so downstream tasks don't crash
    .exceptionally(ex -> {
        System.err.println("Error encountered: " + ex.getMessage());
        return 0; // Fallback balance
    });
    ```


* ⚙️ **Visual Flow:**

    ```text
    supplyAsync() [💥 Throws Exception] ──(Bypasses normal stages)──► exceptionally(ex -> fallback) ──► CompletableFuture<Integer> (0)
    ```

* 💡 **Senior Interview Takeaway:** *"`exceptionally()` catches upstream exceptions and swaps in fallback 
data without crashing the execution chain or blocking threads in `try-catch` blocks."*
* 

---

<h3> 10. `handle()` </h3>

* 📌 **Role & Signature:** Acts as a **`catch-finally` hybrid stage**. **Always executes**, regardless of whether 
upstream succeeded or failed.

    * **Input:** `BiFunction<T, Throwable, U>` (Receives result `T` **AND** exception `Throwable`, returns type `U`).
    * **Returns:** `CompletableFuture<U>` (allows transforming success/failure into a new type `U`).

* 💻 **Practical Code Example:**

    ```java
    CompletableFuture<ResponseDTO> responseFuture = CompletableFuture.supplyAsync(() -> userService.getUser("101"))
        // Always runs: result is non-null on success; ex is non-null on error
        .handle((user, ex) -> {
            if (ex != null) {
                return new ResponseDTO(500, "Failed: " + ex.getMessage());
            }
            return new ResponseDTO(200, "Success: " + user.getName());
        });
    ```

* ⚙️ **Visual Flow:**

    ```text
    Upstream Task ──► [ Success (user) OR Error (ex) ] ──► handle((user, ex) -> ResponseDTO) ──► CompletableFuture<ResponseDTO>
    ```


* 💡 **Senior Interview Takeaway:** *"Use `handle()` when you need to inspect both success result (`T`) and 
exception (`Throwable`) simultaneously to map them into a single, unified output DTO (e.g., HTTP 200 vs 500 response)."*


--------------


# Q - What is Virtual Thread?

A virtual thread is a `java.lang.Thread` instance that is **not** tied one-to-one to an OS thread. Instead,
the JVM runs it on a **carrier thread**(a real OS/Platform thread from a small `ForkJoinPool()`) only while
it is executing. When the virtual thread hits a blocking operation (like a socket read), the JVM **unmounts** it from the 
carrier thread and parks its stack on the heap, and frees the carrier thread to run other virtual threads.

Both normal (platform) threads and virtual threads are instances of `java.lang.Thread`.

This was one of the most brilliant design decisions made by the
OpenJDK team during **Project Loom**. Rather than introducing a
completely new class (like `VirtualThread` or `Task`),
they kept `java.lang.Thread` as the unifying abstraction.

---

<h2> The Class Hierarchy Under the Hood </h2>

In Java 21+, `java.lang.Thread` is the common parent/class for both thread types:

```
                  ┌───────────────────────┐
                  │   java.lang.Thread    │
                  └───────────┬───────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
    ┌──────────────────┐            ┌───────────────────┐
    │  PlatformThread  │            │   VirtualThread   │
    │  (1:1 OS Thread) │            │ (M:N Heap Thread) │
    └──────────────────┘            └───────────────────┘
```

Because both implement `java.lang.Thread`, **100% of existing Java libraries,
debugging tools, thread locals, and exception handlers work out of the box
with Virtual Threads** without needing API redesigns.

---

<h2> How You Create Them in Code </h2>

Java 21 updated the `Thread` API with builder patterns so you can explicitly
choose which type of `java.lang.Thread` instance to create:

```java
// 1. Traditional Platform Thread (1:1 with OS Thread - Builder Pattern)
Thread platformThread = Thread.ofPlatform()
				.name("my-platform-thread")
				.start(() -> System.out.println("Running on OS thread"));

// 2. Virtual Thread (M:N, managed by JVM on Heap - Builder Pattern)
Thread virtualThread = Thread.ofVirtual()
		.name("my-virtual-thread")
		.start(() -> System.out.println("Running on Virtual thread"));

// 3. Virtual Thread (M:N, managed by JVM on Heap - Shorthand Convenience Method)
Thread quickVirtualThread = Thread.startVirtualThread(() -> {
	System.out.println("Running on Virtual thread via static shorthand!");
});
```

---

<h2> How to Check at Runtime </h2>

Since both are instances of `java.lang.Thread`, how do you tell them apart programmatically?
Java added a dedicated method to `java.lang.Thread`:

```java
Thread current = Thread.currentThread();

if (current.isVirtual()) {
        System.out.println("I am a Virtual Thread on the heap!");
} else {
        System.out.println("I am a heavy Platform Thread backed 1:1 by the OS!");
}
```

---

<h2> Example of Virtual Thread </h2>

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


-------------------


# Q - What is Thread Local?

`ThreadLocal` (`java.lang.ThreadLocal`) is a Java utility class that provides thread-confined variables.

Here is a clear, concrete breakdown of how `ThreadLocal` works under the hood.

---

<h3> 1. The Core Architecture (The Inverted Model) </h3>

The most important concept to grasp is **who owns what**:

* Every `Thread` instance owns a private map. Inside the JDK `java.lang.Thread` class, there is a field:

  ```java
  ThreadLocal.ThreadLocalMap threadLocals;
  ```

* The `ThreadLocal` instance is the KEY inside that map.
* Your data payload is the VALUE.


---

<h3> 2. What Happens Step-by-Step in Memory </h3>

Imagine you declare a single static `ThreadLocal` variable in your code:

```java
public static ThreadLocal<UserContext> context = new ThreadLocal<>();

```

<h4> When **Thread A** calls `context.set(userA)`: </h4>

1. Java fetches the currently running thread: `Thread current = Thread.currentThread()` (Thread A).
2. Java opens **Thread A's own private map** (`current.threadLocals`).
3. It inserts an entry into Thread A's map:
   * **Key:** `context` (the `ThreadLocal` instance reference, wrapped in a `WeakReference`).
   * **Value:** `userA`.


<h4> When **Thread B** calls `context.set(userB)`: </h4>

1. Java fetches the currently running thread: `Thread B`.
2. Java opens **Thread B's own private map** (`current.threadLocals`).
3. It inserts an entry into Thread B's map:
   * **Key:** `context` (the exact same `ThreadLocal` instance).
   * **Value:** `userB`.


<h4> When **Thread A** calls `context.get()`: </h4>

1. Java gets `Thread.currentThread()` (Thread A).
2. Java looks up Thread A's internal `threadLocals` map.
3. It looks for the entry where `Key == context` and returns `userA`.


---

<h3> 3. Visual Layout of Memory </h3>

```text
Thread A Instance
 └── threadLocals (ThreadLocalMap)
       └── Entry
            ├── Key   : [WeakReference -> context]
            └── Value : userA

Thread B Instance
 └── threadLocals (ThreadLocalMap)
       └── Entry
            ├── Key   : [WeakReference -> context]
            └── Value : userB
```

---

<h3> 4. Why Is It Designed This Way? </h3>

If `ThreadLocal` were a central map (like `ConcurrentHashMap<Thread, Value>`), every thread 
in the application would hit the **same map simultaneously**, causing heavy lock contention 
and slowing down execution.

By placing the map **inside each individual `Thread` instance**:

* Thread A **only reads and writes its own map**.
* Thread B **only reads and writes its own map**.
* **Result:** Zero shared memory across threads, zero synchronization, zero locks, and maximum speed.


<h2> Code Example: The "Context Holder" Pattern </h2>

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

<h2> The Danger: Memory Leaks (The "Dirty Thread" Problem) </h2>

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


--------------------



# Q - What is CountDownLatch

<h2> The Problem </h2>

Suppose you have three worker threads.

```text
Worker 1
Worker 2
Worker 3
```

Each performs some work.

The main thread should continue **only after all three workers finish**.

Without any coordination:

```java
worker1.start();
worker2.start();
worker3.start();

System.out.println("All workers finished");
```

This is wrong.

The output might be:

```
All workers finished
Worker 2 done
Worker 1 done
Worker 3 done
```

The main thread has no idea when the workers are actually finished.

---

<h2> Before `CountDownLatch` </h2>

People solved this by calling `join()`.

```java
worker1.join();
worker2.join();
worker3.join();

System.out.println("All workers finished");
```

This works.

But notice the limitation.

The main thread must know **every thread**.

If tomorrow there are:

* 20 workers
* dynamically created workers
* tasks running in an `ExecutorService`

`join()` becomes inconvenient because you don't have `Thread` objects to join.

---

<h2> The Idea Behind `CountDownLatch` </h2>

Instead of waiting for **threads**, wait for **events**.

For example:

```text
Need 3 events.

Worker 1 finishes → count = 2

Worker 2 finishes → count = 1

Worker 3 finishes → count = 0

↓

Main thread continues.
```

That's exactly what `CountDownLatch` does.

---

<h2> How It Works </h2>

Suppose there are three workers.

```java
CountDownLatch latch = new CountDownLatch(3);
```

The internal counter is:

```text
3
```

Worker 1:

```java
latch.countDown();
```

Counter:

```text
2
```

Worker 2:

```java
latch.countDown();
```

Counter:

```text
1
```

Worker 3:

```java
latch.countDown();
```

Counter:

```text
0
```

Any thread waiting here:

```java
latch.await();
```

is immediately released.

---

<h2> Example </h2>

```java
CountDownLatch latch = new CountDownLatch(3);

ExecutorService executor = Executors.newFixedThreadPool(3);

for (int i = 0; i < 3; i++) {

    executor.submit(() -> {

        System.out.println(Thread.currentThread().getName());

        latch.countDown();
    });
}

latch.await();

System.out.println("All workers completed");
```

Execution:

```text
pool-1-thread-2

pool-1-thread-1

pool-1-thread-3

All workers completed
```

The order of workers doesn't matter.

The main thread proceeds only after the counter reaches zero.

---

<h2> Why Was It Introduced? </h2>

It solves a problem that `join()` doesn't solve elegantly.

Imagine using an `ExecutorService`.

```java
executor.submit(task1);
executor.submit(task2);
executor.submit(task3);
```

There are no `Thread` objects available.

You cannot write:

```java
thread.join();
```

because you don't own the worker threads.

Instead, each task simply says:

```java
latch.countDown();
```

The coordinating thread waits:

```java
latch.await();
```

This works regardless of which thread executed the task.

---

<h2> Another Common Use Case </h2>

Suppose a server cannot start until three components are initialized.

```
Database

↓

Cache

↓

Message Queue
```

Each initialization thread calls:

```java
countDown();
```

The server thread waits:

```java
await();
```

Only after all components are ready does the server begin accepting requests.

---

<h2> One-Time Synchronization </h2>

A `CountDownLatch` is **one-shot**.

```text
3

↓

2

↓

1

↓

0
```

Once it reaches zero:

* it **cannot** be reset.
* you need a new `CountDownLatch` if you want to repeat the process.

This distinguishes it from `CyclicBarrier`, which is reusable.


--------------------


# Q - What is CyclicBarrier?

Imagine four friends participating in a treasure hunt. The game is divided into multiple stages. 
At the end of each stage, **every friend must reach a checkpoint before anyone is allowed to continue
to the next stage**.

Even if one friend finishes early, they must wait at the checkpoint until the others arrive. 
Once the last friend reaches the checkpoint, **the gate opens**, everyone proceeds to the 
next stage together, and the checkpoint is ready to be used again for the following stage.

This checkpoint is exactly how a `CyclicBarrier` works.

A `CyclicBarrier` is a synchronization aid that allows a fixed number of threads (called **parties**) to 
wait for each other at a common synchronization point (the **barrier**). When the last participating 
thread reaches the barrier by calling `await()`, all waiting threads are released
simultaneously, and the barrier automatically resets so it can be reused in the next cycle.

This makes `CyclicBarrier` ideal for problems where multiple threads perform 
work in **repeated phases**, and **no thread should begin the next phase until every thread has 
completed the current one**.

Let's understand this using a multiplayer game where four players participate in multiple rounds. 
Each player must wait for the others to finish the current round before the next round begins.

Here's a complete, minimal example of `CyclicBarrier`.

```java
import java.util.concurrent.CyclicBarrier;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class CyclicBarrierDemo {

    public static void main(String[] args) {

        // One barrier shared by all 4 threads
        CyclicBarrier barrier = new CyclicBarrier(
                4,
                () -> System.out.println("\n>>> All players finished the round. Starting next round...\n")
        );

        ExecutorService executor = Executors.newFixedThreadPool(4);

        // Start 4 player threads
        for (int i = 1; i <= 4; i++) {

            int playerId = i;

            executor.submit(() -> player(playerId, barrier));
        }

        executor.shutdown();
    }

    static void player(int playerId, CyclicBarrier barrier) {

        try {

            for (int round = 1; round <= 3; round++) {

                System.out.println(
                        "Player " + playerId +
                        " is playing Round " + round
                );

                // Simulate different players taking different time
                Thread.sleep((long) (Math.random() * 3000));

                System.out.println(
                        "Player " + playerId +
                        " finished Round " + round
                );

                // Wait for everyone
                barrier.await();

                System.out.println(
                        "Player " + playerId +
                        " starts Round " + (round + 1)
                );
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

<h2> What happens? </h2>

<h3> Initially </h3>

Main thread creates

```java
CyclicBarrier barrier = new CyclicBarrier(4);
```

There is **one** barrier object.

---

Then

```java
ExecutorService executor =
        Executors.newFixedThreadPool(4);
```

creates a thread pool with 4 worker threads.

---

Next

```java
executor.submit(() -> player(playerId, barrier));
```

is called four times.

So the executor starts

```text
Thread-1 ---> player(1, barrier)

Thread-2 ---> player(2, barrier)

Thread-3 ---> player(3, barrier)

Thread-4 ---> player(4, barrier)
```

Notice something important.

Each thread receives **the same barrier object**.

<h2> Round 1 </h2>

Suppose execution happens like this:

```text
Player 2 finished Round 1
```

Player 2 executes

```java
barrier.await();
```

Barrier:

```text
Waiting = 1
```

Player 2 blocks.

---

Next

```text
Player 1 finished Round 1
```

Barrier:

```text
Waiting = 2
```

Player 1 blocks.

---

Next

```text
Player 4 finished Round 1
```

Barrier:

```text
Waiting = 3
```

---

Finally

```text
Player 3 finished Round 1
```

Barrier:

```text
Waiting = 4
```

Now the barrier says:

> Everyone has arrived.

It runs the barrier action:

```text
>>> All players finished the round. Starting next round...
```

Then wakes **all four threads**.

---

Immediately all four continue.

```text
Player 1 starts Round 2

Player 2 starts Round 2

Player 3 starts Round 2

Player 4 starts Round 2
```

---

<h2> The same thing happens again </h2>

Round 2

↓

Barrier

↓

Round 3

↓

Barrier

↓

Finished

---

<h2> Possible Output </h2>

Your output will vary because of the random delays, but it might look like:

```text
Player 1 is playing Round 1
Player 2 is playing Round 1
Player 3 is playing Round 1
Player 4 is playing Round 1

Player 2 finished Round 1
Player 4 finished Round 1
Player 1 finished Round 1
Player 3 finished Round 1

>>> All players finished the round. Starting next round...

Player 2 starts Round 2
Player 1 starts Round 2
Player 3 starts Round 2
Player 4 starts Round 2
```

---

<h3> The key idea </h3>

There are **four independent threads** all running the same `player()` method concurrently.

Each thread eventually reaches:

```java
barrier.await();
```

The **same `CyclicBarrier` instance** counts how many threads have arrived. 
When the fourth thread calls `await()`, it releases **all** waiting threads and automatically resets 
itself so it can synchronize the next round. This shared barrier is what allows the threads to 
progress through each round together.


--------------------


# Q - What is Semaphore?

<h2> The Problem </h2>

Suppose your application has a database connection pool with **10 connections**.

At some point, **100 threads** want to access the database.

```text
100 Threads

↓

Database
```

Can all 100 access it simultaneously?

No.

There are only **10 database connections**.

The remaining 90 threads must wait until a connection becomes available.

How do we enforce this limit?

---

<h2> Before Semaphore </h2>

One naive approach is to write your own logic.

```java
if (connectionsAvailable > 0) {
    connectionsAvailable--;
    // Use database
}
```

But this is unsafe.

Imagine two threads executing simultaneously.

```text
Connections Available = 1
```

Thread A reads:

```text
1
```

Before it decrements...

Thread B also reads:

```text
1
```

Now both think they can proceed.

You've just handed out the same database connection twice.

You now need:

* synchronization
* waiting
* waking threads
* fairness

This becomes surprisingly difficult to implement correctly.

---

<h2> The Idea Behind Semaphore </h2>

Instead of protecting **one resource**, imagine you have **N identical resources**.

A semaphore simply keeps track of:

> **How many permits are currently available?**

Think of a permit as a ticket.

Initially:

```text
10 permits
```

Every thread that wants the resource must first obtain a permit.

---

<h2> Analogy: Parking Lot </h2>

Imagine a parking lot with **5 parking spaces**.

```text
Parking Lot

[ ][ ][ ][ ][ ]
```

Cars arrive.

Car 1

↓

Gets a spot.

```text
[X][ ][ ][ ][ ]
```

Car 2

↓

```text
[X][X][ ][ ][ ]
```

Eventually:

```text
[X][X][X][X][X]
```

The parking lot is full.

Now another car arrives.

Can it enter?

❌ No.

It waits outside.

Only when a car leaves...

```text
[X][X][ ][X][X]
```

does the waiting car get the free spot.

This is exactly how a `Semaphore` works.

The parking spaces are the **permits**.

---

<h2> How It Works </h2>

Suppose there are three permits.

```java
Semaphore semaphore = new Semaphore(3);
```

Initially:

```text
Available Permits = 3
```

Thread A

```java
semaphore.acquire();
```

Available:

```text
2
```

---

Thread B

```text
1
```

---

Thread C

```text
0
```

Now all permits are in use.

---

Thread D arrives.

```java
semaphore.acquire();
```

There are no permits left.

Thread D blocks.

---

Later,

Thread B finishes.

```java
semaphore.release();
```

Available:

```text
1
```

The semaphore immediately wakes Thread D.

Thread D acquires the permit.

Available:

```text
0
```

---

<h2> Example </h2>

Imagine only two printers.

```java
Semaphore printerSemaphore = new Semaphore(2);

void printDocument() {

    printerSemaphore.acquire();

    try {
        System.out.println("Printing...");
        Thread.sleep(3000);
    } finally {
        printerSemaphore.release();
    }
}
```

If ten threads call `printDocument()`:

```text
10 Threads

↓

Only 2 print simultaneously

↓

8 wait

↓

When one printer becomes free,
another waiting thread starts printing.
```

---

<h2> Why Was It Introduced? </h2>

Many real-world resources are **limited**.

Examples:

* Database connections
* Printer devices
* API rate limits
* GPU resources
* File handles
* Network connections

The problem is not:

> "Only one thread at a time."

The problem is:

> "Only **N** threads at a time."

That's exactly what a semaphore solves.

---


<h2> Difference from a Lock </h2>

Suppose there is one bathroom.

Only one person can enter.

That's a lock.

```text
1 permit
```

Now imagine five bathroom stalls.

Five people can enter simultaneously.

Everyone else waits.

That's a semaphore.

```text
5 permits
```

In fact,

```java
new Semaphore(1)
```

behaves similarly to a mutex (though there are semantic differences, such as ownership—any thread can call `release()` on a semaphore).

------------


# Q - BlockingQueue (why introduced)

# Q - ConcurrentHashMap (how it avoids full locking)

# Q - What is ReentrantReadWriteLock?

A `ReentrantReadWriteLock` is a more advanced lock that separates access into two different modes: **Read** and **Write**.

Unlike a standard `ReentrantLock` (or `synchronized`) which is **Exclusive** (only one thread enters, period),
a `ReadWriteLock` **allows multiple threads** to read data simultaneously, as long as no one is writing.

<h2> The Purpose: Performance </h2>

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


<h2> Code Example: A Thread-Safe Cache </h2>

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

<h3> Visualizing the difference </h3>

If you run get() from 5 threads:
* With ReentrantLock:
    * Thread 1 enters... exits.
    * Thread 2 enters... exits.
    * (Serial execution)
* With ReentrantReadWriteLock:
    * Thread 1, 2, 3, 4, 5 enter simultaneously.
    * (Parallel execution)

<h2> Critical "Senior Dev" Warning </h2>

Don't blindly use this everywhere. `ReentrantReadWriteLock` has overhead.

* It is more complex to manage than a standard lock.
* If you have **mostly writes** (or equal reads/writes), it is actually **slower** than a standard `ReentrantLock` 
because of the extra logic to track readers.
* Modern Alternative: Java 8 introduced `StampedLock`, which is faster and supports "Optimistic Reads," often 
replacing `ReentrantReadWriteLock` in high-performance code.


# Q - What is Monitor object?

In Java, a Monitor is the internal synchronization mechanism used to handle concurrency. 
It is the theoretical concept behind the `synchronized` keyword and `wait()`/`notify()`.

Every object in Java is associated with a Monitor. You don't see it explicitly in code, but the JVM creates 
it when you use synchronization.

<h2> The Mental Model: "The Secure Room" </h2>

Imagine the Monitor as a special building with three distinct areas:

1\. **The Entry Set (The Hallway):** Where threads wait before they can enter the synchronized block. 
They are fighting to get in.

2\. **The Owner (The Room):** The critical section. **Only one thread** can be here at a time. It holds the "Key" (Lock).

3\. **The Wait Set (The Waiting Room):** A separate room where threads go if they voluntarily give up the key
(`via wait()`) because they are waiting for a condition to change.


<h2> How it maps to Code </h2>

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

<h3> Example 1: The Simplest Example (Mutual Exclusion) </h3>

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

<h3> Example 2: The Classic "Wait/Notify" Example (Coordination) </h3>

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

<h3> Example 3: The "Modern" Explicit Monitor (ReentrantLock) </h3>

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

# Q - Which object shouldn't be used as a Monitor object?

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

<h2> What SHOULD be used instead </h2>

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


# Q - Is it valid to use a synchronized block inside a Lambda expression?

Yes, absolutely. A lambda expression is just a shorthand for an implementation of a functional interface. 
You can write any valid Java code inside the curly braces `{ ... }`, including a synchronized block.

However, there is a **critical scope rule** you must know.

<h2> The Code Example </h2>

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

<h2> The "Gotcha" (Scope of this) </h2>

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

You can synchronize inside a lambda.

* **Best Practice:** Lock on a specific, private final object (like `lock` in the first example) rather than `this` to 
avoid confusion about lexical scoping.
* **Constraint:** Any local variable you lock on (captured from outside) must be **effectively final**.

# Q - Does thread release the lock after OS preemption?

When the Operating System preempts a thread (forcing it to pause so another thread can run), 
that **thread does NOT release** any Java locks (`synchronized` blocks) it currently holds. 
Crucially, if you call `thread.getState()` on a thread that has been preempted by the OS (kicked off the CPU), 
it will return `RUNNABLE`, because from the JVM's perspective, the thread is fully ready to execute and is simply
waiting for a time slice from the Operating System.

# Q - What is the as-if-serial rule in Java, and what does it allow the JVM to do?

The **as-if-serial** rule allows the JVM to reorder, optimize, or eliminate statements as long as these 
changes do not alter the observable behavior of a single-threaded program.

<h2> What "do not alter the observable behavior" really means </h2>

It means you cannot observe any difference in:

* printed output
* returned values
* exceptions
* control flow

If the result looks the same, the JVM is free to optimize.

<h2> Example 1: </h2>

<h3> Allowed reordering (no observable effect) </h3>

```java
int a = 1;
int b = 2;
```

JVM may swap these internally because:

* no one can tell
* no output depends on the order

✅ Allowed.

<h3> Not allowed (observable difference) </h3>

```java
int a = 1;
System.out.println(a);
```

JVM cannot print before assigning `a`.

❌ Not allowed.

<h2> Important clarification (very important) </h2>

* As-if-serial applies to a single thread
* It says nothing about correctness across threads
* That's why concurrency needs volatile, synchronized, locks, etc.

# Q - Is it possible for JVM to re-order statements inside a synchronized block?

Yes, The JVM is free to reorder instructions inside a synchronized block as long as it adheres 
to the **"As-If-Serial"** semantics.

Here is the detailed breakdown for your interview answer.

<h2> 1. The "As-If-Serial" Rule </h2>

This rule basically tells the compiler: "You can change the order of execution however you want to optimize 
performance (e.g., for CPU pipelining), provided that the final result remains exactly the same for the thread 
executing the code."

Example of Reordering:

```java
synchronized (this) {
    int a = 1;  // Independent assignment
    int b = 2;  // Independent assignment
    
    // The JVM might execute 'b=2' BEFORE 'a=1' 
    // because they don't depend on each other.
}
```

To the thread executing this code, it makes no difference whether `a` or `b` is assigned first. 
The result is the same. Therefore, the "As-If-Serial" rule allows this swap.

<h2> 2. Why doesn't this break the program? </h2>

You might ask: "If the JVM swaps `a` and `b`, won't another thread see `b=2` while a is still `0`?"

This is where `synchronized` saves the day. The correctness remains intact 
because `synchronized` provides **Mutual Exclusion**:

* **The Wall:** No other thread can look inside the synchronized block while the current thread is executing it.
* **The Flush:** The reordering is "hidden" inside the block. Other threads are forced to wait until the lock is released.
* **The Result:** By the time the lock is released (monitor exit), the Java Memory Model forces a "flush" of 
all variables. Other threads only see **the final, consistent state** (where both a=1 and b=2), never the messy
intermediate state where they were reordered.

Summary

* **Inside the block:** It is a "Wild West" of optimizations. The JVM reorders code to 
run as fast as possible (As-If-Serial).
* **Outside the block:** It looks like a perfect atomic transaction because the lock prevented 
anyone from witnessing the reordering.

# Q - What is AtomicReference?

`AtomicReference` is a class in the `java.util.concurrent.atomic` package that acts as a container for an object reference. 
It allows you to update that reference atomically (all or nothing) without using locks (`synchronized`).

Think of it as a thread-safe "Box" that holds one object. You can safely replace the object inside the box, ensuring 
that no other thread is modifying it at the exact same moment.

It relies on a hardware primitive called **CAS (Compare-And-Swap)**: _"Set the value to B, but ONLY IF the
current value is still A."_

Note that normal assignment is atomic and just overwrites a value without checking what's there first, 
but `AtomicReference`lets you check and update in one single step so other threads can't 
sneak in and mess things up.

<h2> Traditional solution: synchronized </h2>

```java
synchronized (lock) {
    State old = currentState;
    State next = compute(old);
    currentState = next;
}
```


This works because:

* Only one thread runs this code at a time
* Others block and wait

But blocking has downsides:

* Context switches
* Contention
* Reduced scalability

<h2> What AtomicReference changes </h2>

`AtomicReference` lets you say:
> "Update the reference **only if** it hasn’t changed since I last looked at it."
>

That is the core idea.

<h2> The key operation: Compare-And-Set (CAS) </h2>

```java
ref.compareAndSet(expected, newValue)
```

Meaning:
> If the current reference is **exactly the same object** as `expected`,
then replace it with `newValue`.
>

This check-and-update happens:

* atomically
* in one CPU instruction

No one can slip in between.

<h2> Why this avoids locking </h2>

With CAS:

* Threads do not block
* Threads may retry instead
* Only the thread that "wins" updates the reference

<h2> Example pattern: </h2>

```java
while (true) {
    State old = ref.get();
    State next = compute(old);

    if (ref.compareAndSet(old, next)) {
        break; // success
    }
    // else: someone else changed it → retry
}
```

This is called **optimistic concurrency**.

<h2> Conceptual difference from synchronized </h2>

* `synchronized` protects a block of code
* `AtomicReference` protects a single decision


<h2> When `AtomicReference` makes sense conceptually </h2>

Use it when:

* The shared state can be replaced as a whole
* Updates are simple and fast
* Retrying is acceptable
* You don't need waiting or coordination

<h2> When it does NOT make sense </h2>

Do not use it when:

* You need to protect multiple operations together
* You need `wait/notify`
* You need fairness or ordering
* You are mutating shared objects

<h2> One core mental model (this is the key) </h2>

> `AtomicReference` is for atomically replacing state, not for guarding code.
>

The following is a realistic example of `AtomicReference`

<h2> Example: Lock Free Stack </h2>

The following code implements a thread-safe Stack (LIFO) without using `synchronized`. Using a lock would be a 
bottleneck if 10 threads are pushing/popping simultaneously.

Instead, we use `AtomicReference` to hold the "Head" node.

```java
import java.util.concurrent.atomic.AtomicReference;

public class LockFreeStack<T> {
    
    // Node structure
    private static class Node<T> {
        final T value;
        Node<T> next;

        Node(T value) { this.value = value; }
    }

    // The "Head" is managed atomically
    private final AtomicReference<Node<T>> head = new AtomicReference<>();

    public void push(T value) {
        Node<T> newHead = new Node<>(value);
        Node<T> currentHead;
        
        // CAS LOOP
        do {
            currentHead = head.get();
            newHead.next = currentHead;
            
            // "I think the head is X. If it is still X, change it to Y."
            // If false, it means another thread pushed something in between. Loop again.
        } while (!head.compareAndSet(currentHead, newHead));
    }

    public T pop() {
        Node<T> currentHead;
        Node<T> newHead;
        
        do {
            currentHead = head.get();
            if (currentHead == null) {
                return null; // Stack is empty
            }
            newHead = currentHead.next;
            
        } while (!head.compareAndSet(currentHead, newHead));
        
        return currentHead.value;
    }
}
```

The "critical section" is extremely small (just pointer swapping). Blocking threads with locks would waste more 
CPU time on context switching than doing the actual work.


<h2> The Trade-off  </h2>

While `AtomicReference` avoids "Context Switching" and It's generally fast, but there is one catch:

High Contention = High CPU Usage Because `AtomicReference` uses a loop (Spin Lock) to retry failures:

* If 100 threads try to update the same `AtomicReference` at once:
    * 1 succeeds.
    * 99 fail and retry immediately.
    * The CPU usage spikes to 100% because those 99 threads are frantically spinning in while loops.

Summary:

* `synchronized`: "I'll go to sleep until it's my turn." (Low CPU, High Latency)
* `AtomicReference`: "I'll keep banging on the door until it opens." (High CPU, Low Latency)


# Q - When would you use AtomicReference instead of synchronized?

Atomic references are ideal for atomic replacement of immutable objects, while synchronized blocks remain
the right choice for protecting multi-step operations and invariants.

# Q - What is Cache-Coherence?

<h2> Step 1: Start with a simple machine (no problem yet) </h2>

Imagine one CPU core.

```java
int x = 0;
x = 1;
System.out.println(x);
```

* One core
* One cache
* One memory

Everything is simple. The core writes `1`, reads `1`.

No confusion. No cache coherence problem.

<h2> Step 2: Now add a second CPU core </h2>

Now imagine two CPU cores:

* Core 1
* Core 2

Each core has its **own cache**.

Memory is shared.

```text
Main Memory: x = 0

Core 1 Cache: empty
Core 2 Cache: empty
```

<h2> Step 3: Core 1 reads `x` </h2>

Core 1 executes:

```java
int a = x;
```

What happens:
* `x` is loaded from memory into **Core 1's cache**

```text
Main Memory: x = 0
Core 1 Cache: x = 0
Core 2 Cache: empty
```

<h2> Step 4: Core 2 also reads `x` </h2>

Core 2 executes:

```java
int b = x;
```

Now:

```text
Main Memory: x = 0
Core 1 Cache: x = 0
Core 2 Cache: x = 0
```

Both cores now have **their own copy** of `x`. So far, still fine.

<h2> Step 5: Core 1 updates `x` </h2>

Core 1 executes:

```java
x = 1;
```

Now ask yourself:
> What happens to Core 2’s cached copy?
>

If nothing happens, we get:

```text
Main Memory: x = 1
Core 1 Cache: x = 1
Core 2 Cache: x = 0  ❌ stale
```

Now the system is **broken**:

* Core 1 sees `1`
* Core 2 sees `0`

This is the **cache coherence problem**.

<h2> Step 6: What cache coherence does </h2>

Cache coherence is the rule system that says:

> "This situation is NOT allowed."

So when Core 1 writes `x = 1`:

* Core 2's cached copy of x must be:
    * **invalidated**, or
    * **updated**

After coherence kicks in:

```text
Main Memory: x = 1
Core 1 Cache: x = 1
Core 2 Cache: x = invalid
```

Now if Core 2 reads `x` again:

* It must reload from memory
* It will see `1`

<h2> Step 7: What this guarantees (important) </h2>

Cache coherence guarantees:
> All cores will eventually agree on the value of x.
>

It does not guarantee:

* when Core 2 will read again
* in what order multiple writes happen
* correctness of multithreaded logic

Just value agreement.

<h2> Step 8: Why this alone is not enough (Java example) </h2>

```java
// Thread 1 (Core 1)
x = 1;
y = 1;

// Thread 2 (Core 2)
if (y == 1) {
    System.out.println(x);
}
```

Even with cache coherence:

* Core 2 may see `y = 1`
* but still see `x = 0`

Why?

* Writes can be reordered
* Visibility timing is not guaranteed

This is why Java needs:

* `volatile`
* `synchronized`
* memory barriers

Cache coherence only keeps **values consistent**, not **logic correct**.

Everything we just saw is called **Cache Coherence**.

Formal definition:

> Cache coherence ensures that when multiple CPU cores cache the same memory location, updates made by one 
core are made visible to the others in a consistent way.

# Q - What is False Sharing?

<h2> What is a Cache Line? </h2>

Processors do not read memory one byte at a time; that would be too slow. Instead, they fetch memory in 
chunks called **Cache Lines**.

* **The Size:** A typical cache line is 64 bytes.
* **The Concept:** If you ask the CPU for a single long (8 bytes), it doesn't just grab that variable. 
It grabs the entire 64-byte block surrounding it from **L3 (Shared Cache)** or RAM and loads it into 
its **L1 (Private Cache)**.
* **The Logic:** The CPU assumes that if you need one variable, you will likely need its 
neighbors soon (**Spatial Locality**).

<h2> The Visualization: The "Ping-Pong" Problem (False Sharing) </h2>

Imagine two threads running on two different CPU cores. They are working on an array of `long` values.

```java
// Contiguous memory locations
long[] data = new long[] { ValueA, ValueB };
```

Since `ValueA` and `ValueB` are right next to each other in memory, they fit inside the **same 64-byte Cache Line**.

The Scenario

* **Core 1** wants to update `ValueA`.
* **Core 2** wants to update `ValueB`.

Here is what happens inside the hardware hierarchy:

<h2> 1. Initial State (Shared) </h2>

Both cores read the data.

* The 64-byte Cache Line is loaded into Core 1's L1 Cache.
* The same 64-byte Cache Line is loaded into Core 2's L1 Cache.
* Status: The line is marked as Shared in both L1 caches

<h2> 2. Core 1 Modifies ValueA </h2>

Thread 1 updates `ValueA`.

* **Core 1:** Updates the line in its **L1 Cache**. The line is now marked `Modified`.
* **The Coherence Protocol (MESI):** To maintain consistency, the hardware must invalidate any other copies of this line.
* **Core 2:** Its copy of the line in **L1 Cache** is instantly marked `Invalid` (effectively deleted).

<h2> 3. Core 2 Tries to Modify ValueB </h2>

Thread 2 tries to update `ValueB`.

* **L1 Miss:** Core 2 checks its L1 Cache and sees the line is `Invalid`. It cannot write to it.
* **The Flush:** Core 1 is forced to flush its dirty cache line down to the **L3 Cache** (or send it directly to Core 2 via interconnect).
* **The Reload:** Core 2 re-fetches the updated line from **L3** into its **L1 Cache**.
* **The Write:** Now Core 2 finally updates `ValueB` and marks the line `Modified`.
* **The Cost:** This operation invalidates the line in **Core 1**, restarting the cycle.

<h2> The Result: "Thrashing the L3" </h2>

Even though the threads are touching different variables, the CPU cores are fighting over the **same Cache Line**. 
Instead of working purely in their fast **L1 Caches** (1-2 ns latency), they are constantly pausing to push/pull data 
through the slower **L3 Cache** (10-20 ns latency) or main RAM. 

**This is False Sharing**. The system is slow not because of logic, but because the layout of data in memory 
causes physical contention in the cache hierarchy.

# Q - What is Cache Affinity?

Cache Affinity (also known as CPU Affinity) is essentially **"Thread Loyalty" to a specific CPU core**.

It is the strategy used by the Operating System scheduler to keep a specific thread running on the **same CPU core** 
as long as possible, rather than moving it around to different cores.

<h2> The "Why": Warm vs. Cold Cache </h2>

This concept is directly related to the **Cache Lines** we just discussed.

1\. **Warm Cache (Good):** When a thread runs on **Core 1**, it pulls data from RAM into Core 1's L1 and L2 caches. 
If the OS pauses the thread and resumes it later on the **same Core 1**, that data is likely still there. 
The thread resumes immediately at top speed.

2\. **Cold Cache (Bad):** If the OS moves the thread to **Core 2**, that new core has none of the thread's data.

* The thread must wait while data is fetched from L3 or Main RAM.
* It also effectively "pollutes" Core 2's cache, potentially evicting useful data needed by whatever 
was running there before.

<h2> Types of Affinity </h2>

<h3> 1. Soft Affinity (Natural) </h3>

* **What it is:** The OS scheduler tries to keep a thread on the same core, but it doesn't promise anything. 
If the original core is busy and another is free, the OS will migrate the thread to keep the system load balanced.
* **Java context:** This is the default behavior for all standard Java threads. The Linux scheduler (CFS) is 
generally good at this naturally.

<h3> 2. Hard Affinity (Pinned) </h3>

* **What it is:** You explicitly command the OS: _"This thread MUST run on Core 3 and NOWHERE else."_
* **Pros:** Guaranteed cache locality; zero context switch overhead from migration.
* **Cons:** If Core 3 is busy, the thread waits, even if Core 4 is completely idle.

<h2> Hard Affinity in Java </h2>

Standard Java (`java.lang.Thread`) does not have an API for Hard Affinity. Java is designed to 
be "Write Once, Run Anywhere," and CPU topology is too hardware-specific.

# Q - Why False Sharing is more likely happen with ExecutorService?

Consider the following code:

```java
class Data {
    volatile long a;
    volatile long b;
}
```

Assume (this is realistic):

```text
a and b are on the SAME cache line
```

<h2> Case 1: NO ExecutorService (single-threaded) </h2>

```java
public static void main(String[] args) {
    Data d = new Data();

    // Task A
    for (int i = 0; i < 1_000_000; i++) {
        d.a++;
    }

    // Task B
    for (int i = 0; i < 1_000_000; i++) {
        d.b++;
    }
}
```

What happens in time

```text
Time →
Core 0: AAAAAAAA BBBBBBBB
Core 1: -------- --------
```

* Only one core writes
* Even though a and b share a cache line
* The cache line never "bounces"

👉 False sharing is impossible here


<h2> Case 2: ExecutorService (THIS is the difference) </h2>

```java
ExecutorService pool = Executors.newFixedThreadPool(2);
Data d = new Data();

pool.submit(() -> {
    for (int i = 0; i < 1_000_000; i++) {
        d.a++;
    }
});

pool.submit(() -> {
    for (int i = 0; i < 1_000_000; i++) {
        d.b++;
    }
});

pool.shutdown();
```

What happens in time

```text
Time →
Core 0: AAAAAAAA AAAAAAAA
Core 1: BBBBBBBB BBBBBBBB
```

* Both cores write at the same time
* Both touch the same cache line
* Cache line keeps moving between cores
👉 False sharing happens

<h2> Why ExecutorService keeps coming up </h2>

Because `ExecutorService`:

* forces parallel execution
* guarantees "same time" writes
* exposes cache-line issues that sequential code hides

It does NOT:

* create sharing
* move objects
* break correctness

`ExecutorService` increases false sharing exposure because it makes independent writes occur concurrently on 
different cores, which is required for cache-line ping-pong to happen.

✅ Note: Assuming `a` and `b` belong to different cache lines, then False-sharing is impossible


# Q - How to provide initial value when using ThreadLocal?

In Java, there are two correct and interview-relevant ways to provide an initial value for a `ThreadLocal` variable.

<h2> 1. Override initialValue() (Legacy / Pre-Java 8 style) </h2>

You can create an anonymous subclass of `ThreadLocal` and override the `initialValue()` method.

**Example:**

```java
ThreadLocal<Integer> counter = new ThreadLocal<>() {
    @Override
    protected Integer initialValue() {
        return 0;
    }
};
```

<h3> Behavior </h3>

* `initialValue()` is invoked once per thread
* It is called lazily, i.e., the first time `get()` is invoked in that thread
* Each thread gets its own independent copy

<h3> When to mention this </h3>

* Important for interviews involving older Java versions
* Demonstrates understanding of `ThreadLocal` internals


<h2> 2. Use ThreadLocal.withInitial() (Recommended, Java 8+) </h2>

Java 8 introduced a factory method that accepts a `Supplier`.

**Example:**

```java
ThreadLocal<Integer> counter = ThreadLocal.withInitial(() -> 0);
```

<h3> Behavior </h3>

* Functionally identical to `initialValue()`
* Cleaner, functional style
* Still lazy and per-thread

<h3> Advantages </h3>

* More readable
* Encourages immutable or well-scoped initialization
* Standard approach in modern codebases

<h2> Key Rules (Very Important for Interviews) </h2>

❌ Constructor does NOT set per-thread value

```java
new ThreadLocal<>(0); // ❌ INVALID — no such constructor
```

`ThreadLocal` does not store a value itself. It stores values inside each Thread's `ThreadLocalMap`.

<h2> Lifecycle Summary </h2>

| Event                  | What Happens                     |
|------------------------|----------------------------------|
| Thread created         | No value created                 |
| `threadLocal.get()`    | Initial value created if absent  |
| `threadLocal.set(x)`   | Overrides current thread’s value |
| `threadLocal.remove()` | Deletes value for current thread |


<h2> Common Interview Trap Question </h2>

Q: When is `initialValue()` executed?

A: Only when `get()` is called for the first time by a thread, and only for that thread.


# Q - What is InheritableThreadLocal?



# Q - What is ThreadLocalMap?

