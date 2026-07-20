# GATE Points ⭐

## Must Remember

- A **process** is a **resource owner**.
- A **thread** is the **smallest unit of CPU execution (or scheduling)**.
- Every process has **at least one thread**.
- A process can contain **multiple threads**.

---

## Shared by Threads

Threads of the same process share:

- Address Space
- Code Segment (Text)
- Heap
- Global Variables
- Open Files

---

## Private to Each Thread

Each thread has its own:

- Program Counter (PC)
- Registers
- Stack
- Thread State
- Thread ID (TID)

---

## Context Switching

### Process Context Switch

- Switches PCB
- Switches Address Space
- Switches Page Tables
- Higher overhead

### Thread Context Switch

- Switches TCB (or thread-specific context)
- Switches Registers
- Switches Stack Pointer
- Lower overhead
- Shared address space remains unchanged

---

## Communication

### Processes

- Require **IPC**
- Higher communication overhead

### Threads

- Communicate through **shared memory**
- Faster communication

---

## Performance

| Operation | Faster |
|-----------|---------|
| Creation | Thread |
| Termination | Thread |
| Context Switch | Thread |
| Communication | Thread |

---

## Isolation

### Process

- Better fault isolation
- One process crash usually does **not** affect others

### Thread

- Lower fault isolation
- One faulty thread can crash the entire process

---

# Frequently Asked GATE Concepts

✅ Process = Resource Owner

✅ Thread = Execution Unit

✅ CPU executes **threads**, not processes directly.

✅ Threads share memory but have independent execution states.

✅ Thread context switching is cheaper than process context switching.

✅ Processes communicate using IPC.

✅ Threads communicate using shared memory.

---

# Common GATE Traps ⚠️

❌ Threads have separate address spaces.

✔ Threads share the same address space.

---

❌ Each thread has its own heap.

✔ Heap belongs to the process and is shared.

---

❌ CPU schedules processes only.

✔ Modern operating systems schedule **threads**.

---

❌ Processes can directly access each other's memory.

✔ Processes require IPC to communicate.

---

❌ Thread context switching changes the address space.

✔ Threads of the same process already share the same address space.

---

# PYQ Focus 🎯

Practice questions on:

- Process vs Thread comparison
- Shared vs Private resources
- Context switch overhead
- Process vs Thread communication
- Thread scheduling basics
- Advantages and disadvantages of multithreading