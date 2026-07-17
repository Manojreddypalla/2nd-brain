# 1. Program vs Process (GATE OS)

This is **the first concept of Operating Systems**, and almost everything else builds on it.

---

# Intuition

Imagine you have a recipe book.

The recipe for making pizza is printed on paper.

Can you eat the recipe?

No.

You must **start cooking**.

The moment you begin cooking, ingredients are gathered, utensils are used, the oven is turned on, and the cooking process begins.

Here:

- **Recipe = Program**
    
- **Cooking = Process**
    

The recipe is passive.

Cooking is active.

---

# Real Computer Example

Suppose you have

```
chrome.exe
```

stored on your SSD.

It is just a file.

Nothing is happening.

```
Disk

chrome.exe
```

This is a **Program**.

Now double-click it.

Immediately the OS starts doing many things.

- Creates memory
    
- Gives CPU time
    
- Opens files
    
- Creates threads
    
- Allocates stack
    
- Allocates heap
    

Now Chrome is running.

This running Chrome is a **Process**.

---

# Definition

## Program

A **Program** is a passive collection of instructions stored on secondary storage.

Examples

```
chrome.exe

python.exe

game.exe

hello.out
```

Programs are simply executable files.

---

## Process

A **Process** is a program that is currently executing.

It contains

- instructions
    
- current execution state
    
- memory
    
- CPU registers
    
- open files
    
- stack
    
- heap
    
- program counter
    

A process is therefore much more than the executable file.

---

# Visualize

Before execution

```
SSD

-------------------
Chrome.exe
VSCode.exe
Game.exe
-------------------
```

Nothing is running.

---

After execution

```
RAM

+----------------------+
Chrome Process
+----------------------+

Code
Data
Heap
Stack
Registers
Program Counter

+----------------------+
```

Notice

The **Program stays on disk.**

The **Process lives in RAM.**

---

# Think Like the OS

Suppose you click Chrome.

The OS performs roughly this sequence:

```
User

↓

Double Click

↓

Operating System

↓

Create Process

↓

Allocate Memory

↓

Load Program

↓

Initialize Registers

↓

Create Stack

↓

Create Heap

↓

Program Counter = main()

↓

Give CPU

↓

Execution Starts
```

Now it becomes a process.

---

# What Does a Process Contain?

A running process contains many things.

```
Process

│

├── Program Code

├── Data

├── Heap

├── Stack

├── Registers

├── Program Counter

├── Open Files

├── PID

├── Priority

├── Process State
```

Notice something important.

The executable file alone does **not** contain:

- current function
    
- local variables
    
- register values
    
- current instruction
    
- open files
    

Those belong to the process.

---

# Memory Layout

A running process typically looks like this:

```
High Address
+-------------------+
| Stack             |
| function calls    |
| local variables   |
+-------------------+

|                   |

| Free Space        |

|                   |

+-------------------+
| Heap              |
| malloc/new        |
+-------------------+

| Initialized Data  |
+-------------------+

| Global Variables  |
+-------------------+

| Code(Text)        |
+-------------------+

Low Address
```

The **program** is just the code stored on disk.

The **process** includes this entire memory image while it runs.

---

# One Program → Multiple Processes

A single program can have many processes.

Example

```
chrome.exe
```

You open Chrome.

```
Chrome Process 1
```

Open another Chrome profile.

```
Chrome Process 2
```

Open Incognito.

```
Chrome Process 3
```

Same executable.

Different processes.

Each has

- different PID
    
- different stack
    
- different heap
    
- different registers
    
- different execution state
    

---

# Multiple Processes Example

```
Program

python.exe

↓

Process A

PID = 101

↓

Process B

PID = 215

↓

Process C

PID = 320
```

One program can create many independent processes.

---

# Everyday Analogy

Think of a movie.

Movie file

```
Avengers.mp4
```

is stored on your SSD.

That is the **Program**.

When VLC starts playing it,

- decoder works
    
- audio buffer fills
    
- video buffer fills
    
- playback position changes
    

Everything happening while the movie plays is like a **Process**.

---

# Program vs Process

|Feature|Program|Process|
|---|---|---|
|Nature|Passive|Active|
|Stored in|Secondary Storage (Disk/SSD)|Main Memory (RAM)|
|State|No execution state|Has execution state|
|CPU|Does not use CPU|Uses CPU|
|Memory|Static file|Dynamic memory allocation|
|Lifetime|Exists until deleted|Exists until terminated|
|Multiple copies|One file|Many processes possible|

---

# GATE Traps

### Trap 1

> A program is an active entity.

❌ False

Program is passive.

---

### Trap 2

> A process is stored on disk.

❌ False

The executable is stored on disk.

The process exists primarily in RAM while executing.

---

### Trap 3

> Multiple processes cannot execute the same program.

❌ False

One executable can have many running processes.

---

### Trap 4

> A process contains only instructions.

❌ False

A process contains:

- code
    
- data
    
- stack
    
- heap
    
- registers
    
- program counter
    
- open files
    
- process state
    

---

# GATE Corner 📝

Remember these one-liners:

- **Program = Passive executable stored on disk**
    
- **Process = Active instance of a program**
    
- **Program → Process** when execution begins
    
- **One program can create multiple processes**
    
- **A process has its own execution context (registers, stack, heap, program counter, etc.)**
    
- **Processes are managed by the Operating System**
    

---

## What's Next?

Now that we know **what a process is**, the natural question is:

> **If many processes exist at the same time, what stages does each process go through while it's running?**

That's exactly what we'll study next: **2. Process States**.