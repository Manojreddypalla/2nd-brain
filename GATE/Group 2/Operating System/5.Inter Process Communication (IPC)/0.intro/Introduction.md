# Inter Process Communication (IPC)

## What is IPC?

**Inter Process Communication (IPC)** is a collection of mechanisms that allow **two or more processes to communicate, exchange data, and synchronize their execution**.

Without IPC, each process runs independently in its own memory space and cannot directly access another process's data.

---

## Why IPC?

Modern operating systems rarely run just one program.

For example:

- Your web browser downloads data while displaying a webpage.
- A music player reads audio files while playing music.
- A database server communicates with multiple client applications.
- A chat application exchanges messages with another computer over the network.

In all these cases, multiple processes need to **share information** and **coordinate their activities**.

This communication is made possible using **Inter Process Communication (IPC)**.

---

## Why Can't Processes Communicate Directly?

Operating systems provide **Process Isolation**.

Each process has its own:

- Address Space
- Variables
- Stack
- Heap
- Registers

This isolation improves:

- Security
- Stability
- Reliability

However, because of this isolation, one process **cannot directly access another process's memory**.

Therefore, special IPC mechanisms are required.

---

## Need for IPC

IPC is used to:

- Exchange data between processes.
- Share resources efficiently.
- Coordinate process execution.
- Synchronize concurrent processes.
- Improve system performance.
- Support client-server communication.

---

## Communication Models

Operating systems mainly provide two communication models:

### 1. Shared Memory

- Multiple processes share the same memory region.
- Processes communicate by reading and writing shared data.
- Fast because data is not copied between processes.
- Requires synchronization mechanisms (e.g., Semaphores, Mutexes).

---

### 2. Message Passing

- Processes communicate by sending and receiving messages.
- The operating system transfers the data.
- Easier to implement than shared memory.
- More suitable for distributed systems.

---

## Common IPC Mechanisms

- Shared Memory
- Message Passing
- Pipes
- Named Pipes (FIFO)
- Message Queues
- Shared Memory Segments
- Signals
- Sockets

---

## Real-Life Analogy

Imagine two people working in separate rooms.

Without IPC:

- They cannot communicate because the rooms are isolated.

With IPC:

They can communicate using different methods:

- 📄 Passing notes → Message Passing
- 📦 Shared whiteboard → Shared Memory
- ☎ Telephone → Sockets
- 🔔 Doorbell → Signals
- 📮 Mailbox → Message Queue
- 🚰 Water Pipe → Pipe

Different communication methods are suitable for different situations.

---

## GATE Corner ⭐

Remember the flow:

```text
Process Isolation
        ↓
Need to Share Data
        ↓
Inter Process Communication (IPC)
        ↓
Communication Models
   ├── Shared Memory
   └── Message Passing
        ↓
IPC Mechanisms
   ├── Pipes
   ├── Named Pipes
   ├── Message Queues
   ├── Shared Memory
   ├── Signals
   └── Sockets
```

---

## One-Line Summary ⭐

> **IPC is a set of operating system mechanisms that enables isolated processes to communicate, exchange data, and synchronize their execution safely and efficiently.**