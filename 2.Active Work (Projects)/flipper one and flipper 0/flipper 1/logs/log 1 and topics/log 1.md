Yep, that's **Dev Log #1** for the upcoming device **Flipper One**, and honestly it's much more interesting than a typical "we changed some buttons" update.

What caught my eye is that this log reveals what kind of device Flipper One is actually becoming.

## Big Picture

Most people think:

**Flipper Zero = hacking gadget**

But Flipper One looks more like:

**Flipper Zero + Linux computer + cyberdeck + portable development machine**

---

## 1. RK3576 Hardware Video Decoding



They mention:

> Hardware video decoding now works on RK3576 in the mainline kernel.

Why is this important?

Normally:

```
Video
 ↓
CPU decodes frames
 ↓
High CPU usage
```

With hardware decoding:

```
Video
 ↓
Dedicated Video Decoder
 ↓
CPU stays free
 ↓
Lower power usage
```

This means:

- smoother video playback
    
- less battery drain
    
- Linux desktop becomes more usable
    

And the interesting part:

**mainline kernel**

[[what is this actually]]

That means they are contributing support directly into Linux rather than maintaining a giant custom fork.

This is exactly the kind of thing Linux and embedded developers care about.

---

## 2. They Added eSIM Support

This part is huge:

> Added an eSIM chip and dual-SIM analog switch

Meaning:

```
Physical SIM
      OR
eSIM
      ↓
Linux
```

So theoretically:

- LTE connectivity
    
- mobile internet
    
- remote SSH
    
- portable networking projects
    

without needing an external modem.

---

## 3. Debug Probe

This is my favorite part.

They designed a dedicated debug probe based on:

Raspberry Pi RP2350

Why?

Because debugging embedded systems is painful.

Normally:

```
Code crashes
↓
No screen
↓
No logs
↓
Cry
```

A debug probe gives:

```
PC
 ↓
Debug Probe
 ↓
Flipper One
```

Allowing:

- flashing firmware
    
- reading logs
    
- stepping through code
    
- recovering bricked devices
    

This is very developer-focused hardware.

---

## 4. GPIO Power Isolation

This sounds boring but is actually good engineering.

They changed:

```
GPIO 3.3V
```

to be isolated from internal rails.

Before:

```
Short GPIO
↓
Entire device affected
```

After:

```
Short GPIO
↓
Protection
↓
Device survives
```

This matters because people will connect random electronics.

---

## 5. Boot Flow Documentation

For someone like you who likes:

- Linux
    
- low-level systems
    
- operating systems
    
- embedded devices
    

this is gold.

Understanding boot flow means understanding:

```
Power Button
 ↓
ROM Code
 ↓
Bootloader
 ↓
U-Boot
 ↓
Kernel
 ↓
Init System
 ↓
Userspace
```

This is basically the journey from pressing the power button to seeing Linux.

---

## 6. Why I Think You'd Enjoy Following This

Based on your interests:

- Homelab
    
- Linux
    
- Self-hosting
    
- Embedded systems
    
- Raspberry Pi Pico
    
- Low-level programming
    
- DIY cyberdeck project
    

Flipper One is almost a combination of all of them.

It's not just:

> "Use Linux"

It's:

> "Help build Linux support for a new hardware platform."

That exposes you to:

- kernel development
    
- device drivers
    
- bootloaders
    
- hardware design
    
- PCB design
    
- firmware
    
- Linux internals
    

all in one project.

---

## If we zoom out

The dev log is really saying:

```
Hardware
  ↓
Bootloader (U-Boot)
  ↓
Linux Kernel
  ↓
Drivers
  ↓
UI
  ↓
Applications
```

And they're actively developing every layer.

For someone trying to understand computers from the bottom up, reading these logs is surprisingly educational because you see real engineers solving real hardware/software integration problems instead of simplified tutorial examples.

The part I'd recommend paying special attention to is:

1. U-Boot
    
2. RK3576 boot flow
    
3. Linux drivers
    
4. Debug Probe
    
5. MCU firmware
    

Those five topics connect directly to your long-term interests in low-level systems, embedded development, and eventually understanding how devices like your DIY cyberdeck actually work internally.