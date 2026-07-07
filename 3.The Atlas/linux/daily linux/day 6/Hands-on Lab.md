Run these commands:

```
ip addr
```

See your network interfaces and IP addresses.

```
ip route
```

View your routing table.

```
hostname -I
```

Display your current IP address.

```
ss -tuln
```

List listening TCP and UDP ports.

```
ping -c 4 8.8.8.8
```

Test connectivity using Google's public DNS server.

---

## 🔬 Mini Experiment (5 min)

Run:

```
ping -c 4 google.com
```

Then compare it with:

```
ping -c 4 8.8.8.8
```

Notice:

- Using `google.com` requires **DNS**.
- Using `8.8.8.8` skips DNS and uses the IP directly.