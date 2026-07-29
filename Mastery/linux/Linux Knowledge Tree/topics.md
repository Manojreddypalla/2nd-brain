```
LINUX
│
├── 1. Linux Architecture ⭐⭐⭐⭐⭐
├── 2. Process Management ⭐⭐⭐⭐⭐
├── 3. Memory Management ⭐⭐⭐⭐⭐
├── 4. Filesystem ⭐⭐⭐⭐⭐
├── 5. Permissions & Security ⭐⭐⭐⭐⭐
├── 6. Users & Groups ⭐⭐⭐⭐☆
├── 7. Linux Networking ⭐⭐⭐⭐⭐
├── 8. Namespaces ⭐⭐⭐⭐⭐
├── 9. Cgroups ⭐⭐⭐⭐⭐
├── 10. Capabilities ⭐⭐⭐⭐⭐
├── 11. Seccomp ⭐⭐⭐⭐⭐
├── 12. System Calls ⭐⭐⭐⭐⭐
├── 13. Signals ⭐⭐⭐⭐☆
├── 14. Scheduling ⭐⭐⭐⭐☆
├── 15. Boot Process ⭐⭐⭐⭐☆
├── 16. systemd ⭐⭐⭐⭐⭐
├── 17. Storage & Mounting ⭐⭐⭐⭐☆
├── 18. Shell & Bash ⭐⭐⭐⭐☆
├── 19. IPC ⭐⭐⭐⭐☆
├── 20. Containers Internals ⭐⭐⭐⭐⭐
├── 21. Kernel Modules ⭐⭐⭐☆☆
├── 22. Virtualization ⭐⭐⭐⭐☆
├── 23. Performance & Debugging ⭐⭐⭐⭐⭐
└── 24. Security Hardening ⭐⭐⭐⭐☆
```
# 1. Linux Architecture ⭐⭐⭐⭐⭐

The foundation.

Topics:

- Kernel vs User Space
- Shell
- Bootloader
- Init System
- Libraries
- Process Life Cycle
- System Calls

Mental model:

```
Application      │System Call      │Kernel      │Hardware
```

---

# 2. Process Management ⭐⭐⭐⭐⭐

Everything in Linux starts here.

Topics:

- Process
- Thread
- fork()
- exec()
- clone()
- Parent/Child
- Zombie
- Orphan
- Daemon
- Process States
- Process Priority

Commands

```
pstophtoppstreekillnicerenice
```

---

# 3. Memory Management ⭐⭐⭐⭐⭐

Understand RAM like an OS developer.

Topics

- Virtual Memory
- Physical Memory
- Stack
- Heap
- mmap()
- Paging
- Swapping
- OOM Killer
- Page Cache

Commands

```
freevmstatcat /proc/meminfo
```

---

# 4. Filesystem ⭐⭐⭐⭐⭐

Topics

- VFS
- ext4
- XFS
- inode
- superblock
- hard links
- soft links
- mount
- OverlayFS

Commands

```
lsblkdfdumountfindmnt
```

---

# 5. Linux Security ⭐⭐⭐⭐⭐

Topics

- Permissions
- ACL
- SUID
- SGID
- Sticky Bit
- Capabilities
- Seccomp
- SELinux
- AppArmor

---

# 6. Users & Groups

Topics

- UID
- GID
- passwd
- shadow
- sudo
- PAM

Commands

```
idwhoamigroupspasswdsudo
```

---

# 7. Networking ⭐⭐⭐⭐⭐

Topics

- TCP/IP
- Routing
- Bridge
- NAT
- DNS
- ARP
- VLAN
- Bonding
- Firewall
- iptables
- nftables

Commands

```
ipsspingtraceroutedigtcpdump
```

---

# 8. Namespaces ⭐⭐⭐⭐⭐

Remember

```
What can I see?
```

Types

- PID
- Mount
- Network
- UTS
- IPC
- User
- Time

Commands

```
unsharelsnsnsenter
```

---

# 9. Cgroups ⭐⭐⭐⭐⭐

Remember

```
How much can I use?
```

Topics

- CPU
- Memory
- I/O
- PID
- cgroup v2

Commands

```
cat /proc/self/cgroupls /sys/fs/cgroup
```

---

# 10. Linux Capabilities ⭐⭐⭐⭐⭐

Remember

```
What am I allowed to do?
```

Commands

```
getcapsetcapcapsh
```

---

# 11. Seccomp ⭐⭐⭐⭐⭐

Remember

```
Which system calls may I make?
```

Topics

- seccomp
- seccomp-bpf
- Docker default profile

---

# 12. System Calls ⭐⭐⭐⭐⭐

Most important topic.

Topics

- open()
- read()
- write()
- close()
- fork()
- execve()
- clone()
- wait()
- socket()
- mmap()

Tool

```
strace
```

---

# 13. Signals

Topics

- SIGINT
- SIGTERM
- SIGKILL
- SIGSTOP
- SIGCONT

Commands

```
killkillalltrap
```

---

# 14. Scheduling

Topics

- CFS
- nice
- realtime scheduling
- CPU affinity

Commands

```
nicerenicetaskset
```

---

# 15. Boot Process

Topics

```
BIOS/UEFI↓GRUB↓Kernel↓initramfs↓systemd↓Services↓Login
```

---

# 16. systemd ⭐⭐⭐⭐⭐

Topics

- Units
- Services
- Targets
- Timers
- Journal

Commands

```
systemctljournalctl
```

---

# 17. Storage

Topics

- Partitions
- LVM
- RAID
- Mount
- UUID
- fstab

Commands

```
lsblkblkidmountumount
```

---

# 18. Bash

Topics

- Variables
- Pipes
- Redirection
- grep
- awk
- sed
- xargs

---

# 19. IPC

Topics

- Pipes
- Named Pipes
- Shared Memory
- Semaphores
- Message Queues
- UNIX Sockets

---

# 20. Container Internals ⭐⭐⭐⭐⭐

Everything comes together.

```
Namespaces        +Cgroups        +Capabilities        +Seccomp        +OverlayFS        +Networking        +OCI Runtime        +containerd        +Docker
```

---

# 21. Kernel Modules

Topics

- insmod
- modprobe
- lsmod
- rmmod

---

# 22. Virtualization

Topics

- KVM
- QEMU
- Hypervisor
- VM vs Container

---

# 23. Performance & Debugging ⭐⭐⭐⭐⭐

Commands

```
tophtopvmstatiostatiotopperfstracelsofsstcpdump
```

These are heavily used in production.

---

# 24. Security Hardening

Topics

- Firewall
- Fail2Ban
- SSH Hardening
- SELinux
- AppArmor
- Auditd

---

# 🚀 The Linux Revision Sheet (One-Line Memory)

```
Kernel           → Controls hardwareShell            → Talks to kernelProcess          → Running programThread           → Lightweight execution pathfork()           → Create processexecve()         → Replace process imageclone()          → Create process/thread with shared resourcesSystem Call      → Gateway to kernelVirtual Memory   → Process memory abstractionStack            → Function callsHeap             → Dynamic allocationmmap()           → Memory mappingFilesystem       → Organizes storageinode            → Metadata of a fileMount            → Attach filesystemOverlayFS        → Layered filesystemPermission       → rwxCapability       → Fine-grained root privilegesNamespace        → What can I see?Cgroup           → How much can I use?Seccomp          → Which syscalls can I make?Signal           → Process notificationScheduler        → CPU allocationDaemon           → Background servicesystemd          → Service managerNetwork Namespace→ Isolated network stackveth             → Virtual cableBridge           → Virtual switchNAT              → Internet access
```

---

# The Learning Order I'd Recommend

```
Linux Architecture        │Processes        │Memory        │Filesystem        │Users & Permissions        │Networking        │System Calls        │Signals        │Namespaces        │Cgroups        │Capabilities        │Seccomp        │OverlayFS        │systemd        │Containers        │Docker        │Kubernetes
```

This order isn't random—it mirrors how the Linux kernel is structured. Each topic builds naturally on the previous one, so by the time you reach Docker and Kubernetes, you'll understand the underlying mechanisms rather than treating them as separate technologies. It's also an excellent progression for Linux, DevOps, systems programming, backend, and security interviews.