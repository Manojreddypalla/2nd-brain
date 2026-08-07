# OS — System Calls: Short Notes

### 1. Why System Calls?

- User programs **cannot be trusted** with critical hardware/OS operations.
    
- A malicious or incorrect user program could cause **system failure**.
    
- Therefore, users request OS services through **system calls**.
    

### 2. User Mode vs Kernel Mode

|User Mode|Kernel Mode|
|---|---|
|Less privileged|Highly privileged|
|User applications execute here|OS/kernel executes here|
|Cannot execute privileged instructions|Can execute privileged instructions|
|Cannot directly access kernel memory/hardware|Can access system memory & hardware|
|Mode bit = **1**|Mode bit = **0**|

Examples of privileged operations: **I/O, CPU/memory management, modifying PC, accessing kernel memory**.

**Key idea:**

> User → System Call → Kernel → Service → User

---

## 3. What is a System Call?

A **system call is a controlled entry point into the kernel** through which a user program requests an OS service.

Examples:

- Open a file
    
- Read/write a file
    
- Create a process
    
- Execute a program
    
- Communicate with another process
    

---

## 4. System Call Execution ⭐

Remember this flow:

```text
User Process
   │
   │ system call
   ▼
Wrapper / API
   │
   │ syscall instruction / trap
   ▼
Kernel Mode
   │
   │ system call handler
   ▼
OS performs operation
   │
   │ return
   ▼
User Mode
```

### Important steps

1. Program calls a **wrapper function**.
    
2. Wrapper puts **system-call number + arguments** into registers.
    
3. Executes special instruction such as `syscall`.
    
4. CPU switches **User → Kernel mode**.
    
5. Kernel uses the system-call number to locate the required service.
    
6. Kernel performs the operation.
    
7. Return value is placed in a register.
    
8. CPU switches **Kernel → User mode**.
    

---

## 5. System Call Table ⭐

- Every system call has a **unique number**.
    
- OS maintains a **system call table**.
    
- The number indexes the table and identifies the required kernel function.
    
- Usually implemented as an array of **function pointers**.
    
- Provides **protected entry points** into kernel code.
    

```text
System Call Number
       ↓
+---+---+---+-----+
| 0 | 1 | 2 | ... |
+---+---+---+-----+
  ↓   ↓   ↓
Kernel service functions
```

---

## 6. API vs System Call

Applications generally **don't directly invoke system calls**.

Instead:

```text
Application
     ↓
API / Library function
     ↓
System Call
     ↓
Kernel
```

Example:

```c
printf("Hello");
```

may eventually use a lower-level `write()` system call. The standard library acts as an interface/wrapper.

**Important:** Not every API/library function requires a system call. Some functions are implemented completely in user space.

---

## 7. Function Call vs System Call ⭐

### Function Call

- Caller and callee are generally in the **same process/domain**.
    
- No privilege-level switch is required.
    

### System Call

- Crosses **user → kernel protection boundary**.
    
- OS is trusted; user program is not.
    
- Requires mode switching and additional OS work.
    

---

## 8. `syscall` / Trap

A user program cannot simply call a kernel function like a normal function.

A special CPU instruction such as **`syscall`** causes the transition to kernel mode.

The system-call mechanism saves the old execution state and transfers control to the kernel's system-call handler.

---

## 9. System Call Overhead ⭐

System calls are **slower than normal function calls** because they involve:

- Protection-domain/mode switching
    
- Argument validation
    
- Memory mapping adjustments
    
- Cache effects
    
- Kernel execution and return
    

Therefore:

> **Programs should minimize unnecessary system calls.**

**GATE trap:**  
❌ "System calls are as fast as function calls."  
✅ **False** — system calls are slower.

---

## 10. Types of System Calls ⭐

### Process Control

- Create process
    
- Terminate process
    
- Load / execute
    
- Get/set process attributes
    

Examples: `fork()`, `exec()`, `exit()`, `wait()`

### File Management

- Create/delete file
    
- Open/close
    
- Read/write
    

Examples: `open()`, `read()`, `write()`, `close()`

### Device Management

- Request device
    
- Release device
    

### Information Maintenance

- Get/set time or date
    
- Get/set system information
    

---

# 🔥 GATE Corner — Remember These

1. **System call = controlled entry into kernel.**
    
2. User mode → **mode bit = 1**.
    
3. Kernel mode → **mode bit = 0** _(as used in this lecture)_.
    
4. System call causes **user → kernel → user** transition.
    
5. System-call number identifies the required OS service.
    
6. **System call table** maps number → kernel service.
    
7. API/library function may **wrap** a system call.
    
8. **Not every library/API function is a system call.**
    
9. System calls are slower than ordinary function calls.
    
10. `fork()` → process creation.
    
11. `open/read/write/close` → file operations.
    
12. User programs cannot directly execute **privileged instructions**.
    

### One-line mental model

> **API → System Call → Trap/Syscall → Kernel Mode → System Call Table → Kernel Service → Return to User Mode**

This is the **core chain** you should be able to reproduce from memory for GATE.