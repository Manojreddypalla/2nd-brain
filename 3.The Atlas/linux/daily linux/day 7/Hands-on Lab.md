## Linux Namespaces Lab Commands

### 1. List all namespaces on the system

```bash
lsns
```

---

### 2. Show your current shell's namespace links

```bash
ls -l /proc/$$/ns
```

---

### 3. Show your current shell's PID

```bash
echo $$
```

---

### 4. Create a new UTS (hostname) namespace

```bash
sudo unshare --uts bash
```

---

### 5. Check the hostname inside the new namespace

```bash
hostname
```

---

### 6. Change the hostname (inside the namespace)

```bash
sudo hostname linux-lab
# or
hostname linux-lab
```

---

### 7. Verify the hostname changed

```bash
hostname
```

---

### 8. Open another terminal and verify the host is unchanged

```bash
hostname
```

---

### 9. Exit the namespace

```bash
exit
```

---

### 10. Verify you're back to the host namespace

```bash
hostname
```

---

# View Namespace Information

### Show namespace symbolic links

```bash
ls -l /proc/self/ns
```

### Show another process's namespaces

```bash
ls -l /proc/<PID>/ns
```

Example:

```bash
ls -l /proc/1/ns
```

---

# Create Individual Namespaces

### PID Namespace

```bash
sudo unshare --pid --fork bash
```

Check processes:

```bash
ps -ef
```

---

### Mount Namespace

```bash
sudo unshare --mount bash
```

View mounts:

```bash
mount
```

---

### Network Namespace

```bash
sudo unshare --net bash
```

View interfaces:

```bash
ip addr
```

---

### IPC Namespace

```bash
sudo unshare --ipc bash
```

List IPC objects:

```bash
ipcs
```

---

### User Namespace

```bash
sudo unshare --user bash
```

Check UID:

```bash
id
```

---

### Cgroup Namespace

```bash
sudo unshare --cgroup bash
```

View cgroup:

```bash
cat /proc/self/cgroup
```

---

### Time Namespace (if supported by your kernel)

```bash
sudo unshare --time bash
```

---

# Create Multiple Namespaces Together

```bash
sudo unshare --mount --uts --ipc --net --pid --fork bash
```

Or include user namespace:

```bash
sudo unshare --user --mount --uts --ipc --net --pid --fork bash
```

---

# Enter an Existing Namespace

```bash
sudo nsenter --target <PID> --mount --uts --ipc --net --pid
```

Example:

```bash
sudo nsenter --target 1234 --mount --uts --ipc --net --pid
```

---

# Useful Inspection Commands

```bash
hostname
```

```bash
ip addr
```

```bash
mount
```

```bash
ps -ef
```

```bash
id
```

```bash
cat /proc/self/cgroup
```

```bash
cat /proc/self/uid_map
```

```bash
cat /proc/self/gid_map
```

```bash
readlink /proc/self/ns/pid
```

```bash
readlink /proc/self/ns/net
```

```bash
readlink /proc/self/ns/mnt
```

```bash
readlink /proc/self/ns/uts
```

```bash
readlink /proc/self/ns/user
```

```bash
readlink /proc/self/ns/ipc
```

```bash
readlink /proc/self/ns/cgroup
```

These commands cover the most common namespace operations you'll use while learning Linux internals and containers.