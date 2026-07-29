
# The Linux Roadmap I Would Follow

Don't start with kernel development.

That's like learning engine design before learning how a car works.

Instead:

```text
Linux User Space
        ↓
Processes
        ↓
Memory
        ↓
Filesystems
        ↓
Networking
        ↓
Systemd
        ↓
Kernel Internals
        ↓
Drivers
        ↓
Kernel Development
```

---

# Phase 1 — Become a Linux Power User

Goal:

```text
Know exactly what the OS is doing.
```

Topics:

### Filesystem

Learn:

```bash
/
├── bin
├── etc
├── home
├── proc
├── sys
├── dev
├── var
```

Questions:

```text
Why does /proc exist?
What is /sys?
What is /dev?
```

---

### Permissions

```bash
chmod
chown
groups
sudo
```

Understand:

```text
User
Group
Other
```

---

### Processes

Commands:

```bash
ps
top
htop
kill
pgrep
jobs
```

Understand:

```text
PID
Parent PID
Process Tree
Zombie Process
Daemon
```

---

### Essential Tool

```bash
man
```

Use it constantly.

---

# Phase 2 — Understand What Happens After Boot

This is where Linux gets interesting.

---

### Boot Process

Learn:

```text
BIOS/UEFI
 ↓
Bootloader (GRUB)
 ↓
Kernel
 ↓
initramfs
 ↓
systemd
 ↓
Services
```

Since you've already broken GRUB once 😄, this will make everything click.

---

### Systemd

Commands:

```bash
systemctl
journalctl
```

Understand:

```text
Services
Targets
Units
Boot Sequence
```

Examples:

```bash
systemctl status docker
systemctl restart jellyfin
```

---

# Phase 3 — Processes and Memory

This is the foundation of operating systems.

---

### Process Creation

Understand:

```c
fork()
exec()
wait()
```

Mental model:

```text
Parent
  |
fork()
  |
Child
```

Every Linux process starts here.

---

### Memory

Learn:

```text
Stack
Heap
Virtual Memory
Pages
Swap
```

Questions:

```text
Why does swap exist?
What is a page fault?
```

---

### Useful Commands

```bash
free -h
vmstat
top
htop
```

---

# Phase 4 — Networking

You'll love this because of cybersecurity.

---

### Learn

```text
IP
MAC
ARP
Routing
DNS
NAT
TCP
UDP
```

Then Linux tools:

```bash
ip
ss
netstat
ping
traceroute
tcpdump
```

---

### Important

Learn:

```bash
ip addr
ip route
```

instead of older:

```bash
ifconfig
```

---

# Phase 5 — Filesystems

This is a huge topic.

Learn:

```text
ext4
xfs
btrfs
fat32
ntfs
```

Understand:

```text
inode
block
mount
journal
```

Questions:

```text
How does Linux find a file?
What is an inode?
```

---

# Phase 6 — Kernel Fundamentals

Now you're ready.

---

### Learn What the Kernel Actually Does

```text
Process Scheduling
Memory Management
Filesystem Management
Networking
Drivers
```

Think:

```text
Kernel = Resource Manager
```

Everything goes through it.

---

### Learn

```text
System Calls
```

Examples:

```c
open()
read()
write()
close()
```

User Space:

```text
Application
```

Kernel Space:

```text
Kernel
```

System calls are the bridge.

---

# Phase 7 — Device Drivers

Since you're interested in hardware.

Learn:

```text
PCIe
USB
Kernel Modules
Drivers
```

Commands:

```bash
lsmod
modprobe
lspci
lsusb
```

---

### Understand

When you run:

```bash
nvidia-smi
```

What actually happens?

How does the NVIDIA driver talk to hardware?

This is where those answers live.

---

# Phase 8 — Kernel Development

Only after everything above.

Learn:

```c
Kernel Modules
```

Hello World module:

```c
module_init()
module_exit()
```

Then:

```text
Character Drivers
Device Drivers
Interrupts
DMA
```

---

# Books I'd Use

### Beginner

📖 **How Linux Works (Brian Ward)**

This is probably the single best book for you right now.

---

### Intermediate

📖 **Linux Command Line (William Shotts)**

---

### Advanced

📖 **Linux Kernel Development**  
by Robert Love

---

# For You Specifically

Because you're interested in:

- Cybersecurity
    
- Home Labs
    
- Docker
    
- Servers
    
- Linux Mint
    
- Hardware
    
- Kernel internals
    

I'd spend the next 30 days like this:

```text
Week 1
Filesystem + Permissions + Processes

Week 2
Boot Process + Systemd + Services

Week 3
Memory + Networking

Week 4
Kernel + System Calls + Drivers
```

By the end of that month, you'll understand _why_ Linux behaves the way it does, not just which commands to type. That's the point where debugging Docker, GPU drivers, Samba, Tailscale, or server issues becomes dramatically easier because you can reason about what's happening underneath.