Good. This is the right attitude.

We will go **very strict, very slow, zero jumps**.

One micro-step at a time.

---

## STEP 0 — JVM PROCESS STARTS

You run:

```
java NioServer
```

---

## 🧠 Process Structure (Kernel View)

```
task_struct (PID 1000)
   └── files
         └── fdtable
```

---

## 📂 FD Table (Initial)

```
0 → stdin
1 → stdout
2 → stderr
```

---

## 🌐 Kernel TCP Tables

```
tcp_listen_hash: empty
tcp_ehash: empty
```

---

Nothing else exists.

No sockets.
No epoll.
No networking.

---


Good.

---

## STEP 1 — `ServerSocketChannel.open()`

Java:

```java
ServerSocketChannel server = ServerSocketChannel.open();
```

We go strictly step by step.

---

## 1️⃣ JVM Enters Native Code

HotSpot calls native:

```
socket(AF_INET, SOCK_STREAM, 0)
```

---

## 2️⃣ Kernel Allocates FD Number

Kernel looks at:

```
task_struct → files → fdtable
```

Finds first free slot:

```
FD 3
```

Reserves it.

---

## 3️⃣ Kernel Creates Networking Objects

Kernel allocates:

* `struct sock` (state = TCP_CLOSE)
* `struct socket`
* `struct file`

Links them:

```
struct file
   ↓
struct socket
   ↓
struct sock (TCP_CLOSE)
```

---

## 4️⃣ Kernel Installs in FD Table

```
fdtable[3] = struct file*
```

---

# 🔎 SYSTEM STATE NOW

---

## 📂 FD Table

```
0 → stdin
1 → stdout
2 → stderr
3 → struct file
        ↓
     struct socket
        ↓
     struct sock (TCP_CLOSE)
```

---

## 🌐 Kernel Tables

```
tcp_listen_hash: empty
tcp_ehash: empty
```

---

## ☕ JVM

```
ServerSocketChannelImpl
   └── FileDescriptor(fd=3)
```

---

Important:

* No bind
* No listen
* Not visible on network
* No queues exist

---

Stop here.

Is this socket reachable from network yet?

Yes or no?


You skipped the check, but that’s okay — answer is:

👉 **No**, it is not reachable yet.

Now we move.

---

## STEP 2 — `server.bind(8081)`

Java:

```java
server.bind(new InetSocketAddress(8081));
```

Very important:
In Java NIO, `bind()` internally performs:

```
bind()
listen()
```

We trace both.

---

## 1. Native bind(fd=3)

Kernel:

* Assigns local address (0.0.0.0)
* Assigns port 8081
* Inserts into **bind hash table**

State still:

```
TCP_CLOSE
```

(Temporary state before listen)

---

## 2. Native listen(fd=3)

Kernel now:

* Changes state → `TCP_LISTEN`
* Inserts into **tcp_listen_hash**
* Allocates:

    * SYN queue
    * Accept queue

---

# 🔎 SYSTEM STATE NOW

---

## 📂 FD Table

```
0 → stdin
1 → stdout
2 → stderr

3 → struct file
        ↓
     struct socket
        ↓
     struct sock (TCP_LISTEN)
          ├── SYN queue: empty
          └── Accept queue: empty
```

---

## 🌐 Kernel Tables

```
tcp_listen_hash:
   0.0.0.0:8081 → struct sock (FD 3)

tcp_ehash:
   empty
```

---

Important:

Now the server is visible on the network.

But:

* No epoll yet
* No selector yet
* No thread sleeping yet

---

Stop here.


You skipped the check again 🙂 — but answer is:

👉 Kernel checks **tcp_ehash first**, then **tcp_listen_hash**.

Now we continue.

---

## STEP 3 — `Selector.open()`

Java:

```java
Selector selector = Selector.open();
```

---

## 1️⃣ JVM Calls Native epoll_create1()

Kernel allocates:

```
struct eventpoll
```

---

## 2️⃣ Kernel Allocates New FD

Kernel finds next free slot:

```
FD 4
```

Installs:

```
fdtable[4] = struct file → struct eventpoll
```

---

# 🔎 SYSTEM STATE NOW

---

## 📂 FD Table

```
0 → stdin
1 → stdout
2 → stderr

3 → listening socket (TCP_LISTEN)
        SYN queue: empty
        Accept queue: empty

4 → struct file
        ↓
     struct eventpoll
          ├── Interest tree: empty
          └── Ready list: empty
```

---

## 🌐 Kernel Tables

```
tcp_listen_hash:
   8081 → FD 3

tcp_ehash:
   empty
```

---

Important:

* epoll exists
* But it is watching NOTHING yet

---

Stop here.

At this moment, if client connects,
will selector wake up?

Yes or no?


Answer would have been:

👉 **No**, because nothing is registered in epoll yet.

Now we move forward carefully.

---

## STEP 4 — `server.register(selector, OP_ACCEPT)`

Java:

```java
server.register(selector, SelectionKey.OP_ACCEPT);
```

---

## 1️⃣ JVM Creates SelectionKey

Java object:

```
SelectionKeyImpl
   channel = ServerSocketChannel(fd=3)
   selector = EPollSelector(fd=4)
   interestOps = OP_ACCEPT
```

This is JVM bookkeeping only.

---

## 2️⃣ JVM Calls Native epoll_ctl()

Native call:

```
epoll_ctl(
    epfd = 4,
    EPOLL_CTL_ADD,
    fd = 3,
    EPOLLIN
)
```

Important mapping:

```
OP_ACCEPT  →  EPOLLIN (for listening socket)
```

---

## 3️⃣ Kernel Updates epoll Interest Tree

Inside `struct eventpoll` (FD 4):

```
Interest tree:
    FD 3
Ready list:
    empty
```

---

# 🔎 SYSTEM STATE NOW

---

## 📂 FD Table

```
3 → listening socket (TCP_LISTEN)
4 → epoll
       Interest:
          FD 3
       Ready:
          empty
```

---

## 🌐 Kernel Tables

```
tcp_listen_hash:
   8081 → FD 3

tcp_ehash:
   empty
```

---

Now epoll is watching the listening socket.

---

Next in code:

```java
selector.select();
```

Say **next** and we go into the blocking stage and then simulate SYN → SYN-ACK → ACK properly.


Good. Now we move into the blocking stage and then simulate the full handshake properly.

---

## STEP 5 — `selector.select()`

Java executes:

```java
selector.select();
```

---

## 1️⃣ JVM Calls Native

```
epoll_wait(epfd=4, ...)
```

---

## 2️⃣ Kernel Checks eventpoll

Current epoll state:

```
FD 4 → struct eventpoll
       Interest:
          FD 3 (listening socket)
       Ready:
          empty
```

Ready list is empty.

So kernel:

* Puts thread into epoll wait queue
* Marks thread as sleeping
* Scheduler removes it from CPU

---

# 🔎 SYSTEM STATE NOW

---

## 📂 FD Table

```
3 → listening socket (TCP_LISTEN)
       SYN queue: empty
       Accept queue: empty

4 → epoll
       Interest:
          FD 3
       Ready:
          empty
```

---

## 🌐 Kernel Tables

```
tcp_listen_hash:
   8081 → FD 3

tcp_ehash:
   empty
```

---

## 🧵 Thread State

```
Sleeping inside epoll_wait
```

---

Now we simulate a client connecting.

---

# STEP 6 — CLIENT SENDS SYN

Client sends:

```
SYN (seq = X)
```

---

## 🌐 Kernel Processing

* 1️⃣ Check tcp_ehash → not found
* 2️⃣ Check tcp_listen_hash → found FD 3

---

## 🔧 Kernel Action

* Create `request_sock`
* Insert into SYN queue
* Send SYN-ACK

---

## 📦 Listening Socket State

```
FD 3 (TCP_LISTEN)
   ├── SYN queue:
         request_sock #R1 (SYN_RECV)
   └── Accept queue:
         empty
```

---

## 📂 FD Table

Unchanged.

---

## 🧵 Thread

Still sleeping.

Why?

Because accept queue is still empty.

---

Stop here.

Handshake is not complete yet.

Say **next** and we simulate:

SYN-ACK delivered → client sends ACK → promotion to ESTABLISHED → epoll wakeup.



Excellent. Now we complete the handshake **properly and strictly**.

We are currently:

```
FD 3 → TCP_LISTEN
   SYN queue: 1 (request_sock #R1)
   Accept queue: empty

FD 4 → epoll
   Interest: FD 3
   Ready: empty

Thread: sleeping in epoll_wait
```

---

# STEP 7 — Client Receives SYN-ACK and Sends Final ACK

Client sends:

```
ACK  (seq = X+1)
```

This completes the 3-way handshake.

---

## 🔧 Kernel Promotion Happens

Kernel processes the ACK:

### 1️⃣ Remove request_sock from SYN queue

```
SYN queue: becomes empty
```

---

### 2️⃣ Allocate Full struct sock

Kernel creates:

```
struct sock #C1
   state = TCP_ESTABLISHED
   receive buffer: empty
   send buffer: empty
```

---

### 3️⃣ Insert into tcp_ehash

```
tcp_ehash:
   (clientIP:clientPort → 8081) → struct sock #C1
```

Now data packets can be routed to this connection.

---

### 4️⃣ Insert into Accept Queue

```
FD 3 (TCP_LISTEN)
   ├── SYN queue: empty
   └── Accept queue:
         struct sock #C1 (ESTABLISHED)
```

---

# 🔔 epoll Reaction

Condition for OP_ACCEPT:

```
Accept queue non-empty
```

This just became true.

So kernel:

* Adds FD 3 to epoll ready list
* Wakes sleeping thread

---

# 🔎 SYSTEM STATE NOW

---

## 📂 FD Table

```
3 → listening socket
       Accept queue: 1 connection

4 → epoll
       Interest:
           FD 3
       Ready:
           FD 3
```

---

## 🌐 Kernel Tables

```
tcp_listen_hash:
   8081 → FD 3

tcp_ehash:
   connection → struct sock #C1
```

---

## 🧵 Thread

* epoll_wait returns 1
* selector.select() returns 1

---

Now your Java loop will process:

```java
key.isAcceptable()
```

And it will be TRUE.

---

Say **next** and we trace:

* server.accept()
* FD allocation for client
* updated fdtable
* accept queue removal
* exact state after accept()


Excellent. Now we execute:

```java
SocketChannel client = server.accept();
```

We go strictly step by step.

---

# STEP 8 — accept()

Current state before accept():

```
FD 3 (TCP_LISTEN)
   Accept queue:
       struct sock #C1

FD 4 (epoll)
   Ready:
       FD 3

tcp_ehash:
   connection → struct sock #C1
```

---

## 1️⃣ JVM Calls Native accept(fd=3)

Kernel performs:

### a) Remove connection from Accept queue

```
FD 3
   Accept queue: empty
```

---

### b) Allocate New FD for Client

Kernel finds next free slot in fdtable:

```
FD 5
```

Creates:

* struct file
* struct socket
* links it to existing `struct sock #C1`

Installs into fdtable.

---

# 🔎 SYSTEM STATE AFTER accept()

---

## 📂 FD Table

```
0 → stdin
1 → stdout
2 → stderr

3 → listening socket (TCP_LISTEN)
       SYN queue: empty
       Accept queue: empty

4 → struct eventpoll
       Interest:
           FD 3
       Ready:
           empty

5 → client socket (TCP_ESTABLISHED)
       receive buffer: empty
       send buffer: empty
```

---

## 🌐 Kernel Tables

```
tcp_listen_hash:
   8081 → FD 3

tcp_ehash:
   connection → struct sock #C1
```

Note:

* The struct sock was already in tcp_ehash before accept().
* accept() only creates the file descriptor wrapper.

---

## 🔔 epoll State

```
FD 4 → epoll
       Interest:
           FD 3
       Ready:
           empty   (FD 3 consumed)
```

---

## ☕ JVM Objects

```
ServerSocketChannel (fd=3)
Selector (fd=4)

SocketChannelImpl
   └── FileDescriptor(fd=5)
```

---

Now the next Java lines usually are:

```java
client.configureBlocking(false);
client.register(selector, OP_READ);
```

Say **next** and we trace:

* non-blocking change
* epoll_ctl for FD 5
* full updated system state
* ready to receive data


Good. Now we continue strictly.

---

# STEP 9 — `client.configureBlocking(false)`

Java executes:

```java
client.configureBlocking(false);
```

---

## 1️⃣ JVM Calls Native fcntl()

Kernel executes:

```
fcntl(fd=5, F_SETFL, O_NONBLOCK)
```

---

## 2️⃣ What Changes?

Only this:

```
struct file (FD 5) → f_flags |= O_NONBLOCK
```

Nothing else changes.

---

# 🔎 SYSTEM STATE NOW

---

## 📂 FD Table

```
0 → stdin
1 → stdout
2 → stderr

3 → listening socket (TCP_LISTEN)
       SYN queue: empty
       Accept queue: empty
       

4 → epoll
       Interest:
           FD 3
       Ready:
           empty

5 → client socket (TCP_ESTABLISHED)
       receive buffer: empty
       send buffer: empty
       file flags: O_NONBLOCK
```

---

## 🌐 Kernel Tables

```
tcp_listen_hash:
   8081 → FD 3

tcp_ehash:
   connection → struct sock #C1
```

---

Important:

* FD 5 is still NOT registered with epoll.
* If client sends data right now, selector will NOT wake up.

---

Next Java line:

```java
client.register(selector, SelectionKey.OP_READ);
```

Say **next** and we will update:

* epoll interest list
* full system state
* and then simulate client sending data.


Good. Now we register the client socket for reading.

---

# STEP 10 — `client.register(selector, OP_READ)`

Java:

```java
client.register(selector, SelectionKey.OP_READ);
```

---

## 1️⃣ JVM Creates SelectionKey

New Java object:

```
SelectionKeyImpl
   channel = SocketChannel(fd=5)
   selector = EPollSelector(fd=4)
   interestOps = OP_READ
```

---

## 2️⃣ JVM Calls Native epoll_ctl()

Kernel executes:

```
epoll_ctl(
    epfd = 4,
    EPOLL_CTL_ADD,
    fd = 5,
    EPOLLIN
)
```

Important meaning:

For a connected socket:

```
EPOLLIN ⇔ receive buffer non-empty
```

---

## 3️⃣ Kernel Updates epoll Interest Tree

Now epoll watches both sockets.

---

# 🔎 SYSTEM STATE NOW

---

## 📂 FD Table

```
0 → stdin
1 → stdout
2 → stderr

3 → listening socket (TCP_LISTEN)
       SYN queue: empty
       Accept queue: empty

4 → epoll
       Interest:
           FD 3   (accept readiness)
           FD 5   (read readiness)
       Ready:
           empty

5 → client socket (TCP_ESTABLISHED)
       receive buffer: empty
       send buffer: empty
       flags: O_NONBLOCK
```

---

## 🌐 Kernel Tables

```
tcp_listen_hash:
   8081 → FD 3

tcp_ehash:
   connection → struct sock #C1
```

---

Now the loop will call:

```java
selector.select();
```

Thread goes to sleep again.

---

Next, we simulate:

Client sends application data.

Say **next** and we’ll track:

* tcp_ehash lookup
* receive buffer update
* epoll ready list
* selector wakeup
* and the exact state before read().


Excellent. Now we simulate client sending application data.

---


## STEP 11 — Client Sends `"hello"`

Client sends:

```
PSH, ACK
payload = "hello"
```

---

## 1️⃣ NIC → TCP Stack

Kernel receives packet.

---

## 2️⃣ Lookup in tcp_ehash

Kernel uses 4-tuple:

```
(clientIP, clientPort, serverIP, 8081)
```

Finds:

```
struct sock #C1  (FD 5)
```

Listening socket (FD 3) is NOT involved.

---

## 3️⃣ Append Data to Receive Buffer

Kernel copies payload into:

```
struct sock #C1 → receive buffer
```

Now:

```
FD 5
   receive buffer: "hello"
```

---

## 4️⃣ epoll Condition Check

FD 5 is registered with EPOLLIN.

Condition for EPOLLIN (connected socket):

```
receive buffer non-empty
```

This is now TRUE.

---

## 5️⃣ Kernel Updates epoll Ready List

Inside `struct eventpoll` (FD 4):

```
Interest:
   FD 3
   FD 5

Ready:
   FD 5
```

---

## 6️⃣ Thread Wakeup

Thread sleeping inside:

```
epoll_wait(fd=4)
```

Kernel wakes thread.

`selector.select()` returns 1.

---

# 🔎 SYSTEM STATE BEFORE read()

---

## 📂 FD Table

```
3 → listening socket (TCP_LISTEN)

4 → epoll
       Interest:
           FD 3
           FD 5
       Ready:
           FD 5

5 → client socket (TCP_ESTABLISHED)
       receive buffer: "hello"
```

---

## 🌐 Kernel Tables

```
tcp_ehash:
   connection → struct sock #C1
```

---

Now Java loop will see:

```java
key.isReadable() == true
```

---

Next step in code:

```java
int bytes = client.read(buffer);
```

Say **next** and we trace:

* exact memory copy
* receive buffer after read
* epoll ready list behavior
* what happens if you do NOT drain fully.


Excellent. Now we execute:

```java
int bytes = client.read(buffer);
```

We go strictly step by step.

---

# STEP 12 — read()

Current state before read:

```
FD 5 → struct sock (TCP_ESTABLISHED)
        receive buffer: "hello"

FD 4 → epoll
        Ready:
            FD 5
```

---

## 1️⃣ JVM Calls Native read(fd=5)

Kernel performs:

```
copy_from_kernel(
    source: struct sock receive buffer
    dest:   ByteBuffer memory
)
```

Assume buffer has enough space.

---

## 2️⃣ Kernel Updates Receive Buffer

After copying 5 bytes:

```
struct sock receive buffer: empty
```

TCP state still ESTABLISHED.

---

## 3️⃣ What Happens to epoll Ready List?

We are using level-triggered behavior.

After read():

Condition for EPOLLIN:

```
receive buffer non-empty ?
```

Now it is FALSE.

So FD 5 will NOT be added to ready list again.

Ready list becomes empty after this select cycle completes.

---

# 🔎 SYSTEM STATE AFTER read()

---

## 📂 FD Table

```
3 → listening socket (TCP_LISTEN)

4 → epoll
       Interest:
           FD 3
           FD 5
       Ready:
           empty

5 → client socket (TCP_ESTABLISHED)
       receive buffer: empty
```

---

## 🌐 Kernel Tables

```
tcp_ehash:
   connection → struct sock #C1
```

---

Now the loop will call:

```java
selector.select();
```

Thread sleeps again.

---

# Critical Understanding Check

If instead of reading all bytes,
your buffer had capacity 2, and only 2 bytes were read:

What would happen?

Specifically:

* What remains in receive buffer?
* Will FD 5 stay ready?
* Will selector.select() immediately return again?

Think carefully and answer.


Correct. ✅
That is **exactly** how level-triggered readiness works.

Now let’s make it precise and strict.

---

# Scenario: Partial Read

Assume:

* Client sent `"hello"` (5 bytes)
* Your ByteBuffer capacity = 2

You call:

```java
client.read(buffer);
```

---

## 1️⃣ Kernel Copies Only 2 Bytes

From:

```
struct sock receive buffer: "hello"
```

After copy:

```
struct sock receive buffer: "llo"
```

Still 3 bytes remaining.

---

## 2️⃣ EPOLLIN Condition

For connected socket:

```
EPOLLIN ⇔ receive buffer non-empty
```

Receive buffer still contains `"llo"`.

So:

👉 Condition remains TRUE.

---

## 3️⃣ What Happens Next?

After your loop iteration completes and you call:

```java
selector.select();
```

Kernel checks:

```
Is FD 5 still ready?
```

Yes.

So:

* epoll_wait returns immediately
* selector.select() returns immediately
* key.isReadable() is TRUE again

---

# Final State

```
FD 5 → receive buffer: "llo"
FD 4 → epoll
        Ready:
            FD 5
```

This will continue until:

* You drain the receive buffer completely, OR
* The connection closes

---

# Why This Matters

This is why in C we often write:

```c
while (read(fd) != EAGAIN)
```

And in Java NIO, robust code often reads in a loop until:

```
read() returns 0
```

Because otherwise:

👉 You’ll keep getting readability events.

---

You now understand:

* Level-triggered behavior
* Why draining fully matters
* Why infinite CPU loops happen

---

Now let’s go one final step.

What happens when client sends FIN (closes connection)?

Specifically:

* What changes in struct sock?
* What does read() return?
* What happens in epoll ready list?

Think it through.

Good. Now we simulate client closing the connection properly.

We are currently in steady state:

```
FD 5 → TCP_ESTABLISHED
       receive buffer: empty

FD 4 → epoll
       Interest: FD 3, FD 5
       Ready: empty

tcp_ehash:
   connection → struct sock #C1
```

---

# STEP 13 — Client Sends FIN

Client closes connection.

It sends:

```
FIN, ACK
```

---

## 1️⃣ Kernel Processing

Packet arrives.

Lookup:

```
tcp_ehash → finds struct sock #C1
```

---

## 2️⃣ TCP State Change

Kernel:

* Marks connection as half-closed
* State becomes:

```
TCP_CLOSE_WAIT
```

Meaning:

> Peer closed its write side.

---

## 3️⃣ What Happens to Receive Buffer?

FIN is not application data.

Kernel sets an internal flag:

```
sk->sk_shutdown |= RCV_SHUTDOWN
```

Meaning:

* No more data will arrive.
* End-of-stream condition reached.

---

## 4️⃣ epoll Reaction

For a connected socket:

When peer sends FIN:

Condition becomes true for:

```
EPOLLIN
```

Because:

> read() will now return 0 (EOF)

So kernel adds FD 5 to epoll ready list.

---

# 🔎 SYSTEM STATE AFTER FIN ARRIVES

---

## 📂 FD Table

```
3 → listening socket

4 → epoll
       Interest:
           FD 3
           FD 5
       Ready:
           FD 5

5 → client socket
       state: TCP_CLOSE_WAIT
       receive buffer: empty
       shutdown flag: RCV_SHUTDOWN
```

---

## 🌐 Kernel Tables

```
tcp_ehash:
   connection → struct sock #C1
```

Connection still exists.

---

## 🧵 Thread

selector.select() returns.

key.isReadable() will be TRUE.

---

# STEP 14 — read() After FIN

Java executes:

```java
int bytes = client.read(buffer);
```

Kernel behavior:

* Sees RCV_SHUTDOWN flag
* No data in receive buffer
* Returns:

```
0  → in C
-1 → in Java NIO
```

Important distinction:

In Java NIO:

```
read() returns -1
```

Meaning:

> End of stream.

---

Now your Java code should:

```java
if (bytes == -1) {
    client.close();
}
```

---

# STEP 15 — client.close()

Now we trace what happens when you close FD 5.

Say **next**, and we’ll go into:

* FD table removal
* struct file destruction
* struct sock reference counting
* removal from tcp_ehash
* final TCP state transition to CLOSED.


Excellent. Now we close the connection properly.

We are currently:

```
FD 5 → TCP_CLOSE_WAIT
FD 4 → epoll (watching FD 3 and FD 5)
tcp_ehash contains struct sock #C1
```

Java executes:

```java
client.close();
```

---

# STEP 15 — close(fd=5)

---

## 1️⃣ JVM Calls Native close(5)

Kernel begins close process.

---

## 2️⃣ Remove From epoll Interest List

Because FD 5 is being closed:

Kernel automatically removes FD 5 from:

```
eventpoll → interest tree
```

So epoll now watches only FD 3.

---

## 3️⃣ Remove From Process FD Table

Kernel:

```
fdtable[5] = NULL
```

FD 5 is now free.

---

## 4️⃣ struct file Reference Count Drops to 0

Kernel destroys:

```
struct file
```

---

## 5️⃣ TCP State Transition

Because peer already sent FIN:

We were in:

```
TCP_CLOSE_WAIT
```

When application calls close():

Kernel sends:

```
FIN
```

Connection enters:

```
TCP_LAST_ACK
```

After peer ACKs:

```
TCP_CLOSED
```

---

## 6️⃣ Remove From tcp_ehash

When fully closed:

```
struct sock #C1 removed from tcp_ehash
struct sock memory freed
```

---

# 🔎 SYSTEM STATE AFTER close()

---

## 📂 FD Table

```
0 → stdin
1 → stdout
2 → stderr

3 → listening socket (TCP_LISTEN)

4 → epoll
       Interest:
           FD 3
       Ready:
           empty
```

FD 5 no longer exists.

---

## 🌐 Kernel Tables

```
tcp_listen_hash:
   8081 → FD 3

tcp_ehash:
   empty
```

Connection is fully gone.

---

# 🔁 Server Is Back To Steady State

Ready to accept new connections.

Thread returns to:

```java
selector.select();
```

Sleeping until next event.

---

# 🔥 You Now Have The Full Mental Model

From:

* JVM process start
* socket creation
* bind + listen
* epoll creation
* register
* SYN
* SYN-ACK
* ACK
* accept queue
* FD allocation
* read
* partial read behavior
* FIN
* close
* tcp_ehash removal

All tracked through:

* fdtable
* tcp_listen_hash
* tcp_ehash
* SYN queue
* accept queue
* epoll interest list
* epoll ready list

---

If you want, next we can:

* Simulate 10k concurrent clients
* Add backpressure (slow consumer case)
* Or compare this entire flow with Netty’s architecture

Your call.
