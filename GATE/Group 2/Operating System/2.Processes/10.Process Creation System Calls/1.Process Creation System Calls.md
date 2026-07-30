# Module 2 — Process Creation System Calls

# 1. `fork()`

## Definition

> **`fork()`** is a system call used to create a **new child process** by duplicating the calling (parent) process.

After `fork()`, **two processes exist**:

- Parent Process
- Child Process

Both continue execution from the instruction immediately after `fork()`.

---

## Syntax

```c
pid_t fork();
```

---

## Working

```text
Parent Process
      │
      ▼
    fork()
      │
      ├───────────────┐
      ▼               ▼
 Parent Process   Child Process
```

Both processes execute independently.

---

## Return Value ⭐⭐⭐

This is one of the most important GATE concepts.

| Return Value | Meaning |
|--------------|---------|
| > 0 | Returned to Parent (PID of Child) |
| 0 | Returned to Child |
| -1 | Process creation failed |

Example

```c
pid_t pid = fork();

if(pid == 0)
{
    // Child Process
}
else if(pid > 0)
{
    // Parent Process
}
else
{
    // Error
}
```

---

## Important Facts

- Child gets a new PID.
- Child has its own PCB.
- Child has its own address space.
- Parent and child execute concurrently.
- Execution order is NOT guaranteed.

---

# GATE Corner ⭐

## Remember

```text
fork()

↓

One Process

↓

Two Processes
```

Return Values

```text
Parent → Child PID (>0)

Child → 0

Failure → -1
```

### GATE Tricks

❌ Parent executes first.

False.

Scheduler decides.

---

❌ Parent and child have same PID.

False.

---

❌ Parent and child share PCB.

False.

---

# 2. `exec()`

## Definition

> **`exec()` replaces the current process image with a new program.**

Unlike `fork()`,

**No new process is created.**

---

## Intuition

Before

```text
Process

↓

Program A
```

After

```text
exec()

↓

Program B
```

Same PID.

Same Process.

Different Program.

---

## Flow

```text
Parent

↓

fork()

↓

Child

↓

exec()

↓

Runs New Program
```

This is how Linux launches programs.

---

## Why Use exec()?

`fork()` creates a child.

`exec()` makes the child run another program.

Example

```text
Shell

↓

fork()

↓

Child

↓

exec(ls)
```

---

## Important Facts

- PID remains same.
- PCB remains same.
- Memory image replaced.
- Code replaced.
- Data replaced.
- Stack replaced.
- Heap replaced.

---

# GATE Corner ⭐

### Difference

```text
fork()

Creates Process
```

```text
exec()

Replaces Program
```

---

### Trick

❌ exec() creates child process.

False.

---

❌ PID changes after exec().

False.

---

# 3. `wait()`

## Definition

> **`wait()`** causes the parent process to wait until one of its child processes terminates.

---

## Syntax

```c
wait(NULL);
```

---

## Flow

```text
Parent

↓

fork()

↓

Child Running

↓

Parent calls wait()

↓

Parent Sleeps

↓

Child Finishes

↓

Parent Continues
```

---

## Why wait()?

Without `wait()`

Child may terminate.

Parent continues.

Zombie Process may occur.

---

## Important Facts

- Parent blocks.
- Child executes.
- Parent resumes after child terminates.
- Parent collects exit status.

---

# GATE Corner ⭐

### Remember

```text
wait()

↓

Synchronization

↓

Collect Exit Status
```

---

### Trick

❌ wait() kills child.

False.

Child already finished.

---

# 4. Zombie Process

## Definition

> A **Zombie Process** is a process that has **finished execution** but whose **PCB still exists** because the parent has not yet collected its exit status.

---

## Why?

Child finishes.

↓

Exit status stored.

↓

Parent never calls wait().

↓

PCB remains.

↓

Zombie.

---

## Flow

```text
Parent

↓

fork()

↓

Child

↓

exit()

↓

Parent doesn't call wait()

↓

Zombie Process
```

---

## Characteristics

- Process already terminated.
- Memory released.
- CPU released.
- Only PCB remains.
- Occupies Process Table entry.

---

## Solution

Parent should call

```c
wait();
```

PCB removed.

Zombie disappears.

---

# GATE Corner ⭐⭐⭐

### Remember

Zombie

```text
Dead Process

+

Alive PCB
```

---

### Trick

Zombie consumes CPU?

❌ No.

Zombie uses Memory?

❌ No.

Zombie occupies Process Table?

✅ Yes.

---

# 5. Orphan Process

## Definition

> An **Orphan Process** is a child process whose **parent terminates before the child**.

---

## Flow

```text
Parent

↓

fork()

↓

Child Running

↓

Parent Terminates

↓

Child Becomes Orphan
```

---

## What Happens?

Operating System assigns

Parent = init/systemd

Linux automatically adopts the child.

---

## Flow

```text
Parent dies

↓

init/systemd

↓

Adopts Child

↓

Child Continues Normally
```

---

## Important Facts

- Child is still running.
- Parent already terminated.
- init/systemd becomes new parent.
- Child continues execution.

---

# GATE Corner ⭐⭐⭐

### Remember

```text
Parent Dies

↓

Child Alive

↓

Orphan
```

---

### Trick

Orphan is dangerous?

❌ No.

Linux automatically adopts it.

---

# Zombie vs Orphan ⭐⭐⭐⭐⭐

| Feature | Zombie | Orphan |
|----------|---------|---------|
| Parent Alive | ✅ Yes | ❌ No |
| Child Alive | ❌ No | ✅ Yes |
| Child Finished | ✅ Yes | ❌ No |
| Parent Finished | ❌ No | ✅ Yes |
| PCB Exists | ✅ Yes | ✅ Yes (running process) |
| CPU Used | ❌ No | ✅ Yes |
| Memory Used | ❌ Mostly released | ✅ Yes |
| Solution | wait() | init/systemd adopts |

---

# Complete Process Lifecycle

```text
Program
     │
     ▼
Process Created
     │
     ▼
fork()
     │
     ├───────────────┐
     ▼               ▼
Parent           Child
                    │
                    ▼
                exec()
                    │
                    ▼
           New Program Executes
                    │
                    ▼
                  exit()
                    │
                    ▼
        Parent calls wait()?
            │             │
          YES             NO
           │               │
           ▼               ▼
     Process Removed    Zombie
```

---

# If Parent Dies First

```text
Parent

↓

Dies

↓

Child Still Running

↓

Orphan

↓

Adopted by init/systemd

↓

Continues Execution
```

---

# 🎯 Ultimate GATE Cheat Sheet

## `fork()`

- Creates child process
- Returns twice
- Parent gets Child PID
- Child gets 0
- New PID
- New PCB

---

## `exec()`

- Creates NO process
- Replaces current program
- Same PID
- Same PCB

---

## `wait()`

- Parent blocks
- Waits for child
- Collects exit status
- Prevents Zombie

---

## Zombie

- Child finished
- Parent alive
- Parent didn't call wait()
- PCB remains

---

## Orphan

- Parent finished
- Child alive
- init/systemd adopts child

---

# Memory Trick 🧠

```text
fork()

↓

Create Child

↓

exec()

↓

Run New Program

↓

wait()

↓

Collect Exit Status

↓

No Zombie
```

```text
Parent Dies

↓

Orphan

↓

init/systemd
```

```text
Child Dies

↓

Parent ignores

↓

Zombie
```