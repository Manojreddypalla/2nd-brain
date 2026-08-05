# 🧪 Linux Internals — Day 26: eBPF Lab

Today don't worry about writing an eBPF program from scratch. First **see eBPF working**.

## Lab 1 — Check Kernel

```bash
uname -r
```

Example:

```text
6.12.10-amd64
```

`uname` → Unix name  
`-r` → kernel **release**

Modern Linux kernels have extensive eBPF support.

---

## Lab 2 — Check the Tools

```bash
bpftool version
```

Then:

```bash
bpftrace --version
```

If you get:

```text
command not found
```

### Debian / Ubuntu / Kali

```bash
sudo apt update
sudo apt install bpftrace bpftool
```

Don't worry if `bpftool` installation/package naming differs on your distro—we can handle that if it happens.

---

# Lab 3 — Look at Loaded eBPF Programs

Run:

```bash
sudo bpftool prog list
```

You might see:

```text
15: cgroup_device  name ...
    loaded_at ...
    uid 0
    xlated ...
```

Each entry represents an eBPF program currently loaded into the kernel.

Think:

```text
User/tool
    │
    │ loaded
    ▼
Linux Kernel
┌─────────────────────┐
│ BPF Program #15     │
│ BPF Program #23     │
│ BPF Program #31     │
└─────────────────────┘
```

If the list is empty, that's also fine.

---

# 🔥 Lab 4 — Your First Actual eBPF Trace

First see available syscall tracepoints:

```bash
sudo bpftrace -l 'tracepoint:syscalls:sys_enter_*' | head
```

You should get entries resembling:

```text
tracepoint:syscalls:sys_enter_openat
tracepoint:syscalls:sys_enter_read
tracepoint:syscalls:sys_enter_write
...
```

These are places where we can attach our eBPF tracing program.

Now the fun part.

Run:

```bash
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { printf("%s\n", comm); }'
```

Don't type anything else in that terminal.

Open **another terminal** and run:

```bash
ls
```

Then:

```bash
cat /etc/hostname
```

Go back to the first terminal.

You should see process names appearing, such as:

```text
ls
cat
bash
...
```

🎯 **You just used eBPF to observe file-open activity happening through that syscall tracepoint.**

Stop with:

```text
Ctrl + C
```

---

# 🧠 Understand What You Just Did

This command:

```bash
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { printf("%s\n", comm); }'
```

looks scary, but conceptually it's only:

```text
WHEN:
    openat syscall happens

DO:
    print process name
```

Internally:

```text
Process
   │
   │ openat()
   ▼
Linux Kernel
   │
   ▼
[sys_enter_openat tracepoint]
   │
   ├──────→ eBPF program
   │             │
   │             ▼
   │       print process name
   │
   ▼
Kernel continues
```

This is exactly the:

```text
EVENT
  ↓
HOOK
  ↓
eBPF PROGRAM
  ↓
ACTION
```

model from your notes.

---

# Lab 5 — Count Instead of Print

Now let's change **what our BPF program does**.

Run:

```bash
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { @[comm] = count(); }'
```

Use your system normally for ~10 seconds.

Run commands in another terminal:

```bash
ls
cat /etc/hostname
pwd
ls /etc
```

Then return and press:

```text
Ctrl + C
```

You may get something like:

```text
@[bash]: 4
@[ls]: 15
@[cat]: 2
```

Now notice something important.

Same event:

```text
openat()
```

But we changed our program from:

```text
PRINT
```

to:

```text
COUNT
```

That's the **programmable** part of eBPF.

---

# 🆚 Lab 6 — Compare With `strace`

Run:

```bash
strace ls
```

You'll see lots of:

```text
openat(...)
read(...)
write(...)
close(...)
```

`strace` is basically answering:

> What syscalls is **this `ls` process** making?

Whereas our eBPF experiment was asking:

> Whenever **any process** hits this particular syscall tracepoint, show/count it.

```text
              strace

                ls
                │
                ▼
             syscalls
                │
                ▼
              Kernel


               eBPF

bash ───┐
ls ─────┤
cat ────┼──→ openat event
python ─┤         │
... ────┘         ▼
                 eBPF
```

---

## ✅ What You Need to Understand Today

Don't memorize the `bpftrace` syntax yet.

Just make sure this clicks:

```text
I selected an event
        ↓
openat syscall entry
        ↓
I attached an eBPF program
        ↓
event occurred
        ↓
my program executed
        ↓
I observed the result
```

That's your **first real eBPF mental model**.

If Lab 4 works, Day 26 is basically successful.