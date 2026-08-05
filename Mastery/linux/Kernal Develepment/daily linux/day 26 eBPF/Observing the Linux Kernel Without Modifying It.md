# 🐧 Linux Internals — Day 26: eBPF Full Notes

## 1. What is eBPF?

**eBPF = Extended Berkeley Packet Filter**

eBPF is a Linux kernel technology that allows us to load **small, verified programs** that execute when specific events occur.

It lets us **observe and, for supported hook types, influence system behavior** without directly modifying and recompiling the Linux kernel.

### Mental Model

Think:

> **“When event X happens, run my eBPF program.”**

```text
Event X happens
      │
      ▼
  Kernel Hook
      │
      ▼
 eBPF Program
      │
      ▼
Observe / Count / Measure / Filter / Act
```

---

# 2. Why Do We Need eBPF?

Suppose we want to know:

```text
Which processes are opening files?

Which process created this network connection?

How long is a kernel operation taking?

Which processes are making certain system calls?

What packets are entering the machine?
```

We need visibility into what Linux is doing internally.

One approach would be to instrument or modify kernel code.

```text
Modify kernel/module
        ↓
Compile
        ↓
Load/reboot if needed
        ↓
Test
```

That's complicated and potentially dangerous.

eBPF provides a much more flexible mechanism:

```text
Write eBPF program
        ↓
Kernel verifies it
        ↓
Load program
        ↓
Attach to supported hook
        ↓
Event occurs
        ↓
Program executes
```

So:

> **eBPF extends/observes kernel behavior without directly changing kernel source code.**

---

# 3. eBPF Does NOT Mean Editing the Kernel

This distinction is important.

### Kernel modification

You literally change kernel source:

```text
Kernel Source

function xyz()
{
    ...
    MY NEW CODE
    ...
}
```

Then you have to build/use that changed code.

### eBPF

You leave the kernel source alone.

Instead:

```text
        Linux Kernel
             │
             ▼
        Existing Hook
             │
             ├──────→ eBPF Program
             │
             ▼
        Kernel continues
```

So a useful intuition is:

> **“I want to temporarily attach my own small behavior/observer to something happening in Linux.”**

Technically, you're attaching a program to a supported **hook**, not arbitrarily editing kernel code.

---

# 4. What is a Hook?

A **hook** is a point where an eBPF program can be attached so that it executes when a particular event occurs.

For example:

```text
Something happens
       ↓
     HOOK
       ↓
eBPF program executes
```

There are hooks associated with many areas of the system.

Examples include:

```text
System calls
Kernel functions
Tracepoints
Network processing
User-space functions
Security-related hooks
```

---

# 5. Example — File Activity

Suppose a process opens a file.

```text
Application
     │
     │ openat()
     ▼
Linux Kernel
     │
     ▼
Filesystem
```

We might attach tracing around an appropriate event:

```text
Application
     │
     │ openat()
     ▼
Kernel Event
     │
     ├────→ eBPF Program
     │          │
     │          └── Record PID / process / etc.
     │
     ▼
Filesystem
```

Important:

> **This is only ONE example of eBPF.**

eBPF is not a file-monitoring technology specifically.

---

# 6. Example — System Calls

You already know:

```text
Application
     │
     │ syscall
     ▼
Linux Kernel
```

eBPF can trace events associated with system calls.

```text
Application
     │
     │ syscall
     ▼
  [Hook]
     │
     ├────→ eBPF
     │
     ▼
Kernel handles syscall
```

This can help answer questions such as:

```text
How many times was a syscall called?

Which process called it?

How long did an operation take?
```

---

# 7. Example — Networking

Networking is a major eBPF use case.

Normally:

```text
Network
   │
   ▼
Packet
   │
   ▼
Linux Networking
   │
   ▼
Application
```

eBPF can attach at different networking points.

For example, with **XDP**:

```text
Network Packet
      │
      ▼
  NIC Driver
      │
      ▼
   XDP/eBPF
   /   |    \
  /    |     \
DROP  PASS  REDIRECT
       │
       ▼
Linux Network Stack
```

An eBPF program can potentially decide:

```text
PASS packet
DROP packet
Redirect packet
Collect statistics
```

This is why eBPF is powerful for:

- networking
    
- firewalls
    
- load balancing
    
- traffic monitoring
    
- DDoS mitigation
    

---

# 8. Example — Scheduler / Performance

Remember the scheduler:

```text
Process A
Process B
Process C
     │
     ▼
 Linux Scheduler
     │
     ▼
     CPU
```

eBPF can observe scheduling-related events.

For example:

```text
Process A stops
      ↓
Scheduler event
      ↓
    eBPF
      ↓
Record timing
      ↓
Process B runs
```

This helps performance engineers investigate things like:

```text
Why is my application slow?

How long was a process waiting?

Where is CPU time going?

Which kernel operations have high latency?
```

---

# 9. How eBPF Works Internally

Now the important architecture.

```text
Your eBPF Program
        │
        ▼
┌─────────────────┐
│  BPF Verifier   │
└────────┬────────┘
         │
      Accepted
         │
         ▼
   Program Loaded
         │
         ▼
 Attach to a Hook
         │
         ▼
     Event occurs
         │
         ▼
 eBPF Program Runs
```

There are three concepts you should remember:

```text
Program
Verifier
Hook
```

---

# 10. BPF Verifier

Running arbitrary code in kernel context would be extremely dangerous.

A bad kernel program could:

```text
Access invalid memory
Crash the system
Corrupt kernel data
Cause security problems
```

Therefore Linux has the **BPF verifier**.

```text
eBPF Program
     │
     ▼
┌──────────────┐
│ BPF Verifier │
│              │
│ Check program│
└──────┬───────┘
       │
    accepted
       │
       ▼
Kernel loads program
```

The verifier analyzes the program and enforces BPF's safety constraints before allowing it to run.

### Mental model

```text
eBPF program
      ↓
Security checkpoint
      ↓
Kernel
```

That checkpoint = **Verifier**.

---

# 11. eBPF Maps

Suppose your eBPF program counts system calls.

It needs somewhere to store:

```text
PID 100 → 56 calls
PID 200 → 21 calls
PID 300 → 89 calls
```

This is where **BPF Maps** are useful.

A BPF map is a kernel-managed data structure that BPF programs and, depending on configuration/type, user-space programs can use to exchange/store data.

```text
        Kernel
┌──────────────────────┐
│                      │
│ eBPF Program         │
│       │              │
│       ▼              │
│    BPF Map           │
│                      │
└──────────┬───────────┘
           │
           ▼
     User-space Tool
```

Think:

> **BPF Map = storage/shared state for eBPF programs.**

---

# 12. eBPF is Event Driven

Normally an eBPF tracing program isn't continuously doing:

```text
while(true)
    check_kernel();
```

Instead:

```text
WAIT

Event happens
     ↓
eBPF runs
     ↓
finishes

WAIT

Another event
     ↓
eBPF runs
```

So the key pattern is:

> **Event → Hook → eBPF Program**

---

# 13. `strace` vs eBPF

You already have a tool that can observe system calls:

```bash
strace ls
```

You may see calls such as:

```text
openat(...)
read(...)
write(...)
close(...)
```

Conceptually:

```text
        strace
           │
           ▼
          ls
           │
        syscalls
           │
           ▼
         Kernel
```

`strace` is fantastic when asking:

> **“What system calls is this process making?”**

eBPF can answer much broader questions.

```text
             eBPF
              │
     ┌────────┼─────────┐
     ▼        ▼         ▼
  Syscalls  Network   Scheduler
     │        │         │
     ▼        ▼         ▼
 Filesystem Packets   Processes
```

### Difference

|`strace`|eBPF|
|---|---|
|Mainly syscall/signal tracing|Many kinds of hooks/events|
|Usually selected processes|Can observe system-wide activity|
|User-space debugging tool|Kernel-supported programmable framework|
|Great for debugging one program|Great for observability/network/security/performance|

---

# 14. eBPF + XDP

**XDP = eXpress Data Path**

XDP allows BPF programs to process packets very early in the Linux networking path.

```text
Internet
   │
   ▼
Packet
   │
   ▼
NIC
   │
   ▼
Driver
   │
   ▼
XDP/eBPF
   │
   ├── DROP
   │
   ├── PASS
   │
   └── REDIRECT
```

Because processing happens early, it can be extremely efficient.

Used for things like:

```text
Packet filtering
DDoS mitigation
Load balancing
Traffic processing
```

---

# 15. eBPF in Containers / Kubernetes

Containers generate lots of:

```text
Processes
Network connections
System calls
Packets
```

We want visibility and control without modifying every application.

eBPF is a great fit.

For example, [Cilium](https://cilium.io/?utm_source=chatgpt.com) uses eBPF heavily for cloud-native networking, security and observability.

Conceptually:

```text
Container A
     │
     ▼
    eBPF
     │
     ├── Networking
     ├── Security
     └── Observability
     │
     ▼
Container B / Network
```

---

# 16. Important eBPF Tools

### `bpftool`

Low-level utility for inspecting/managing BPF objects.

Check:

```bash
bpftool version
```

List loaded programs:

```bash
sudo bpftool prog list
```

Think:

> `bpftool` → inspect/manage BPF.

---

### `bpftrace`

A higher-level tracing tool that makes it easier to write tracing programs backed by eBPF.

```bash
bpftrace --version
```

Think:

> `bpftrace` → quickly trace Linux behavior using BPF.

---

# 17. Where eBPF is Used

|Area|Purpose|
|---|---|
|🔍 Observability|See what's happening|
|⚡ Performance|Find bottlenecks/latency|
|🌐 Networking|Process/filter packets|
|🛡️ Security|Observe/enforce system behavior|
|📦 Containers|Monitor workloads|
|☸️ Kubernetes|Networking/security/observability|
|🐛 Debugging|Investigate system behavior|

---

# 🧠 Most Important Architecture

This is the diagram worth remembering:

```text
                 User Space
                     │
              eBPF Program
                     │
                     ▼
              BPF Verifier
                     │
                  verified
                     │
                     ▼
══════════════════════════════════
                 KERNEL
                     │
              Attach to Hook
                     │
       ┌─────────────┼────────────┐
       ▼             ▼            ▼
    Syscalls      Network     Scheduler
       │             │            │
       └─────────────┼────────────┘
                     │
                  EVENT
                     │
                     ▼
             eBPF Program Runs
                     │
                     ▼
                  BPF Map
                     │
══════════════════════════════════
                     │
                     ▼
               User-space Tool
```

---

# ✍️ Short Revision Notes

**eBPF:** Linux technology for running small, verified programs attached to supported kernel/user-space events and hooks.

**Hook:** Point/event where an eBPF program is attached.

**Verifier:** Checks an eBPF program against kernel safety rules before loading.

**BPF Map:** Kernel-managed storage/state used by BPF programs and often user-space applications.

**XDP:** High-performance eBPF-based packet processing at an early networking stage.

**bpftool:** Utility for inspecting/managing BPF objects.

**bpftrace:** High-level tracing tool built using BPF capabilities.

### ⭐ One line to remember

```text
EVENT → HOOK → eBPF PROGRAM → OBSERVE / MEASURE / ACT
```

Or in plain English:

> **eBPF lets you attach small programs to specific events in Linux so you can observe or influence what happens, without directly modifying the kernel source.**