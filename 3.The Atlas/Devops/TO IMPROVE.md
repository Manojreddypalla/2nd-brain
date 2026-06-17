❌ Ansible

❌ Kubernetes

❌ Helm

❌ Kustomize

❌ Terraform

❌ Prometheus

❌ Grafana

❌ Loki / ELK Stack

❌ Alertmanager

❌ CI/CD Pipelines (deep knowledge)

❌ Cloud Infrastructure Fundamentals (AWS/Azure/GCP)


### Looking at your interests (homelab, DevOps, self-hosting, Linux), I think your next growth phase is:

Not learning more tools.

Instead ask for every tool:

1. What problem does it solve?
2. How does it work internally?
3. What files/processes are involved?
4. What happens step-by-step when I run a command?
5. What would break if this component disappeared?

For example:

- Docker → namespaces, cgroups, overlayfs.
- Git → objects, commits, trees, blobs.
- Linux users → passwd, shadow, PAM, UID/GID.
- Kubernetes → control loops, reconciliation, etcd.
- Terraform → state, providers, dependency graph.

That's the difference between:+


I actually think this interview gave you a very useful signal.

Looking at everything I know about you:

- You already run a homelab.
    
- You've deployed GitLab, Docker services, Kasm, Penpot, MongoDB.
    
- You've used Linux for years.
    
- You're interested in DevOps, self-hosting, automation, AI infrastructure.
    

Your problem is **not lack of tools**.

Your problem is:

> "I can do it, but I can't explain why it works."

That's a different gap.

---

# Roadmap: Linux + DevOps (Interview-Ready Engineer)

Think of this as 4 layers.

## Layer 1 — Linux Internals (Most Important)

Spend 1-2 months here.

### Users & Permissions

Understand:

- UID
    
- GID
    
- `/etc/passwd`
    
- `/etc/shadow`
    
- `/etc/group`
    
- PAM
    
- sudo
    

Questions you should answer:

- What happens when `useradd` runs?
    
- Why can't normal users read `/etc/shadow`?
    
- Difference between `su` and `sudo`?
    
- What is root actually?
    

---

### Processes

Understand:

- PID
    
- PPID
    
- Process states
    
- Signals
    
- Daemons
    

Questions:

- What happens when you run a command?
    
- What is a zombie process?
    
- What is SIGKILL vs SIGTERM?
    

---

### Systemd

Understand:

- Services
    
- Targets
    
- Journald
    

Questions:

- What happens during boot?
    
- How does a service start automatically?
    

---

### Filesystem

Understand:

- Inodes
    
- Hard links
    
- Soft links
    
- Mounts
    

Questions:

- Why can a file have multiple names?
    
- What is actually stored in an inode?
    

---

### Networking

Must know:

- TCP/IP
    
- DNS
    
- Routing
    
- NAT
    
- Ports
    
- SSH
    

Questions:

- What happens when you type `google.com`?
    
- Difference between TCP and UDP?
    
- Why does SSH use port 22?
    

---

# Layer 2 — Linux Administration

This is where many Linux Admin interviews come from.

### Storage

- LVM
    
- Partitions
    
- Filesystems
    
- RAID
    

Questions:

- Disk full. What do you check?
    
- How do you extend a filesystem?
    

---

### Monitoring

Tools:

- top
    
- htop
    
- vmstat
    
- iostat
    
- free
    
- ss
    
- netstat
    

Questions:

- Server slow. Where do you start?
    
- CPU high. What do you check?
    

---

### Logs

Know:

- journalctl
    
- /var/log
    

Questions:

- Service isn't starting. How do you troubleshoot?
    

---

# Layer 3 — DevOps Foundations

Now everything starts connecting.

### Git

Not just commands.

Understand:

- Blob
    
- Tree
    
- Commit
    
- Branch
    

Question:

- What is a commit internally?
    

---

### Docker

Understand:

- Namespaces
    
- Cgroups
    
- OverlayFS
    

Question:

- How is a container different from a VM?
    

---

### Ansible

Understand:

- Inventory
    
- Playbook
    
- Roles
    
- Idempotency
    

Question:

- Why use Ansible instead of SSH?
    

---

### Terraform

Understand:

- State
    
- Providers
    
- Dependency graph
    

Question:

- Why do we need tfstate?
    

---

# Layer 4 — Kubernetes

Only after the above is solid.

### Core Objects

- Pod
    
- Deployment
    
- Service
    
- Ingress
    
- ConfigMap
    
- Secret
    

---

### Internal Components

- API Server
    
- Scheduler
    
- Controller Manager
    
- etcd
    
- kubelet
    
- kube-proxy
    

Question:

- What happens when you run:
    

```bash
kubectl apply -f nginx.yaml
```

You should be able to explain the entire journey.

---

# The Learning Method

For every topic:

### Step 1

Learn what it does.

Example:

> Docker runs containers.

---

### Step 2

Learn how it works.

Example:

> Docker uses namespaces and cgroups.

---

### Step 3

Trace the flow.

Example:

```bash
docker run nginx
```

Ask:

1. What process starts?
    
2. What files are created?
    
3. What kernel features are used?
    
4. What network is created?
    

---

This is how senior engineers think.

---

If I were mentoring you personally for the next 6 months, I'd actually make you follow:

**Linux Internals → Networking → Docker → Git → Ansible → Terraform → Kubernetes**

Not the other way around.

Because Kubernetes is basically:

> Linux + Networking + Containers + Automation glued together.

Without the foundation, Kubernetes becomes memorization. With the foundation, Kubernetes feels obvious.