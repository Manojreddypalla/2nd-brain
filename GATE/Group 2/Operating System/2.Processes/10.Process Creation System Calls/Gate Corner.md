# 🎯 GATE Corner — `fork()`, `exec()`, `wait()`, Zombie & Orphan

## Weightage

⭐⭐⭐⭐⭐ **One of the most important topics in Operating Systems.**

Questions are frequently asked from:

- `fork()` return values
- `exec()`
- `wait()`
- Zombie Process
- Orphan Process
- Process Tree
- Parent–Child relationship

---

# 1. `fork()` ⭐⭐⭐⭐⭐

## Must Remember

- Creates a **new child process**.
- Child is almost an exact copy of the parent.
- Parent and child execute **concurrently**.
- Execution order is **not guaranteed**.

### Return Values ⭐⭐⭐

| Return Value | Process |
|--------------|---------|
| **> 0** | Parent (receives Child PID) |
| **0** | Child |
| **-1** | Failure |

This table is asked very frequently.

---

### GATE Tricks

❌ `fork()` creates a new program.

**False**

It creates a **new process**.

---

❌ Parent always executes first.

**False**

Scheduler decides.

---

❌ Parent and child have same PID.

**False**

Different PID.

---

# 2. `exec()` ⭐⭐⭐⭐

## Must Remember

- Creates **NO new process**.
- Replaces the current program with another.
- PID remains unchanged.
- PCB remains the same.
- Never returns if successful.

---

### GATE Tricks

❌ `exec()` creates child process.

**False**

---

❌ PID changes after `exec()`.

**False**

---

❌ After successful `exec()`, old program continues.

**False**

Old program is completely replaced.

---

# 3. `wait()` ⭐⭐⭐⭐

## Must Remember

- Parent blocks until child terminates.
- Collects child's exit status.
- Prevents Zombie Process.

---

### GATE Tricks

❌ `wait()` terminates child.

**False**

Child has already terminated.

---

❌ `wait()` creates synchronization.

**True**

---

# 4. Zombie Process ⭐⭐⭐⭐⭐

## Definition

Child has **terminated**.

Parent is **still alive**.

Parent has **not called `wait()`**.

Result:

PCB remains in the process table.

---

### Characteristics

| Property | Zombie |
|----------|---------|
| Child Alive | ❌ No |
| Parent Alive | ✅ Yes |
| CPU Used | ❌ No |
| Memory Used | ❌ Mostly released |
| PCB Exists | ✅ Yes |

---

### GATE Tricks

❌ Zombie is still executing.

**False**

---

❌ Zombie consumes CPU.

**False**

---

❌ Zombie occupies Process Table entry.

**True**

---

### Solution

```c
wait();
```

---

# 5. Orphan Process ⭐⭐⭐⭐⭐

## Definition

Parent terminates before child.

Child continues executing.

Linux assigns the child to **init/systemd (PID 1)**.

---

### Characteristics

| Property | Orphan |
|----------|---------|
| Parent Alive | ❌ No |
| Child Alive | ✅ Yes |
| CPU Used | ✅ Yes |
| Memory Used | ✅ Yes |
| New Parent | init/systemd |

---

### GATE Tricks

❌ Orphan is an error.

**False**

---

❌ Orphan is adopted by init/systemd.

**True**

---

# Zombie vs Orphan ⭐⭐⭐⭐⭐

| Feature | Zombie | Orphan |
|---------|---------|---------|
| Parent Alive | ✅ | ❌ |
| Child Alive | ❌ | ✅ |
| Child Terminated | ✅ | ❌ |
| CPU Usage | ❌ | ✅ |
| Memory Usage | ❌ Mostly released | ✅ |
| PCB Exists | ✅ | ✅ |
| Fixed By | `wait()` | init/systemd adoption |

---

# Most Asked Concept ⭐⭐⭐⭐⭐

```text
fork()

↓

Creates Child

↓

exec()

↓

Child Runs New Program

↓

exit()

↓

wait()

↓

Child Completely Removed
```

---

# Process State Questions

```
fork()
        ↓
Ready
        ↓
Running
        ↓
exit()
        ↓
Terminated

Parent ignores wait()

↓

Zombie
```

```
fork()

↓

Parent Dies

↓

Child Running

↓

Orphan

↓

init/systemd adopts
```

---

# Common MCQs

### Q1

Which system call creates a child process?

✅ **fork()**

---

### Q2

Which system call replaces the current program?

✅ **exec()**

---

### Q3

Which system call prevents Zombie Processes?

✅ **wait()**

---

### Q4

Who adopts an orphan process?

✅ **init/systemd (PID 1)**

---

### Q5

What does `fork()` return to the child?

✅ **0**

---

### Q6

What does `fork()` return to the parent?

✅ **PID of the child (>0)**

---

### Q7

Which process still occupies an entry in the process table?

✅ **Zombie Process**

---

### Q8

Which process continues executing after its parent dies?

✅ **Orphan Process**

---

# 30-Second Revision 🚀

| System Call | Purpose |
|-------------|---------|
| `fork()` | Create child process |
| `exec()` | Replace current program |
| `wait()` | Wait for child & collect exit status |

---

| Process | Condition |
|----------|-----------|
| Zombie | Child terminated, parent alive, `wait()` not called |
| Orphan | Parent terminated, child still running |

---

# Memory Trick 🧠

```text
fork()
↓
Create Process

exec()
↓
Change Program

wait()
↓
Collect Child

Child Dies + Parent Ignores
↓
Zombie

Parent Dies + Child Lives
↓
Orphan
```

---

# PYQ Keywords

If a question contains these words, immediately think of this topic:

- Parent Process
- Child Process
- PID
- Process Tree
- `fork()`
- `exec()`
- `wait()`
- `exit()`
- Zombie
- Orphan
- init/systemd
- Process Table