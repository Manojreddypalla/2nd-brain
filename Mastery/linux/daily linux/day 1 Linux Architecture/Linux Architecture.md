# Linux Internals – Day 1 Notes

## Linux Architecture

```
Application (bash, python, chrome)
        │
   System Calls
        │
   Linux Kernel
        │
CPU • RAM • Disk • Network • Devices
```

**Key Idea:** Applications never access hardware directly. They communicate with the kernel using **system calls**.

---

# Linux Kernel

The **kernel** is the core of the operating system.

### Responsibilities

- Process Management
    
- CPU Scheduling
    
- Memory Management
    
- File System Management
    
- Device Management (Drivers)
    
- Networking
    
- Security & Permissions
    

**Loaded at boot and remains in memory until shutdown.**

---

# Kernel Space vs User Space

## User Space

- Runs user applications
    
- Limited privileges
    
- Cannot directly access hardware
    
- Examples:
    
    - Bash
        
    - Python
        
    - Chrome
        
    - VS Code
        

---

## Kernel Space

- Runs the Linux kernel
    
- Full hardware access
    
- Manages system resources
    
- Contains:
    
    - Scheduler
        
    - Memory Manager
        
    - Device Drivers
        
    - Network Stack
        
    - File Systems
        

```
+----------------------+
| User Space           |
| Applications         |
+----------------------+
        │
   System Calls
        │
+----------------------+
| Kernel Space         |
| Linux Kernel         |
+----------------------+
```

---

# Shell

A **Shell** is a command interpreter.

Functions:

- Reads user commands
    
- Starts programs
    
- Passes requests to the kernel
    

Examples:

- bash
    
- zsh
    
- fish
    
- sh
    
- dash
    

Check current shell:

```bash
echo $SHELL
```

---

# System Calls

A **System Call** is the interface between **User Space** and **Kernel Space**.

Applications request kernel services using system calls.

Examples:

|System Call|Purpose|
|---|---|
|open()|Open file|
|read()|Read file|
|write()|Write file|
|fork()|Create process|
|execve()|Execute program|
|mmap()|Allocate memory|
|socket()|Create socket|
|connect()|Network connection|
|kill()|Send signal|
|wait()|Wait for child process|

Flow:

```
Application
     ↓
System Call
     ↓
Kernel
     ↓
Hardware
```

---

# Important Linux Directories

## /proc

- Virtual filesystem
    
- Provides kernel and process information
    
- Created dynamically (not stored on disk)
    

Useful files:

```bash
/proc/cpuinfo
/proc/meminfo
/proc/version
/proc/<PID>
```

Commands:

```bash
cat /proc/cpuinfo
cat /proc/meminfo
cat /proc/version
```

---

## /sys

- Virtual filesystem
    
- Hardware and kernel configuration
    
- Device information
    
- Driver information
    

Command:

```bash
ls /sys
```

---

## /dev

- Device files
    
- Represents hardware devices
    

Examples:

```
/dev/sda
/dev/nvme0n1
/dev/null
/dev/random
/dev/tty
```

Command:

```bash
ls /dev
```

---

## /etc

- System-wide configuration files
    

Examples:

```
/etc/hosts
/etc/passwd
/etc/fstab
/etc/os-release
/etc/ssh/
```

Commands:

```bash
cat /etc/os-release
cat /etc/hosts
```

---

## /usr

Contains:

- Installed applications
    
- Libraries
    
- Documentation
    
- Shared resources
    

Common directories:

```
/usr/bin
/usr/lib
/usr/share
```

---

## /bin

Essential user commands.

Examples:

```
ls
cp
mv
rm
cat
echo
pwd
```

Check location:

```bash
which ls
```

> Modern Linux distributions often make `/bin` a symbolic link to `/usr/bin`.

---

## /var

Stores variable data.

Contains:

- Logs
    
- Cache
    
- Mail
    
- Databases
    
- Temporary runtime data
    

Examples:

```
/var/log
/var/cache
/var/spool
```

---

## /home

User home directories.

Example:

```
/home/retr0
```

Contains:

- Documents
    
- Downloads
    
- Desktop
    
- Pictures
    
- Videos
    

---

# Command Flow

```
User
   ↓
Shell
   ↓
Program
   ↓
System Call
   ↓
Kernel
   ↓
Hardware
```

---

# Useful Commands

```bash
uname -a                 # Kernel information

cat /proc/version        # Kernel version

cat /proc/cpuinfo        # CPU details

cat /proc/meminfo        # Memory details

ls /proc                 # Process information

ls /sys                  # Hardware information

ls /dev                  # Device files

ls /etc                  # Configuration files

ls /usr                  # Installed software

ls /var                  # Variable data

echo $SHELL              # Current shell

ps                       # Running processes

which ls                 # Binary location
```

---

# Revision Cheat Sheet

| Component    | Purpose                                               |
| ------------ | ----------------------------------------------------- |
| Kernel       | Core of Linux; manages hardware and resources         |
| User Space   | Runs applications with limited privileges             |
| Kernel Space | Protected area where the kernel executes              |
| Shell        | Command interpreter between user and kernel           |
| System Call  | Interface for applications to request kernel services |
| `/proc`      | Process and kernel information (virtual)              |
| `/sys`       | Hardware and kernel interface (virtual)               |
| `/dev`       | Device files                                          |
| `/etc`       | Configuration files                                   |
| `/usr`       | User applications, libraries, documentation           |
| `/bin`       | Essential command binaries                            |
| `/var`       | Logs, cache, databases, variable data                 |
| `/home`      | User home directories                                 |