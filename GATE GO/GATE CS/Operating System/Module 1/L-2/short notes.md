Yep. For **handwritten notes**, don’t copy the giant lecture notes I gave earlier. You want **compact GATE revision notes**: definition → key idea → diagram → important exam points.

For this Lecture 2, write it like this:

# Lecture 2 — Process Fundamentals

## 1. Compilation Process

A **source program** must be converted into an executable before it can run.

```text
Source Code (.c)
      ↓
Preprocessor
      ↓
Compiler
      ↓
Assembly Code (.s)
      ↓
Assembler
      ↓
Object Code (.o)
      ↓
Linker
      ↓
Executable (a.out)
```

### Important

- **Compiler** converts source code into lower-level code.
    
- **Assembler** converts assembly code into object/machine code.
    
- **Linker** combines object files + required libraries.
    
- Final result → **Executable file**.
    

```text
Program → Compile → Executable → Load into RAM → Execute
```

---

# 2. Logical vs Physical Address

## Logical Address

Address generated/used from the **process's point of view**.

Also called:

```text
Logical Address ≈ Virtual Address
```

## Physical Address

Actual location of data in **main memory (RAM)**.

Example:

```text
Process Address Space          RAM

Logical Address 1000
        │
        └──────────────→ Physical Address 2000
```

Therefore:

```text
Logical Address ≠ Physical Address
```

### Important

Each process gets its **own logical address space**.

Two processes can have:

```text
Same Logical Address
but
Different Physical Addresses
```

---

# 3. Process

### Definition ⭐

> **Process = A program in execution.**

```text
Program
(passive)
   ↓ Load + Execute
Process
(active)
```

Example:

```text
a.out on disk → Program

./a.out running → Process
```

A process competes for resources such as:

```text
CPU
Memory
I/O
Files
```

---

# 4. Process Context

### Definition ⭐

> **Process context = Information required to resume the execution of a process later.**

Process context includes:

```text
• Program Counter (PC)
• CPU Registers
• Stack contents
• Memory information
• Open files
```

### Why?

If CPU changes from:

```text
P1 → P2
```

OS must save P1's current execution information so P1 can continue later.

---

# 5. PCB — Process Control Block

### Definition

**PCB is an OS data structure used to store information about a process.**

Contains:

```text
PCB
├── Process ID (PID)
├── Process State
├── Program Counter
├── CPU Registers
├── Scheduling Information
├── Memory Information
└── Open Files
```

### Key idea

```text
Process stops
     ↓
Context saved in PCB
     ↓
Another process runs
     ↓
Context restored later
     ↓
Process continues
```

⭐ **PCB stores process context.**

---

# 6. `fork()` System Call

### Definition ⭐

> `fork()` creates a new process called the **child process**.

```text
Before fork()

      Parent

         │
       fork()
         ↓

After fork()

     ┌────────┐
     │ Parent │
     └────────┘

     ┌────────┐
     │ Child  │
     └────────┘
```

### Most Important Rule ⭐⭐⭐

After `fork()`:

> **Parent and child both continue execution from the instruction immediately after `fork()`.**

Example:

```c
fork();
printf("Hello");
```

After `fork()` there are 2 processes.

Therefore:

```text
Parent → Hello
Child  → Hello

Total = 2 times
```

---

# 7. Return Value of `fork()`

Write this in a box:

```text
        fork()
          │
    ┌─────┴─────┐
    ↓           ↓
 Parent       Child

Child  → returns 0
Parent → returns Child PID (>0)
```

Therefore:

```c
int pid = fork();

if (pid == 0)
{
    // Child
}
else
{
    // Parent
}
```

⭐ **Child = 0, Parent = Child PID**

---

# 8. Memory After `fork()`

After `fork()`, parent and child are **separate processes**.

Example:

```c
int a = 10;

fork();
```

Conceptually:

```text
Parent              Child

a = 10              a = 10

Logical             Logical
Address 1000        Address 1000
    ↓                   ↓
Physical X          Physical Y
```

Therefore:

> **Same logical address does NOT imply same physical address.**

Also:

```text
Changing Parent's a
does NOT change Child's a

Changing Child's a
does NOT change Parent's a
```

---

# 9. `exec()` System Call

### Definition ⭐

> `exec()` replaces the current process's program with a new program.

Example:

```text
Process running A
      │
   exec(B)
      ↓
Process now running B
```

Think:

```text
fork() → CREATE
exec() → REPLACE
```

### Very Important ⭐

If `exec()` succeeds:

```c
printf("A");

exec(B);

printf("C");
```

Execution becomes:

```text
Print A
   ↓
exec(B)
   ↓
B executes
```

`printf("C")` **will not execute**.

Because the old program was replaced.

---

# 10. `fork()` vs `exec()`

Draw this small table:

|`fork()`|`exec()`|
|---|---|
|Creates new process|Replaces program|
|Parent + Child|Same process|
|Process count increases|Process count doesn't increase|
|Execution continues after `fork()`|Old program replaced|

### Memory trick

```text
fork = CREATE
exec = REPLACE
```

---

# 11. Process Creation Using `fork()` + `exec()`

This is the big concept of the lecture.

Suppose in Linux terminal:

```bash
./a.out
```

The **shell itself is already a process**.

To execute `a.out`:

```text
             SHELL
               │
             fork()
               │
        ┌──────┴──────┐
        ↓             ↓
   Parent Shell    Child
        │             │
        │           exec(a.out)
        │             │
        │             ↓
        │           a.out
        │
   Shell remains
```

### Why both?

If shell only used:

```text
exec(a.out)
```

then the shell itself would be replaced.

Instead:

```text
fork()
↓
Create child

exec()
↓
Replace child with requested program
```

So:

> **`fork()` creates space/process; `exec()` loads the required program into it.**

---

# 12. How Shell Runs a Command ⭐

Example:

```bash
ls
```

Flow:

```text
User enters "ls"
       ↓
Shell calls fork()
       ↓
Child process created
       ↓
Child calls exec(ls)
       ↓
Child becomes "ls"
       ↓
ls executes
```

So remember:

```text
Shell
  ↓
fork()
  ↓
Child
  ↓
exec()
  ↓
Command
```

---

# Final GATE Revision Box ⭐⭐⭐

Put this at the **end of your handwritten notes**:

```text
PROCESS
= Program in execution

CONTEXT
= Information needed to resume a process

PCB
= OS structure storing process information/context

LOGICAL ADDRESS
= Address visible to process

PHYSICAL ADDRESS
= Actual RAM address

fork()
= Creates child process

After fork():
Parent + Child continue after fork()

fork() return:
Child  → 0
Parent → Child PID

Parent & Child:
Separate address spaces
Same logical address ≠ same physical location

exec()
= Replaces current program with another program

Successful exec():
Code after exec does NOT execute

fork + exec:
fork → CREATE process
exec → REPLACE/load program

Shell command:
Shell → fork → Child → exec(command)
```

That's what I'd put in the notebook. **Roughly 4–5 handwritten pages**, not 15 pages. The lecture is really building one chain: **Program → Process → Context/PCB → `fork()` → `exec()` → how the shell creates processes.**