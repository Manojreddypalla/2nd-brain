# GATE Focus Points — System Calls

## ⭐ Must-Know Concepts

- Definition of **System Call**
- Need for System Calls
- User Mode vs Kernel Mode
- Mode Switch (User → Kernel → User)
- Privileged vs Non-Privileged Instructions
- Why System Calls are slower than Function Calls

---

## ⭐ Types of System Calls

- Process Control
- File Management
- Device Management
- Information Maintenance
- Communication (IPC)

---

## ⭐ Important Linux System Calls

### Process
- `fork()`
- `exec()`
- `wait()`
- `exit()`

### File
- `open()`
- `read()`
- `write()`
- `close()`

### Information
- `getpid()`
- `getuid()`

### Communication
- `pipe()`
- `socket()`

---

## ⭐ Comparisons (Frequently Asked)

- API vs System Call
- Library Function vs System Call
- User Mode vs Kernel Mode
- Process vs Program *(later in Processes chapter)*

---

## ⭐ Standards

- POSIX
- glibc (GNU C Library)

---

## ⭐ Frequently Asked GATE Facts

- System Call is the **interface between User Program and Kernel**.
- System Calls execute in **Kernel Mode**.
- Applications execute in **User Mode**.
- System Calls require a **Mode Switch**.
- Hardware cannot be accessed directly from User Mode.
- `fork()` creates a child process.
- `exec()` replaces the current process image.
- `wait()` waits for child termination.
- `open() → read()/write() → close()` is the standard file operation sequence.
- Library functions often invoke system calls internally.