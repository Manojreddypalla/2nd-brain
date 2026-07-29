Linux **Namespaces** are one of the most important kernel features to understand if you want to learn **Docker, Kubernetes, containers, DevOps, Linux internals, cybersecurity, or even operating system design**.

Think of namespaces as **creating multiple "virtual views" of the same Linux kernel**.

---

# First, Why Do Namespaces Exist?

Imagine a normal Linux machine.

```
                Linux Kernel
                    |
     -------------------------------
     |      |      |      |        |
   Proc1  Proc2  Proc3  Proc4   Proc5
```

Every process sees:

- Same PID list
    
- Same hostname
    
- Same network interfaces
    
- Same mounted disks
    
- Same IPC objects
    
- Same users
    

Everything is shared.

---

Now imagine Docker.

You run

```
docker run ubuntu
```

Inside Docker

```
ps
```

shows

```
PID 1 bash
PID 12 nginx
```

But outside

```
ps aux
```

shows

```
23891 bash
23892 nginx
```

How?

Docker didn't install another kernel.

It uses **Namespaces**.

---

# The Big Mental Model

Imagine one huge office building.

```
Entire Linux Machine

Floor 1
Floor 2
Floor 3
Floor 4
```

Normally everyone can walk everywhere.

Namespaces install invisible walls.

```
Container A

Floor 1 only

Container B

Floor 2 only

Container C

Floor 3 only
```

Everyone thinks

> "This whole building belongs to me."

Reality?

They're sharing the same building.

---

# Kernel View

Internally every process has

```
task_struct
```

Inside

```
task_struct
   |
   |---- nsproxy
              |
      -------------------
      |   |   |   |   |
     pid net ipc mnt uts user
```

Every process points to a namespace object.

Processes pointing to the same namespace belong together.

Example

```
Process A ----\
               \
Process B ------> PID Namespace X

Process C -----> PID Namespace Y
```

A and B can see each other.

C cannot.

---

# What Happens During fork()

Normally

```
fork()
```

copies

- memory
    
- file descriptors
    
- registers
    

But namespace information is also copied.

Unless

```
clone()
```

creates a new namespace.

Example

```
clone(CLONE_NEWPID)
```

creates

```
Parent

PID Namespace A

Child

PID Namespace B
```

The child now sees a completely different process tree.

---

# Linux has Seven Major Namespaces

```
PID
NET
MNT
IPC
UTS
USER
CGROUP
```

Let's study each deeply.

---

# 1. PID Namespace

Probably the easiest.

Without namespace

```
PID

1 init
100 ssh
250 chrome
500 nginx
```

Everyone sees

```
1
100
250
500
```

---

Inside new PID namespace

```
Container

1 bash
5 nginx
10 python
```

Outside

```
23891 bash
23892 nginx
23895 python
```

One process.

Two PID values.

---

Kernel keeps translation tables.

```
Real PID

23891

Namespace PID

1
```

Every namespace has its own numbering.

---

Special Rule

PID 1 is special.

If PID 1 exits

the namespace dies.

Exactly why Docker stops when the main process exits.

---

Command

```
unshare --pid --fork bash
```

Now

```
ps
```

may show

```
PID CMD

1 bash
```

---

# 2. Mount Namespace

Every process has a filesystem view.

Normally

```
/

bin
etc
home
mnt
```

Everyone sees same mounts.

---

Create mount namespace

```
unshare --mount bash
```

Now

```
mount
```

inside affects only this namespace.

Example

```
mount /dev/sdb /mnt
```

Outside

```
nothing changed
```

---

Docker uses this heavily.

Container filesystem

```
/

bin
etc
usr
tmp
```

is actually

OverlayFS

```
Lower Layer
Upper Layer
Merged View
```

Each container gets different mount namespace.

---

# 3. Network Namespace

This is magic.

Normally

```
ip addr
```

shows

```
eth0
lo
wlan0
```

Everyone sees same interfaces.

---

New network namespace

```
unshare --net bash
```

Now

```
ip addr
```

shows only

```
lo
```

No internet.

Why?

No network devices attached yet.

---

Docker creates

```
veth pair
```

Think of a virtual Ethernet cable.

```
Host

veth0 ---------------- veth1

Container
```

Host connects

```
veth0
```

to

```
docker0 bridge
```

Now container gets internet.

---

Inside container

```
eth0
```

Outside

```
veth2d91...
```

---

# 4. UTS Namespace

UTS

stands for

```
UNIX Time Sharing
```

Today it mainly isolates

```
hostname
domainname
```

Example

Outside

```
hostname

ubuntu
```

Inside

```
unshare --uts bash

hostname container1
```

Now

Inside

```
container1
```

Outside

```
ubuntu
```

---

Docker uses this.

Each container has

```
hostname
```

matching container ID.

---

# 5. IPC Namespace

IPC

Inter Process Communication

Includes

```
Shared Memory

Semaphore

Message Queue
```

Without namespace

all processes can access IPC objects.

With namespace

only container members can.

---

Example

Database creates shared memory.

Container A

```
Shared Memory

1234
```

Container B

Cannot see it.

---

Commands

```
ipcs
```

```
ipcrm
```

---

# 6. User Namespace

This one is mind-blowing.

Suppose

Inside container

```
root

UID 0
```

Outside

```
UID 1001
```

Same process.

Different user mapping.

---

Kernel stores mapping

```
Inside UID

0

Outside UID

1001
```

Inside

```
whoami

root
```

Outside

Not root.

Huge security feature.

Root inside container ≠ root on host.

---

Check

```
cat /proc/self/uid_map
```

Example

```
0 100000 65536
```

Means

```
Container UID 0

maps to

Host UID 100000
```

---

# 7. Cgroup Namespace

Not resource control.

That's cgroups.

This namespace hides cgroup paths.

Without it

```
cat /proc/self/cgroup
```

shows host hierarchy.

Inside namespace

container only sees its own hierarchy.

Useful for isolation.

---

# How Docker Starts a Container

Simplified

```
clone(
    CLONE_NEWNS |
    CLONE_NEWNET |
    CLONE_NEWPID |
    CLONE_NEWIPC |
    CLONE_NEWUTS |
    CLONE_NEWUSER
)
```

Then

```
mount filesystem

↓

configure network

↓

set hostname

↓

change root

↓

exec bash
```

Done.

Container created.

---

# Where Are Namespace Details Stored?

Look at

```
ls -l /proc/self/ns
```

Example

```
mnt -> mnt:[4026531840]
pid -> pid:[4026531836]
net -> net:[4026531993]
uts -> uts:[4026532001]
ipc -> ipc:[4026531992]
user -> user:[4026531837]
cgroup -> cgroup:[4026531835]
```

Each link points to a namespace object managed by the kernel. Processes with the same namespace ID share that namespace.

---

# Creating Namespaces with `unshare`

Create a new UTS namespace:

```bash
sudo unshare --uts bash
hostname container-test
hostname
```

Create a new PID namespace:

```bash
sudo unshare --pid --fork bash
ps -ef
```

Create a new network namespace:

```bash
sudo unshare --net bash
ip addr
```

Create a new mount namespace:

```bash
sudo unshare --mount bash
mount
```

---

# Switching into Another Process's Namespace

Instead of creating a new namespace, you can join an existing one using `nsenter`.

Example:

```bash
sudo nsenter --target <PID> --mount --uts --ipc --net --pid
```

This is extremely useful for debugging containers because you can "step inside" the namespaces of a running process.

---

# Namespaces vs Cgroups

Many people confuse these, but they solve different problems.

|Feature|Namespace|Cgroup|
|---|---|---|
|Purpose|Isolation|Resource control|
|Hides processes|✅|❌|
|Hides filesystem|✅|❌|
|Hides network|✅|❌|
|Limits CPU|❌|✅|
|Limits RAM|❌|✅|
|Limits I/O|❌|✅|

A container typically combines **both**:

- **Namespaces** make the process _feel_ like it's on its own machine.
    
- **Cgroups** prevent it from consuming all the host's resources.
    

---

# The Big Picture

Everything connects like this:

```
                Linux Kernel
                     │
        ┌────────────┴────────────┐
        │                         │
   Namespaces                 Cgroups
   (Isolation)          (Resource Limits)
        │                         │
        └────────────┬────────────┘
                     │
                 Container Runtime
             (Docker, Podman, containerd)
                     │
                One "Container"
```

The key realization is that **a container is not a virtual machine**. There is **one Linux kernel** shared by all containers. Namespaces give each container its own isolated view of kernel resources, while cgroups enforce resource limits. Together, they create the illusion that every container has its own independent Linux system, even though they're all running on the same kernel.