# GATE CSE OS — Module 2: Processes (Ultimate Cheat Sheet)

> **Goal:** One-page-to-few-pages revision before PYQs/interviews. Focus on concepts, traps, and formulas.

---

# 1. Program vs Process ⭐⭐⭐⭐⭐

## Program

- Passive entity.
    
- Stored on disk.
    
- Collection of instructions.
    
- Does not execute by itself.
    

Examples

```text
chrome.exe
a.out
notepad.exe
```

---

## Process

- Active entity.
    
- Program in execution.
    
- Stored in main memory.
    
- Has CPU state, PCB, stack, heap, registers, etc.
    

---

## Relationship

```text
Program
(Disk)

        │ Execute

        ▼

Process
(Main Memory)
```

---

## Differences

|Program|Process|
|---|---|
|Passive|Active|
|Stored on Disk|Stored in RAM|
|No PCB|Has PCB|
|Static|Dynamic|
|No CPU State|Has CPU State|

---

## GATE Traps

❌ Program executes.

✔ False.

Process executes.

---

❌ Multiple processes cannot come from one program.

✔ False.

Example

```text
Chrome.exe

↓

Tab 1

Tab 2

Tab 3
```

---

# 2. Process States ⭐⭐⭐⭐⭐

## Five-State Model

```text
            +---------+
            |  New    |
            +---------+
                 |
                 ▼
            +---------+
            | Ready   |
            +---------+
                 |
         Scheduler selects
                 |
                 ▼
            +---------+
            | Running |
            +---------+
            /         \
           /           \
      I/O Wait      Time Slice Over
         |               |
         ▼               ▼
   +-----------+     +---------+
   | Waiting   |     | Ready   |
   +-----------+     +---------+
         |
     I/O Complete
         |
         ▼
      Ready
```

Running → Exit → Terminated

---

## States

### New

Process being created.

---

### Ready

Ready for CPU.

Waiting in Ready Queue.

---

### Running

Currently executing.

---

### Waiting / Blocked

Waiting for I/O or an event.

Cannot execute.

---

### Terminated

Finished execution.

---

## GATE Traps

Blocked ≠ Ready

Blocked waits for event.

Ready waits for CPU.

---

# 3. Process Life Cycle ⭐⭐⭐⭐

```text
Create

↓

Ready

↓

Running

↓

Waiting

↓

Ready

↓

Running

↓

Exit
```

---

## Events

|Transition|Cause|
|---|---|
|New → Ready|Admitted|
|Ready → Running|Scheduler|
|Running → Ready|Time quantum expires|
|Running → Waiting|I/O request|
|Waiting → Ready|I/O completes|
|Running → Exit|Process terminates|

---

# 4. Process Control Block (PCB) ⭐⭐⭐⭐⭐

## Definition

> PCB is the **kernel data structure** that stores all information about a process.

Without PCB

↓

OS cannot manage processes.

---

## PCB Contents

- PID
    
- Process State
    
- Program Counter
    
- CPU Registers
    
- Scheduling Information
    
- Memory Information
    
- Open File Descriptors
    
- I/O Status
    
- Accounting Information
    

---

## Visual

```text
PCB

├── PID
├── State
├── Program Counter
├── Registers
├── Stack Pointer
├── Priority
├── Memory Info
├── File Descriptors
└── Scheduling Info
```

---

## GATE Trap

PCB is stored

✔ Kernel Space

Not User Space.

---

# 5. Context Switching ⭐⭐⭐⭐⭐

## Definition

> Saving one process's CPU state and restoring another process's CPU state.

---

## Steps

```text
Running P1

↓

Save PCB(P1)

↓

Load PCB(P2)

↓

Run P2
```

---

## Saved

- Program Counter
    
- Registers
    
- Stack Pointer
    
- CPU State
    

---

## Important

- Pure overhead.
    
- No useful work done.
    
- More context switches → Lower CPU efficiency.
    

---

## GATE Traps

Context switch

❌ Does not switch memory.

✔ Switches CPU context.

---

# 6. Dispatcher ⭐⭐⭐⭐

## Definition

Dispatcher gives CPU to the selected process.

Scheduler

↓

Chooses process

↓

Dispatcher

↓

Runs process

---

## Dispatcher Responsibilities

- Context switch
    
- Switch to user mode
    
- Jump to correct instruction
    

---

## Dispatcher Latency

Time taken to switch CPU.

Smaller is better.

---

# 7. Process Creation ⭐⭐⭐⭐

Created using

- fork()
    
- CreateProcess() (Windows)
    

Reasons

- User login
    
- Running program
    
- Batch jobs
    
- Services
    

---

# 8. Process Termination ⭐⭐⭐⭐

Reasons

- Normal completion
    
- Error
    
- Fatal error
    
- Killed by another process
    

---

Termination releases

- Memory
    
- CPU
    
- Files
    
- Resources
    

---

# 9. Parent & Child Process ⭐⭐⭐⭐⭐

```text
Parent

↓

fork()

↓

Child
```

Child

- New PID
    
- New PCB
    
- Separate virtual address space
    
- Executes independently
    

Parent may create multiple children.

---

## GATE Trap

Parent and Child

❌ Same PID

✔ Different PID

---

# 10. `fork()` ⭐⭐⭐⭐⭐

## Definition

Creates a child process by duplicating the parent.

---

## Important

Called once.

Returns twice.

---

## Return Values ⭐⭐⭐⭐⭐

|Value|Returned To|Meaning|
|---|---|---|
|>0|Parent|Child's actual PID|
|0|Child|"I am child"|
|-1|Parent|Child creation failed|

---

## Important

The return value

≠

Process PID

Use

```c
getpid();
```

to obtain actual PID.

---

## Example

```c
pid_t pid=fork();

if(pid==0)
{
    // Child
}
else if(pid>0)
{
    // Parent
}
else
{
    // Error
}
```

---

## GATE Traps

Child PID

❌ 0

✔ Positive integer

---

Parent gets

✔ Child PID

---

fork()

✔ Called once

✔ Returns twice

---

# 11. `exec()` ⭐⭐⭐⭐⭐

## Definition

Replaces current process image with a new program.

---

## Visual

```text
Program A

↓

exec()

↓

Program B
```

---

## Important

No new process.

Same PID.

Same process.

New executable.

---

## Memory Replaced

- Code
    
- Data
    
- Heap
    
- Stack
    

---

## GATE Trap

exec()

❌ Creates child

✔ False

---

# 12. `wait()` ⭐⭐⭐⭐⭐

## Definition

Parent waits until child terminates.

---

```c
wait(NULL);
```

---

## Purpose

- Synchronization
    
- Collect exit status
    
- Prevent Zombie
    

---

## GATE Trap

wait()

❌ Kills child

✔ Child already exited

---

# 13. Zombie Process ⭐⭐⭐⭐⭐

## Definition

Child terminated

Parent didn't call wait()

=

Zombie

---

## Characteristics

- Dead process
    
- PCB remains
    
- Occupies process table
    
- CPU released
    
- Memory mostly released
    

---

## Solution

```c
wait();
```

---

## GATE Trap

Zombie

❌ Running

✔ Already dead

---

# 14. Orphan Process ⭐⭐⭐⭐⭐

## Definition

Parent terminates

↓

Child still alive

↓

Orphan

---

Linux

↓

init/systemd

↓

Adopts child

---

## Characteristics

- Child still running
    
- Gets new parent (init/systemd)
    

---

## GATE Trap

Orphan

❌ Dangerous

✔ Normal

---

# Zombie vs Orphan ⭐⭐⭐⭐⭐

|Feature|Zombie|Orphan|
|---|---|---|
|Parent Alive|✅|❌|
|Child Alive|❌|✅|
|Parent Dead|❌|✅|
|Child Dead|✅|❌|
|PCB Exists|✅|✅|
|CPU Used|❌|✅|
|Memory Used|❌ Mostly Released|✅|
|Solution|wait()|init/systemd adopts|

---

# Process Creation Flow ⭐⭐⭐⭐⭐

```text
Program

↓

Loaded into Memory

↓

Process Created

↓

Ready

↓

Running

↓

fork()

↓

Parent + Child

↓

exec()

↓

New Program

↓

exit()

↓

wait()

↓

Process Removed
```

---

# Important System Calls Summary ⭐⭐⭐⭐⭐

|System Call|Purpose|
|---|---|
|fork()|Create child process|
|exec()|Replace program|
|wait()|Wait for child|
|exit()|Terminate process|
|getpid()|Get own PID|
|getppid()|Get parent's PID|

---

# PCB Revision ⭐⭐⭐⭐⭐

```text
PCB

↓

Identity
(PID)

↓

Execution
(PC, Registers)

↓

Scheduling
(State, Priority)

↓

Memory

↓

Open Files

↓

Accounting
```

---

# Context Switch Revision ⭐⭐⭐⭐⭐

```text
Running P1

↓

Save PCB(P1)

↓

Load PCB(P2)

↓

Running P2
```

Remember

✔ CPU context changes.

❌ Process is not recreated.

---

# Memory Tricks 🧠

## Program vs Process

```text
Program

↓

Passive

↓

Disk
```

```text
Process

↓

Active

↓

RAM
```

---

## Process States

```text
New

↓

Ready

↓

Running

↓

Waiting

↓

Ready

↓

Running

↓

Exit
```

---

## Process Creation

```text
fork()

↓

Create Child

↓

exec()

↓

Run New Program

↓

exit()

↓

wait()

↓

No Zombie
```

---

## Zombie

```text
Child Dead

+

Parent Alive

+

No wait()

=

Zombie
```

---

## Orphan

```text
Parent Dead

+

Child Alive

=

Orphan

↓

init/systemd
```

---

# Ultimate GATE One-Liners ⭐⭐⭐⭐⭐

- **Program:** Passive file stored on disk.
    
- **Process:** Program in execution.
    
- **PCB:** Kernel structure storing process information.
    
- **Context Switch:** Save one process state, restore another.
    
- **Dispatcher:** Gives CPU to the selected process.
    
- **fork():** Creates a child process; called once, returns twice.
    
- **exec():** Replaces the current program; creates no new process.
    
- **wait():** Parent blocks until child terminates.
    
- **Zombie:** Dead child whose PCB remains because the parent didn't call `wait()`.
    
- **Orphan:** Running child whose parent terminated; adopted by `init/systemd`.
    

---

# 15-Minute Revision Order ⭐⭐⭐⭐⭐

```text
Program vs Process
        ↓
Process States
        ↓
Life Cycle
        ↓
PCB
        ↓
Context Switch
        ↓
Dispatcher
        ↓
Parent & Child
        ↓
fork()
        ↓
exec()
        ↓
wait()
        ↓
Zombie
        ↓
Orphan
        ↓
Solve PYQs
```