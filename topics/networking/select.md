# select() working

## KEY IDEA (Very Important)

`readfds` changes its meaning:

| Stage             | Meaning                          |
|-------------------|----------------------------------|
| Before `select()` | which sockets you want to watch  |
| After `select()`  | which sockets are actually ready |

This is confusing at first, but it’s how `select()` works.

Let’s walk through one entire iteration of the select() loop, step-by-step, using actual sets 
written exactly like this:

```text
readfds = {3, 4, 5}
```

I’ll show everything that happens from the moment we enter the loop to the moment we enter the next
loop, with real file descriptor numbers. This will MAKE IT CRYSTAL CLEAR.

### Assume this situation before the loop begins:

* `listen_fd = 3`
* two clients already connected → `client_fd1 = 4`, `client_fd2 = 5`

### FULL ITERATION STARTS

**Step 1: CLEAR the set**

```c
FD_ZERO(&readfds);
```

Now:

```c
readfds = {}
```

Empty basket.

**Step 2: ADD sockets we want to watch**

Add listening socket:

```c
FD_SET(3, &readfds);
```

Now:

```c
readfds = {3}
```

Add each connected client:

```c
FD_SET(4, &readfds);
FD_SET(5, &readfds);
```

Now:

```c
readfds = {3, 4, 5}
```

This is the **WATCH set** — sockets we care about.

### **Step 3: Call select()**

```c
select(maxfd+1, &readfds, NULL, NULL, NULL);
```

The server is now **blocked** (sleeping).
Nothing happens until **one of these becomes READY**.


### ASSUME what happens during blocking:

Let’s say:

* No new client connects → 3 does NOT become ready
* Client on fd 4 sends data → 4 becomes readable
* Client on fd 5 stays silent

Kernel marks:

```text
fd 4 is readable
```

### **Step 4: select() RETURNS**

`select()` wakes up and modifies the `readfds` set:

Before select():

```text
readfds = {3, 4, 5}
```

After select():

```text
readfds = {4}
```

ONLY the sockets that are **ready** remain in the set.

➡️ Because fd 4 has data waiting to be read.


### Step 5: Server checks READY sockets

Check listening socket:

```c
if (FD_ISSET(3, &readfds)) ...
```

```text
No → 3 is not in {4}
```

Check client 4:

```c
if (FD_ISSET(4, &readfds)) read(4)
```

```text
Yes → 4 is ready
```

Server reads from socket 4.

Check client 5:

```c
if (FD_ISSET(5, &readfds)) read(5)
```

```text
No → 5 is not in {4}
```

### Step 6: END OF LOOP

We handled everything in the ready set {4}, Now we go to the next iteration.

### SECOND ITERATION — COMPLETE VISUAL WALKTHROUGH

At the end of Iteration 1:

* We handled client_fd = 4
* No new client connected
* All sockets are still open

Current active sockets:

```text
listen_fd = 3
client1 = 4
client2 = 5
```

### SECOND ITERATION STARTS NOW

### Step 1: CLEAR the set

```c
FD_ZERO(&readfds);
```

Now:

```text
readfds = {}
```

### Step 2: ADD sockets we want to watch again

You must rebuild the watch list every iteration.

```c
FD_SET(3, &readfds);
FD_SET(4, &readfds);
FD_SET(5, &readfds);
```

Now:

```text
readfds = {3, 4, 5}
```

This is the **WATCH SET** for iteration 2.


### Step 3: select() BLOCKS again

Server calls:

```c
select(maxfd+1, &readfds, NULL, NULL, NULL);
```

Server sleeps, waiting for:

* new connection (fd 3 becomes readable)
* data from client 4
* data from client 5


### LET’S ASSUME NEW EVENTS IN ITERATION 2

Let’s choose a scenario:

👉 Scenario:

* Client 5 sends data in this iteration
* No new client connects
* Client 4 is idle (already handled in iteration 1)

So kernel marks:

```text
fd 5 is READABLE
```

### Step 4: select() RETURNS

Before select():

```text
readfds = {3, 4, 5}
```

After select(), modified by kernel:

```text
readfds = {5}
```

Because only socket 5 has data waiting.

This is the **READY SET**.


### Step 5: Server checks READY sockets

Check listen_fd (3):

```text
FD_ISSET(3)?  → No
```

Check client 4:

```text
FD_ISSET(4)?  → No
```

Check client 5:

```text
FD_ISSET(5)?  → YES
```

So server does:

```text
read(5, buffer)
```

And handles client 5’s request.

### Step 6: End of Iteration 2

We handled all readable sockets (only fd 5), now we return to the start of the loop.


## Main issue with select() = O(N) scan of all file descriptors

**Setup (what we have)**
* listen_fd = 3 (listening socket)
* client_fds = {4,5,6,7,8} (five connected clients)
* Server loop uses select() with a readfds set to watch these fds.

I'll show the **full cycle** of one **select()** iteration and explain each step in plain language.


### Step A — Server prepares the watch list (user-space)

What happens: server clears and rebuilds readfds from its own list of sockets.  

```c
FD_ZERO(&readfds);            // clear set
FD_SET(3, &readfds);
FD_SET(4, &readfds);
FD_SET(5, &readfds);
FD_SET(6, &readfds);
FD_SET(7, &readfds);
FD_SET(8, &readfds);
select(maxfd+1, &readfds, NULL, NULL, NULL);
```

**Important detail:** The server keeps a separate permanent list (its roster). `readfds` is just the temporary 
clipboard built each loop.

**Cost:** server did O(N) work to build the set (loop over its client list).

### **Step B — The kernel receives the watch list**

**What happens:** the kernel copies the readfds set into kernel memory so the kernel can check readiness.

**Cost:** copying the bitset may cost O(size_of_bitmask) — small but nonzero.

### **Step C — Kernel scans all watched descriptors**

What happens: inside the kernel, it checks each fd in the set to see if it’s 
readable (data available or a pending connection). Example check order:

```text
check fd 3 → not ready
check fd 4 → not ready
check fd 5 → READY (client sent data)
check fd 6 → not ready
check fd 7 → not ready
check fd 8 → not ready
```

**Result:** kernel builds a ready set (e.g. {5}).

**Cost:** O(N) work inside kernel — it examined every watched fd.

### Step D — Kernel returns, select() unblocks, readfds is modified

**What happens:** `select()` returns and the readfds you passed is now changed to keep only the 
ready fds (kernel wrote the result back).

So before select: `readfds = {3,4,5,6,7,8}`

after select returns: `readfds = {5}`

**Note:** the original readfds variable was overwritten in place by the kernel.

**Analogy (array = bitmask; indexes = fds)**

Think of `fd_set` as an array or bitmap where:

* each index represents a file descriptor number
* each value (0 or 1) represents whether that descriptor is active in the set

Before `select()`:

```text
Index (fd):   0 1 2 3 4 5 6 7 8
Value:        0 0 0 1 1 1 1 1 0
```

After `select()` determines that only fd 5 is ready, it rewrites the array:

```text
Index (fd):   0 1 2 3 4 5 6 7 8
Value:        0 0 0 0 0 1 0 0 0
```

### Step E — Server checks which fds are ready (user-space scan)

**What happens:** server code tests each fd with FD_ISSET to discover which ones are ready:

```c
if (FD_ISSET(3,&readfds)) ...   // false
if (FD_ISSET(4,&readfds)) ...   // false
if (FD_ISSET(5,&readfds)) ...   // true -> read(5)
#...
```

**Cost:** server does O(N) checks again (even though kernel already checked). This is why select causes scanning on 
both sides.


### Step F — Server handles ready sockets

**What happens:** server calls `read(5, ...)` and processes the incoming data. If new connections were pending, 
server would call `accept()`.

**Effect on state:** after reading, the socket's receive buffer may be drained (so fd 5 might not be ready next 
time unless more data arrives).


### Step G — Loop repeats: server rebuilds watch list

**What happens:** after finishing, server prepares the next iteration: it clears `readfds` and rebuilds it from its 
permanent list `{3,4,5,6,7,8}` (or updated list if some sockets closed).

```c
FD_ZERO(&readfds);
FD_SET(3, &readfds);
... add other client fds ...
select(...);
```

**Cost:** O(N) work to rebuild the set.

### Summary of costs (why select() is O(N) and costly)

* Server builds the watch set: O(N) per loop.
* Kernel scans the watch set to test readiness: O(N) per loop.
* Server scans the returned ready set with FD_ISSET checks: O(N) per loop.
* You must rebuild the set each loop because select() modifies it in place.

So the work per iteration is proportional to the number of watched fds — that's the O(N) problem.


## Sample select() server in Java (NIO)

```java
var serverSocketChannel = ServerSocketChannel.open();
serverSocketChannel.configureBlocking(false);
// Creates a non-blocking TCP listening socket (socket() + fcntl(O_NONBLOCK))

var selector = Selector.open();
// On Linux → epoll_create()
// On macOS → kqueue()
// On Windows → IOCP

serverSocketChannel.bind(new InetSocketAddress(portNumber));
// bind() + listen() under the hood

serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);
// epoll_ctl(ADD, server_fd, EPOLLIN)

while (true) {

    if (selector.select() == 0) continue;
    // selector.select() = epoll_wait()
    // Blocks until some fd becomes ready

    for (var key : selector.selectedKeys()) {

        if (key.isAcceptable()) {
            // This means: server_fd got EPOLLIN → accept() won't block

            var clientChannel = serverSocketChannel.accept();
            // accept() = new client fd

            clientChannel.configureBlocking(false);
            // fcntl(client_fd, O_NONBLOCK)

            clientChannel.register(selector, SelectionKey.OP_READ);
            // epoll_ctl(ADD, client_fd, EPOLLIN)
        }

        else if (key.isReadable()) {
            // client_fd triggered EPOLLIN → data is available

            var clientChannel = (SocketChannel) key.channel();
            // read(client_fd, ...)
        }
    }
}
```