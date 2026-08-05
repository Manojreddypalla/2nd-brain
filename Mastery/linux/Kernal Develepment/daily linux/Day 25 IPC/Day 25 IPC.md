Yep. Here’s a **short-note version that still explains what’s happening internally**, so it’s useful for Linux internals rather than just memorizing definitions.

# 🐧 Linux Internals — Day 25: IPC

## 1. What is IPC?

**IPC = Inter-Process Communication**

Each Linux process normally has its **own virtual address space**.

```text
Process A Memory          Process B Memory
┌──────────────┐          ┌──────────────┐
│ Code         │          │ Code         │
│ Heap         │    ✗     │ Heap         │
│ Stack        │ <------> │ Stack        │
└──────────────┘          └──────────────┘
```

Process A cannot simply read/write Process B's memory.

So Linux provides **IPC mechanisms** that allow processes to:

- exchange data
    
- send events/messages
    
- synchronize their execution
    
- coordinate access to shared resources
    

The **kernel usually acts as the mediator**.

---

# 2. Pipe `|`

A **pipe** is a kernel-managed byte stream connecting processes.

```text
Process A
   │ write()
   ▼
┌─────────────┐
│ Kernel Pipe │
│   Buffer    │
└─────────────┘
   │ read()
   ▼
Process B
```

Example:

```bash
ls | wc -l
```

The shell:

1. creates a pipe
    
2. connects `ls` stdout → pipe
    
3. connects pipe → `wc` stdin
    

### Key Point

Pipes are commonly used between **related processes**, such as parent/child processes.

They are usually **one-way**.

---

# 3. Named Pipe — FIFO

A **FIFO** works like a pipe but has a **name in the filesystem**.

```bash
mkfifo mypipe
```

Then:

```text
Process A → mypipe → Process B
```

Because processes can open the FIFO by its pathname, they **don't need a parent-child relationship**.

### Pipe vs FIFO

|Pipe|FIFO|
|---|---|
|No filesystem name|Has filesystem name|
|Usually related processes|Can connect unrelated processes|
|Temporary|Exists until removed|

---

# 4. Shared Memory

Shared memory maps the **same memory region into multiple processes**.

```text
Process A             Process B
    │                     │
    └──────┐       ┌──────┘
           ▼       ▼
       ┌─────────────┐
       │ Shared RAM  │
       └─────────────┘
```

Instead of:

```text
A → Kernel → B
```

both processes directly access:

```text
A ─┐
   ├→ Shared Memory
B ─┘
```

### Why is it fast?

After setup, processes can access the shared pages directly instead of repeatedly transferring data through kernel buffers.

⚠️ But now both processes might modify the same data simultaneously.

That creates **race conditions**.

Therefore:

```text
Shared Memory + Semaphore/Mutex
```

are commonly used together.

---

# 5. Message Queue

A message queue is a **kernel-managed queue of messages**.

```text
Process A
   │
   ▼
┌───────────────┐
│ Message 1     │
│ Message 2     │
│ Message 3     │
└───────────────┘
   │
   ▼
Process B
```

Unlike pipes, communication is naturally organized as **separate messages**, rather than just a stream of bytes.

Useful when applications want:

```text
send(message)
receive(message)
```

instead of processing a raw byte stream.

---

# 6. Semaphore

A semaphore is mainly for **synchronization**, not transferring application data.

Imagine two processes accessing shared memory:

```text
Process A ─┐
           ├── Shared Memory
Process B ─┘
```

If both modify it simultaneously → 💥 **race condition**.

A semaphore controls access:

```text
Process A
   │
   ▼
 acquire
   │
   ▼
Shared Resource
   │
 release
   ▼
Process B can enter
```

Mental model:

> 🚦 Semaphore = traffic signal for processes/threads.

---

# 7. UNIX Domain Socket

A **UNIX domain socket** allows client-server communication between processes on the **same machine**.

```text
Client Process
      │
      ▼
 UNIX Socket
      │
      ▼
Server Process
```

Unlike network sockets, it doesn't need normal IP-based network communication.

Common pattern:

```text
Application → UNIX Socket → Local Service
```

Used heavily by local Linux services and daemons.

---

# 🧠 IPC Big Picture

Think about IPC as solving **two different problems**:

```text
                 IPC
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
   Transfer Data        Synchronize
        │                   │
   Pipe                    Semaphore
   FIFO
   Shared Memory
   Message Queue
   UNIX Socket
```

And remember the important connection:

```text
Separate Process Memory
        ↓
Need IPC
        ↓
Shared Memory gives speed
        ↓
Sharing creates race conditions
        ↓
Semaphore provides synchronization
```

That's the chain worth understanding.

## 🔧 Commands

```bash
echo "Hello Linux" | cat

mkfifo mypipe

ipcs

ipcrm --help

ls | wc -l
```

**`ipcs`** → shows System V IPC objects such as shared memory, message queues, and semaphores.

**`ipcrm`** → removes IPC resources.

### ⚡ 30-second revision

**Pipe** → related processes, byte stream  
**FIFO** → named pipe, unrelated processes  
**Shared Memory** → fastest bulk sharing, same memory pages  
**Message Queue** → structured messages  
**Semaphore** → synchronization/access control  
**UNIX Socket** → local client-server IPC