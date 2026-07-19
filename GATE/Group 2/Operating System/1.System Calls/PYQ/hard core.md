# GATE-Level System Calls Questions (Hard)

---

## Q1. GATE CSE 2023 (MSQ)

Which one or more of the following options **guarantee** that a computer system will transition from **User Mode to Kernel Mode**?

A) Function Call

B) malloc() Call

C) Page Fault

D) System Call

---

## Q2. GATE CSE

Which one or more of the following statements regarding **fork()** are TRUE?

A) fork() creates a new process.

B) The child process gets a copy of the parent's address space.

C) Parent and child always execute in a fixed order.

D) fork() returns twice.

---

## Q3. GATE CSE

Consider the following code.

```c
int x = 10;

if(fork()==0)
    x = x + 5;
else
    x = x - 5;

printf("%d\n",x);
```

Which of the following statements is correct?

A) Parent prints 5, Child prints 15

B) Parent prints 15, Child prints 5

C) Both print 10

D) Undefined behavior

---

## Q4. GATE CSE

After a successful call to `fork()`,

A) Parent and child share the same PID.

B) Parent and child execute independently.

C) Child begins execution from `main()`.

D) Child always executes before parent.

---

## Q5. GATE CSE

Which of the following statements about `exec()` is TRUE?

A) Creates a new child process.

B) Replaces the current process image.

C) Terminates the parent process.

D) Waits for child completion.

---

## Q6. GATE CSE

Consider the following sequence.

```
fork()
exec()
wait()
```

Which process executes `wait()`?

A) Child

B) Parent

C) Both

D) Neither

---

## Q7. GATE CSE

Which one of the following **must** be executed in Kernel Mode?

A) Integer addition

B) Function Call

C) Context Switch

D) Variable Assignment

---

## Q8. GATE CSE

Which one or more of the following operations necessarily require a System Call?

A) Reading a file

B) Creating a process

C) Opening a socket

D) Multiplying two integers

---

## Q9. GATE CSE

Which statement is TRUE regarding a System Call?

A) Every System Call causes a Context Switch.

B) Every System Call causes a Mode Switch.

C) Every Mode Switch causes a Process Switch.

D) Every Function Call is a System Call.

---

## Q10. GATE CSE

Which one of the following statements is FALSE?

A) System Calls execute in Kernel Mode.

B) Library functions may invoke System Calls.

C) User programs can directly execute privileged instructions.

D) POSIX defines a standard interface.

---

## Q11. GATE CSE

Which of the following is NOT guaranteed after a successful `fork()`?

A) Parent and child have different PIDs.

B) Child inherits open file descriptors.

C) Parent executes before child.

D) Both continue execution after fork().

---

## Q12. GATE CSE

Suppose a process executes

```c
fd = open("abc.txt", O_RDONLY);
read(fd, buf, 100);
close(fd);
```

How many System Calls are guaranteed?

A) 1

B) 2

C) 3

D) 4

---

## Q13. GATE CSE

Which one of the following is NOT a POSIX System Call?

A) read()

B) write()

C) open()

D) printf()

---

## Q14. GATE CSE

Which statement best explains why applications normally invoke System Calls through library wrappers instead of directly executing the `syscall` instruction?

A) Library wrappers improve portability.

B) System Calls cannot be called directly.

C) Kernel cannot understand syscall numbers.

D) User programs always execute in Kernel Mode.

---

## Q15. GATE CSE

A process executes

```c
pid = fork();

if(pid == 0)
    exec("P2");
else
    wait(NULL);
```

Which statement is correct?

A) Parent is replaced by P2.

B) Child is replaced by P2.

C) Both become P2.

D) Parent waits before fork().

---

## Q16. GATE CSE

Which of the following statements about **System Calls vs Library Functions** is TRUE?

A) Every library function causes a mode switch.

B) Every system call is a library function.

C) A library function may execute completely in User Mode.

D) System Calls execute entirely in User Mode.

---

## Q17. GATE CSE

Which one or more of the following events can transfer control from User Mode to Kernel Mode?

A) System Call

B) Hardware Interrupt

C) Page Fault

D) Divide-by-zero Exception

---

## Q18. GATE CSE

Which one of the following is a **Process Control** System Call?

A) read()

B) fork()

C) getpid()

D) socket()

---

## Q19. GATE CSE

Which one of the following System Calls is primarily used for **Inter-Process Communication**?

A) pipe()

B) open()

C) exec()

D) getpid()

---

## Q20. GATE CSE

Which statement is TRUE?

A) A System Call always creates a new process.

B) A System Call always performs disk I/O.

C) A System Call is the interface between user programs and the kernel.

D) A System Call is identical to an API.