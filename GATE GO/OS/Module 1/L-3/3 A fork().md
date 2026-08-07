Yep — **short notes only**, the kind you can directly put into your GATE notebook.

# OS — `fork()` Short Notes

## 1. `fork()`

- `fork()` is a **system call used to create a child process**.
    
- After successful `fork()`, **both parent and child continue from the next instruction**.
    

```text
Before:       P

After fork:   P
             / \
            P   C
```

---

## 2. Return Value of `fork()`

|Process|`fork()` return|
|---|--:|
|Parent|`> 0` → Child PID|
|Child|`0`|
|Failure|`< 0` → No child created|

### Pattern

```c
if (fork() == 0)
    // Child
else
    // Parent
```

---

## 3. Number of Processes

If **every process reaches every `fork()`**:

$$ 
\boxed{\text{Total processes}=2^n}  
$$

$$
\boxed{\text{Child processes}=2^n-1}  
$$

where `n` = number of successful forks.

```text
1 fork → 2
2 forks → 4
3 forks → 8
4 forks → 16
```

---

## 4. `fork()` in Loop

```c
for(i=0; i<n; i++)
    fork();
```

If every process executes every iteration:

[  
\boxed{2^n\text{ total processes}}  
]

[  
\boxed{2^n-1\text{ child processes}}  
]

**Don't count loop iterations blindly — count actual `fork()` executions.**

---

## 5. Conditional `fork()`

Never blindly apply `2^n`.

Check:

> **Which processes reach the next `fork()`?**

Statements that can change the process tree:

```text
if / else
break
continue
return
exit
wait / waitpid
```

---

## 6. Process Tree

Use a tree to track processes:

```text
       P
      / \
     P   C
    / \
   P   C
```

**Process tree → number of processes**

**Scheduler → execution/output order**

---

## 7. `wait()` / `waitpid()`

- Used for **process synchronization**.
    
- Parent can wait for child completion.
    
- Restricts the possible output order.
    

---

## 8. Variables After `fork()`

Parent and child have **separate process states/address spaces**.

```text
Before:
x = 10

After fork:

Parent → x = 10
Child  → x = 10
```

Changing `x` in one does not directly change the other's copy.

---

# ⭐ GATE Traps

1. **Negative `fork()` return ≠ negative PID** → fork failed.
    
2. Child also executes code after `fork()`.
    
3. `2^n` only when **all processes reach all forks**.
    
4. Don't count `fork()` statements; track **which processes execute them**.
    
5. `break` affects only the current process.
    
6. `continue` affects only the current process.
    
7. `wait()`/`waitpid()` affects **ordering**, not simply process creation.
    
8. Process tree tells **HOW MANY**; scheduling determines **IN WHAT ORDER**.
    

---

# 🔥 GATE Examples From Lecture

### GATE 2012

```c
fork();
fork();
fork();
```

$$
2^3-1=\boxed{7\text{ children}}  
$$

### GATE 2019

```c
for(i=0;i<10;i++)
    if(i%2==0)
        fork();
```

Even values:

```text
0,2,4,6,8 → 5 forks
```

$$
2^5-1=\boxed{31}  
$$

### GATE 2026

`fork()` + `continue` + `break` → **4 executions of `printf()`**.

---

## 🧠 One Rule to Remember

> **For every `fork()`, ask: "WHO reaches this fork?"**

That is the key to almost every `fork()` GATE question.