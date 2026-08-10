# Lecture 2 — Process Fundamentals, `fork()` and `exec()`

These notes follow the full Lecture 2 PDF. The lecture’s four main goals are: **compilation + logical/physical memory, process fundamentals, `fork()`/`exec()`, and process creation using the fork–exec pair.**

---

# 1. Why Do We Need an Operating System?

A programmer should not have to manually manage every hardware resource.

The OS manages things such as:

- CPU
    
- Memory
    
- Files
    
- I/O devices
    
- **Processes**
    

One of the most important things managed by an OS is the **process**.

---

# 2. From Source Code to Running Program

Suppose we write:

```c
int main() {
    printf("Hello");
}
```

and save it as:

```text
hello.c
```

The source program itself cannot directly execute on the CPU.

It goes through a compilation pipeline.

```text
hello.c
   ↓
Preprocessor
   ↓
hello.i
   ↓
Compiler
   ↓
hello.s
   ↓
Assembler
   ↓
hello.o
   ↓
Linker
   ↓
Executable
```

The lecture specifically illustrates the pipeline from source program → preprocessor → compiler → assembler → linker → executable.

### Meaning

**Preprocessor**

Handles things such as:

```c
#include
#define
```

**Compiler**

Converts the preprocessed C program into assembly.

**Assembler**

Converts assembly instructions into machine/object code.

**Linker**

Combines object files with required libraries/functions and creates the final executable.

For example:

```bash
gcc hello.c
```

may produce:

```text
a.out
```

---

# 3. Executable vs Running Program

This distinction is extremely important.

Suppose:

```text
hello.c
   ↓ compile
a.out
```

`a.out` is an **executable file**.

It is still just a file stored on storage.

When you execute:

```bash
./a.out
```

the OS loads the executable into memory.

So:

```text
Program on disk
      ↓
     load
      ↓
Program in memory
      ↓
   execution
      ↓
    Process
```

The lecture summarizes execution as: write program → compile → request OS to run it → OS loads it into memory → OS initializes registers and starts execution.

---

# 4. Logical Memory vs Physical Memory

This is one of the central ideas behind modern operating systems.

Imagine the generated code contains something conceptually like:

```text
LOAD R1, 1000
```

What does `1000` mean?

It does **not necessarily mean RAM location 1000**.

It represents an address inside the program's own address space.

That is a:

> **Logical address**

---

# 5. Logical Address

A logical address is the address used/viewed by the running program.

The compiler behaves as though the program has its own memory/address space.

For example:

```text
0
│
│
1000 ← variable a
│
│
2000 ← variable b
│
...
```

So the program might believe:

```text
a → address 1000
```

But `a` does not have to physically exist at RAM address 1000.

---

# 6. Physical Address

The **physical address** represents the actual location in RAM.

Example:

```text
Logical Memory             Physical Memory

1000 → a   ───────────→    2000 → a
```

Therefore:

```text
Logical address of a  = 1000
Physical address of a = 2000
```

The lecture uses exactly this kind of logical-to-physical mapping to show that the address used by the program need not equal its RAM location.

---

# 7. What Does `%p` Print?

Consider:

```c
int main() {
    int a = 5;

    printf("%p", &a);
}
```

The address printed by the program corresponds to the address visible in the process's address space.

In the lecture's terminology:

> `printf("%p", &a)` prints the **logical address**, not the actual physical RAM location.

---

# 8. What Is a Process?

The lecture gives the core informal definition:

> **A process is a program in execution.**

Think of the difference like this:

```text
hello.c / a.out
      │
      │ program
      ▼
loaded + executing
      │
      ▼
   PROCESS
```

A **program** is passive.

A **process** is active.

### Example

`chrome` executable:

```text
Program
```

Running Chrome:

```text
Process
```

Multiple Chrome instances/processes can exist from the same program.

---

# 9. Loading Does Not Necessarily Mean Running

Suppose a program has been loaded into memory.

It may still have to wait because another process is currently using the CPU.

Conceptually:

```text
Disk
 │
 │ load
 ▼
Memory
 │
 ├── waiting
 │
 ▼
CPU
```

The lecture introduces this using the idea of a **ready queue**: after loading, a program may wait before getting CPU time.

---

# 10. Multiple Processes

Memory may contain many processes simultaneously.

For example:

```text
Main Memory

┌─────────────┐
│ Process 1   │
├─────────────┤
│ Process 2   │
├─────────────┤
│ Process 3   │
└─────────────┘
```

Each process has its own execution information.

That information is called its **context**.

---

# 11. Context of a Process

This is an important definition:

> **The context of a process is the information required to resume its execution later.**

Imagine the CPU is executing:

```text
Process P1
```

and suddenly the OS needs to run:

```text
Process P2
```

The OS cannot simply forget where P1 stopped.

It needs to remember:

```text
Which instruction was next?
What were the register values?
What was on the stack?
What files were open?
What memory belonged to P1?
```

All of this forms part of the **process context**.

---

# 12. Important Components of Process Context

The lecture lists:

```text
Process Context
│
├── Program Counter (PC)
├── CPU Registers
├── Stack contents
├── Memory information
└── Open files
```

### Program Counter

The PC tells the CPU:

> Which instruction should execute next?

Example:

```text
100  instruction 1
104  instruction 2
108  instruction 3  ← PC
112  instruction 4
```

If the process is paused here, restoring the PC allows execution to continue from the correct location.

### CPU Registers

Suppose:

```text
R1 = 10
R2 = 50
```

If another process uses the CPU, those registers will change.

So P1's register values must be saved.

---

# 13. Process Control Block — PCB

Where does the OS store process information?

In a data structure called the:

# Process Control Block (PCB)

The lecture describes the PCB as containing information such as:

```text
PCB
│
├── Process state
├── Process number / PID
├── Program Counter
├── CPU registers
├── Scheduling information
├── Memory-management information
├── Memory limits
├── Open files
└── ...
```

Think of a PCB as:

> **The OS's record/bookkeeping structure for a process.**

---

# 14. Why PCB Matters

Suppose:

```text
CPU executing P1
```

The OS decides to switch to P2.

It first saves P1's current context:

```text
P1
 │
 ▼
PCB(P1)
```

Then it can later restore P1.

Conceptually:

```text
P1 running
    ↓
save context
    ↓
PCB(P1)

P2 running
```

Later:

```text
PCB(P1)
    ↓
restore context
    ↓
P1 continues
```

The lecture explicitly illustrates saving a process's context into its PCB before another process executes.

---

# 15. `a.out` vs `./a.out`

Important Linux distinction:

```text
a.out
```

is the **executable filename**.

Whereas:

```bash
./a.out
```

is a **command asking the shell to execute that file**.

The `./` means:

```text
.  → current directory
/  → path separator
```

So:

```bash
./a.out
```

means:

> Execute `a.out` from the current directory.

---

# 16. `fork()` System Call

Now comes one of the most important topics in this lecture.

```c
fork();
```

`fork()` creates a **new process**.

Before:

```text
Parent
```

After:

```text
        fork()
          │
      ┌───┴───┐
      ▼       ▼
   Parent    Child
```

The new process is called the:

> **Child process**

The original process is the:

> **Parent process**

---

# 17. The Most Important Rule of `fork()`

After `fork()`:

> **Both parent and child continue executing from the instruction immediately after the `fork()` call.**

This is explicitly highlighted as the lecture's “most important point.”

Example:

```c
int main() {
    fork();

    printf("Hello");

    return 0;
}
```

Before `fork()`:

```text
1 process
```

After:

```text
Parent
Child
```

Both execute:

```c
printf("Hello");
```

Therefore:

```text
HelloHello
```

Two prints.

---

# 18. Mental Model for `fork()`

Don't think:

> "`fork()` creates a child that starts from `main()`."

Instead think:

> "`fork()` duplicates the current running process, and both continue from the next instruction."

Visual:

```text
            fork()
              │
       ┌──────┴──────┐
       │             │
     Parent        Child
       │             │
       ▼             ▼
 next statement   next statement
```

Both then run independently.

---

# 19. Why Do We Need `fork()`?

A running process sometimes needs to create another process.

The lecture gives examples:

- A **shell** creates a process when you run a command.
    
- A **web server** may create a process to handle a client request.
    
- A **compiler** may create helper processes during compilation.
    

So `fork()` provides process creation.

---

# 20. Return Value of `fork()`

This is extremely important for GATE.

Consider:

```c
int pid = fork();
```

There are now two processes executing this statement, but they receive **different return values**.

### Child

```text
fork() returns 0
```

### Parent

```text
fork() returns Child PID
```

So memorize the relationship:

```text
Child  → 0
Parent → Child PID (> 0)
```

The lecture repeatedly emphasizes this distinction.

---

# 21. Distinguishing Parent and Child

Because the return values differ, we can write:

```c
int pid = fork();

if (pid == 0) {
    // child
}
else {
    // parent
}
```

Example from the lecture:

```c
int pid = fork();

if (pid == 0)
    printf("A");
else
    printf("B");
```

Child receives:

```text
pid = 0
```

so child prints:

```text
A
```

Parent receives the child's PID, so:

```text
pid != 0
```

and parent prints:

```text
B
```

Therefore:

```text
Child  → A
Parent → B
```

The **order** may depend on scheduling.

---

# 22. Parent and Child Address Spaces

Consider:

```c
int a = 10;

if (fork() == 0)
    printf("%p", &a);
else
    printf("%p", &a);
```

The lecture demonstrates that parent and child can print the **same logical/virtual address** for `a`.

This seems weird at first.

You might think:

> If both print the same address, aren't they modifying the same RAM location?

No.

This is where logical vs physical memory becomes important.

---

# 23. Same Logical Address ≠ Same Physical Location

After `fork()`, parent and child have separate process address spaces.

Conceptually:

```text
Parent                    Child

logical 1000 → a          logical 1000 → a
       │                         │
       ▼                         ▼
physical RAM X             physical RAM Y
```

Therefore:

```text
same logical address
        ≠
same physical RAM location
```

The lecture explicitly demonstrates this with parent and child using the same logical address while mapping to different physical frames/locations.

This is one of the key conceptual links in the lecture:

```text
fork()
  ↓
separate processes
  ↓
separate address spaces
  ↓
same logical address can map differently
```

---

# 24. Variables After `fork()`

Consider:

```c
int a = 10;

if (fork() == 0) {
    a = a + 5;
    printf("%d", a);
}
else {
    a = a + 10;
    printf("%d", a);
}
```

Initially:

```text
a = 10
```

After `fork()`:

```text
Parent                Child
a = 10                a = 10
```

Child:

```text
a = a + 5
  = 15
```

Parent:

```text
a = a + 10
  = 20
```

Therefore:

```text
Child  → 15
Parent → 20
```

Changing the child's `a` does not change the parent's `a`.

---

# 25. Parent and Child Memory Layout

The lecture illustrates the process address space roughly as:

```text
High Address
┌────────────────┐
│     Stack      │
│       ↓        │
│                │
│       ↑        │
│      Heap      │
├────────────────┤
│ Static / Data  │
├────────────────┤
│ Code / Text    │
└────────────────┘
Low Address
```

After `fork()`, the child gets an address-space layout corresponding to the parent's process image.

The key conceptual point for this lecture is:

```text
Parent address space
        │
       fork
        │
        ▼
Child address space
```

They are separate processes.

---

# 26. GATE-Style `fork()` Question

The lecture includes GATE CSE 2005 Question 72.

Conceptually:

```c
if (fork() == 0) {
    a = a + 5;
    printf("%d, %p", a, &a);
}
else {
    a = a - 5;
    printf("%d, %p", a, &a);
}
```

Suppose:

```text
parent prints → u, v
child prints  → x, y
```

Because both started from the same original value:

```text
Child:
x = original + 5

Parent:
u = original - 5
```

Therefore:

```text
x - u = 10
```

or:

```text
u + 10 = x
```

And their logical addresses can appear equal:

```text
v = y
```

Hence the relation identified in the lecture is:

```text
u + 10 = x
v = y
```

This question is testing **both** concepts simultaneously:

```text
fork()
+
logical address spaces
```

---

# 27. `exec()` System Call

Now compare `fork()` with `exec()`.

`fork()`:

> **Creates a new process.**

`exec()`:

> **Replaces the current process's program with another program.**

This difference is absolutely fundamental.

---

# 28. What Happens During `exec()`?

Imagine process A is running:

```text
a.out

printf("hello");

exec(b.out);

printf("I am back");
```

Before `exec()`:

```text
Memory

┌─────────────┐
│    a.out    │
└─────────────┘
```

When:

```c
exec(b.out);
```

succeeds, the current process image is replaced:

```text
Before exec             After exec

┌──────────┐            ┌──────────┐
│  a.out   │   exec     │  b.out   │
└──────────┘  ───────→  └──────────┘
```

The lecture illustrates exactly this replacement.

---

# 29. Code After Successful `exec()` Does Not Execute

Suppose:

```c
printf("Hello");

exec("b.out");

printf("I am back");
```

If `exec()` succeeds:

```text
printf("Hello")
      ↓
exec(b.out)
      ↓
a.out replaced
      ↓
b.out executes
```

Therefore:

```c
printf("I am back");
```

does **not** execute.

This is a major exam point.

The lecture's examples show execution moving into the new program instead of returning to the old program's statements.

---

# 30. Chained `exec()` Example

Suppose:

```text
a.c

print("hello")
exec(b.out)
print("hey it's me")
print("please save me")
```

and:

```text
b.c

print("I am b")
exec(c.out)
print("I won")
```

and:

```text
c.c

print("I am c")
```

Execution:

```text
a
│
├─ print "hello"
│
└─ exec(b)
       │
       ├─ print "I am b"
       │
       └─ exec(c)
              │
              └─ print "I am c"
```

So the lecture gives the resulting conceptual output as:

```text
hello
I am b
I am c
```

Statements after successful `exec()` calls don't execute.

---

# 31. `fork()` vs `exec()`

This is the cleanest mental model:

|`fork()`|`exec()`|
|---|---|
|Creates a new process|Loads/replaces with a new program|
|Parent remains|Current program image gets replaced|
|Number of processes increases|Does not itself create an additional process|
|Parent + child continue|Same process begins executing another program|
|Child initially corresponds to parent's state|Old program code/state is replaced by new program image|

Think:

```text
fork = duplicate process

exec = replace program
```

---

# 32. Why `fork()` and `exec()` Are Used Together

Now we reach the big picture of the lecture.

Suppose you're inside a shell and type:

```bash
./a.out
```

The shell itself is already a running process.

We don't want to replace the shell permanently with `a.out`.

If the shell directly did:

```text
exec(a.out)
```

then:

```text
Shell → gone
a.out → takes its place
```

That's not what we normally want.

Instead:

```text
Shell
  │
 fork()
  │
  ├───────────────┐
  │               │
Parent Shell     Child Shell
                  │
                exec(a.out)
                  │
                  ▼
                a.out
```

The lecture uses this shell example to motivate the fork–exec pair.

---

# 33. How the Shell Executes a Command

Suppose:

```bash
ls
```

is entered into a shell.

Initially:

```text
Shell Process
```

### Step 1 — `fork()`

The shell creates a child:

```text
             fork()
               │
        ┌──────┴──────┐
        ▼             ▼
 Parent Shell     Child Shell
```

### Step 2 — Child calls `exec()`

```text
Child Shell
    │
  exec(ls)
    │
    ▼
   ls
```

So overall:

```text
Shell
  │
 fork
  ├──────────── Parent shell remains
  │
  └──── Child
          │
         exec
          │
          ▼
        command
```

The final pages visualize this exact sequence: shell state → `fork()` creates parent/child → the child is replaced by the requested command through `exec()`.

---

# 34. Simplified Shell Logic

The lecture ends with a rough shell-code idea:

```c
while (true) {

    scanf("%s", command);

    pid = fork();

    if (pid == 0) {
        exec(command);
    }
}
```

Conceptually:

```text
Shell waits for command
        ↓
      fork()
        ↓
 ┌──────┴───────┐
Parent         Child
 │               │
 │             exec()
 │               │
 │            command
 │
continues shell
```

---

# 35. Complete Mental Model of Lecture 2

Everything in this lecture connects into one story:

```text
SOURCE CODE
   │
   │ compile
   ▼
EXECUTABLE FILE
   │
   │ OS loads
   ▼
MEMORY
   │
   ▼
PROCESS
   │
   ├── logical address space
   ├── registers
   ├── program counter
   ├── stack
   ├── open files
   └── other context
           │
           ▼
          PCB
```

Then:

```text
PROCESS
   │
   ├── fork()
   │      │
   │      └── creates CHILD process
   │
   └── exec()
          │
          └── replaces current program
```

And Unix/Linux commonly combines them:

```text
Shell
  │
fork()
  │
  ├── Parent → shell continues
  │
  └── Child
        │
       exec()
        │
        ▼
     new program
```

That is the core architecture Lecture 2 is trying to build. The final slide marks all four objectives—compilation/logical-vs-physical memory, process, `fork`/`exec`, and fork–exec process creation—as the completed “Process Fundamentals” foundation.

---

# GATE Quick Revision

```text
PROGRAM
→ passive executable/code

PROCESS
→ program in execution

CONTEXT
→ information required to resume a process

PCB
→ OS data structure storing process information

Logical Address
→ address visible to/generated for the process

Physical Address
→ actual RAM location

fork()
→ creates a child process

After fork()
→ parent + child continue from instruction after fork()

fork return value:
Child  → 0
Parent → Child PID

Parent/Child:
→ separate processes
→ can have same logical addresses
→ same logical address does NOT imply same physical RAM location
→ modifying normal variables in one does not modify the other's copy

exec()
→ replaces current process's program image
→ does NOT create a new process
→ code after successful exec() is not executed

fork + exec:
→ fork creates process
→ exec loads new program into that process

Typical shell:
Shell
 → fork()
    → Parent keeps shell
    → Child exec(command)
```

### ⭐ The five lines I'd absolutely lock into memory

**1. Process = program in execution.**

**2. Context = information required to resume a process later.**

**3. `fork()` = create a new child process; both continue after the `fork()`.**

**4. Child gets `0` from `fork()`, parent gets the child's PID.**

**5. `exec()` = replace the current process image with a new program.**

If those five are crystal clear, most of the lecture's GATE questions become reasoning problems rather than memorization problems.