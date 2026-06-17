This dev log is actually very interesting for someone like you because it touches **Linux booting, embedded systems, hardware design, firmware, and product engineering** all at once.

Think of Flipper One as:

> "A tiny Linux computer + microcontroller + custom hardware + handheld device."

Let's break it section by section.

---

# 🐧 Linux (CPU Software)

This is the Linux side running on the main CPU.

---

## 1. New TV Media Box target

They added a new **systemd target** called:

```bash
tv-media-box
```

You already know systemd services.

A **target** is basically a group of services.

Examples:

```bash
multi-user.target
graphical.target
```

Flipper is creating:

```bash
tv-media-box.target
```

When booted into this target:

- Kodi starts
    
- Chromium starts when needed
    
- Device behaves like an Android TV box
    

Instead of:

```text
One device
One purpose
```

They want:

```text
One hardware
Many personalities
```

---

## Why?

Imagine:

Profile 1:

```text
Gaming handheld
```

Profile 2:

```text
TV Media Box
```

Profile 3:

```text
Developer workstation
```

Profile 4:

```text
Portable server
```

Same hardware.

Different boot environment.

---

# Future Profile Isolation

This is the coolest part.

They say:

> Profiles will reboot into isolated environments.

Meaning:

Today:

```text
Linux Installation
 ├── Gaming apps
 ├── Kodi
 ├── Browser
 └── Development tools
```

Everything shares one OS.

---

Future:

```text
Base System

Profile A
 └── Gaming

Profile B
 └── TV

Profile C
 └── Dev
```

If you break Profile B:

```text
rm -rf something
```

Only Profile B breaks.

Not the entire device.

Very similar idea to:

- Snapshots
    
- NixOS generations
    
- Containers
    
- VM rollback
    

---

# 2. Falcon Mode

This is extremely low-level.

Normally Linux boots like:

```text
Power On
   ↓
Boot ROM
   ↓
SPL
   ↓
U-Boot
   ↓
Linux Kernel
   ↓
systemd
```

---

U-Boot is a bootloader.

Think:

```text
BIOS → GRUB → Linux
```

on a PC.

---

Falcon Mode removes U-Boot.

Instead:

```text
Power On
   ↓
Boot ROM
   ↓
SPL
   ↓
Linux
```

Direct jump.

---

Why?

Faster boot.

They achieved:

```text
3.5 sec
```

until Linux userspace starts.

That's crazy fast for a Linux device.

---

# What is SPL?

SPL means:

```text
Secondary Program Loader
```

Tiny bootloader.

Its job:

```text
Initialize RAM
Load Linux
```

Usually it loads U-Boot.

Now it loads Linux directly.

---

# 3. Device Tree Preparation

You will see Device Trees everywhere in ARM Linux.

---

On PCs:

```text
BIOS tells Linux hardware details
```

On ARM:

Linux needs a file describing hardware.

Example:

```text
CPU
RAM
GPIO
USB
I2C
SPI
Display
Battery
```

This description is called:

```text
Device Tree
```

---

They prepared a new Device Tree for:

```text
Hardware Revision 2.F0B1C2
```

Meaning:

New motherboard revision coming.

---

# 4. Building Images Locally

Previously some repositories were private.

Now:

```text
rkbin
rockchip-linux
```

became public.

Meaning:

You can clone everything.

Build your own Flipper OS.

Like:

```bash
git clone
make image
```

and get a complete OS image.

This is a huge win for open-source users.

---

# SDDM Seat Support

SDDM = Login manager.

Like:

```text
GDM
LightDM
SDDM
```

---

A seat means:

```text
Keyboard
Mouse
Monitor
```

assigned to a user session.

Useful for multi-user or special embedded setups.

---

# microSD Fix

Bug:

```text
Boot from UFS
↓
microSD not initialized
```

Fixed.

---

# Cog Browser Fix

DRM here means:

```text
Direct Rendering Manager
```

Linux graphics subsystem.

NOT movie DRM.

Browser crashed when graphics device wasn't ready.

Fixed.

---

# ⚙️ Mechanics

This is physical engineering.

---

## Speaker Upgrade

Selected:

```text
1W speaker
15x11x2.5mm
```

Tiny speaker.

---

Rubber gasket added.

Without gasket:

```text
Sound leaks inside case
```

Like speaking into a pillow.

---

With gasket:

```text
Speaker
 ↓
Outside World
```

Much louder.

---

## microSD slot depth

Before:

```text
Card sticks out
```

Now:

```text
Card sits deeper
```

Less chance of breaking it.

---

## Bigger lanyard hole

You can pass:

- USB cable
    
- Ethernet cable
    

through it.

Tiny change.

Huge usability improvement.

---

# 🔌 Hardware

Electrical engineering section.

---

## USB-C Power Delivery Output

Flipper One can now act like a charger.

Output:

```text
15V
3A
```

Power:

```text
P = V × I

15 × 3 = 45W
```

Enough for many laptops.

---

They even tested:

```text
MacBook Pro charging
```

from Flipper One.

Pretty wild.

---

## HDMI Protection

HDMI provides:

```text
5V line
```

Risk:

```text
Short Circuit
Reverse Current
```

Could damage board.

---

Added protection chip.

Now safer.

---

## Heatsink Grounding

Think:

```text
Static electricity
```

touching device.

Can damage electronics.

---

Grounding heatsink gives:

```text
ESD protection
```

Electrostatic Discharge.

Very important in hardware products.

---

# 🕹️ MCU Firmware

This is separate from Linux.

Remember:

```text
Main CPU
  ↳ Linux

MCU
  ↳ Firmware
```

Two brains.

---

## Headphone Debouncing

You know button debouncing?

Same idea.

---

Without fix:

```text
Loud audio
↓
Electrical noise
↓
MCU thinks headphones connected
↓
Disconnected
↓
Connected
```

Rapidly.

---

Added debounce logic.

Meaning:

```text
Wait
Confirm
Then trigger event
```

---

# RAM / Flash Dashboard

This is awesome engineering.

Imagine every commit.

Instead of:

```text
Code changed
```

they also track:

```text
RAM usage
Flash usage
```

Example:

```text
Commit A
RAM = 40 KB

Commit B
RAM = 60 KB
```

Dashboard immediately shows:

```text
+20 KB
```

So developers catch memory bloat early.

---

Very useful in embedded systems.

Because:

```text
RAM is limited
Flash is limited
```

---

# 🎨 User Interface

They are asking community:

```text
What should power menu labels be?
```

Simple sounding.

Actually difficult.

---

Example:

Bad:

```text
Suspend
Hibernate
```

Users confused.

---

Good:

```text
Sleep
Power Off
Restart
```

Much clearer.

---

# What excites me most here?

For your interests (Linux + homelab + embedded + systems), the most interesting things are:

1. **Falcon Mode** (bootloader optimization)
    
2. **Device Trees** (ARM Linux internals)
    
3. **Profile Isolation** (NixOS/container-like ideas)
    
4. **MCU + Linux dual architecture**
    
5. **Open-source build system**
    

This dev log is basically showing how a real hardware company incrementally builds a Linux handheld from prototype toward a production device. It's a goldmine if you want to learn embedded Linux and product engineering.