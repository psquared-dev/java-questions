# UDP: What Really Happens

## 1. Client sends a UDP packet

* The client constructs a UDP datagram.
* It sends it to server’s IP + port.

## 2. Server’s OS receives the packet

Your server process is not involved yet.

The OS kernel does this:

* Finds the matching socket (by port)
* Appends the packet to the socket’s UDP receive queue

Think of it like a mailbox:

```text
UDP Receive Queue (Kernel Memory)
+--------+--------+--------+
| pkt#1  | pkt#2  | pkt#3  |
+--------+--------+--------+
```

## 3. `socket.receive(packet)` is BLOCKING

When your Java thread calls:

```java
socket.receive(packet);
```
It blocks until:

* the queue contains at least one message.

Then the kernel gives the FIRST packet in the queue to Java.


## 4. What if another UDP packet arrives WHILE I'm processing the first one?

The new packet is simply placed in the UDP receive queue.

For example:

```text
Time T1: packet #1 arrives → queue = [1]
Time T2: your thread receives packet #1 → queue = []
Time T3: packet #2 arrives → queue = [2]
Time T4: packet #3 arrives → queue = [2,3]
Time T5: your code is still processing packet #1
```

Nothing is lost.

When your code calls `socket.receive()` again:

* It immediately gets packet #2 (no blocking)
* Next call gets packet #3


## 5. UDP is message-based (not stream-based)

Each UDP packet stays intact.

* You always receive entire packets.
* Not partial bytes.
* Not mixed packets.

This is different from TCP.

## 6. What happens if packets arrive faster than you process?

The OS queue grows.

**If the queue gets full → newer packets are DROPPED.**

* UDP does not backpressure.
* UDP does not guarantee delivery.
* UDP does not reorder