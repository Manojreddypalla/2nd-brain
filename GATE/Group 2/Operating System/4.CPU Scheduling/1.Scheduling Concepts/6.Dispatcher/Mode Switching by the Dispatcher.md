# Mode Switching by the Dispatcher

## Why is Mode Switching Needed?

The Dispatcher executes inside the **Operating System (Kernel Mode)**.

However, user programs must execute in **User Mode** for protection.

Before giving the CPU to a user process, the Dispatcher switches:

```text
Kernel Mode
      │
      ▼
User Mode
```

---

# Who Performs Mode Switching?

The **Dispatcher** initiates the mode switch.

The **CPU hardware** performs the actual switch by changing the **Mode Bit** (Privilege Bit).

> Dispatcher requests the switch; CPU hardware executes it.

---

# What is the Mode Bit?

Modern CPUs contain a special hardware bit called the **Mode Bit**.

Example:

```text
Mode Bit = 0 → Kernel Mode

Mode Bit = 1 → User Mode
```

*(Some architectures use the opposite convention. For GATE, remember only that a hardware mode bit distinguishes the two modes.)*

---

# Mode Switching Process

```text
Running Process Ends
        │
        ▼
CPU enters Kernel Mode
        │
Scheduler selects next process
        │
Dispatcher restores context
        │
Dispatcher requests User Mode
        │
CPU changes Mode Bit
        │
Execution resumes in User Mode
```

---

# Why Doesn't the Process Change the Mode?

Imagine if a user program could execute:

```cpp
mode = Kernel;
```

Then it could:

- Access any memory.
- Delete system files.
- Read passwords.
- Crash the operating system.

Therefore:

❌ User processes **cannot** change the mode.

Only the **Operating System**, executing in **Kernel Mode**, can trigger a valid mode switch.

---

# Hardware Support

The CPU provides hardware mechanisms for:

- Mode Bit
- Privileged Instructions
- Interrupts
- Exceptions
- System Calls

The Dispatcher uses these hardware features to safely transfer execution.

---

# Connection with Context Switching

```text
Save P1 Context
        │
Load P2 Context
        │
Kernel Mode
        │
Switch Mode Bit
        │
User Mode
        │
Resume P2
```

Mode switching is one step of the dispatch process.

---

# Important Facts

- Mode switching is performed **after** context switching.
- The Dispatcher switches from **Kernel Mode → User Mode** before the selected process executes.
- The CPU hardware changes the Mode Bit.
- User programs cannot directly switch themselves into Kernel Mode.

---

# Common Mistakes

❌ Dispatcher physically changes the CPU hardware.

✔ The Dispatcher invokes the mode change, but the CPU hardware updates the Mode Bit.

---

❌ User programs can switch themselves to Kernel Mode.

✔ Only controlled mechanisms such as **system calls, interrupts, or exceptions** allow entry into Kernel Mode.

---

# Quick Revision

```text
Scheduler
      │
Dispatcher
      │
Restore Context
      │
Kernel → User
      │
Transfer Control
      ▼
Running Process
```

## One-Liner

> **The Dispatcher requests the CPU to switch from Kernel Mode to User Mode by using the processor's hardware mode bit before transferring control to the selected process.**