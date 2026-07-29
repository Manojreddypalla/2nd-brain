# 1. What is a Network Interface?

A **Network Interface** is the connection between your computer and a network.

Think of it as a **door**.

Without a door...

Nothing enters.

Nothing leaves.

---

## Mental Model

Imagine your house.

```
House│├── Front Door├── Back Door└── Garage Door
```

Data also needs doors.

```
Computer│├── Ethernet Port├── WiFi Adapter├── Loopback└── Virtual Interfaces
```

These are called **Network Interfaces**.

---

## Examples

```
eth0
```

Ethernet cable

---

```
wlan0
```

Wireless LAN (WiFi)

---

```
lo
```

Loopback Interface

The computer talking to itself.

---

Modern Linux often names interfaces like

```
enp5s0wlp2s0
```

instead of `eth0` or `wlan0`.

---

## Why Multiple Interfaces?

One computer can have

```
EthernetWiFiVPNDocker NetworkVirtual MachineLoopback
```

Each is a separate interface.

---

Visualization

```
          Computer      ┌──────┼──────┐      │      │      │   WiFi   Ethernet  VPN
```

The kernel decides which one to use.

---

# 2. What is an IP Address?

An **IP Address** is the logical address of a device on a network.

Think of it like a house address.

Example

```
192.168.1.20
```

or

```
8.8.8.8
```

or IPv6

```
2001:db8::1
```

---

Why?

Imagine delivering pizza.

Without an address...

Where do you go?

Exactly.

Networks work the same way.

---

Visualization

```
Internet│├── Computer A│      IP = 192.168.1.10│├── Computer B│      IP = 192.168.1.20│└── Router
```

The IP identifies **where data should go**.

---

## Important

IP addresses can change.

Today

```
192.168.1.20
```

Tomorrow

```
192.168.1.50
```

Nothing is physically different.

The address simply changed.

---

# 3. What is a MAC Address?

Every network card has a unique hardware identifier.

This is called the

> **MAC Address**

Example

```
00:1A:2B:3C:4D:5E
```

---

Think of it like

```
Person↓Fingerprint
```

or

```
Laptop↓Serial Number
```

The MAC identifies the **network interface itself**, not where it is on the network.

---

Difference

```
IP↓Location
```

```
MAC↓Identity
```

---

Imagine moving houses.

You still have the same fingerprint.

Your house address changes.

Likewise,

```
MAC staysIP changes
```

---

Communication inside the same local network

```
Laptop A↓Needs MAC of Laptop B↓Uses ARP↓Gets MAC↓Sends frame
```

---

# IP vs MAC

|IP Address|MAC Address|
|---|---|
|Logical address|Physical hardware identifier|
|Can change|Usually stays the same|
|Used across networks|Used only on the local network|
|Assigned by network|Assigned to the network interface|

---

# 4. What is a Routing Table?

Suppose your computer has

```
WiFiEthernetVPN
```

Which one should Linux use?

The answer is

> The Routing Table

---

The routing table is simply a list of rules.

Example

```
Destination↓Use WiFi
```

```
Destination↓Use VPN
```

```
Destination↓Use Ethernet
```

---

Think of Google Maps.

You ask

```
Go to Airport
```

Maps chooses

```
Road ARoad BRoad C
```

Similarly,

Linux asks

```
Destination IP?
```

Routing table answers

```
Use interface wlan0Gateway = Router
```

---

Visualization

```
Destination↓Routing Table↓Choose Interface↓Router
```

---

Typical Entry

```
DestinationGatewayNetwork Interface
```

Example

```
0.0.0.0↓Gateway↓192.168.1.1↓Interface↓wlan0
```

This means

> Everything unknown goes through my router.

This is called the **Default Route**.

---

# 5. What is a Socket?

This is one of the most important Linux concepts.

A **Socket** is a communication endpoint between an application and the kernel's networking stack.

Applications do **not** send packets directly.

They create sockets.

---

Mental Model

Think of a telephone.

```
You↓Telephone↓Telephone Network↓Friend
```

Applications use sockets exactly like telephones.

---

Chrome

```
Chrome↓Socket↓Kernel↓Network
```

---

SSH

```
SSH↓Socket↓Kernel↓Remote Machine
```

---

Every network application

```
BrowserSSHDiscordSteamVS CodeDockerGitFTPEmail
```

uses sockets.

---

# What does a Socket contain?

A socket typically contains

```
Source IPDestination IPSource PortDestination PortProtocolConnection State
```

---

Visualization

```
Socket│├── My IP├── Destination IP├── My Port├── Destination Port└── TCP / UDP
```

---

# Why Ports?

Suppose

Chrome

Discord

Spotify

are all using the internet.

How does Linux know which application should receive incoming data?

Using **Ports**.

Example

```
Chrome↓Port 52100
```

```
Discord↓Port 50120
```

```
SSH Server↓Port 22
```

A socket is essentially:

```
IP Address + Port + Protocol
```

---

# Complete Journey of a Packet

Imagine Chrome sends

```
GET google.com
```

Step 1

```
Chrome↓Creates Socket
```

---

Step 2

```
Kernel receives request
```

---

Step 3

```
Routing Table↓Choose WiFi
```

---

Step 4

```
Kernel asks↓What is router's MAC?
```

Uses **ARP** if necessary.

---

Step 5

```
Kernel builds packet↓Destination IP↓Destination MAC↓Protocol↓Port
```

---

Step 6

```
Network Interface↓Converts data to electrical/radio signals
```

---

Step 7

```
Router↓Internet↓Google
```

---

# Putting Everything Together

```
Application     │Creates Socket     │Kernel Networking Stack     │Routing Table     │Choose Interface     │Find Destination MAC     │Network Card     │Router     │Internet     │Destination Computer
```

---

# 🧠 Internal Kernel Flow

Whenever an application calls functions like `connect()`, `send()`, or `recv()` (system calls into the kernel), the networking subsystem roughly performs these steps:

```
Application↓System Call↓Kernel Networking Stack↓Socket Lookup↓Routing Table Lookup↓ARP (if local MAC is unknown)↓Build Ethernet Frame + IP Packet↓Driver↓Network Interface↓Wire / WiFi
```

The application only says:

> "Send these bytes to this IP and port."

The kernel figures out **how**.

---

# 🔑 Key Takeaways

- A **Network Interface** is the computer's connection to a network (Ethernet, Wi-Fi, loopback, VPN, etc.).
- An **IP Address** is a logical address that identifies **where** data should be delivered.
- A **MAC Address** is the hardware identity of a network interface, used for communication on the local network.
- A **Routing Table** is the kernel's roadmap that decides **which interface and gateway** should be used to reach a destination.
- A **Socket** is the interface between an application and the kernel's networking stack. Applications communicate through sockets rather than directly with network hardware.
- **Ports** allow multiple network applications to communicate simultaneously over the same IP address.
- The Linux kernel manages the entire journey from the application to the network interface, handling routing, addressing, packet creation, and transmission.

---

# 🚀 Preview of the Next Step

Now that you understand the theory, the next layer is **observing these concepts on a real Linux system**. You'll learn commands such as:

- **`ip`** _(short for Internet Protocol utility)_ — view interfaces, IP addresses, and routes.
- **`ss`** _(Socket Statistics; modern replacement for `netstat`)_ — inspect open sockets and network connections.
- **`ping`** _(named after sonar "ping" echoes, not an acronym)_ — test network reachability.
- **`traceroute`** — discover the path packets take through routers.
- **`arp`** or **`ip neigh`** — inspect the mapping between IP addresses and MAC addresses.

We'll examine each command by explaining **what the command name means, what every option does, how it works internally, and then use it**, following your preferred learning style.