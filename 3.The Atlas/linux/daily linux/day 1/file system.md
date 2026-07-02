# Linux Filesystem Hierarchy (FHS)

Everything in Linux starts from a single directory called the **root directory (`/`)**.

Unlike Windows, there are **no drive letters (C:, D:, E:)**. Every file, partition, USB drive, or network share becomes part of one unified directory tree.

```
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

---

# `/` (Root)

The top-most directory.

Everything begins here.

```
/
```

Examples:

```
/home/retr0
/etc/passwd
/usr/bin/python
```

---

# `/bin`

**Essential user commands** needed for booting and basic system operation.

Examples:

```
ls
cp
mv
rm
cat
echo
pwd
mkdir
```

Location:

```
/bin/ls
/bin/cp
```

> Modern Linux distributions often make `/bin` a symbolic link to `/usr/bin`.

---

# `/sbin`

**Essential system administration binaries.**

Mostly used by the root user.

Examples:

```
fsck
mount
reboot
shutdown
mkfs
iptables
```

Location:

```
/sbin/fsck
/sbin/reboot
```

---

# `/boot`

Contains files required to boot Linux.

Contents:

```
Kernel Image
initramfs
GRUB files
Bootloader configuration
```

Example:

```
/boot/vmlinuz
/boot/initrd.img
```

---

# `/dev`

Contains **device files**.

Linux treats hardware as files.

Examples:

```
/dev/sda
/dev/nvme0n1
/dev/null
/dev/random
/dev/tty
/dev/usb
```

Useful devices:

```
/dev/null
```

Discard output.

```
echo hello > /dev/null
```

---

# `/etc`

Contains **system-wide configuration files**.

Examples:

```
/etc/passwd
/etc/shadow
/etc/fstab
/etc/hosts
/etc/hostname
/etc/ssh/
```

Never store user documents here.

---

# `/home`

Personal directories for users.

Example:

```
/home/retr0
```

Contains:

```
Downloads
Desktop
Documents
Pictures
Music
Videos
```

Every user gets their own home directory.

---

# `/root`

Home directory of the **root user**.

Different from `/`.

```
/root
```

Only the administrator typically uses this directory.

---

# `/usr`

Contains **installed software and shared resources**.

Think of it as the "program files" directory.

Important subdirectories:

```
/usr/bin
/usr/lib
/usr/share
/usr/local
```

---

## `/usr/bin`

Most user applications.

Examples:

```
python
gcc
git
node
vim
nano
docker
```

---

## `/usr/lib`

Libraries used by applications.

Examples:

```
libc.so
libstdc++
shared libraries
```

---

## `/usr/share`

Architecture-independent shared files.

Contains:

```
Icons
Documentation
Fonts
Themes
Manual pages
```

---

## `/usr/local`

Software installed manually by the administrator.

Example:

```
/usr/local/bin
```

---

# `/lib`

Contains **essential shared libraries** required by programs in `/bin` and `/sbin`.

Examples:

```
libc.so
libpthread.so
```

Modern systems may use:

```
/lib -> /usr/lib
```

---

# `/proc`

A **virtual filesystem** created by the kernel.

Not stored on disk.

Provides:

- Process information
    
- CPU information
    
- Memory information
    
- Kernel settings
    

Examples:

```
/proc/cpuinfo
/proc/meminfo
/proc/version
```

Each running process has its own directory:

```
/proc/1
/proc/2456
/proc/9032
```

---

# `/sys`

Another virtual filesystem.

Provides information about:

- Hardware
    
- Drivers
    
- Kernel modules
    
- Power management
    

Examples:

```
/sys/class
/sys/block
/sys/devices
```

---

# `/var`

Stores data that **changes frequently**.

Contains:

```
Logs
Cache
Databases
Mail
Package information
```

Important directories:

```
/var/log
/var/cache
/var/tmp
```

---

## `/var/log`

System logs.

Examples:

```
syslog
kern.log
auth.log
journal
```

Useful command:

```bash
journalctl
```

---

# `/tmp`

Temporary files.

Anyone can use it.

Usually cleared on reboot.

Example:

```
/tmp/test.txt
```

---

# `/run`

Stores runtime information.

Examples:

```
PID files
Sockets
Lock files
```

Exists only while the system is running.

---

# `/media`

Automatically mounted removable devices.

Examples:

```
USB drives
DVDs
External HDDs
```

Example:

```
/media/retr0/PENDRIVE
```

---

# `/mnt`

Temporary mount point.

Often used manually.

Example:

```bash
sudo mount /dev/sdb1 /mnt
```

---

# `/opt`

Optional third-party software.

Examples:

```
Google Chrome
JetBrains IDEs
Oracle Software
```

---

# `/srv`

Data served by services.

Examples:

```
Web server files
FTP server data
```

---

# Filesystem Hierarchy

```
/
├── bin        → Essential commands
├── boot       → Boot files
├── dev        → Device files
├── etc        → Configuration files
├── home       → User directories
├── lib        → Essential libraries
├── media      → Auto-mounted devices
├── mnt        → Temporary mounts
├── opt        → Third-party software
├── proc       → Process & kernel info (virtual)
├── root       → Root user's home
├── run        → Runtime data
├── sbin       → System binaries
├── srv        → Service data
├── sys        → Hardware & kernel info (virtual)
├── tmp        → Temporary files
├── usr        → Applications & libraries
└── var        → Logs, cache, databases
```

# Memory Trick

|Directory|Remember As|
|---|---|
|`/`|Root of everything|
|`/bin`|Basic commands|
|`/sbin`|System admin commands|
|`/boot`|Boot files|
|`/dev`|Devices as files|
|`/etc`|Configuration|
|`/home`|User data|
|`/root`|Administrator's home|
|`/usr`|Installed software|
|`/lib`|Libraries|
|`/proc`|Process & kernel info (virtual)|
|`/sys`|Hardware & driver interface (virtual)|
|`/var`|Variable data (logs, cache, DBs)|
|`/tmp`|Temporary files|
|`/run`|Runtime data|
|`/media`|Auto-mounted removable media|
|`/mnt`|Manual mount point|
|`/opt`|Optional/third-party software|
|`/srv`|Service data|

## Visual Mental Model

```
/
├── System
│   ├── /bin
│   ├── /sbin
│   ├── /boot
│   ├── /lib
│   └── /etc
│
├── Users
│   ├── /home
│   └── /root
│
├── Hardware
│   ├── /dev
│   ├── /proc
│   └── /sys
│
├── Programs
│   ├── /usr
│   └── /opt
│
└── Runtime Data
    ├── /var
    ├── /tmp
    ├── /run
    ├── /media
    ├── /mnt
    └── /srv
```

This hierarchy is defined by the **Filesystem Hierarchy Standard (FHS)**, which provides conventions for where different kinds of files and directories should live on Unix-like systems. Understanding it will make navigating, administering, and troubleshooting Linux systems much easier.