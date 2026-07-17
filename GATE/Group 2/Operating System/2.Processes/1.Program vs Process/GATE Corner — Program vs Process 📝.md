# GATE Corner — Program vs Process 📝

### Core Definitions

- **Program:** A passive executable file stored on secondary storage (disk/SSD).
    
- **Process:** An active instance of a program that is currently executing.
    

---

### Key Differences

|Program|Process|
|---|---|
|Passive entity|Active entity|
|Stored on disk|Exists in RAM while executing|
|No execution state|Has execution state|
|Does not use CPU|Uses CPU time|
|Static|Dynamic|

---

### Important Facts

- A **program becomes a process** when it is executed.
    
- A **single program can have multiple processes** running simultaneously.
    
- Every process has its own:
    
    - Process ID (PID)
        
    - Program Counter (PC)
        
    - CPU Registers
        
    - Stack
        
    - Heap
        
    - Data section
        
    - Execution state
        

---

### Memory

- **Program:** Stored permanently on secondary storage.
    
- **Process:** Loaded into main memory (RAM) for execution.
    

---

### System Call Connection

- Process creation: `fork()`
    
- Execute a new program: `exec()`
    
- Wait for child process: `wait()`
    
- Terminate process: `exit()`
    

---

### Frequently Asked GATE Points

- ✔ **Program is passive; Process is active.**
    
- ✔ **One program can create multiple processes.**
    
- ✔ **A process is not just code—it includes its execution context (PC, registers, stack, heap, etc.).**
    
- ✔ **Processes are created, managed, and terminated by the Operating System.**
    
- ✔ **Process management operations are performed through system calls.**
    

---

### One-Line Revision

> **Program = Passive executable on disk. Process = Active executing instance of a program in memory with its own execution context.**