# GATE PYQs & Practice Questions — System Calls

> **Instructions:** Try to solve all questions before checking the answers. The answer key is at the end.

---

## Q1. GATE (Concept)

A system call is primarily used for

A) Communication between two user processes only

B) Requesting services from the operating system kernel

C) Executing arithmetic operations

D) Compiling programs

---

## Q2. GATE (Concept)

Which mode has the privilege to execute hardware-specific instructions?

A) User Mode

B) Kernel Mode

C) Supervisor Mode of Applications

D) None of the above

---

## Q3. GATE (Concept)

A process requests a service from the operating system through

A) Interrupt

B) System Call

C) Context Switch

D) Scheduler

---

## Q4. GATE (Concept)

Which of the following is **NOT** a system call?

A) fork()

B) printf()

C) read()

D) open()

---

## Q5. GATE (Concept)

Which sequence correctly represents file operations in Linux?

A) read → open → write → close

B) open → read/write → close

C) close → open → read

D) write → read → open

---

## Q6. GATE (Concept)

The CPU switches from User Mode to Kernel Mode when

A) A variable is declared

B) A function is called

C) A system call occurs

D) A loop executes

---

## Q7. GATE (Concept)

Which of the following creates a new child process?

A) exec()

B) fork()

C) wait()

D) open()

---

## Q8. GATE (Concept)

The primary purpose of `exec()` is to

A) Create a child process

B) Wait for child completion

C) Replace the current process image with a new program

D) Terminate the process

---

## Q9. GATE (Concept)

Which system call is used by a parent process to wait for a child process?

A) fork()

B) exit()

C) wait()

D) exec()

---

## Q10. GATE (Concept)

Why are system calls slower than normal function calls?

A) They use recursion

B) They require mode switching and kernel intervention

C) They use dynamic memory

D) They execute twice

---

## Q11. GATE (Concept)

Which of the following belongs to **Information Maintenance** system calls?

A) read()

B) getpid()

C) write()

D) exec()

---

## Q12. GATE (Concept)

Which one of the following statements is TRUE?

A) User programs can directly access hardware.

B) Applications always execute in Kernel Mode.

C) System calls provide controlled access to kernel services.

D) Kernel executes in User Mode.

---

## Q13. GATE (Concept)

Which one of the following is **not** a file management system call?

A) open()

B) read()

C) fork()

D) close()

---

## Q14. GATE (Concept)

POSIX primarily defines

A) CPU Architecture

B) Programming language syntax

C) Standard operating system interfaces including system calls

D) Network topology

---

## Q15. GATE (Concept)

Which of the following best describes the relationship between a library function and a system call?

A) Every library function is a system call.

B) Every system call is a library function.

C) A library function may internally invoke one or more system calls.

D) They are unrelated.

---

# Answer Key

| Q | Answer |
|---|--------|
| 1 | **B** |
| 2 | **B** |
| 3 | **B** |
| 4 | **B** (`printf()` is a library function) |
| 5 | **B** |
| 6 | **C** |
| 7 | **B** |
| 8 | **C** |
| 9 | **C** |
| 10 | **B** |
| 11 | **B** |
| 12 | **C** |
| 13 | **C** |
| 14 | **C** |
| 15 | **C** |

---

# GATE Tip

Questions from this topic are usually **conceptual**, not code-based. Focus on:
- User Mode vs Kernel Mode
- System Call vs Library Function
- API vs System Call
- POSIX
- `fork()`, `exec()`, `wait()`
- `open()`, `read()`, `write()`, `close()`
- Types of System Calls
- Mode Switching