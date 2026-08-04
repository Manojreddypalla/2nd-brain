# Linux Kernel Modules (LKMs)

### 🎯 Goal

Understand how Linux can **add functionality to the running kernel dynamically** without rebuilding the entire kernel.

The basic idea:

```text
Linux Kernel
     │
     ├── Core functionality
     │
     └── Kernel Modules
            ├── Wi-Fi driver
            ├── GPU driver
            ├── USB driver
            └── Filesystem support
```

---

## 1. What is a Kernel Module?

A **Linux Kernel Module (LKM)** is a piece of code that can be dynamically **loaded into or removed from the running kernel**.

Kernel modules commonly provide:

- Device drivers
    
- Filesystem support
    
- Network protocols/features
    
- Hardware support
    
- Security/kernel features
    

For example, your Wi-Fi adapter needs a driver.

Instead of permanently building every possible Wi-Fi driver into Linux:

```text
Kernel
  │
  └── Load Wi-Fi module when needed
```

Once loaded, the module becomes part of **kernel space** and can interact directly with kernel functionality.

---

# 2. Why Do Kernel Modules Exist?

Imagine Linux had every possible driver compiled directly into the kernel:

```text
Kernel
 ├── Thousands of network drivers
 ├── Thousands of GPU drivers
 ├── Thousands of storage drivers
 ├── Filesystem drivers
 ├── USB drivers
 └── Hardware you don't even own
```

Most of that would be unnecessary for your machine.

Instead Linux uses a modular design.

```text
Core Kernel
    +
Modules actually needed
```

### Benefits

**Smaller core kernel**

Not every optional driver must be built directly into the kernel image.

**Dynamic hardware support**

Modules can be loaded when hardware is detected.

**Memory efficiency**

Unused loadable modules don't need to remain loaded.

**Easier driver management**

Some drivers can be installed/updated as modules without rebuilding the entire kernel.

---

# 3. Kernel vs Kernel Module

The distinction is simple.

### Kernel

The kernel provides the fundamental operating-system mechanisms:

```text
Kernel
 ├── Process scheduling
 ├── Memory management
 ├── System calls
 ├── Interrupt handling
 ├── Device infrastructure
 └── Core filesystem/networking infrastructure
```

### Kernel Module

A module **extends the kernel**.

Example:

```text
Linux Kernel
     │
     └── Wi-Fi Module
              ↓
         Wi-Fi Hardware
```

Think:

> **Kernel = core engine**  
> **Module = plugin for the engine**

But there's one important difference from normal plugins:

Kernel modules run in **kernel space**.

---

# 4. Modules Run in Kernel Space

This is extremely important.

A normal application:

```text
User Space
──────────────
Firefox
VS Code
bash
Python

      ↓ system calls

──────────────
Kernel Space
Linux Kernel
```

A kernel module is different:

```text
User Space
────────────────

Applications
      │
      │ system calls
      ▼

Kernel Space
────────────────

Linux Kernel
 ├── Core kernel
 ├── NVIDIA module
 ├── Wi-Fi module
 └── Filesystem module
```

Once loaded, a module operates with kernel privileges.

Therefore, a buggy kernel module can potentially:

```text
Crash the system
Corrupt memory
Cause hardware problems
Create security vulnerabilities
```

That's why you shouldn't randomly remove modules just to experiment.

---

# 5. What Are Modules Used For?

## Device Drivers

Probably the most common use.

Examples:

```text
Wi-Fi
Bluetooth
GPU
USB
Sound
Storage controllers
```

For example:

```text
Application
    ↓
Kernel
    ↓
Wi-Fi Driver Module
    ↓
Wi-Fi Hardware
```

---

## Filesystems

Filesystem support can also be modular.

Examples include support for:

```text
ext4
Btrfs
NTFS
FAT
NFS
```

---

## Networking

Kernel modules can add networking functionality such as:

```text
Network protocols
Firewall/netfilter functionality
VPN-related support
Network device drivers
```

---

# 6. Kernel Module Files

Kernel modules normally use the extension:

```text
.ko
```

Meaning:

**Kernel Object**

Example:

```text
loop.ko
```

They may also be compressed:

```text
.ko.xz
.ko.zst
```

depending on your distribution.

---

# 7. Where Are Kernel Modules Stored?

Modules for installed kernels are normally stored under:

```text
/lib/modules/
```

Look:

```bash
ls /lib/modules/
```

You may see directories like:

```text
6.12.0-...
6.14.0-...
```

Each kernel version has its **own modules**.

Your current kernel version:

```bash
uname -r
```

So:

```bash
ls /lib/modules/$(uname -r)
```

means:

```text
$(uname -r)
      ↓
current kernel version
      ↓
/lib/modules/current-kernel/
```

This connects directly to yesterday's boot lesson.

```text
Kernel version
      ↕
Modules built for that kernel
```

Modules are tightly connected to the kernel they were built for.

---

# 8. `lsmod` — Loaded Modules

Run:

```bash
lsmod
```

It shows the modules currently loaded into the kernel.

For easier viewing:

```bash
lsmod | head
```

Typical structure:

```text
Module          Size    Used by

bluetooth       ...     ...
snd             ...     ...
loop            ...     ...
```

There are three important columns.

### Module

Module name.

### Size

Approximate module size in memory.

### Used by

Indicates references/dependencies involving that module.

So:

> `lsmod` answers: **What modules are currently loaded?**

---

# 9. Where Does `lsmod` Get This Information?

Here's a nice Linux-internals connection.

Try:

```bash
cat /proc/modules
```

You'll see information similar to `lsmod`.

Conceptually:

```text
Kernel
   ↓
/proc/modules
   ↓
lsmod
   ↓
You
```

Remember `/proc`?

It's a virtual filesystem exposing runtime kernel/process information.

So modules connect directly back to your `/proc` lesson.

---

# 10. `modinfo` — Inspect a Module

To inspect information about a module:

```bash
modinfo loop
```

or:

```bash
modinfo usbcore
```

Depending on the module, you'll see fields such as:

```text
filename
license
description
author
depends
vermagic
parameters
```

### `filename`

Where the `.ko` file exists.

### `description`

What the module does.

### `author`

Module author information if provided.

### `depends`

Other modules it depends on.

For example:

```text
Module A
   ↓ depends on
Module B
```

This dependency idea becomes important with `modprobe`.

---

# 11. `insmod` — Directly Insert Module

You can insert a module using:

```bash
sudo insmod module.ko
```

`insmod` means:

> **Insert Module**

But `insmod` is very basic.

Suppose:

```text
Module A
   ↓
requires Module B
```

If you directly do:

```bash
insmod A.ko
```

`insmod` doesn't automatically resolve the whole dependency situation for you.

That's why administrators normally use:

```text
modprobe
```

instead.

---

# 12. `modprobe` — Load Modules Properly

Example:

```bash
sudo modprobe loop
```

`modprobe` loads the requested module while handling module dependencies.

Suppose:

```text
Module A
   ↓ needs
Module B
   ↓ needs
Module C
```

With `insmod`, you'd have to deal with that manually.

With:

```bash
modprobe A
```

the tools can resolve and load required dependencies in the appropriate order.

Conceptually:

```text
modprobe A

   ↓

Check dependencies

   ↓

C
↓
B
↓
A
```

### Key rule

> Prefer `modprobe` for normal module management.

---

# 13. `rmmod` — Remove Module

To remove a loaded module:

```bash
sudo rmmod module_name
```

`rmmod` = **remove module**

But again, dependency relationships can matter.

A module may be in use by:

```text
Another module
Hardware
Kernel subsystem
```

So removal might fail or cause problems if you don't know what you're doing.

A commonly preferred approach is:

```bash
sudo modprobe -r module_name
```

because `modprobe` understands module dependencies better.

### Don't randomly test removal

Especially avoid experimenting with modules related to:

```text
Storage
Filesystem
GPU
Network
USB controllers
```

on a system you're actively using.

---

# 14. Module Dependencies

This is one of the most important concepts today.

Modules can depend on other modules.

Example:

```text
Driver A
   ↓
Subsystem B
   ↓
Kernel functionality
```

Linux maintains module dependency information under:

```text
/lib/modules/$(uname -r)/
```

A relevant file is:

```text
modules.dep
```

You can inspect it:

```bash
less /lib/modules/$(uname -r)/modules.dep
```

This dependency information is what tools such as `modprobe` use.

---

# 15. `depmod`

There's another command behind this system:

```bash
depmod
```

It analyzes kernel modules and generates dependency information.

Mental model:

```text
Kernel module files
       ↓
     depmod
       ↓
Dependency database
       ↓
    modprobe
       ↓
Correct modules loaded
```

You won't normally need to manually run it during today's lab, but knowing why `modprobe` understands dependencies is useful.

---

# 16. How Does Linux Automatically Load Drivers?

This is the cool part.

You usually don't plug in a USB device and manually type:

```bash
modprobe some_driver
```

Linux handles much of this automatically.

Conceptually:

```text
Plug hardware in
      ↓
Kernel detects device
      ↓
Device identification/event
      ↓
Userspace device management + module aliases
      ↓
Correct module requested
      ↓
Kernel loads driver
      ↓
Device works
```

So when you connect hardware, Linux can dynamically load the appropriate driver module.

That's one of the major reasons LKMs are useful.

---

# 17. Example — Wi-Fi

Imagine your laptop has a Wi-Fi adapter.

```text
Wi-Fi Hardware
      ↓
Kernel detects device
      ↓
Appropriate driver module
      ↓
Module loaded
      ↓
Kernel can communicate with hardware
      ↓
Network interface appears
```

You can investigate PCI devices and their drivers with:

```bash
lspci -k
```

Look for something like:

```text
Network controller
Kernel driver in use: ...
Kernel modules: ...
```

This is a great practical connection between:

**hardware → kernel → module → device**

---

# 18. Finding Module Files

Run:

```bash
find /lib/modules/$(uname -r) -name "*.ko*" | head
```

Break it down:

```text
find
```

Search filesystem.

```text
/lib/modules/$(uname -r)
```

Search modules belonging to the current kernel.

```text
-name "*.ko*"
```

Find kernel object files, including compressed variants.

```text
| head
```

Only show the first few results.

---

# 19. Hands-On Lab

### 1. Current kernel

```bash
uname -r
```

---

### 2. List loaded modules

```bash
lsmod | head
```

Observe:

```text
Module
Size
Used by
```

---

### 3. Inspect a module

```bash
modinfo loop
```

Look for:

```text
filename
description
license
depends
```

---

### 4. Find module directory

```bash
ls /lib/modules/$(uname -r)
```

---

### 5. Find `.ko` files

```bash
find /lib/modules/$(uname -r) -name "*.ko*" | head
```

---

### 6. Compare `lsmod` with `/proc`

```bash
cat /proc/modules | head
```

Then:

```bash
lsmod | head
```

Notice the connection.

---

### 7. Find your hardware drivers

```bash
lspci -k
```

Look for:

```text
Kernel driver in use:
Kernel modules:
```

This is probably the most useful experiment today.

---

# 20. `lsmod` vs `modinfo` vs `modprobe`

Don't mix these up.

```text
lsmod
   ↓
What's loaded?

modinfo
   ↓
What is this module?

modprobe
   ↓
Load/remove this module properly.

insmod
   ↓
Directly insert this .ko file.

rmmod
   ↓
Directly remove a module.
```

If you remember those questions, you don't need to memorize definitions.

---

# 21. Connection to the Boot Process

Yesterday:

```text
GRUB
 ↓
Kernel
 ↓
initramfs
 ↓
systemd
```

Remember what initramfs can contain?

**Kernel modules.**

Suppose Linux needs a storage driver before it can access `/`.

```text
Kernel starts
      ↓
initramfs
      ↓
Load required storage module
      ↓
Disk becomes accessible
      ↓
Mount real /
      ↓
systemd
```

So yesterday's lesson and today's lesson connect directly.

---

# 22. Connection to Containers

Containers do **not** normally run their own independent kernel.

They share the host kernel.

```text
Container A ─┐
Container B ─┼── Host Linux Kernel
Container C ─┘
                    │
              Kernel Modules
```

Therefore kernel modules belong to the **host kernel**, not separately to each normal container.

This is one reason:

```text
Container ≠ Virtual Machine
```

A VM normally has its own guest kernel.

A container shares the host kernel.

---

# 23. Connection to System Calls

Your overall Linux picture is becoming:

```text
Application
     │
     │ System Call
     ▼
Linux Kernel
     │
     ├── Memory Management
     ├── Scheduler
     ├── Filesystems
     │
     └── Kernel Modules
            │
            ├── GPU Driver
            ├── Wi-Fi Driver
            ├── USB Driver
            └── Filesystem Module
                    │
                    ▼
                 Hardware
```

An application doesn't normally talk directly to your Wi-Fi hardware.

It interacts through kernel interfaces, while the kernel and its drivers/modules manage the hardware.

---

# 🧠 Quick Questions

### Why does Linux use kernel modules?

To dynamically extend the kernel with features such as drivers and filesystem/network support without compiling everything directly into the core kernel.

### Kernel vs kernel module?

**Kernel:** core operating-system functionality.

**Module:** dynamically loadable code that extends the kernel.

### What does `lsmod` do?

Lists currently loaded kernel modules.

### What does `modinfo` do?

Displays information about a kernel module.

### `insmod` vs `modprobe`?

```text
insmod
→ directly inserts a .ko file
→ doesn't automatically resolve dependencies

modprobe
→ loads modules
→ handles dependencies
→ preferred for normal use
```

### What is `.ko`?

**Kernel Object** — the typical kernel module file format.

### Where are modules stored?

```text
/lib/modules/<kernel-version>/
```

### Do modules run in user space?

No.

They run in **kernel space**.

---

# 📌 Commands to Remember

```bash
# Current kernel
uname -r

# Loaded modules
lsmod

# Module information
modinfo loop

# Current kernel's modules
ls /lib/modules/$(uname -r)

# Find module files
find /lib/modules/$(uname -r) -name "*.ko*" | head

# Kernel's loaded-module information
cat /proc/modules

# Hardware + associated drivers
lspci -k

# Load module
sudo modprobe module_name

# Remove module with dependency handling
sudo modprobe -r module_name
```

For today's lab, stick mainly to the **inspection commands** rather than removing hardware-related modules.

# 🔁 2-Minute Revision

Remember this flow:

```text
Hardware detected
       ↓
Driver needed
       ↓
Kernel Module (.ko)
       ↓
Module loaded into kernel
       ↓
Kernel can control hardware
```

And the key sentence for Day 21:

> **A Linux Kernel Module is dynamically loadable kernel-space code that extends the running kernel, commonly providing device drivers, filesystem support, and networking features.**

The most important connection from these two days is now **Boot → Kernel → Modules → Hardware**. During boot, the kernel establishes the core OS; modules then let that same kernel adapt itself to the actual hardware and features your machine needs.