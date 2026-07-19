# Module 2 — Parent & Child Process

## Definition

> A **Parent Process** is a process that creates another process.

> The newly created process is called a **Child Process**.

In Unix/Linux, a child process is typically created using the **`fork()`** system call.

---

# Intuition

Think of a family tree.

```text
Parent
   │
   ├── Child 1
   ├── Child 2
   └── Child 3
```

Similarly,

```text
Process P1
      │
      ├── Process P2
      ├── Process P3
      └── Process P4
```

P1 is the **Parent Process**.

P2, P3, and P4 are **Child Processes**.

---

# How is a Child Created?

```text
Parent Process
       │
   fork()
       │
       ▼
Child Process
```

After `fork()`, **both parent and child execute independently**.

---

# Relationship

```text
Parent
      │
      ▼
Creates Child
      │
      ▼
Child Executes
```

A parent can create:

- One child
- Multiple children

A child can also become a parent.

```text
P1
│
├── P2
│   ├── P5
│   └── P6
│
└── P3
```

---

# Parent and Child Characteristics

## Parent Process

- Creates child process.
- Has its own PID.
- Can wait for the child to finish.
- Can create multiple children.

---

## Child Process

- Has its own unique PID.
- Executes independently.
- Has its own PCB.
- Has its own memory space.
- Can create more child processes.

---

# Resource Relationship

The child initially inherits many attributes from the parent:

- Open file descriptors
- Environment variables
- Current working directory
- User credentials

However, the child gets:

- New PID
- New PCB
- Separate Address Space

---

# Process Tree

Example:

```text
init (PID 1)
│
├── bash
│   ├── vim
│   ├── gcc
│   └── python
│
└── ssh
```

Every process (except the first one) has a parent.

---

# Process Hierarchy

```text
Parent
   │
fork()
   │
   ▼
Child
   │
fork()
   │
   ▼
Grandchild
```

Processes form a **tree structure**.

---

# Parent Waiting for Child

The parent may wait until the child finishes.

```text
Parent
    │
wait()
    │
Child finishes
```

This prevents Zombie Processes.

---

# Key Points

- Parent creates child using `fork()`.
- Every child has exactly one parent (at creation).
- Parent and child have different PIDs.
- Both execute concurrently after `fork()`.
- Child gets its own PCB and memory space.
- Child may create more child processes.

---

# 🎯 GATE Corner

## Must Remember ⭐⭐⭐

- Child is created using **fork()**.
- Parent and child have different PIDs.
- Parent and child execute independently.
- Child has its own PCB and address space.
- Parent can synchronize using **wait()**.

---

## GATE Tricks ⚠️

### ❌ Wrong Statement

> Parent and child share the same PID.

**False**

Each process has a unique PID.

---

### ❌ Wrong Statement

> Parent stops executing after creating a child.

**False**

Both parent and child execute concurrently.

---

### ❌ Wrong Statement

> Parent and child share the same PCB.

**False**

Every process has its own PCB.

---

### ❌ Wrong Statement

> A child cannot create another child.

**False**

A child can also become a parent.

---

### ✅ Correct Statement

> Parent and child execute independently after `fork()`.

---

# Common MCQs

### Q1

Which system call creates a child process?

A. exec()

B. fork()

C. wait()

D. exit()

✅ **Answer:** B

---

### Q2

Parent and child processes have

A. Same PID

B. Same PCB

C. Different PIDs

D. Same Address Space

✅ **Answer:** C

---

### Q3

After `fork()`, parent and child

A. Execute one after another

B. Execute independently

C. Parent terminates

D. Child terminates immediately

✅ **Answer:** B

---

### Q4

Which of the following is inherited by the child?

A. Open file descriptors

B. Environment variables

C. Working directory

D. All of the above

✅ **Answer:** D

---

# Formula Corner 🧮

No mathematical formulas.

Remember the relationship:

```text
Parent
   │
fork()
   │
   ▼
Child

Parent PID ≠ Child PID

Parent PCB ≠ Child PCB

Parent Memory ≠ Child Memory
```

---

# 10-Second Revision

- Parent creates child using `fork()`.
- Child gets a new PID and PCB.
- Parent and child run independently.
- Parent may call `wait()`.
- Child can create more child processes.
- Processes form a tree.