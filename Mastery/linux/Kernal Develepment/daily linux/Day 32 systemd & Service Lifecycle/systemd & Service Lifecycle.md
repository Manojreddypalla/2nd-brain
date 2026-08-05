# 🐧 Linux Internals — Day 32: `systemd` & Service Lifecycle

> 🎯 **Core idea:** `systemd` is the manager that starts, stops, monitors, and organizes many of the services/processes that make up a running Linux system.

---

# 1. What Happens When Linux Boots?

You press the power button.

Very simplified:

```text
Power On
   ↓
Firmware
   ↓
Bootloader
   ↓
Linux Kernel
   ↓
Kernel initializes
CPU / Memory / Drivers
   ↓
Starts first userspace process
   ↓
PID 1
   ↓
systemd
```

On most modern distributions using systemd:

```text
PID 1 = systemd
```

Check:

```bash
ps -p 1 -o pid,comm,args
```

You may see:

```text
PID   COMMAND
1     systemd
```

So:

> **Kernel gets the machine running → systemd gets userspace/services running.**

---

# 2. Why Do We Need `systemd`?

Imagine Linux has many services:

```text
SSH server
Web server
Docker
Database
Bluetooth
Network services
Logging
Cron-like timers
...
```

Someone needs to:

```text
Start them
Stop them
Restart them
Track them
Handle dependencies
Collect/manage logs
Start them at boot
Group their processes
```

That's one of systemd's major jobs.

Think:

```text
              systemd
                 │
       ┌─────────┼─────────┐
       ▼         ▼         ▼
      SSH      Docker    Database
       │         │         │
    process   processes   process
```

---

# 3. `systemd` vs `systemctl` ⭐

This distinction matters.

### `systemd`

The actual **init/service manager** running on the system.

```text
systemd
   ↓
PID 1
   ↓
manages services
```

### `systemctl`

A command-line tool used to communicate with systemd.

When you type:

```bash
sudo systemctl start nginx
```

it's conceptually:

```text
You
 │
 ▼
systemctl
 │
 │ request
 ▼
systemd
 │
 ▼
start nginx.service
```

So:

> **`systemd` = manager**
> 
> **`systemctl` = control interface/CLI**

---

# 4. What is a Unit?

Systemd doesn't only manage services.

It manages objects called:

> **Units**

Different unit types represent different things.

Examples:

```text
.service → service/process

.socket  → socket activation

.timer   → scheduled activation

.mount   → filesystem mount

.target  → group/synchronization point

.path    → filesystem path monitoring
```

For now, the most important is:

```text
.service
```

Example:

```text
ssh.service
nginx.service
docker.service
```

---

# 5. What is a Unit File?

A **unit file** tells systemd how a unit should behave.

Common locations include:

```text
/etc/systemd/system/
```

Local/admin-created configuration and overrides.

And depending on distribution:

```text
/usr/lib/systemd/system/
```

or:

```text
/lib/systemd/system/
```

for package-provided units.

---

# 6. Understanding a Service File ⭐

Example:

```ini
[Unit]
Description=My Application
After=network.target

[Service]
ExecStart=/usr/bin/myapp
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Don't memorize it. Understand the three sections.

---

## `[Unit]`

Describes the unit and its relationships/dependencies.

```ini
[Unit]
Description=My Application
After=network.target
```

`Description`:

```text
Human-readable description
```

`After=`:

```text
Ordering relationship
```

So:

```ini
After=network.target
```

means roughly:

> If both units are being started, order this unit after `network.target`.

Important subtlety: **`After=` controls ordering; by itself it does not pull/start the other unit as a dependency.**

---

# 7. `[Service]`

This defines how the service actually runs.

```ini
[Service]
ExecStart=/usr/bin/myapp
Restart=on-failure
```

### `ExecStart`

The command systemd should start.

```text
systemd
   ↓
ExecStart
   ↓
/usr/bin/myapp
   ↓
Process
```

### `Restart=on-failure`

If the service unexpectedly fails, systemd can restart it according to the configured restart policy.

```text
myapp
  ↓
💥 fails
  ↓
systemd notices
  ↓
restart
  ↓
myapp running
```

This is one reason systemd is more than a simple boot script runner.

It actively tracks service state.

---

# 8. `[Install]`

Example:

```ini
[Install]
WantedBy=multi-user.target
```

This is mainly involved when you **enable** the unit.

It describes how the unit should be connected into systemd's startup structure.

We'll come back to `enable`.

---

# 9. Service Lifecycle

A service can move through states such as:

```text
Inactive
   │
   │ start
   ▼
Activating
   │
   ▼
Active
   │
   │ stop
   ▼
Deactivating
   │
   ▼
Inactive
```

Or:

```text
Active
   ↓
Something fails
   ↓
Failed
```

Systemd tracks these states.

Check with:

```bash
systemctl status ssh
```

---

# 10. Starting a Service

Run:

```bash
sudo systemctl start ssh
```

Conceptually:

```text
systemctl
    │
    │ "start ssh"
    ▼
systemd
    │
    ▼
Read ssh.service
    │
    ▼
Resolve/order dependencies
    │
    ▼
Execute service start command
    │
    ▼
SSH process running
```

The important part:

`systemctl` doesn't become the SSH server.

It sends the request to systemd.

---

# 11. Stop

```bash
sudo systemctl stop ssh
```

Conceptually:

```text
systemctl
   ↓
systemd
   ↓
stop service
   ↓
terminate/manage service processes
```

Systemd handles the configured shutdown behavior.

---

# 12. Restart

```bash
sudo systemctl restart ssh
```

Conceptually:

```text
STOP
 ↓
START
```

Useful after configuration changes.

---

# 13. Reload

Some services support:

```bash
sudo systemctl reload nginx
```

This is different from restart.

### Restart

```text
Stop process
    ↓
Start again
```

### Reload

```text
Keep service running
      ↓
Ask it to reload configuration
```

Whether reload works depends on the service/unit configuration.

---

# 14. Start vs Enable ⭐

This confuses almost everyone initially.

### `start`

```bash
sudo systemctl start nginx
```

means:

> **Start it now.**

### `enable`

```bash
sudo systemctl enable nginx
```

means roughly:

> **Configure systemd so this service is pulled in automatically by the appropriate boot target(s).**

So:

```text
start
  ↓
NOW


enable
  ↓
BOOT configuration
```

Often you'll want both:

```bash
sudo systemctl enable --now nginx
```

Meaning:

```text
enable at boot
+
start now
```

---

# 15. Disable

```bash
sudo systemctl disable nginx
```

This removes the enablement links/configuration created by `enable`.

It does **not necessarily stop the currently running service**.

Again:

```text
disable
   ↓
Don't automatically start via that enablement


stop
   ↓
Stop running service now
```

---

# 16. `systemctl status`

One of your most useful commands:

```bash
systemctl status ssh
```

It can show:

```text
Loaded
Active
PID
Recent logs
Service state
```

Think:

> **"Give me a quick health/status view of this service."**

Typical debugging flow:

```text
Service doesn't work
        ↓
systemctl status service
```

This should often be your first step.

---

# 17. `systemctl cat`

Run:

```bash
systemctl cat ssh
```

This shows the unit's configuration fragments as systemd sees them, including relevant drop-ins.

Think:

> **"Show me the definition/configuration behind this service."**

You can inspect things like:

```text
ExecStart=
After=
Requires=
Restart=
Environment=
```

---

# 18. `systemctl show`

Run:

```bash
systemctl show ssh
```

This gives a much larger property dump.

You'll see properties such as:

```text
MainPID=
ActiveState=
SubState=
MemoryCurrent=
TasksCurrent=
...
```

Think:

```text
status → human-friendly summary

show   → detailed machine-friendly properties
```

---

# 19. Dependencies

Services don't always start independently.

Imagine:

```text
My Web Application
        ↓
needs database/network/etc.
```

Systemd has dependency and ordering relationships such as:

```text
Requires=
Wants=
Before=
After=
```

Very simplified intuition:

```text
Requires → stronger dependency relationship

Wants    → weaker dependency relationship

Before   → ordering

After    → ordering
```

Example:

```ini
[Unit]
Wants=network-online.target
After=network-online.target
```

Conceptually:

```text
network-online.target
         ↓
     application
```

The dependency and ordering graph lets systemd start many independent services in parallel while respecting required relationships.

---

# 20. Targets

Older init systems often talked about **runlevels**.

Systemd uses **targets** as synchronization/grouping units.

Examples:

```text
multi-user.target

graphical.target

network.target

rescue.target
```

Think of a target as:

> **A named synchronization/group point containing or depending on other units.**

For example:

```text
graphical.target
      │
      ├── services
      ├── other targets
      └── graphical environment-related units
```

---

# 21. How Boot Starts Services

Simplified:

```text
Kernel
   ↓
systemd (PID 1)
   ↓
default target
   ↓
Dependencies
   ↓
Services
```

Check the default target:

```bash
systemctl get-default
```

You might see:

```text
graphical.target
```

Then systemd follows the dependency graph to bring the system into that target state.

---

# 22. Logging — `journalctl`

Systemd's ecosystem includes:

```text
systemd-journald
```

which collects journal logs.

You inspect them with:

```bash
journalctl
```

For one service:

```bash
journalctl -u ssh
```

`-u` → unit.

Think:

> **"Show journal entries associated with this unit."**

---

# 23. Recent Logs

Instead of reading thousands of lines:

```bash
journalctl -u ssh -n 20
```

`-n 20`:

```text
last 20 entries
```

Very common debugging pattern:

```bash
systemctl status myapp
```

then:

```bash
journalctl -u myapp -n 50
```

Even better for watching live:

```bash
journalctl -u myapp -f
```

`-f` → follow.

Similar idea to:

```bash
tail -f
```

---

# 24. systemd + Processes

Suppose:

```text
nginx.service
```

starts Nginx.

Systemd tracks the service and its processes.

Conceptually:

```text
systemd
   │
   ▼
nginx.service
   │
   ├── master process
   │
   ├── worker process
   │
   ├── worker process
   │
   └── worker process
```

But how does Linux/systemd organize these processes cleanly?

This connects to something you've already studied:

# **cgroups**

---

# 25. systemd + cgroups ⭐

Systemd uses Linux **control groups (cgroups)** to organize units/processes.

Conceptually:

```text
systemd
   │
   ▼
Service
   │
   ▼
cgroup
   │
   ├── Process
   ├── Process
   └── Process
```

This gives systemd a much better model than merely remembering one PID.

Imagine:

```text
web.service
   │
   ├── PID 100
   │
   ├── PID 101
   │
   └── PID 102
```

Those processes can belong to the service's cgroup.

So systemd can reason about the **service as a group of processes**.

---

# 26. See the cgroup Hierarchy

Run:

```bash
systemd-cgls
```

You may see a tree resembling:

```text
/
├─system.slice
│ ├─ssh.service
│ │ └─sshd
│ │
│ └─NetworkManager.service
│
└─user.slice
   └─user-1000.slice
```

Now you're literally seeing:

```text
systemd
   ↓
units
   ↓
cgroups
   ↓
processes
```

This is an important Linux-internals connection.

---

# 27. Why PID 1 is Special

PID 1 isn't just "the first process."

It has special responsibilities in userspace, including handling/reaping orphaned descendant processes that become its responsibility and coordinating system shutdown in a systemd-based system.

Very simplified:

```text
Kernel
   ↓
PID 1
   ↓
systemd
   ↓
Userspace process tree
```

If systemd is PID 1:

```text
systemd
├── services
├── login-related processes
└── other managed units/processes
```

This is why the init system is fundamental.

---

# 28. `systemd` vs Normal Process

Suppose you manually run:

```bash
python3 server.py
```

Your shell starts it:

```text
Shell
  │
  ▼
Python server
```

Close the terminal incorrectly, reboot, or crash the application, and you have to decide how to manage it.

As a systemd service:

```text
systemd
   │
   ▼
myserver.service
   │
   ▼
python server.py
```

Now you can configure:

```text
Start at boot
Restart on failure
Logging
Dependencies
Environment
Resource controls
User/group
```

That's why long-running server applications are commonly managed by a service manager.

---

# 29. Real Example — Your Backend

Imagine you create:

```text
/home/user/myserver
```

Instead of manually doing this after every reboot:

```bash
./myserver
```

you could eventually define:

```text
myserver.service
```

Then:

```bash
sudo systemctl enable --now myserver
```

Now:

```text
Boot
 ↓
systemd
 ↓
myserver.service
 ↓
myserver process
```

If configured with:

```ini
Restart=on-failure
```

then:

```text
myserver crashes
      ↓
systemd detects exit
      ↓
restart policy applies
      ↓
myserver starts again
```

This is how service management becomes practical.

---

# 30. Connect `systemd` + `lsof` + `strace` + `perf`

Now your recent days fit together nicely.

Imagine:

```text
myapp.service is broken
```

### Step 1 — systemd

```bash
systemctl status myapp
```

Ask:

> Is the service running?

---

### Step 2 — Logs

```bash
journalctl -u myapp
```

Ask:

> What errors did it report?

---

### Step 3 — `lsof`

```bash
lsof -p PID
```

Ask:

> What files/sockets/resources does it have open?

---

### Step 4 — `strace`

```bash
strace -p PID
```

Ask:

> What system calls is it making?

---

### Step 5 — `perf`

If it's running but slow:

```text
perf
 ↓
Where is CPU time going?
```

So your debugging toolbox becomes:

```text
systemctl → service state

journalctl → logs

lsof → open resources

strace → syscalls

perf → performance
```

---

# 31. Connection to Containers

This is especially important for what comes next.

You've already studied:

```text
Namespaces
    ↓
Isolation

cgroups
    ↓
Grouping/resource control

Capabilities
    ↓
Split root privileges

Processes
    ↓
Running programs
```

Systemd also heavily uses cgroups:

```text
systemd
   ↓
service
   ↓
cgroup
   ↓
processes
```

Containers use many of the same underlying kernel primitives:

```text
Container
   │
   ├── namespaces
   ├── cgroups
   ├── capabilities
   ├── seccomp
   ├── filesystem setup
   └── processes
```

So containers aren't some completely separate Linux magic.

They're built from mechanisms you're already learning.

---

# ⚡ Quick Revision

### `systemd`

> Init system and service manager commonly running as **PID 1**.

### `systemctl`

> CLI used to control/query systemd.

### Unit

> Object managed by systemd.

Examples:

```text
.service
.socket
.timer
.mount
.target
```

### Unit file

> Configuration describing a unit.

```ini
[Unit]
[Service]
[Install]
```

### Service lifecycle

```text
Inactive
   ↓ start
Active
   ↓ stop
Inactive

or

Active → failure → Failed/restart policy
```

### Important commands

```bash
systemctl status ssh
```

→ service status.

```bash
systemctl cat ssh
```

→ unit configuration.

```bash
systemctl show ssh
```

→ detailed properties.

```bash
journalctl -u ssh
```

→ unit logs.

```bash
systemd-cgls
```

→ cgroup hierarchy.

---

# 🧠 The One Diagram to Remember

```text
                 BOOT
                   │
                   ▼
             Linux Kernel
                   │
                   ▼
             systemd PID 1
                   │
            Default Target
                   │
          Dependency Graph
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
    ssh.service docker.service myapp.service
        │          │          │
        ▼          ▼          ▼
      cgroup     cgroup     cgroup
        │          │          │
        ▼          ▼          ▼
    Process     Processes    Process

                   │
                   ▼
              journald
                   │
                   ▼
              journalctl
```

### ⭐ Most important chain

```text
Kernel
   ↓
systemd (PID 1)
   ↓
Unit
   ↓
cgroup
   ↓
Process(es)

Unit
   ↓
journald
   ↓
Logs
```

And remember the distinction:

> **`systemd` manages the service. `systemctl` talks to systemd. `journalctl` lets you inspect the journal logs.**

Once this model clicks, container internals will feel much less mysterious: you're about to combine **processes + namespaces + cgroups + capabilities + seccomp + filesystems** into one isolated environment.