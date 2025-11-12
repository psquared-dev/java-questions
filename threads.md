## Classic Methods to create threads

There are two classic methods to create threads.

1. By implementing the `Runnable` interface.
1. By extending the `Thread` class


The first approach is generally preferred, as it allows your class to **extend another class** (since Java supports single inheritance) and **implement multiple interfaces at the same time**.

```Java
public class Demo01 {
    public static void main( String[] args ) throws InterruptedException {
        PostClientV1 postClient1 = new PostClientV1(1);
        PostClientV1 postClient2 = new PostClientV1(2);
        PostClientV1 postClient3 = new PostClientV1(3);
        PostClientV1 postClient4 = new PostClientV1(4);

        ArrayList<PostClientV1> postClients = new ArrayList<>();
        postClients.add(postClient1);
        postClients.add(postClient2);
        postClients.add(postClient3);
        postClients.add(postClient4);

        ArrayList<Thread> threads = new ArrayList<>();

        for (PostClientV1 postClient : postClients) {
            Thread th = new Thread(postClient);
            th.start();
            threads.add(th);
        }

        System.out.println("end");
    }
}
```

Here the main thread waits for the worker threads to finish. If we don't want that, we have to set `Thread.setDaemon()` to `true`.


## Drawbacks of classical approach

1. Too much boilerplate code — Creating and managing threads manually adds ceremony.
1. No built-in thread pooling — Each task spawns its own thread; there’s no reuse.
1. Difficult to get results back — Runnable’s `run()` returns void; no way to retrieve computed data.
1. Error handling is cumbersome — Checked exceptions inside run() must be handled manually; exceptions can be silently lost.
1. Poor scalability — Creating many threads leads to context-switch overhead and memory pressure.

## ExecutorService

The Executor framework, and related classes like ThreadPoolExecutor, Future, and Callable) was introduced in Java 5 
as part of `the java.util.concurrent` package.

It was designed to address the aforementioned limitations of manually managed threads.

| Executor Type                    | Method                                               | Description                                                     |
|----------------------------------|------------------------------------------------------|-----------------------------------------------------------------|
| **Single-threaded executor**     | `Executors.newSingleThreadExecutor()`                | Executes tasks sequentially on a single worker thread.          |
| **Fixed thread pool**            | `Executors.newFixedThreadPool(int nThreads)`         | Reuses a fixed number of threads for executing tasks.           |
| **Cached thread pool**           | `Executors.newCachedThreadPool()`                    | Creates new threads as needed and reuses idle ones.             |
| **Work-stealing pool** (Java 8+) | `Executors.newWorkStealingPool()`                    | Uses a ForkJoinPool to balance load across multiple processors. |
| **Scheduled thread pool**        | `Executors.newScheduledThreadPool(int corePoolSize)` | Executes tasks after a delay or periodically (like a cron).     |

The following example demonstrates how to use `ExecutorService` to manage a pool of threads for executing tasks:

```Java
public class Demo02 {
    private static final Logger logger = LoggerFactory.getLogger(Demo02.class);

    public static void main(String[] args) throws InterruptedException, ExecutionException {
        List<PostClientV2> tasks = new ArrayList<>();
        ArrayList<Future<PostResponse>> futures = new ArrayList<>();

        PostClientV2 postClient1 = new PostClientV2(1);
        PostClientV2 postClient2 = new PostClientV2(2);
        PostClientV2 postClient3 = new PostClientV2(3);
        PostClientV2 postClient4 = new PostClientV2(4);

        tasks.add(postClient1);
        tasks.add(postClient2);
        tasks.add(postClient3);
        tasks.add(postClient4);

        ExecutorService executorService = Executors.newFixedThreadPool(5);

        for (PostClientV2 task : tasks) {
            futures.add(executorService.submit(task));
        }

        for (Future<PostResponse> future : futures) {
            logger.info("Main received response: {}", future.get());
        }

        executorService.shutdown();
        System.out.println("end");
    }
}
```

The `executorService.submit(task)` submits a task for asynchronous execution and immediately returns a `Future`, allowing 
the program to continue without waiting — this call is non-blocking. Adding the returned Future to a list (e.g., `futures.add(...)`) 
also doesn’t block; it just stores a reference. However, calling `future.get()` is blocking — it pauses the calling 
thread until the task completes (or a timeout/interruption occurs). In short, `submit()` schedules work asynchronously, 
while `get()` synchronizes to retrieve the result once available.

## CompletableFuture (Java 8+)

The `CompletableFuture` was introduced in Java 8 as an enhancement over `Future`. Unlike `Future`, which only allows blocking 
retrieval of results via `get()`, `CompletableFuture` supports non-blocking, reactive-style composition using callbacks 
like `thenApply()`, `thenAccept()`, and `thenCombine()`. It can complete asynchronously, handle dependent tasks, and 
chain multiple computations together without manual thread management. It also integrates seamlessly with the 
Executor framework, allowing better control over async execution. In short, `CompletableFuture` makes asynchronous 
programming in Java more flexible, composable, and non-blocking.

Here's an example of using `CompletableFuture` to perform asynchronous tasks:

```Java
public class Demo03 {
    private static final Logger logger = LoggerFactory.getLogger(Demo03.class);

    public static void main(String[] args) throws InterruptedException, ExecutionException {
        ExecutorService executorService = Executors.newFixedThreadPool(5);
        List<Integer> postIds = List.of(1, 2, 3, 4, 5);

        List<CompletableFuture<PostResponse>> futures = postIds.stream().map(postId -> {
            return CompletableFuture.supplyAsync(() -> {
                return new PostClientV3().getUser(postId);
            }, executorService);
        }).toList();

        futures.forEach(cf -> {
            cf.thenAccept(postResponse -> {
                logger.info("Received response: {}", postResponse);
            });
        });

        CompletableFuture.allOf(futures.toArray(new CompletableFuture[0])).join();

        List<PostResponse> list = futures.stream()
                .map(CompletableFuture::join)
                .toList();

        logger.info("All responses: {}", list);

        executorService.shutdown();
        System.out.println("end");
    }
}
```

```java
public class Main04 {
    private static final Logger logger = LoggerFactory.getLogger(Main03.class);

    public static void main(String[] args) throws InterruptedException {
        Mono.fromFuture(getName())
                .subscribe(Util.subscriber());

        Thread.sleep(3000);
    }

    public static CompletableFuture<String> getName() {
        return CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(Duration.ofSeconds(2));
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            logger.info("Inside supplyAsync getName");
            return "hello from future";
        });
    }
}
```

**Output:**

```bash
11:02:06.508 [main] DEBUG reactor.util.Loggers -- Using Slf4j logging framework
11:02:06.514 [main] INFO org.example.commons.DefaultSubscriber -- Subscribed
11:02:08.478 [ForkJoinPool.commonPool-worker-1] INFO org.example.sec02.Main03 -- Inside supplyAsync getName
11:02:08.478 [ForkJoinPool.commonPool-worker-1] INFO org.example.commons.DefaultSubscriber --  Received: hello from future
11:02:08.481 [ForkJoinPool.commonPool-worker-1] INFO org.example.commons.DefaultSubscriber --  Completed
```

Here the `subsriber.onNext()` is called from ForkJoinPool thread instead of main thread.

When you have the `Thread.sleep(2s)` inside the future:

* The `CompletableFuture` runs on a different thread (ForkJoinPool worker).
* The main thread continues immediately after `subscribe()`.
* Later, when the future completes, the ForkJoinPool thread triggers the completion callback inside Reactor.
* That’s why `onNext()` and `onComplete()` run on the ForkJoinPool thread.


In the next, example we have removed the `Thread.sleep(2s)` from inside the future:

```java
public class Main04 {
    private static final Logger logger = LoggerFactory.getLogger(Main03.class);

    public static void main(String[] args) throws InterruptedException {
        Mono.fromFuture(getName())
                .subscribe(Util.subscriber());

        Thread.sleep(3000);
    }

    public static CompletableFuture<String> getName() {
        return CompletableFuture.supplyAsync(() -> {
            logger.info("Inside supplyAsync getName");
            return "hello from future";
        });
    }
}
```

**Output:**

```bash
11:10:25.435 [ForkJoinPool.commonPool-worker-1] INFO org.example.sec02.Main03 -- Inside supplyAsync getName
11:10:25.468 [main] DEBUG reactor.util.Loggers -- Using Slf4j logging framework
11:10:25.473 [main] INFO org.example.commons.DefaultSubscriber -- Subscribed
11:10:25.474 [main] INFO org.example.commons.DefaultSubscriber --  Received: hello from future
11:10:25.477 [main] INFO org.example.commons.DefaultSubscriber --  Completed
```

Here the `subsriber.onNext()` is called from main thread instead of ForkJoinPool thread.

When there’s no delay, the future’s computation finishes almost instantly, before `Mono.fromFuture()` even attaches its callback.

So Reactor sees that:
* The future is already completed when the subscription starts.
* Instead of deferring, Reactor directly emits the value synchronously on the calling thread — which here is the main thread.


Mono.defer is used to defer (delay) the creation of a Mono until subscription time. This ensures that the logic inside 
defer is executed only when someone subscribes to the Mono, allowing for dynamic or late-bound behavior.


```java
public class Main05 {
    public static  Logger logger = LoggerFactory.getLogger(Main05.class);
    public static void main(String[] args) {
        Mono.defer(() ->  getName());
    }

    public static Mono<String> getName(){
        logger.info("publisher creation");
        return Mono.fromSupplier(() -> {
            logger.info("publisher execution");
            return "hello from supplier";
        });
    }
}
```

Output:

```bash
11:23:57.804 [main] DEBUG reactor.util.Loggers -- Using Slf4j logging framework
```

Example:

```java
public class Main05 {
    public static Logger logger = LoggerFactory.getLogger(Main05.class);

    public static void main(String[] args) throws InterruptedException {


        Mono.defer(() -> getName())
                .subscribeOn(Schedulers.boundedElastic())
                .subscribe(s -> logger.info("Received: {}", s));

        Thread.sleep(2000);
    }

    public static Mono<String> getName() {
        logger.info("publisher creation");
        return Mono.fromSupplier(() -> {
            logger.info("publisher execution");
            return "hello from supplier";
        });
    }
}
```

**Output:**

```bash
11:29:05.745 [main] DEBUG reactor.util.Loggers -- Using Slf4j logging framework
11:29:05.767 [boundedElastic-1] INFO org.example.sec02.Main05 -- publisher creation
11:29:05.768 [boundedElastic-1] INFO org.example.sec02.Main05 -- publisher execution
11:29:05.768 [boundedElastic-1] INFO org.example.sec02.Main05 -- Received: hello from supplier
```

`subscribeOn()` and `publishOn()` control threading in Reactor but affect different parts of the reactive pipeline. 
`subscribeOn()` influences the upstream, deciding which thread executes the source and all operators before it—essentially 
where data production starts. In contrast, `publishOn()` affects the downstream, switching the execution context for 
all operators and subscribers that come after it. Only one `subscribeOn()` (the first in the chain) takes effect, while 
multiple `publishOn()` operators can appear to shift threads at different points. In short, `subscribeOn()` changes 
where the stream begins, and `publishOn()` changes where it continues.


| Operator                 | Affects        | Meaning                                                                             |
|--------------------------|----------------|-------------------------------------------------------------------------------------|
| `subscribeOn(scheduler)` | **Upstream**   | Moves the *source and upstream operators* to the given scheduler.                   |
| `publishOn(scheduler)`   | **Downstream** | Switches *subsequent (downstream)* operators and subscriber to the given scheduler. |


`Mono.defer()` delays the creation of the publisher until subscription time. When `.subscribe()` is called, 
`subscribeOn(Schedulers.boundedElastic())` schedules the entire upstream execution (i.e., `getName()` and the supplier inside it) 
on a boundedElastic thread. So the publisher creation and execution logs run on that thread, and the emitted value is
delivered to the subscriber (also on the same thread, since no `publishOn()` is used). The `Thread.sleep(2000)` just prevents 
the main thread from exiting before the async task finishes.


## EventLoop Basics - What Happens When a Netty Client Connects to jsonplaceholder

### Cast of Characters

| Character                    | Who they are                        | What they do                                        |
|------------------------------|-------------------------------------|-----------------------------------------------------|
| **You / Your code**          | The one calling `httpClient.get()`  | Tells Netty “please send a request.”                |
| **EventLoop thread**         | Netty’s helper thread               | Continuously loops, juggling events and tasks       |
| **Task Queue**               | Netty’s “to-do list” for Java tasks | Where new work (like connect/write) gets added      |
| **Event Queue (Selector)**   | OS-level notification list          | OS tells Netty when a socket can connect/read/write |
| **Server (jsonplaceholder)** | The remote library                  | Responds with your requested data                   |

### Step-by-Step Breakdown

**Step #1 - You start a request**

You write:

```java
httpClient.get()
    .uri("/posts/1")
    .responseSingle(...)
    .subscribe();
```

* Reactor prepares everything needed for an HTTP GET.
* It doesn’t immediately perform network I/O.
* Instead, it schedules a task for Netty to initiate the connection.


* 🧾 Goes into: Netty’s Task Queue
* 🧵 Thread: main → task queued for eventloop-1


**Step #2 - EventLoop picks up the task**

The EventLoop thread (eventloop-1) runs continuously like this:

```java
while (true) {
    poll selector (event queue);
    handle I/O events;
    run tasks from task queue;
}
```

It sees your “connect to server” task in its task queue, takes it out, and executes it.

* 🧾 From: Task Queue
* 🧵 Thread: eventloop-1


**Step #3 - Netty asks the OS to connect.**

* Netty now calls connect() on a non-blocking socket.
* The OS begins the TCP handshake but doesn’t finish instantly.
* So the socket is registered with the Selector (event queue) with interest OP_CONNECT.


* 🖥️ Registered in: Event Queue (managed by OS)
* 🧵 Thread: eventloop-1

**Step #4 - EventLoop goes to sleep**

* No immediate events yet.
* So the EventLoop calls:

```java
selector.select();
```

* It tells the OS: "Wake me up when something is ready — connect/read/write."
* The EventLoop now sleeps, using no CPU.


* 😴 Waiting for: Event in Event Queue
* 🧠 Managed by: OS kernel

**Step #5 - TCP handshake completes**

* The OS finishes connecting to jsonplaceholder.
* It marks the socket as **connectable** and places it in the **event queue**.
* The selector wakes up Netty’s EventLoop thread.


* 📨 Event Queue gets: `OP_CONNECT`
* 🧵 Thread wakes: eventloop-1


**Step #6 - EventLoop handles the event**

Netty’s event loop sees the `OP_CONNECT` event, calls `handleConnect()`, and marks the channel active — connection established!

* ✅ Handled: Event from Event Queue
* 🧾 Adds new task: "write HTTP request" → Task Queue


**Step #7 - EventLoop writes the request**

Next loop iteration:

* It polls selector again (maybe nothing new yet)
* Then runs its task queue
* Finds your "write HTTP request" task → runs it

Netty encodes the HTTP GET into bytes and calls `socket.write()`.

* 📨 Who writes: Client (Netty)
* 🧾 From: Task Queue
* 🧵 Thread: eventloop-1

The OS puts those bytes into the socket’s send buffer → sends to the server.


**Step #8 - EventLoop waits again**

* No new work immediately, so selector.select() blocks again.
* Now it waits for a readable event (server’s response).


* 😴 Waiting for: Event in Event Queue
* 🧠 Managed by: OS kernel


**Step #9 - Server sends response**

* The jsonplaceholder server processes your request, writes response bytes back.
* Your OS receives them and places them in the socket’s **receive buffer**.
* It marks the socket as **readable** → Event in Event Queue.


* 📨 Event Queue gets: OP_READ
* 🧵 Thread wakes: eventloop-1


**Step #10 - EventLoop reads response**

Netty’s event loop reads those bytes, decodes them through the pipeline (`HttpClientCodec`, etc.),
and delivers the result to Reactor as a `Mono<String>`.


* 📨 Handled: Event from Event Queue
* 🧵 Thread: eventloop-1


**Step #11 - Heavy work (optional offload)**

If your code does something expensive, like:

```java
.map(json -> objectMapper.readValue(json, PostResponse.class))
```

you should offload it with:

```java
.publishOn(Schedulers.boundedElastic())
```

Now that parsing runs on a **different thread pool**,
keeping the **event loop free** for I/O.

* 🧾 New thread: boundedElastic-x
* ✅ Event loop freed up

**Step #12 - Subscriber gets the data**

* Reactor emits the final PostResponse object to your subscriber.
* You print or log the result.
* Netty can now return the connection to its pool.


### Summary of Queue Flow

| Step | What happened                | Where it went                   |
|------|------------------------------|---------------------------------|
| 1️⃣  | New request                  | ➡️ **Task Queue**               |
| 2️⃣  | Connect initiated            | ➡️ OS starts handshake          |
| 3️⃣  | Connection done              | ➡️ **Event Queue (OP_CONNECT)** |
| 4️⃣  | HTTP request write scheduled | ➡️ **Task Queue**               |
| 5️⃣  | Write done, wait for read    | ➡️ OS monitors socket           |
| 6️⃣  | Server sent data             | ➡️ **Event Queue (OP_READ)**    |
| 7️⃣  | Data read + decoded          | ➡️ Reactor emits to app         |

### Tiny checklist (who writes/reads)

* `OP_WRITE` (writable) → client writes request bytes into send buffer.
* `OP_READ` (readable) → client reads response bytes that server wrote.
* All socket I/O and quick decode tasks run on the EventLoop thread unless you offload them.

## Flux examples:

```java
public class Main06 {

    private static final Logger logger = LoggerFactory.getLogger(Main06.class);

    public static Flux<String> namesFlux() {
        logger.info("outside");
        return Flux.fromStream(() -> {
            logger.info("inside");
            return Stream.generate(Main06::getName).limit(10);
        });
    }

    public static String getName() {
        System.out.println("Generating name...");
        try {
            Thread.sleep(Duration.ofSeconds(1));
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        Faker instance = Faker.instance();
        return instance.name().firstName();
    }

    public static void main(String[] args) {
        namesFlux()
                .subscribeOn(Schedulers.boundedElastic())
                .subscribe(Util.subscriber("aa"));

        try {
            Thread.sleep(Duration.ofSeconds(10));
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }
}
```

**Output:**

```bash
19:16:11.300 [main] INFO org.example.sec02.Main06 -- outside
19:16:11.349 [main] DEBUG reactor.util.Loggers -- Using Slf4j logging framework
19:16:11.352 [main] INFO org.example.sec02.Main06 -- inside
Generating name...
19:16:12.440 [main] INFO org.example.commons.DefaultSubscriber -- Subscribed
19:16:12.440 [main] INFO org.example.commons.DefaultSubscriber -- aa Received: Jerome
Generating name...
19:16:13.464 [main] INFO org.example.commons.DefaultSubscriber -- aa Received: Hugo
Generating name...
19:16:14.476 [main] INFO org.example.commons.DefaultSubscriber -- aa Received: Sarina
Generating name...
19:16:15.489 [main] INFO org.example.commons.DefaultSubscriber -- aa Received: Joshua
Generating name...
19:16:16.499 [main] INFO org.example.commons.DefaultSubscriber -- aa Received: Fidel
Generating name...
19:16:17.532 [main] INFO org.example.commons.DefaultSubscriber -- aa Received: Gregorio
Generating name...
19:16:18.551 [main] INFO org.example.commons.DefaultSubscriber -- aa Received: Miguel
Generating name...
19:16:19.566 [main] INFO org.example.commons.DefaultSubscriber -- aa Received: Troy
Generating name...
19:16:20.575 [main] INFO org.example.commons.DefaultSubscriber -- aa Received: Henry
Generating name...
19:16:21.589 [main] INFO org.example.commons.DefaultSubscriber -- aa Received: Arlen
19:16:21.594 [main] INFO org.example.commons.DefaultSubscriber -- aa Completed
```

### How it works:

In this example, there are **no HTTP calls** (so no Netty event loop threads) and **no scheduler specified** (so no context switch).
Therefore, Reactor executes everything — from data generation to subscriber callbacks — on the **main thread**.

Reactor only changes threads when:
* The source itself is asynchronous (like Reactor Netty, Sinks, etc.), or
* You explicitly tell it to use a scheduler via `.subscribeOn()` or `.publishOn()`.

### Understanding Why Flux Makes Sense

If it had been an HTTP call, then the subscriber would be executing **for chunks of data** as they arrive — and this 
is where the behavior truly differs. 

This is also where it starts to make sense why a `Flux` is designed as a **Publisher** instead of simply being a container like a `List<String>`.

We could technically collect the chunks into a list and return that list once everything is received,
but that approach doesn’t fit the reactive model — because a `List` has no concept of `onNext`, `onError`, or `onComplete`.

A `List` represents data that's already fully available in memory, while `Flux` represents data that **arrives over time**, piece by piece.
It's capable of signaling when new data comes (`onNext`), when the stream finishes (`onComplete`), or when an error occurs (`onError`).

So it wouldn’t make sense to use a List for streaming HTTP data —
`Flux` is the right abstraction because it naturally models `a stream of data emitted asynchronously`.

