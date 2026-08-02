# 🐧 Linux Internals — Container Networking Deep Dive

## Bridges, veth Pairs & Network Namespaces

> **Goal:** Understand what actually happens inside Linux when a Docker container communicates with another container or the Internet.

---

# 1. The Core Problem

Suppose Docker starts a container:

```bash
docker run nginx
```

Docker wants that container to behave like an isolated machine.

So it gives it a separate **Network Namespace**.

That namespace gets its own:

```text
Network interfaces
IP addresses
Routing table
Sockets
Firewall/network rules
Neighbor table
```

But isolation creates a problem.

A fresh network namespace basically begins disconnected:

```text
Container Network Namespace

┌─────────────────────────┐
│                         │
│       lo                │
│    127.0.0.1            │
│                         │
│        ❌ Internet      │
│        ❌ Host          │
│        ❌ Containers    │
│                         │
└─────────────────────────┘
```

So Linux needs some way to **connect this isolated network world back to the host**.

The key pieces are:

```text
Network Namespace
       +
veth Pair
       +
Linux Bridge
       +
Routing
       +
NAT
```

Docker combines these primitives for its classic bridge networking.

---

# 2. First: What Does a Network Namespace Isolate?

Normally your host has one network stack:

```text
Linux Host
│
├── eth0 / wlan0
├── IP addresses
├── Routing table
├── Neighbor table
├── Sockets
└── Network configuration
```

Create another network namespace and you effectively create another isolated networking environment:

```text
HOST NET NS                    CONTAINER NET NS

eth0                           lo
192.168.1.20                   127.0.0.1

Routes                         Routes
Neighbors                      Neighbors
Sockets                        Sockets
```

The container can't simply use the host's `eth0`.

That's intentional.

Namespaces provide the **isolation boundary**.

---

# 3. Enter the veth Pair

`veth` means:

> **Virtual Ethernet**

A veth pair is easiest to imagine as a virtual Ethernet cable.

```text
veth-A  <==================>  veth-B
```

The important property:

> A packet entering one endpoint appears at the other endpoint.

Think:

```text
Packet
  ↓
veth-A
  ════════════>
             veth-B
                ↓
             Packet
```

The endpoints are connected.

---

# 4. Why veth Pairs Are Perfect for Containers

Here's the clever part.

Linux can put the two ends in **different network namespaces**.

```text
HOST NETWORK NAMESPACE       CONTAINER NETWORK NAMESPACE

       veth-host  <=========>  veth-container
                                    │
                               renamed eth0
```

Docker typically leaves one end on the host and moves the other into the container's network namespace.

Inside the container, that interface normally appears as:

```text
eth0
```

So:

```text
Host                              Container

vethXXXX   <=================>      eth0
```

Now the isolated namespace has a connection to the host networking environment.

---

# 5. But What About Multiple Containers?

Suppose we have:

```text
Container A
Container B
Container C
```

Each gets a veth pair.

We could theoretically create complicated point-to-point connections, but that's ugly.

Physical networks solved this with switches.

Linux does the same thing in software.

Enter the:

# Linux Bridge

A Linux bridge is essentially a:

> **Layer-2 software switch implemented by Linux.**

Mental model:

```text
              Linux Bridge
         ┌───────────────────┐
         │                   │
vethA ───┤                   ├─── vethB
         │                   │
         └───────────────────┘
```

The bridge forwards Ethernet frames between its ports.

---

# 6. Docker's `docker0`

With Docker's traditional default bridge setup, Docker creates a Linux bridge commonly called:

```text
docker0
```

Architecture:

```text
                  HOST
                   │
          ┌────────┴─────────┐
          │     docker0      │
          │  Linux Bridge    │
          └──────┬─────┬─────┘
                 │     │
              vethA   vethB
                 │     │
                 │     │
              eth0    eth0
                 │     │
          ┌──────┘     └──────┐
          │                   │
     Container A         Container B
     172.17.0.2          172.17.0.3
```

Now both containers effectively plug into the same virtual Ethernet switch.

---

# 7. Container IP Addresses

Docker's default bridge historically commonly uses something like:

```text
172.17.0.0/16
```

For example:

```text
docker0
172.17.0.1

Container A
172.17.0.2

Container B
172.17.0.3
```

Conceptually:

```text
             docker0
            172.17.0.1
                 │
       ┌─────────┴─────────┐
       │                   │
   172.17.0.2          172.17.0.3
   Container A         Container B
```

Exact addresses/subnets can differ depending on Docker/network configuration.

---

# 8. Container-to-Container Communication

Suppose:

```text
Container A = 172.17.0.2
Container B = 172.17.0.3
```

Container A sends data to B.

Packet path:

```text
Application A
     │
     │ socket/send
     ▼
Container A TCP/IP stack
     │
     ▼
eth0
     │
     ▼
veth pair
     │
     ▼
Host-side vethA
     │
     ▼
docker0
     │
     │ Layer 2 forwarding
     ▼
Host-side vethB
     │
     ▼
veth pair
     │
     ▼
Container B eth0
     │
     ▼
Container B TCP/IP stack
     │
     ▼
Application B
```

Notice something important:

**The packets don't need to leave the physical computer.**

The Linux kernel handles the entire path internally.

---

# 9. How Does the Bridge Know Where to Send Frames?

Remember Ethernet switching from networking?

Switches learn:

```text
MAC address → Port
```

A Linux bridge does the same thing.

It maintains a forwarding database:

```text
FDB = Forwarding Database
```

Conceptually:

```text
MAC Address             Interface

02:42:ac:11:00:02  →    vethA
02:42:ac:11:00:03  →    vethB
```

You can inspect it using:

```bash
bridge fdb show
```

So Docker networking directly connects to the Layer-2 switching concepts you've already learned.

---

# 10. IP vs MAC Address

Suppose A wants:

```text
172.17.0.3
```

Ethernet ultimately needs the destination's MAC address.

So A needs:

```text
172.17.0.3
      ↓
      ?
      ↓
MAC address
```

That's where the neighbor/ARP mechanism comes in for IPv4.

Conceptually:

```text
Container A

"Who has 172.17.0.3?"
          │
          ▼
       ARP request
          │
       docker0
          │
          ▼
Container B

"172.17.0.3 is at MAC BB:BB:BB..."
```

A caches the result.

---

# 11. `ip neigh`

Run:

```bash
ip neigh
```

`neigh` means:

> Neighbor table

You might see:

```text
192.168.1.1 dev wlan0 lladdr aa:bb:cc:dd:ee:ff REACHABLE
```

Meaning:

```text
IP
192.168.1.1

      ↓ maps to

MAC
aa:bb:cc:dd:ee:ff
```

Think:

```text
IP address
   ↓
Routing decides interface/next hop
   ↓
Neighbor discovery resolves link-layer address
   ↓
MAC address
   ↓
Ethernet frame
```

For IPv4 this generally involves **ARP**; IPv6 uses **Neighbor Discovery (NDP)**.

---

# 12. Container Routing Table

A container also needs routing.

Inside a container, conceptually:

```text
172.17.0.0/16 → eth0

default → 172.17.0.1
```

Meaning:

```text
Destination inside Docker subnet?
        │
       YES
        ↓
Send through eth0 directly

Destination somewhere else?
        │
       YES
        ↓
Send to default gateway
172.17.0.1
```

The gateway is usually the bridge-side address for that Docker network.

---

# 13. Container → Internet

Now we reach the interesting part.

Suppose:

```text
Container
172.17.0.2
```

wants to connect to:

```text
8.8.8.8
```

Packet starts as:

```text
SOURCE
172.17.0.2

DESTINATION
8.8.8.8
```

It follows the container's default route.

```text
Application
    │
    ▼
socket()
    │
    ▼
Container TCP/IP stack
    │
    ▼
eth0
    │
    ▼
veth
    │
    ▼
docker0
    │
    ▼
Host routing
```

But there's a problem.

---

# 14. Private Container Addresses

Addresses such as:

```text
172.17.0.2
```

are private addresses.

Internet routers won't route packets back to arbitrary Docker private networks.

Your external network knows your host's address, not:

```text
172.17.0.2
```

Therefore Linux/Docker performs:

# NAT

**Network Address Translation**

---

# 15. NAT Mental Model

Suppose:

```text
Container IP
172.17.0.2

Host external IP
192.168.1.20
```

Container creates:

```text
SRC = 172.17.0.2
DST = 8.8.8.8
```

Before the packet leaves the host, NAT can rewrite its source:

```text
BEFORE

SRC = 172.17.0.2
DST = 8.8.8.8


AFTER

SRC = 192.168.1.20
DST = 8.8.8.8
```

Now the outside network sees the connection as coming from the host.

---

# 16. MASQUERADE / SNAT

This type of translation is generally:

```text
SNAT
```

**Source Network Address Translation**

Docker commonly configures Linux firewall/NAT rules to perform masquerading for bridge networks.

Conceptually:

```text
172.17.0.2
     │
     ▼
docker0
     │
     ▼
Host routing
     │
     ▼
SNAT / MASQUERADE
     │
     ▼
Host external address
     │
     ▼
Internet
```

The exact firewall backend may involve iptables/nftables depending on your system and Docker configuration.

---

# 17. Full Container → Internet Journey

This is the diagram worth remembering.

```text
┌────────────────────────────┐
│ Container                  │
│                            │
│ Application                │
│     │                      │
│     │ socket/connect       │
│     ▼                      │
│ TCP/IP Stack               │
│     │                      │
│    eth0                    │
│  172.17.0.2                │
└─────┬──────────────────────┘
      │
      │ veth pair
      ▼
┌────────────────────────────┐
│ Host                       │
│                            │
│ host-side veth             │
│      │                     │
│      ▼                     │
│   docker0                  │
│  172.17.0.1                │
│      │                     │
│      ▼                     │
│ Host routing               │
│      │                     │
│      ▼                     │
│ NAT / MASQUERADE           │
│      │                     │
│      ▼                     │
│ eth0 / wlan0               │
└──────┬─────────────────────┘
       │
       ▼
     Router
       │
       ▼
    Internet
```

That's the core of today's topic.

---

# 18. Return Traffic

Suppose the Internet server replies.

```text
Internet
   │
   ▼
Router
   │
   ▼
Host eth0
   │
   ▼
Connection tracking / NAT
   │
   ▼
172.17.0.2
   │
   ▼
docker0
   │
   ▼
veth-host
   │
   ═══════════>
             eth0
               │
               ▼
          Container
```

Linux tracks the connection so the reply can be translated back and delivered to the correct container.

This is where **connection tracking (`conntrack`)** becomes important internally.

---

# 19. Host Port Publishing

Now consider:

```bash
docker run -p 8080:80 nginx
```

Meaning:

```text
Host port      Container port

8080     →          80
```

A client accesses:

```text
HOST:8080
```

Docker/Linux networking forwards the traffic toward:

```text
Container:80
```

Mental model:

```text
Internet / Host Client
        │
        ▼
Host :8080
        │
   NAT/forwarding
        │
        ▼
docker0
        │
        ▼
veth
        │
        ▼
Container :80
        │
        ▼
nginx
```

So port publishing is another place where container networking and Linux packet filtering/NAT meet.

---

# 20. Inspect Docker Networks

Run:

```bash
docker network ls
```

Possible output:

```text
NETWORK ID     NAME      DRIVER
xxxx           bridge    bridge
xxxx           host      host
xxxx           none      null
```

The important default modes to recognize:

```text
bridge
host
none
```

---

# 21. Bridge Network

```text
bridge
```

Container receives an isolated network namespace and participates in a Docker network.

Conceptually:

```text
Container
    │
   veth
    │
 Docker Bridge
    │
   Host
```

This is the main architecture we're studying.

---

# 22. Host Network

Docker can use:

```bash
docker run --network host ...
```

On Linux, host networking means the container shares the host's network namespace rather than getting the usual separate container network namespace.

Conceptually:

```text
Normal Bridge

Host NS          Container NS
   │                  │
   └──── veth ────────┘


Host Mode

┌──────────────────────┐
│ Host Network NS      │
│                      │
│ Host                 │
│ Container            │
│                      │
└──────────────────────┘
```

Therefore the usual bridge/veth isolation isn't used in the same way.

---

# 23. None Network

```bash
docker run --network none ...
```

The container gets networking isolation but essentially no external network connectivity.

Conceptually:

```text
Container Network Namespace

┌────────────────────┐
│                    │
│        lo          │
│                    │
│     no veth        │
│     no bridge      │
│                    │
└────────────────────┘
```

Great demonstration of what a network namespace looks like before you wire networking into it.

---

# 24. Inspect the Docker Bridge

Run:

```bash
docker network inspect bridge
```

Look for fields describing things such as:

```text
Subnet
Gateway
Containers
Network configuration
```

You might encounter:

```text
Subnet:
172.17.0.0/16

Gateway:
172.17.0.1
```

Again, exact values depend on your setup.

---

# 25. Inspect `docker0`

Run:

```bash
ip addr show docker0
```

Breakdown:

`ip`

Modern Linux networking administration command.

`addr`

Address information.

`show`

Display it.

`docker0`

Specific interface.

You may see an address such as:

```text
inet 172.17.0.1/16
```

That is the host-side gateway address for the default bridge network in a typical setup.

---

# 26. Inspect All Interfaces

Run:

```bash
ip link
```

You might see:

```text
lo
eth0
wlan0
docker0
vethXXXX
vethYYYY
```

Mental grouping:

```text
lo
↓
Loopback

eth0/wlan0
↓
Physical/external network

docker0
↓
Software bridge

vethXXXX
↓
Virtual Ethernet endpoint
```

---

# 27. Why Does `vethXXXX` Look Random?

Docker dynamically creates virtual Ethernet interfaces.

The host side might look like:

```text
veth12ab34
```

while inside the container:

```text
eth0
```

Remember:

```text
Host

veth12ab34
     ║
     ║ same veth pair
     ║
Container

eth0
```

Different interface names.

Same virtual cable.

---

# 28. Inspect Bridge Ports

Run:

```bash
bridge link
```

This shows interfaces participating in bridges.

You may find host-side veth interfaces associated with `docker0`.

Mental model:

```text
docker0
  │
  ├── veth123
  ├── veth456
  └── veth789
```

Each veth can lead toward a container namespace.

---

# 29. Inspect Host Routing

Run:

```bash
ip route
```

You might see something conceptually like:

```text
default via 192.168.1.1 dev wlan0

172.17.0.0/16 dev docker0

192.168.1.0/24 dev wlan0
```

Read this as:

```text
172.17.x.x
     ↓
docker0

192.168.1.x
     ↓
wlan0

everything else
     ↓
default gateway
     ↓
192.168.1.1
```

This is where your earlier networking knowledge connects directly to Linux internals.

---

# 30. Routing vs Bridging

This distinction matters.

## Bridge

Primarily Layer 2.

Works with:

```text
Ethernet frames
MAC addresses
```

Think:

> **Software switch**

## Router

Layer 3.

Works with:

```text
IP packets
IP addresses
routing tables
```

Think:

> **Choose where an IP packet goes next.**

So:

```text
Bridge
MAC → MAC

Router
IP → next hop/interface
```

---

# 31. The Packet Changes Layers

Suppose:

```text
Container A
172.17.0.2
```

contacts:

```text
8.8.8.8
```

Application thinks:

```text
Connect to 8.8.8.8
```

TCP/IP creates:

```text
IP packet

SRC IP = 172.17.0.2
DST IP = 8.8.8.8
```

To reach the next hop over Ethernet, Linux wraps that packet:

```text
Ethernet Frame

┌───────────────────────────┐
│ Destination MAC           │
│ Source MAC                │
│                           │
│   IP Packet               │
│   ┌───────────────────┐   │
│   │ SRC 172.17.0.2    │   │
│   │ DST 8.8.8.8       │   │
│   │ TCP ...           │   │
│   └───────────────────┘   │
└───────────────────────────┘
```

This is the beautiful connection between your **Computer Networks knowledge and Linux internals**.

---

# 32. Networking System Calls

Eventually everything starts with an application.

Suppose Python executes:

```python
socket.connect(...)
```

Underneath, Linux networking uses system calls such as:

```text
socket()
bind()
listen()
accept()
connect()
sendto()
recvfrom()
```

Mental model:

```text
Application
     │
     │ socket()
     ▼
──────── User/Kernel boundary ────────
     │
     ▼
Kernel socket
     │
     ▼
TCP/UDP
     │
     ▼
IP
     │
     ▼
Network interface
```

Containers don't implement their own networking kernel.

They use the **same host Linux kernel**, but network namespaces give processes isolated views/network stacks.

---

# 33. Capabilities Connection

Networking administration is privileged.

Linux provides capabilities such as:

```text
CAP_NET_ADMIN
```

This can permit operations such as modifying certain:

```text
interfaces
routes
firewall/network configuration
```

Another important capability:

```text
CAP_NET_RAW
```

associated with operations involving raw/packet sockets.

Therefore containers can run with reduced networking privileges rather than simply receiving unrestricted root powers.

---

# 34. cgroups Connection

Remember:

```text
Namespaces → isolation

cgroups → resource control/accounting
```

cgroups are not the mechanism that creates the veth or bridge.

They complement container isolation by controlling/accounting for resources such as:

```text
CPU
memory
I/O
process counts
```

Networking controls may involve additional Linux mechanisms such as traffic control (`tc`), eBPF, firewalling, or orchestration/runtime policies.

So don't memorize:

```text
cgroup = networking
```

Instead:

```text
Namespace → isolated network environment

veth → connection

bridge → Layer-2 connectivity

routing → packet path

NAT → address translation

cgroups → broader resource control/accounting
```

---

# 35. Seccomp Connection

Network communication requires system calls.

Examples:

```text
socket()
connect()
bind()
accept()
```

Seccomp filters system calls.

Conceptually:

```text
Container App
      │
      │ socket()
      ▼
Seccomp Filter
      │
   allowed?
    /    \
  yes     no
   │       │
Kernel   reject/action
```

Seccomp therefore adds another security layer.

---

# 36. The Complete Container Networking Stack

Put everything together:

```text
┌───────────────────────────────────┐
│ CONTAINER                         │
│                                   │
│ Application                       │
│      │                            │
│      ▼                            │
│ socket() / connect()              │
│      │                            │
│      ▼                            │
│ Kernel networking                 │
│ (container network namespace)     │
│      │                            │
│      ▼                            │
│ eth0                              │
│ 172.17.0.2                        │
└──────┬────────────────────────────┘
       │
       │ veth pair
       ▼
┌───────────────────────────────────┐
│ HOST                              │
│                                   │
│ host veth                         │
│      │                            │
│      ▼                            │
│ docker0                           │
│ Linux Bridge                      │
│      │                            │
│      ▼                            │
│ Host routing                      │
│      │                            │
│      ▼                            │
│ Firewall / NAT                    │
│      │                            │
│      ▼                            │
│ eth0 / wlan0                      │
└──────┬────────────────────────────┘
       │
       ▼
     Router
       │
       ▼
    Internet
```

---

# 37. Hands-On Lab

First inspect Docker:

```bash
docker network ls
```

Then:

```bash
docker network inspect bridge
```

Look for:

```text
Subnet
Gateway
Containers
```

---

Inspect interfaces:

```bash
ip link
```

Find:

```text
docker0
vethXXXX
```

---

Inspect bridge IP:

```bash
ip addr show docker0
```

---

Inspect routes:

```bash
ip route
```

Find the route corresponding to the Docker subnet.

---

Inspect bridge membership:

```bash
bridge link
```

---

Inspect neighbor information:

```bash
ip neigh
```

---

# 38. 🔬 Better Mini Lab

If Docker is available, start two containers:

```bash
docker run -dit --name c1 alpine sh
docker run -dit --name c2 alpine sh
```

Inspect:

```bash
docker network inspect bridge
```

Find both container IP addresses.

Then inspect host interfaces:

```bash
ip link
```

You should now see veth interfaces.

Check:

```bash
bridge link
```

Now you've directly observed:

```text
Container
   ↓
eth0
   ↓
veth
   ↓
docker0
```

rather than only memorizing the diagram.

Clean up afterward:

```bash
docker rm -f c1 c2
```

---

# 39. 🔬 Best Experiment — Build It Yourself

This is optional, but it's the experiment that makes Docker networking really click.

Conceptually you can manually construct:

```text
Network Namespace
      +
veth Pair
      +
Bridge
      +
IP addresses
      +
Routes
```

Then you've essentially built a tiny version of container networking yourself.

The progression is:

```text
Create namespace
      ↓
Create veth pair
      ↓
Move one endpoint into namespace
      ↓
Create Linux bridge
      ↓
Attach host veth to bridge
      ↓
Assign IP addresses
      ↓
Bring interfaces up
      ↓
Configure routes
      ↓
Ping
```

That's worth doing as a separate **30–45 minute lab**, because it makes `docker0` stop feeling magical.

---

# 40. Common Interview Question

### What is a veth pair?

A pair of connected virtual Ethernet interfaces.

```text
vethA <==========> vethB
```

Packets entering one endpoint emerge from the other.

They are commonly used to connect network namespaces.

---

### What does `docker0` do?

`docker0` is the traditional default Linux bridge created/used by Docker bridge networking.

Think:

```text
docker0 ≈ virtual Ethernet switch
```

It connects host-side veth interfaces belonging to containers on that bridge network.

---

### Why do containers use private IP addresses?

Containers commonly exist on an internal/private bridge network.

NAT allows them to communicate externally through the host without requiring every container to have its own externally routable address.

---

### How does a container reach the Internet?

```text
Application
   ↓
socket()
   ↓
Container TCP/IP stack
   ↓
eth0
   ↓
veth pair
   ↓
docker0
   ↓
Host routing
   ↓
NAT/MASQUERADE
   ↓
Host NIC
   ↓
Router
   ↓
Internet
```

---

# 41. The Five Questions to Ask

Whenever you're debugging container networking, think in this order:

```text
1. NAMESPACE
   Does the process have the expected network environment?

2. INTERFACE
   Does eth0/veth exist and is it UP?

3. ADDRESS
   Does the interface have the correct IP?

4. ROUTE
   Does Linux know where to send the packet?

5. FIREWALL/NAT
   Is traffic allowed and translated correctly?
```

Then investigate DNS separately if:

```text
ping 8.8.8.8
```

works but:

```text
ping example.com
```

doesn't.

That strongly points toward name-resolution configuration rather than basic packet connectivity.

---

# ⚡ 2-Minute Revision

```text
Network Namespace
→ isolated network environment

veth pair
→ virtual Ethernet cable connecting namespaces

Linux Bridge
→ Layer-2 software switch

docker0
→ traditional Docker default bridge

Container eth0
↔
host veth

Bridge
→ forwards Ethernet frames using MAC addresses

Routing
→ decides where IP packets go

ARP / NDP
→ IP-to-link-layer neighbor resolution

NAT
→ translates addresses

MASQUERADE
→ commonly used source NAT for dynamic host addresses

Port publishing
→ host port → container port

CAP_NET_ADMIN
→ privileged network administration operations

Seccomp
→ can restrict networking-related system calls
```

## 🧠 The one diagram to remember

```text
                    INTERNET
                        ▲
                        │
                      Router
                        ▲
                        │
                  Host eth0/wlan0
                        ▲
                        │
                 NAT / MASQUERADE
                        ▲
                        │
                  Host Routing
                        ▲
                        │
                     docker0
                  Linux Bridge
                   ▲         ▲
                   │         │
                vethA       vethB
                   ║         ║
                veth pair  veth pair
                   ║         ║
                  eth0      eth0
                   │         │
              Container A  Container B
              172.17.0.2  172.17.0.3
```

**Namespaces create the isolated network worlds. veth pairs provide the cables. The Linux bridge provides the switch. Routing decides where packets go. NAT provides external connectivity.**

That's Docker bridge networking stripped of the Docker magic.