Flipper One is **not a Flipper Zero 2**. It's a completely different device: a **portable Linux cyberdeck / networking multi-tool** designed for hackers, tinkerers, DevOps engineers, network engineers, and security researchers. ([Flipper Blog](https://blog.flipper.net/flipper-one-we-need-your-help/?utm_source=chatgpt.com "Flipper One — we need your help"))

For someone like you (home lab, DevOps, self-hosting, networking, cybersecurity), Flipper One is actually much more interesting than Flipper Zero.

## What is Flipper One?

Think of it as:

**Raspberry Pi + Network Toolkit + Portable Linux Computer + Cyberdeck**

instead of:

**RFID/NFC/Sub-GHz gadget (Flipper Zero)**. ([GIGAZINE](https://gigazine.net/gsc_news/en/20260522-flipper-one/?utm_source=chatgpt.com "Development is underway on the wireless communication ..."))

---

## Hardware Specifications

### CPU

- Rockchip RK3576
    
- 8-core ARM processor
    
- Mali-G52 GPU
    
- Integrated NPU (~6 TOPS AI acceleration) ([Tom's Hardware](https://www.tomshardware.com/networking/flipper-one-computing-multitool-bristles-with-network-gpio-and-m-2-connectivity-new-keychain-device-is-also-a-fully-open-arm-linux-computer?utm_source=chatgpt.com "New Flipper One computing multitool bristles with network, GPIO, and M.2 connectivity - new keychain device is also a fully open Arm Linux computer"))
    

### RAM

- 8 GB RAM ([Tom's Hardware](https://www.tomshardware.com/networking/flipper-one-computing-multitool-bristles-with-network-gpio-and-m-2-connectivity-new-keychain-device-is-also-a-fully-open-arm-linux-computer?utm_source=chatgpt.com "New Flipper One computing multitool bristles with network, GPIO, and M.2 connectivity - new keychain device is also a fully open Arm Linux computer"))
    

### Co-Processor

A separate microcontroller runs low-power tasks.

- RP2350 MCU
    
- Handles controls, screen, and background functions
    
- Can keep working while Linux is asleep/offline ([Tom's Hardware](https://www.tomshardware.com/networking/flipper-one-computing-multitool-bristles-with-network-gpio-and-m-2-connectivity-new-keychain-device-is-also-a-fully-open-arm-linux-computer?utm_source=chatgpt.com "New Flipper One computing multitool bristles with network, GPIO, and M.2 connectivity - new keychain device is also a fully open Arm Linux computer"))
    

---

## Display

- 2.4-inch color display
    
- 256×144 resolution ([IT-Connect -](https://www.it-connect.tech/flipper-one-full-specs-and-the-surprising-changes-in-the-flipper-zero-successor/?utm_source=chatgpt.com "Flipper One Specs: What Changes in the Flipper Zero Successor"))
    

---

## Connectivity

### Networking

- Wi-Fi 6E
    
- Bluetooth
    
- 2× Gigabit Ethernet ports
    
- USB Ethernet (5 Gbps) ([Flipper Blog](https://blog.flipper.net/flipper-one-we-need-your-help/?utm_source=chatgpt.com "Flipper One — we need your help"))
    

This alone makes it crazy for network work.

Possible uses:

- Travel router
    
- VPN gateway
    
- Network diagnostics
    
- Packet analysis
    
- Portable pentesting box ([Flipper Blog](https://blog.flipper.net/flipper-one-we-need-your-help/?utm_source=chatgpt.com "Flipper One — we need your help"))
    

---

## Expansion

### M.2 Slot

Supports:

- 5G modems
    
- SDR modules
    
- NVMe SSDs
    
- AI accelerators
    
- Additional Wi-Fi cards
    
- Future radio modules ([TechCrunch](https://techcrunch.com/2026/05/21/flipper-unveils-a-linux-powered-networking-gadget-built-for-hackers-and-tinkerers/?utm_source=chatgpt.com "Flipper unveils a Linux-powered networking gadget built ..."))
    

This is where the fun begins.

Imagine plugging:

- LTE/5G modem
    
- HackRF-style SDR
    
- NVMe drive
    
- AI accelerator
    

into a handheld Linux device.

---

## Ports

Reported ports include:

- USB-C
    
- USB-A
    
- HDMI output
    
- MicroSD slot
    
- GPIO header
    
- Audio jack
    
- Dual Ethernet ports ([theregister](https://www.theregister.com/personal-tech/2026/05/21/flipper-one-wants-to-be-the-linux-multi-tool-in-your-pocket/5244165?utm_source=chatgpt.com "Flipper One wants to be the Linux multi-tool in your pocket"))
    

---

## Operating System

### Linux

Not FreeRTOS.

Not embedded firmware.

Actual Linux. The project is aiming for:

- Debian-based OS
    
- Mainline Linux support
    
- Open-source drivers
    
- Full Linux development environment ([Tom's Hardware](https://www.tomshardware.com/networking/flipper-one-computing-multitool-bristles-with-network-gpio-and-m-2-connectivity-new-keychain-device-is-also-a-fully-open-arm-linux-computer?utm_source=chatgpt.com "New Flipper One computing multitool bristles with network, GPIO, and M.2 connectivity - new keychain device is also a fully open Arm Linux computer"))
    

---

## AI Support

Because of the NPU and Linux environment:

- Local AI workloads
    
- Edge AI applications
    
- Future AI agents
    
- On-device inference possibilities ([Tom's Hardware](https://www.tomshardware.com/networking/flipper-one-computing-multitool-bristles-with-network-gpio-and-m-2-connectivity-new-keychain-device-is-also-a-fully-open-arm-linux-computer?utm_source=chatgpt.com "New Flipper One computing multitool bristles with network, GPIO, and M.2 connectivity - new keychain device is also a fully open Arm Linux computer"))
    

---

## Dimensions

Current development specs:

- 155 mm × 67 mm × 40 mm
    
- Larger than Flipper Zero ([Flipper Documentation](https://docs.flipper.net/one/general/tech-specs?utm_source=chatgpt.com "Tech specs - Flipper One Documentation"))
    

---

## What Happened to NFC, RFID, Sub-GHz?

Interesting part:

Those are **not built in**.

Instead:

- Radio functionality becomes modular.
    
- Future M.2 expansion boards can add:
    
    - NFC
        
    - RFID
        
    - Sub-GHz
        
    - Other radio hardware ([Reddit](https://www.reddit.com/r/flipperzero/comments/1jobuwn/the_flipper_one_is_coming_the_next_generation_of/?utm_source=chatgpt.com "The Flipper One Is Coming - The Next Generation of Hacking ..."))
        

So Flipper Zero remains the radio-focused tool.

Flipper One becomes the Linux/network-focused tool.

---

## Intended Use Cases

The creators mention:

- Networking
    
- Pentesting
    
- Travel router
    
- VPN appliance
    
- Portable Linux workstation
    
- Emergency desktop computer
    
- Media center
    
- Hardware development platform ([Heise.de](https://www.heise.de/news/Modularer-Kleincomputer-Flipper-One-wird-universelles-Linux-Cyberdeck-11300412.html?utm_source=chatgpt.com "Modularer Kleincomputer: Flipper One wird universelles ..."))
    

---

## Why It's Interesting for Home Labs

Looking at your interests:

- Self-hosting
    
- GitLab
    
- Docker
    
- DevOps
    
- Networking
    
- Cybersecurity
    
- Home lab experiments
    

Flipper One could potentially become a:

- Portable SSH machine
    
- Portable VPN node
    
- Network diagnostics toolkit
    
- Wi-Fi analysis box
    
- Kubernetes troubleshooting device
    
- Linux development machine
    

basically a **pocket-sized infrastructure toolbox**.

---

## Release Status (June 2026)

Current status:

- Announced publicly
    
- Still under active development
    
- Documentation portal is live
    
- Community feedback is being collected
    
- Not generally available yet ([Flipper Documentation](https://docs.flipper.net/one?utm_source=chatgpt.com "Welcome - Flipper One Documentation"))
    

---

## Flipper Zero vs Flipper One

|Feature|Flipper Zero|Flipper One|
|---|---|---|
|Main Focus|Radio protocols|Linux computing & networking|
|OS|FreeRTOS|Linux|
|CPU|STM32 MCU|RK3576 Octa-Core|
|RAM|KB-scale MCU memory|8 GB|
|Ethernet|No|Dual Gigabit|
|HDMI|No|Yes|
|SSD Support|No|NVMe via M.2|
|AI Acceleration|No|Yes|
|Wi-Fi|External board|Built-in Wi-Fi 6E|
|Target User|RF/RFID hobbyists|Network engineers, hackers, DevOps users|

([Wikipedia](https://en.wikipedia.org/wiki/Flipper_Zero?utm_source=chatgpt.com "Flipper Zero"))

### The simplest description

If Flipper Zero is a **Swiss Army knife for radio protocols**, then Flipper One is a **portable Linux cyberdeck for networking, hacking, and infrastructure work**. ([The Verge](https://www.theverge.com/tech/935202/flipper-devices-one-zero-wireless-multi-tool-linux-open-source-computer?utm_source=chatgpt.com "The new Flipper One is a pocket-sized Linux computer"))

Honestly, among all the new hacker gadgets announced recently, this is one of the few that could genuinely fit into a home-lab/DevOps workflow instead of being just a toy.