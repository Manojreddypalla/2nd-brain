# GATE Focus ⭐⭐⭐⭐⭐
# Inter Process Communication (IPC)

---

# 1. Why IPC is Needed

Processes execute in **separate address spaces** due to process isolation.

Therefore, one process **cannot directly access** another process's memory.

IPC provides a safe mechanism for processes to:

- Exchange data
- Share resources
- Synchronize execution
- Coordinate tasks

### Examples

- Browser ↔ Network Process
- Producer ↔ Consumer
- Parent ↔ Child Process
- Client ↔ Server

> **Remember:** Process Isolation is the reason IPC exists.

---

# 2. Process Isolation

Each process has its own:

- Code
- Data
- Heap
- Stack

```text
Process A

Memory A

❌ Cannot Access

Memory B

Process B
```

The OS prevents direct access for:

- Security
- Stability
- Reliability

IPC is the controlled way to communicate.

---

# 3. IPC Mechanisms

| IPC Mechanism | Best Use |
|--------------|----------|
| Pipe | Related Processes |
| Named Pipe | Unrelated Processes |
| Message Queue | Structured Messages |
| Shared Memory | High-Speed Communication |
| Signal | Notification |
| Socket | Network Communication |

---

# 4. Shared Memory vs Message Passing

| Shared Memory | Message Passing |
|---------------|-----------------|
| Fastest IPC | Slower |
| Shared Memory Region | Messages |
| No Data Copy | Data Copy Required |
| Synchronization Required | Synchronization Mostly Managed by OS |
| Same Machine | Local or Distributed Systems |
| Programmer Manages Access | OS Manages Message Transfer |

### GATE Facts ⭐

- Fastest IPC → Shared Memory
- Safest IPC → Message Passing
- Shared Memory needs synchronization.
- Message Passing avoids race conditions on shared data.

---

# 5. Pipe vs Named Pipe

| Pipe | Named Pipe |
|------|------------|
| Anonymous | Named (FIFO File) |
| Parent-Child Processes | Unrelated Processes |
| Temporary | Exists in File System |
| One-Way | One-Way |
| Same Machine | Same Machine |

### Linux Calls

Pipe

```
pipe()
```

Named Pipe

```
mkfifo()
```

---

# 6. Shared Memory vs Pipe

| Shared Memory | Pipe |
|--------------|------|
| Memory Sharing | Byte Stream |
| Fast | Slower |
| Random Access | Sequential Access |
| Synchronization Required | Kernel Synchronizes Pipe Buffer |

---

# 7. Message Queue vs Pipe

| Pipe | Message Queue |
|------|---------------|
| Byte Stream | Message-Based |
| No Message Boundaries | Preserves Messages |
| Sequential | Priority/Typed Messages Possible |

---

# 8. Socket vs Other IPC

| Socket | Others |
|--------|--------|
| Works Across Networks | Mostly Local Machine |
| Client-Server Model | Local IPC |
| Bidirectional | Depends on Mechanism |

---

# 9. Direct vs Indirect Communication

## Direct

```
Process A

↓

Process B
```

- Sender knows Receiver.
- Receiver knows Sender.
- Tightly Coupled.

---

## Indirect

```
Process A

↓

Mailbox

↓

Process B
```

- Communication through Mailbox/Port.
- Processes need not know each other.
- Loosely Coupled.

---

# 10. Blocking vs Non-Blocking Communication

## Blocking

Process waits.

```
Send

↓

Wait

↓

Continue
```

OR

```
Receive

↓

Wait

↓

Message Arrives

↓

Continue
```

---

## Non-Blocking

Process never waits.

```
Send

↓

Continue
```

or

```
Receive

↓

No Message

↓

Return Immediately
```

---

# 11. Buffering

## Zero Capacity

Buffer Size = 0

```
Sender

↓

Receiver
```

Sender waits every time.

---

## Bounded Buffer

Fixed Size Buffer.

```
Sender

↓

Buffer

↓

Receiver
```

Sender waits only if full.

---

## Unbounded Buffer

Infinite (Logical) Buffer.

Sender never waits because the buffer is full.

Receiver waits only when empty.

---

# 12. Synchronization in Shared Memory

Since multiple processes access the same memory,

Problems:

- Race Condition
- Lost Update
- Data Inconsistency

Solutions:

- Semaphore
- Mutex
- Monitor
- Spinlock

### GATE Point ⭐

Shared Memory **does not provide synchronization**.

Synchronization must be implemented separately.

---

# 13. Signals

Purpose:

Notification.

NOT data transfer.

Examples:

- Ctrl + C → SIGINT
- kill → SIGTERM
- SIGSTOP
- SIGCONT

---

# 14. Sockets

Purpose:

Communication between processes on:

- Same Machine
- Different Machines

Supports:

- TCP
- UDP

Client

```
socket()

↓

connect()
```

Server

```
socket()

↓

bind()

↓

listen()

↓

accept()
```

---

# 15. Linux IPC System Calls

## Pipe

```
pipe()
```

Creates anonymous pipe.

---

## Named Pipe

```
mkfifo()
```

Creates FIFO file.

---

## Shared Memory

```
shmget()
```

Create/Get segment.

```
shmat()
```

Attach.

```
shmdt()
```

Detach.

```
shmctl()
```

Control/Delete.

---

## Message Queue

```
msgget()
```

Create queue.

```
msgsnd()
```

Send message.

```
msgrcv()
```

Receive message.

```
msgctl()
```

Control/Delete queue.

---

## Socket

```
socket()
```

Create socket.

```
bind()
```

Assign address.

```
listen()
```

Wait for clients.

```
accept()
```

Accept connection.

```
connect()
```

Connect to server.

```
send()
```

Send data.

```
recv()
```

Receive data.

```
close()
```

Close socket.

---

## Signals

```
kill()
```

Send signal.

```
signal()
```

Simple handler.

```
sigaction()
```

Advanced handler.

```
raise()
```

Signal current process.

---

# 16. Advantages & Disadvantages

| Mechanism | Advantage | Disadvantage |
|-----------|-----------|--------------|
| Pipe | Simple | Related Processes |
| Named Pipe | Unrelated Processes | Local Only |
| Shared Memory | Fastest | Needs Synchronization |
| Message Queue | Structured Communication | Slower |
| Signal | Very Fast Notification | No Data Transfer |
| Socket | Remote Communication | More Complex |

---

# 17. Frequently Asked GATE Questions ⭐

✔ Why is IPC required?

✔ Which IPC mechanism is the fastest?

✔ Which IPC mechanism requires synchronization?

✔ Difference between Shared Memory and Message Passing.

✔ Difference between Pipe and Named Pipe.

✔ Difference between Blocking and Non-Blocking.

✔ Difference between Direct and Indirect Communication.

✔ Buffering types.

✔ Client-Server socket workflow.

✔ Purpose of common Linux IPC system calls.

✔ Which IPC mechanism can communicate across different machines?

✔ Which IPC mechanism is only for notifications?

---

# 18. Ultimate One-Page Revision ⭐⭐⭐

| Topic | Remember |
|--------|----------|
| Why IPC | Process Isolation |
| Fastest IPC | Shared Memory |
| Needs Synchronization | Shared Memory |
| Notification Only | Signals |
| Remote IPC | Sockets |
| Related Processes | Pipe |
| Unrelated Processes | Named Pipe |
| Structured Communication | Message Queue |
| Shared Memory | No Data Copy |
| Message Passing | Data Copy |
| Blocking | Wait |
| Non-Blocking | No Wait |
| Direct | Process ↔ Process |
| Indirect | Process ↔ Mailbox ↔ Process |
| Zero Buffer | Always Wait |
| Bounded Buffer | Wait If Full |
| Unbounded Buffer | Sender Never Waits (due to full buffer) |
| `pipe()` | Anonymous Pipe |
| `mkfifo()` | Named Pipe |
| `shmget()` | Create Shared Memory |
| `msgget()` | Create Message Queue |
| `socket()` | Create Socket |
| `kill()` | Send Signal |