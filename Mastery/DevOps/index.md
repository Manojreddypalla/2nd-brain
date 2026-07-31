# DevOps Roadmap (Interview + Real Industry)

```
Computer Fundamentals
        │
        ▼
Linux
        │
        ▼
Networking
        │
        ▼
Git
        │
        ▼
Programming/Scripting
        │
        ▼
Build Systems
        │
        ▼
Containers (Docker)
        │
        ▼
Container Orchestration (Kubernetes)
        │
        ▼
Cloud
        │
        ▼
Infrastructure as Code
        │
        ▼
Configuration Management
        │
        ▼
CI/CD
        │
        ▼
Monitoring
        │
        ▼
Logging
        │
        ▼
Security (DevSecOps)
        │
        ▼
Production Architecture
        │
        ▼
Advanced DevOps
        │
        ▼
Interview Preparation
```

---

# PHASE 0 — Computer Fundamentals

Understand how computers actually work.

## OS

- Process
    
- Thread
    
- Scheduling
    
- Context Switch
    
- Memory
    
- Virtual Memory
    
- Paging
    
- Segmentation
    
- Deadlock
    
- IPC
    
- Synchronization
    
- File System
    

---

## Networking

- OSI Model _(5–10 min refresher)_
- TCP/IP Model
- IP Addressing + CIDR + Subnetting
- ARP & ICMP
- TCP
- TCP 3-Way Handshake & 4-Way Termination
- UDP
- DNS
- DHCP
- HTTP
- HTTPS (TLS)
- NAT
- Routing
- Firewall (iptables/nftables)
- VPN
- Load Balancer (L4 vs L7)
- Reverse Proxy (Nginx)
- Ports & Sockets
- Kubernetes Networking (Pods, Services, Ingress) ⭐

---

## Storage

- HDD
    
- SSD
    
- RAID
    
- NAS
    
- SAN
    
- Object Storage
    
- Block Storage
    

---

# PHASE 1 — Linux (Very Deep)

This is the biggest interview topic.

---

## Linux Basics

```
pwd
ls
cd
mkdir
touch
cp
mv
rm
find
locate
tree
```

---

## Permissions

```
chmod
chown
umask
ACL
Sticky Bit
SUID
SGID
```

---

## Users

```
useradd
groupadd
passwd
sudo
/etc/passwd
/etc/shadow
```

---

## Process Management

```
ps
top
htop
nice
renice
kill
killall
pkill
jobs
fg
bg
nohup
systemd
```

---

## Memory

```
free
vmstat
sar
iostat
```

---

## Filesystem

```
mount
umount
fstab
LVM
inode
hard link
soft link
```

---

## Networking

```
ping
curl
wget
nc
telnet
netstat
ss
tcpdump
iptables
nftables
ip
route
```

---

## Logs

```
journalctl
syslog
dmesg
tail
less
grep
awk
sed
```

---

## Shell

- Variables
    
- Loops
    
- Arrays
    
- Functions
    
- Cron
    
- Bash scripting
    

---

## Advanced Linux

- Namespaces
    
- Cgroups
    
- Signals
    
- Process Tree
    
- Proc filesystem
    
- Sys filesystem
    
- Udev
    
- Systemd
    
- Boot process
    
- Kernel modules
    

---

# PHASE 2 — Git

## Basics

```
clone
init
add
commit
push
pull
fetch
status
log
```

---

## Branching

```
branch
merge
rebase
cherry-pick
stash
tag
```

---

## Advanced

- Merge conflicts
    
- Interactive rebase
    
- Squash
    
- Git Hooks
    
- GitFlow
    
- Trunk Based Development
    

---

# PHASE 3 — Programming

Choose

- Python (recommended)
    

OR

- Go
    

---

## Python

- Functions
    
- Classes
    
- JSON
    
- YAML
    
- File Handling
    
- Requests
    
- Regex
    
- APIs
    
- Threading
    
- Multiprocessing
    

---

## Bash

Write scripts for

- Backups
    
- Deployment
    
- Monitoring
    
- Log Parsing
    

---

# PHASE 4 — Build Tools

Understand how code becomes an executable artifact.

Java

- Maven
    
- Gradle
    

Node

- npm
    
- yarn
    
- pnpm
    

Python

- pip
    
- Poetry
    

---

# PHASE 5 — Docker

Very important.

---

## Basics

- Images
    
- Containers
    
- Layers
    
- Dockerfile
    
- Build Context
    

---

## Dockerfile

- FROM
    
- COPY
    
- ADD
    
- RUN
    
- CMD
    
- ENTRYPOINT
    
- ENV
    
- WORKDIR
    
- EXPOSE
    
- USER
    

---

## Networking

- Bridge
    
- Host
    
- Overlay
    
- Macvlan
    

---

## Storage

- Volumes
    
- Bind Mounts
    
- Named Volumes
    

---

## Docker Compose

- Services
    
- Networks
    
- Volumes
    

---

## Advanced

- Multi-stage builds
    
- BuildKit
    
- Health Checks
    
- Registry
    
- Docker Hub
    
- Private Registry
    

---

# PHASE 6 — Kubernetes

The biggest topic.

---

## Architecture

- Master
    
- Worker
    
- kubelet
    
- kube-proxy
    
- etcd
    
- Scheduler
    
- API Server
    

---

## Objects

- Pod
    
- Deployment
    
- ReplicaSet
    
- StatefulSet
    
- DaemonSet
    
- Job
    
- CronJob
    

---

## Networking

- Service
    
- ClusterIP
    
- NodePort
    
- LoadBalancer
    
- Ingress
    

---

## Storage

- PV
    
- PVC
    
- StorageClass
    

---

## Config

- Secret
    
- ConfigMap
    

---

## Scheduling

- Taints
    
- Tolerations
    
- Affinity
    
- Node Selector
    

---

## Security

- RBAC
    
- Service Account
    
- Network Policies
    

---

## Scaling

- HPA
    
- VPA
    

---

## Troubleshooting

```
kubectl logs
kubectl describe
kubectl exec
kubectl get events
```

---

# PHASE 7 — Cloud

Choose one

AWS (recommended)

---

## Compute

- EC2
    
- AMI
    
- Auto Scaling
    

---

## Storage

- S3
    
- EBS
    
- EFS
    

---

## Networking

- VPC
    
- Subnet
    
- Route Table
    
- NAT
    
- Internet Gateway
    
- Security Groups
    
- NACL
    

---

## Identity

- IAM
    
- Roles
    
- Policies
    

---

## Monitoring

- CloudWatch
    

---

## DNS

- Route53
    

---

## Containers

- ECS
    
- EKS
    

---

## Serverless

- Lambda
    

---

## Secrets

- Secrets Manager
    
- Parameter Store
    

---

# PHASE 8 — Infrastructure as Code

Terraform

---

## Basics

- Providers
    
- Resources
    
- Variables
    
- Outputs
    

---

## Advanced

- Modules
    
- State
    
- Backend
    
- Remote State
    
- Workspaces
    

---

# PHASE 9 — Configuration Management

Ansible

---

- Inventory
    
- Playbook
    
- Roles
    
- Templates
    
- Variables
    
- Vault
    
- Galaxy
    

---

# PHASE 10 — CI/CD

Jenkins

---

## Basics

- Pipeline
    
- Stage
    
- Agent
    

---

## Pipeline

```
Build

↓

Test

↓

Lint

↓

Security Scan

↓

Docker Build

↓

Push Image

↓

Deploy Kubernetes

↓

Smoke Test
```

---

Tools

- GitHub Actions
    
- GitLab CI
    
- Jenkins
    
- ArgoCD
    

---

# PHASE 11 — Monitoring

Prometheus

- Metrics
    
- Exporters
    
- Alertmanager
    

Grafana

- Dashboards
    
- Alerts
    

---

# PHASE 12 — Logging

- ELK
    
- EFK
    
- Loki
    
- Fluentd
    
- Fluent Bit
    

Know how logs move from applications to a searchable dashboard.

---

# PHASE 13 — Security (DevSecOps)

- SSH
    
- TLS/SSL
    
- Certificates
    
- Secrets Management
    
- Vault
    
- IAM
    
- Least Privilege
    
- Image Scanning
    
- Trivy
    
- SAST
    
- DAST
    
- Dependency Scanning
    
- Admission Controllers
    

---

# PHASE 14 — Production Architecture

Learn how real systems are deployed.

```
GitHub

↓

GitHub Actions

↓

Docker

↓

Docker Registry

↓

Kubernetes

↓

Ingress

↓

Service

↓

Pods

↓

Database

↓

Monitoring

↓

Logging

↓

Alerts
```

Also study:

- Blue-Green Deployment
    
- Canary Deployment
    
- Rolling Update
    
- Rollback
    
- Load Balancer
    
- CDN
    
- Reverse Proxy (Nginx)
    
- API Gateway
    
- Message Queue (RabbitMQ/Kafka)
    
- Caching (Redis)
    
- High Availability
    
- Disaster Recovery
    

---

# PHASE 15 — Advanced DevOps / Platform Engineering

- Helm
    
- Kustomize
    
- ArgoCD
    
- FluxCD
    
- Crossplane
    
- OpenTofu
    
- Packer
    
- Consul
    
- Nomad
    
- Istio
    
- Linkerd
    
- Envoy
    
- OpenTelemetry
    
- eBPF
    
- Falco
    
- Kyverno
    
- OPA/Gatekeeper
    
- KEDA
    
- Velero
    
- Harbor
    

---

# PHASE 16 — Interview Preparation

## Linux (Most Asked)

- Zombie process
    
- Orphan process
    
- Context switch
    
- Difference between hard and soft links
    
- inode
    
- chmod 755 vs 777
    
- grep vs awk vs sed
    
- systemctl vs service
    
- top vs htop
    
- Load average
    
- Why swap?
    
- Boot process
    
- What happens after power button?
    

---

## Docker

- Image vs Container
    
- CMD vs ENTRYPOINT
    
- COPY vs ADD
    
- Volume vs Bind Mount
    
- Layer Caching
    
- Multi-stage build
    
- Why containers are lightweight?
    
- Docker networking modes
    

---

## Kubernetes

- Pod lifecycle
    
- Pod vs Deployment
    
- Service types
    
- ConfigMap vs Secret
    
- Why etcd?
    
- CrashLoopBackOff
    
- ImagePullBackOff
    
- Liveness vs Readiness Probe
    
- StatefulSet vs Deployment
    
- Taints vs Affinity
    

---

## AWS

- EC2
    
- IAM
    
- S3
    
- VPC
    
- Security Group vs NACL
    
- Auto Scaling
    
- Load Balancer types
    
- Route53
    

---

## Terraform

- State file
    
- Modules
    
- Backend
    
- Variables
    
- Lifecycle
    
- Import
    
- Plan vs Apply
    

---

## Jenkins / CI-CD

- Jenkinsfile
    
- Pipeline stages
    
- Agents
    
- Webhooks
    
- Artifacts
    
- Parallel stages
    
- Rollback strategy
    

---

## Monitoring

- Prometheus pull model
    
- Metrics vs Logs vs Traces
    
- Grafana dashboards
    
- Alerting
    
- SLI, SLO, SLA
    

---

# Capstone Projects (What Recruiters Love)

1. **Full CI/CD Pipeline**: GitHub → GitHub Actions/Jenkins → Docker → Kubernetes → Monitoring (Prometheus + Grafana).
    
2. **AWS Three-Tier Application**: VPC, EC2/EKS, RDS, ALB, Auto Scaling, Terraform, Ansible.
    
3. **GitOps Deployment**: ArgoCD + Helm + Kubernetes with automatic sync from Git.
    
4. **Observability Stack**: Deploy Prometheus, Grafana, Loki, and Alertmanager; instrument a sample app with OpenTelemetry.
    
5. **Production Kubernetes Cluster**: Ingress, TLS, autoscaling, persistent volumes, RBAC, network policies, backups, and rolling/canary deployments.
    

---

## Suggested Learning Order

|Stage|Focus|
|---|---|
|1|Linux (deep)|
|2|Networking|
|3|Git|
|4|Bash + Python|
|5|Build tools|
|6|Docker|
|7|Kubernetes|
|8|AWS|
|9|Terraform|
|10|Ansible|
|11|CI/CD|
|12|Monitoring & Logging|
|13|Security (DevSecOps)|
|14|Production Architecture|
|15|Advanced Platform Engineering|
|16|Mock interviews + Capstone projects|

This roadmap covers the knowledge expected from a strong junior DevOps engineer and extends into the areas commonly expected of mid-level engineers working on modern cloud-native infrastructure.