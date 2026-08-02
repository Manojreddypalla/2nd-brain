# Linux Internals – Day 10

# Network Namespace (Quick Notes)

> **Category:** Linux Internals / Docker / DevOps _(Development Topic)_

---

# What is a Network Namespace?

A **Network Namespace** gives a group of processes **its own isolated network stack**.

Instead of sharing the host's networking, each namespace gets its own:

- Network Interfaces
    
- IP Addresses
    
- Routing Table
    
- ARP Table
    
- Firewall Rules (iptables/nftables)
    
- Listening Ports
    
- Network Sockets
    

**One-Line Definition:**

> A Network Namespace isolates all networking resources for a group of processes.

---

# Why Do We Need It?

Imagine two Docker containers both running an Nginx server.

Without network namespaces:

```text
Host
└── Port 80 (Conflict ❌)
```

Only one process can bind to port 80 on the same network stack.

With network namespaces:

```text
Container A
├── IP: 172.17.0.2
└── Port 80

Container B
├── IP: 172.17.0.3
└── Port 80
```

No conflict because each container has its **own network stack**.

---

# Mental Model

Think of apartments.

Without namespaces:

```text
One Wi-Fi Router
One IP
Everyone shares it.
```

With namespaces:

```text
Apartment A
Own Wi-Fi
Own IP

Apartment B
Own Wi-Fi
Own IP
```

Same building (Linux Kernel), different networks.

---

# What Gets Isolated?

Each Network Namespace has its own:

- 🌐 Network Interfaces (`eth0`, `lo`)
    
- 📍 IP Addresses
    
- 🛣️ Routing Table
    
- 📡 ARP Cache
    
- 🔥 Firewall Rules
    
- 🔌 Open Ports & Sockets
    

---

# Why Does a New Namespace Only Show `lo`?

Run:

```bash
sudo unshare --net bash
ip addr
```

Output:

```text
lo
```

Why?

Because Linux creates a **fresh, empty network stack**.

Only the **Loopback Interface (`lo`)** exists by default.

No Ethernet (`eth0`) or Wi-Fi (`wlan0`) is connected yet.

Docker later connects the namespace using **veth pairs**.

---

# How Docker Uses It

When Docker starts a container:

1. Creates a Network Namespace.
    
2. Creates a **veth pair** (virtual Ethernet cable).
    
3. One end stays on the host.
    
4. Other end goes inside the container as `eth0`.
    
5. Connects the host side to a bridge (`docker0`).
    

```text
Host
 │
docker0 Bridge
 │
├── veth ── Container A (eth0)
└── veth ── Container B (eth0)
```

This gives every container its own IP address.

---

# Important Commands

### View Interfaces

```bash
ip addr
```

Shows interfaces like:

- `lo`
    
- `eth0`
    
- `wlan0`
    

---

### View Routing Table

```bash
ip route
```

Shows how packets leave your machine.

Example:

```text
default via 192.168.1.1
```

---

### Create a Network Namespace

```bash
sudo unshare --net bash
```

Creates an isolated network stack.

---

### View Listening Ports

```bash
ss -tuln
```

Shows active TCP/UDP listening sockets.

Inside a new namespace, you'll usually see almost nothing because no services are running there.

---

# Interview Corner ⭐

### What does a Network Namespace isolate?

**Answer:** Interfaces, IPs, routing table, ARP cache, firewall rules, ports, and sockets.

---

### Why does a new namespace only have `lo`?

Because Linux starts with an empty network stack. Only the loopback interface is created automatically.

---

### How can two containers both use Port 80?

Because they have **different IP addresses inside different Network Namespaces**.

Example:

```text
Container A
172.17.0.2:80

Container B
172.17.0.3:80
```

No conflict.

---

### How does Docker connect containers?

Using:

- **Network Namespace**
    
- **veth pair**
    
- **docker0 bridge**
    

---

# Commands Learned

```bash
ip addr
```

```bash
ip route
```

```bash
sudo unshare --net bash
```

```bash
ss -tuln
```

```bash
exit
```

---

# Day 10 Summary

- A **Network Namespace** gives processes their own isolated network stack.
    
- Each namespace has its own interfaces, IP addresses, routing table, firewall rules, and ports.
    
- A new namespace starts with only the **loopback (`lo`)** interface.
    
- Docker uses **Network Namespaces + veth pairs + bridges** to give each container independent networking.
    
- Two containers can both run a service on **port 80** because they're using different network namespaces (and typically different IP addresses).
    

## Connection So Far

```text
Namespaces
│
├── PID     → Own process tree
├── Mount   → Own filesystem view
└── Network → Own network stack
```

Tomorrow you'll learn **cgroups (Control Groups)**, which answer a different question:

> Namespaces ask: **"What can this process see?"**  
> cgroups ask: **"How much CPU, RAM, and I/O can this process use?"**