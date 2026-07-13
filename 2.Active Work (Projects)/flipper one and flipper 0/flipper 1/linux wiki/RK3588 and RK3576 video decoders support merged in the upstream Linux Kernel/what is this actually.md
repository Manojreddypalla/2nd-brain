Well, I came across this information from [[log 1]], and I'm trying to learn more about it and understand Linux from a **kernel developer's perspective**.


````markdown
# RK3576 Hardware Video Decoding — Learning Notes

## Device Tree Binding

Look for:

```dts
video-decoder
````

or

```dts
vdpu383
```

This answers:

> How does Linux know the decoder hardware exists?

The Device Tree tells the Linux kernel:

- What hardware is present on the board
    
- Memory addresses of peripherals
    
- Interrupt lines
    
- Clocks
    
- Power domains
    
- Hardware capabilities
    

Without a Device Tree entry, the kernel may not even know the decoder exists.

---

# H.264 Decode Path

A simplified flow:

```text
Compressed H.264 Video Frame
                ↓
Prepare Hardware Registers
                ↓
Configure Decoder State
                ↓
Start VDPU383 Decoder
                ↓
Hardware Decodes Frame
                ↓
Interrupt Generated
                ↓
Driver Handles Interrupt
                ↓
Decoded Frame Returned
                ↓
Display Pipeline
```

This is the heart of the hardware video decoder driver.

The driver acts as a translator between:

```text
Linux Software
       ↔
VDPU383 Hardware Decoder
```

---

# Video Playback Stack

When playing a video using mpv:

```text
mpv
 ↓
FFmpeg
 ↓
V4L2 (Video4Linux2)
 ↓
RK3576 Decoder Driver
 ↓
VDPU383 Hardware Decoder
 ↓
Decoded Frame Buffer
 ↓
GPU / Display Controller
 ↓
Screen
```

The most interesting code lives in:

```text
RK3576 Decoder Driver
VDPU383 Hardware Decoder
```

---

# Technologies Involved

## Userspace

### mpv

Role:

- Video player
    
- Requests video decoding
    
- Displays output
    

Project:

[https://github.com/mpv-player/mpv](https://github.com/mpv-player/mpv)

---

### FFmpeg

Role:

- Demuxing containers
    
- Parsing codecs
    
- Hardware decode integration
    

Project:

[https://github.com/FFmpeg/FFmpeg](https://github.com/FFmpeg/FFmpeg)

---

## Linux Multimedia Framework

### V4L2 (Video4Linux2)

Role:

- Standard Linux video API
    
- Connects userspace to decoder drivers
    

Subsystem:

```text
Linux Kernel
└── Media Subsystem
    └── V4L2
```

Documentation:

[https://www.kernel.org/doc/html/latest/userspace-api/media/](https://www.kernel.org/doc/html/latest/userspace-api/media/)

---

### Media Controller Framework

Role:

- Connects video devices
    
- Builds media pipelines
    

Examples:

```text
Camera
Decoder
ISP
Display
```

---

## Kernel Driver Layer

### Rockchip Video Decoder Driver

Possible files:

```text
drivers/media/platform/rockchip/
```

Examples:

```text
rkvdec.c
rkvdec-h264.c
rkvdec-hevc.c
```

Role:

- Programs decoder registers
    
- Manages buffers
    
- Handles interrupts
    
- Interfaces with V4L2
    

---

### VDPU383 Support

Role:

- RK3576 hardware decoder block
    
- H.264
    
- H.265 / HEVC
    
- Other supported codecs
    

This is the actual silicon performing decode operations.

---

## Memory Management

### DMA (Direct Memory Access)

Role:

```text
Decoder
   ↓
Writes Frames Directly
   ↓
RAM
```

without CPU copying data.

---

### DMA-BUF

Role:

Share frame buffers between:

```text
Decoder
GPU
Display
```

without memory copies.

---

### IOMMU

Role:

```text
Hardware Device
      ↓
Virtual Address
      ↓
Physical Memory
```

Provides:

- Isolation
    
- Security
    
- Memory translation
    

Important for modern multimedia pipelines.

---

## Interrupt System

Role:

```text
Decoder Finished
        ↓
Generate Interrupt
        ↓
Kernel Interrupt Handler
        ↓
Wake Waiting Process
```

Typical function:

```c
irq_handler(...)
```

---

## Display Stack

### DRM (Direct Rendering Manager)

Role:

- Linux graphics subsystem
    
- Frame presentation
    

Subsystem:

```text
Linux Kernel
└── DRM
```

---

### KMS (Kernel Mode Setting)

Role:

- Resolution control
    
- Refresh rate control
    
- Display pipeline configuration
    

---

### Display Controller

Role:

```text
Decoded Frame
       ↓
Display Controller
       ↓
HDMI
       ↓
Screen
```

---

# Open Source Projects Used

## Linux Kernel

Role:

- Core operating system
    
- Driver framework
    
- V4L2
    
- DRM
    
- DMA-BUF
    
- IOMMU
    

Project:

[https://github.com/torvalds/linux](https://github.com/torvalds/linux)

---

## U-Boot

Role:

- Bootloader
    
- Hardware initialization
    

Project:

[https://github.com/u-boot/u-boot](https://github.com/u-boot/u-boot)

---

## FFmpeg

Role:

- Codec integration
    
- Hardware acceleration support
    

Project:

[https://github.com/FFmpeg/FFmpeg](https://github.com/FFmpeg/FFmpeg)

---

## mpv

Role:

- Userspace media player
    

Project:

[https://github.com/mpv-player/mpv](https://github.com/mpv-player/mpv)

---

## Collabora Upstream Work

Role:

- Developed and upstreamed VDPU383 support
    
- Integrated RK3576 decoder support into mainline Linux
    

Organization:

Collabora

---

# Important Linux Concepts To Learn

Study these in roughly this order:

1. Device Tree (DTS)
    
2. Platform Drivers
    
3. Memory-Mapped I/O (MMIO)
    
4. Interrupts
    
5. DMA
    
6. DMA-BUF
    
7. V4L2
    
8. DRM/KMS
    
9. IOMMU
    
10. Linux Media Subsystem
    
11. U-Boot
    
12. Kernel Driver Development
    

---

# End-to-End Decode Flow

```text
Video File
    ↓
mpv
    ↓
FFmpeg
    ↓
V4L2 API
    ↓
Rockchip Decoder Driver
    ↓
VDPU383 Hardware Decoder
    ↓
DMA Buffer
    ↓
DRM/KMS
    ↓
Display Controller
    ↓
HDMI / LCD
    ↓
Screen
```

This entire pipeline is what "Hardware Video Decoding on RK3576 in Mainline Linux" actually means.