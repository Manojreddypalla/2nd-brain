# DevOps — Complete Foundation Notes

> [!abstract] Core Idea  
> DevOps is not simply a collection of tools such as Docker, Kubernetes, Terraform, and Jenkins.
> 
> It is a way of building, testing, deploying, operating, and improving software **quickly, reliably, securely, and repeatedly through collaboration and automation**.

---

# 1. DevOps Culture

## What problem does DevOps solve?

Traditionally:

```text
Developers
    ↓
Write application code
    ↓
Operations Team
    ↓
Deploy + Maintain production
```

Developers mainly focused on:

- Building features
    
- Writing code
    
- Shipping changes
    

Operations mainly focused on:

- Stability
    
- Servers
    
- Deployment
    
- Reliability
    
- Production
    

This separation often created:

- Communication gaps
    
- Slow releases
    
- Friction between teams
    
- Deployment problems
    
- Responsibility silos
    

DevOps tries to remove these silos.

```text
Development + Operations
          ↓
        DevOps
          ↓
Build → Test → Deploy → Operate
```

## DevOps Philosophy

A useful idea is:

> **You build it, you run it.**

The team takes responsibility for the complete software lifecycle rather than throwing code from one team to another.

---

# 2. Roles Around DevOps

Several job titles exist around the same ecosystem.

## DevOps Engineer

Main focus:

- Automation
    
- CI/CD pipelines
    
- Deployment workflows
    
- Infrastructure
    
- Containers
    
- Cloud
    

## SRE

**SRE = Site Reliability Engineer**

Main focus:

- Uptime
    
- Reliability
    
- Incident response
    
- Reliability targets
    
- Production stability
    

## Platform Engineer

Builds internal platforms and tools that developers can use.

The goal is to make development and deployment easier for application teams.

## Cloud / Solutions Architect

Focuses more on designing the overall infrastructure and architecture.

> [!note]  
> These roles frequently overlap.
> 
> Don't obsess over job titles while learning. Their foundational skills are heavily shared.

---

# 3. DevOps Foundation Stack

The learning progression presented in the video is roughly:

```text
DevOps Culture
      ↓
Linux / Windows
      ↓
Scripting
      ↓
Networking
      ↓
Cloud
      ↓
Git
      ↓
CI/CD
      ↓
Docker
      ↓
Kubernetes
      ↓
DevSecOps
      ↓
Infrastructure as Code
      ↓
GitOps
      ↓
Observability
      ↓
AI Infrastructure / MLOps / AIOps
```

The important thing is that these are **connected layers**, not isolated technologies.

---

# 4. Linux

## Why Linux?

Linux is one of the most important DevOps foundations.

Many DevOps environments run Linux:

- Cloud VMs
    
- Containers
    
- Kubernetes nodes/pods
    
- CI runners
    
- Production servers
    

Therefore, Linux administration is considered a fundamental DevOps skill.

---

## What should you know?

You should be comfortable using the terminal.

Important areas:

### Filesystem

Understand how to:

- Navigate directories
    
- Create/remove files
    
- Move files
    
- Find files
    
- Inspect files
    

You should also understand important directories such as places where:

- Logs are stored
    
- Configuration exists
    
- Applications exist
    

---

## Processes

You should know how to determine:

```text
What is running?
Which process is consuming CPU?
Which process is consuming memory?
Did a process crash?
```

---

## Disk Usage

The transcript specifically mentions commands such as:

```bash
df
```

and

```bash
du
```

Conceptually:

```text
df
↓
Filesystem/disk usage

du
↓
Space consumed by files/directories
```

---

## Services

A DevOps engineer should be able to investigate:

```text
Application is down
        ↓
Is its service running?
        ↓
Why did the service stop?
        ↓
Check logs
        ↓
Fix problem
        ↓
Restart service
```

---

## Logs

Logs help answer:

> What happened inside the system?

You should know:

- Where logs are stored
    
- How to inspect them
    
- How to use them for debugging
    

---

## Permissions

Understand:

```text
Read
Write
Execute
```

You should understand who can access files and what operations they are allowed to perform.

---

## Windows Alternative

Linux dominates many DevOps environments, but Windows skills can also matter.

Especially in enterprise and Azure-based environments.

Useful Windows skills include:

- Windows Server administration
    
- PowerShell
    

---

# 5. Scripting

Once you can operate the system manually, the next step is:

> **Automate repetitive work.**

Good starting languages:

```text
Bash
 ↓
Python
```

---

## Automation Rule

A useful rule from the video:

> If you're doing a task manually for the second time, consider scripting it.

Examples:

- Rotating logs
    
- Cleaning disk space
    
- Checking services
    
- Checking health across multiple servers
    
- Restarting failed services
    

---

# 6. Bash Automation Example

The transcript demonstrates a simple service-monitoring idea.

Conceptually:

```text
Check service
     ↓
Is it running?
   /      \
 YES       NO
  ↓         ↓
Report    Restart
healthy   service
```

A script may start with:

```bash
#!/bin/bash
```

This is called the:

**Shebang**

It tells the operating system which interpreter should execute the script.

Example variable:

```bash
SERVICE=nginx
```

Instead of repeatedly hardcoding:

```text
nginx
```

you store it once and reuse the variable.

---

## systemctl

The example uses:

```bash
systemctl
```

to interact with services.

The script checks whether a service is running.

If it is running:

```text
Print confirmation
```

Otherwise:

```text
Service is down
      ↓
Restart service
```

This represents the shift from:

```text
Typing commands manually
```

to:

```text
Writing commands that execute automatically
```

---

# 7. How Deep Should Linux/Scripting Knowledge Go?

According to the video, you don't need to become a Linux kernel expert.

You should be able to:

- Log into a server
    
- Navigate the system
    
- Understand why an application is down
    
- Inspect services
    
- Inspect logs
    
- Check resource usage
    
- Restart services
    
- Write basic automation scripts
    

Think:

> **Operate the system confidently.**

---

# 8. Networking

Servers rarely operate alone.

They communicate with:

- Other servers
    
- Databases
    
- APIs
    
- Load balancers
    
- Clients
    
- Cloud services
    

Therefore networking is another core DevOps foundation.

---

# 9. DNS

**DNS = Domain Name System**

Purpose:

```text
Domain Name
     ↓
IP Address
```

Example mental model:

```text
example.com
     ↓
DNS lookup
     ↓
IP Address
```

DNS problems are a common source of outages.

---

# 10. IP Addressing, CIDR and Subnets

Important networking concepts include:

- IP addresses
    
- CIDR ranges
    
- Subnets
    

These become particularly important when designing cloud networks.

Example:

```text
VPC
│
├── Subnet
├── Subnet
└── Subnet
```

As a DevOps engineer, you may need to choose CIDR ranges and create subnets for resources.

---

# 11. Ports and Protocols

You should understand:

- Ports
    
- TCP
    
- UDP
    

A common debugging question is:

> Which port is the application actually listening on?

Example:

```text
Client
   ↓
Port
   ↓
Application
```

If the expected port is unavailable or misconfigured, health checks may fail.

---

# 12. Firewalls and Security Groups

They determine which traffic is allowed.

Think:

```text
Incoming Traffic
       ↓
Firewall / Security Group
       ↓
Allowed?
   /       \
 YES        NO
  ↓          ↓
Pass       Block
```

Understand both:

```text
Inbound Traffic
Outbound Traffic
```

---

# 13. Load Balancers

A load balancer distributes traffic across multiple servers.

Instead of:

```text
Users
  ↓
Server
```

you can have:

```text
             ┌→ Server 1
Users → LB ──┼→ Server 2
             └→ Server 3
```

This helps avoid depending on a single server.

The video recommends understanding differences around:

- Internal load balancers
    
- External load balancers
    
- Application load balancers
    
- Network load balancers
    

---

# 14. Layer 4 vs Layer 7

This is an important networking distinction.

## Layer 4

Routes primarily using information such as:

```text
IP Address
Port
```

Mental model:

```text
IP + Port
   ↓
Routing
```

## Layer 7

Can route using application-level information such as:

```text
URL
Hostname
Headers
```

Mental model:

```text
HTTP Request
     ↓
Hostname / URL / Header
     ↓
Routing decision
```

---

# 15. Cloud Computing

Once you understand:

```text
Linux + Networking
```

you need somewhere to run those systems.

Modern applications frequently run in the cloud.

Examples mentioned:

- AWS
    
- Azure
    
- GCP
    
- OCI
    

---

# 16. What Is Cloud?

A simple mental model from the transcript:

> Cloud is essentially someone else's data center that you interact with through APIs.

Traditionally:

```text
Buy physical server
        ↓
Wait
        ↓
Install/configure
        ↓
Use server
```

Cloud:

```text
Request resource
      ↓
Cloud API
      ↓
Resource provisioned
```

This allows infrastructure to be created much faster.

---

# 17. Why Cloud?

Important advantages mentioned include:

- Rapid provisioning
    
- Scaling up
    
- Scaling down
    
- Paying for used resources
    
- Reduced dependence on physical hardware provisioning
    

---

# 18. Core Cloud Building Blocks

Cloud providers expose similar categories of services.

## Compute

Examples include:

```text
Virtual Machines
Serverless Functions
```

The video maps Linux servers to services such as:

```text
AWS EC2
Azure VM
GCP Compute Engine
```

---

## Storage

Important categories:

```text
Object Storage
File Storage
Block Storage
```

---

## Networking

Cloud networking translates your networking knowledge into concepts such as:

```text
VPC
Subnets
Security Groups
Firewalls
Load Balancers
```

---

## IAM

**IAM = Identity and Access Management**

Core question:

> Who can do what?

---

## Managed Databases

Instead of manually provisioning and managing every database component, cloud providers can provide managed database services.

---

# 19. How to Learn Cloud

The recommendation in the video is:

> Pick ONE cloud provider and commit to it for a few months.

Don't try:

```text
AWS + Azure + GCP + OCI
```

simultaneously.

Instead:

```text
Choose one
   ↓
Learn core categories
   ↓
Build projects
   ↓
Understand concepts deeply
```

Once you understand one cloud, many concepts can be mapped to another provider.

Also:

> Create billing alerts before experimenting with cloud resources.

---

# 20. Git

After the infrastructure foundations, the delivery chain begins with code.

Git provides version control.

The transcript distinguishes:

```text
Git
↓
Local version control system

GitHub
↓
Remote platform for hosting Git projects
```

---

# 21. Standard Git Workflow

A typical workflow:

```text
Main Branch
     ↓
Create Branch
     ↓
Make Changes
     ↓
Push Changes
     ↓
Pull Request
     ↓
Automated Checks
     ↓
Team Review
     ↓
Merge
     ↓
Main Branch
```

The merge into the main branch can trigger downstream automation.

---

# 22. Feature Branching

Each change lives in a short-lived branch.

```text
main
 ├── feature-A
 ├── feature-B
 └── bug-fix
```

After completion:

```text
feature branch
      ↓
Pull Request
      ↓
Review
      ↓
main
```

---

# 23. Trunk-Based Development

Developers merge small changes into the main branch frequently.

```text
small change
     ↓
main

small change
     ↓
main

small change
     ↓
main
```

The idea is to keep changes small, making problems easier to identify and roll back.

---

# 24. Git in DevOps

Git can version:

```text
Application Code
+
Infrastructure Configuration
```

It supports:

- Collaboration
    
- Branching
    
- Pull requests
    
- CI/CD triggers
    
- GitOps workflows
    

---

# 25. CI/CD

After code is merged, something needs to:

```text
Check it
Build it
Test it
Deploy it
```

Automation handles this through CI/CD.

---

# 26. CI — Continuous Integration

**CI = Continuous Integration**

Core question:

> Is this change safe to merge?

Typical pipeline:

```text
Code Change
     ↓
Install Dependencies
     ↓
Lint
     ↓
Tests
     ↓
Build
     ↓
Pass / Fail
```

If something fails, the change should not proceed.

---

# 27. CD — Continuous Delivery

The pipeline:

```text
Build
 ↓
Test
 ↓
Package
 ↓
Human Approval
 ↓
Production
```

The important part is:

> Production deployment still requires human approval.

---

# 28. Continuous Deployment

Different from Continuous Delivery.

```text
Build
 ↓
Test
 ↓
Checks Pass
 ↓
Automatically Deploy
 ↓
Production
```

No manual deployment approval is required.

---

# 29. Delivery vs Deployment

Remember:

```text
Continuous Delivery
→ Human approval before production

Continuous Deployment
→ Automatic production deployment
```

The video notes that organizations may keep a human in the loop for production and move toward fully automated deployment only when the automation and rollback workflows are reliable.

---

# 30. How to Learn CI/CD

Don't just watch multiple tutorials.

Build one complete pipeline:

```text
Code
 ↓
Test
 ↓
Build
 ↓
Package
 ↓
Deploy
```

One functioning pipeline gives you the complete mental model.

---

# 31. Artifacts

CI/CD does not simply "ship code."

It produces an:

**Artifact**

In modern containerized applications, that artifact can be a:

```text
Container Image
```

This leads to Docker.

---

# 32. Docker

A container packages:

```text
Application
+
Dependencies
+
Runtime
+
Configuration
```

into a portable unit.

---

# 33. Why Containers?

Without containers:

```text
Developer Environment
        ≠
Production Environment
```

which can cause environment-related failures.

Containers package the application with the environment it expects.

Conceptually:

```text
Application
Dependencies
Runtime
Configuration
      ↓
 Container
```

---

# 34. Dockerfile → Image → Container

This relationship is extremely important.

```text
Dockerfile
    ↓
 docker build
    ↓
Image
    ↓
 docker run
    ↓
Container
```

## Dockerfile

A text file containing instructions for building the application's environment.

## Image

The read-only package produced during the build process.

## Container

A running instance of an image.

Therefore:

```text
Image = Blueprint/package

Container = Running instance
```

One image can create multiple identical containers.

---

# 35. Containers vs Virtual Machines

Containers don't carry an entire operating system in the same way a VM does.

They share the host kernel.

Conceptually:

```text
Virtual Machines

Hardware
   ↓
Host
   ↓
Hypervisor
   ↓
Guest OS
   ↓
Application
```

Compared conceptually with containers:

```text
Hardware
   ↓
Host OS / Kernel
   ↓
Container Runtime
   ↓
Containers
```

This contributes to containers being lightweight and fast to start.

---

# 36. Container Registry

During CI:

```text
Source Code
     ↓
Build Image
     ↓
Container Image
     ↓
Registry
```

Registries mentioned include:

- Docker Hub
    
- Cloud-provider-managed registries
    

The image can later be pulled from the registry for deployment.

---

# 37. Docker Topics to Learn

The video recommends understanding:

- Docker fundamentals
    
- Architecture
    
- Networking
    
- Volumes
    
- Building images
    
- Running containers
    

Then connect Docker with CI/CD in an end-to-end project.

---

# 38. Kubernetes

Docker solves:

> How do I package and run a container?

But production can involve:

```text
Hundreds / Thousands of Containers
```

across many servers.

Managing them manually becomes difficult.

This is where Kubernetes comes in.

---

# 39. Kubernetes = Container Orchestrator

Instead of managing containers individually, you describe what you want.

Example:

```text
I want:

3 replicas
Specific CPU resources
Application available on a port
```

This is the:

**Desired State**

Kubernetes tries to make reality match that desired state.

---

# 40. Kubernetes Desired State

You define configuration using YAML.

```yaml
Desired State
```

Kubernetes continuously works toward:

```text
Actual State == Desired State
```

Example:

```text
Desired replicas = 3

Actual replicas = 2

Kubernetes
    ↓
Creates another replica

Actual replicas = 3
```

---

# 41. Kubernetes Self-Healing

If a container crashes:

```text
Crash
 ↓
Kubernetes detects it
 ↓
Replacement/restart
```

If a server/node fails:

```text
Node fails
    ↓
Workloads affected
    ↓
Kubernetes reschedules them elsewhere
```

If traffic increases, workloads can also be scaled.

---

# 42. Kubernetes Objects

## Pod

The smallest deployment unit mentioned in the video.

A Pod can contain one or more containers.

```text
Pod
├── Container
└── Container
```

## Deployment

Manages things such as:

- Multiple replicas
    
- Rolling updates
    

Conceptually:

```text
Deployment
    ↓
Pods
├── Pod
├── Pod
└── Pod
```

## Service

Provides a stable endpoint through which an application can be accessed.

```text
Client
  ↓
Service
  ↓
Pods
```

---

# 43. Kubernetes Skills

Important areas mentioned include:

- Cluster setup
    
- Storage
    
- Networking
    
- RBAC
    
- Debugging
    
- Troubleshooting
    

**RBAC = Role-Based Access Control**

It manages access within the cluster.

---

# 44. DevSecOps

Traditional model:

```text
Build application
       ↓
Months of development
       ↓
Security Review
       ↓
Many vulnerabilities discovered
       ↓
Release delayed
```

Problem:

Security happens too late.

---

# 45. Shift Left Security

DevSecOps integrates security earlier.

```text
Code
 ↓
Security Checks
 ↓
Build
 ↓
Security Checks
 ↓
Deploy
```

This is called:

> **Shift Left**

Meaning security problems are detected earlier when they are easier and cheaper to fix.

---

# 46. Dependency Scanning

Checks dependencies/packages for known vulnerabilities.

Tools mentioned:

- Trivy
    
- Snyk
    

---

# 47. Secret Scanning

Detects things such as:

```text
API Keys
Secrets
Credentials
```

before they are pushed to external repositories/platforms.

---

# 48. Static Analysis

Scans source code for insecure patterns.

---

# 49. Container Image Scanning

Before an image reaches the registry:

```text
Build Image
     ↓
Scan Image
     ↓
Critical vulnerability?
   /             \
 YES              NO
  ↓                ↓
Fail Pipeline     Push
```

Security becomes part of the delivery pipeline itself.

---

# 50. Infrastructure as Code

Originally, infrastructure might be created manually:

```text
Cloud Console
      ↓
Click
Click
Configure
Click
```

or through manual CLI commands.

The problem is repeatability and automation.

---

# 51. IaC

**IaC = Infrastructure as Code**

Instead of manually creating infrastructure:

```text
Write infrastructure definition
        ↓
Tool reads definition
        ↓
Cloud APIs
        ↓
Infrastructure created
```

Infrastructure becomes defined in files.

---

# 52. Terraform

Terraform is one IaC tool mentioned in the transcript.

You might define:

```text
1 VPC
2 Subnets
Cluster
Database
Security Rules
```

Then run:

```bash
terraform apply
```

Terraform communicates with provider APIs and creates the infrastructure.

---

# 53. Desired State in IaC

Suppose your file describes:

```text
VPC
├── Subnet A
└── Subnet B
```

You later modify the configuration:

```text
VPC
├── Subnet A
├── Subnet B
└── New Security Rule
```

The IaC tool works to make deployed infrastructure match the newly defined state.

---

# 54. Other IaC Tools

The transcript mentions:

## Pulumi

Allows infrastructure to be provisioned through programming languages.

## CloudFormation

AWS-specific infrastructure provisioning.

---

# 55. Why IaC?

## Repeatability

The same infrastructure can be recreated.

## Multiple Environments

For example:

```text
Development
QA
Production
```

## Reviewability

Infrastructure changes can go through Git:

```text
Change
 ↓
Git
 ↓
Pull Request
 ↓
Review
 ↓
Apply
```

## Disaster Recovery

If one region fails, infrastructure definitions can help provision infrastructure elsewhere.

---

# 56. Important IaC Areas

The video highlights topics such as:

- State management
    
- Multi-cloud provisioning
    
- Automation
    
- Custom resources
    

---

# 57. GitOps

Traditional CI/CD can work like:

```text
CI/CD System
      ↓
Production Cluster
```

The pipeline directly deploys to production.

This means the CI/CD system may need production credentials.

That creates security concerns.

---

# 58. GitOps Model

GitOps changes the direction.

Instead of:

```text
CI
 ↓
Push directly to Kubernetes
```

you get:

```text
CI
 ↓
Update Git
 ↓
GitOps Operator
 ↓
Kubernetes
```

Git contains the desired state.

---

# 59. Git as Source of Truth

Git can contain:

```text
Application Code
+
Infrastructure Configuration
+
Deployment Configuration
```

For example:

```text
Image Version
Replica Count
Configuration
```

Git therefore becomes the:

> **Single Source of Truth**

---

# 60. Reconciliation Loop

A GitOps operator continuously compares:

```text
Desired State
      ↓
     Git

Actual State
      ↓
 Kubernetes
```

and tries to make them match.

Tools mentioned:

- Argo CD
    
- Flux CD
    

---

# 61. GitOps Advantages

Every deployment can correspond to a Git change.

Therefore:

```text
Git History
    ↓
Audit Trail
```

Rollback can conceptually become:

```text
git revert
```

and the GitOps system reconciles the desired state.

---

# 62. CI/CD vs GitOps

A useful distinction from the video:

```text
CI/CD
↓
Build + Validate

GitOps
↓
Deploy / Reconcile desired state
```

They can work together.

```text
Code
 ↓
CI
 ↓
Build + Test
 ↓
Image
 ↓
Update Git configuration
 ↓
GitOps
 ↓
Kubernetes
```

---

# 63. Observability

Deployment is only half the job.

After deployment you need to understand:

> What is actually happening in production?

This is observability.

The video describes three major pillars:

```text
Observability
├── Metrics
├── Logs
└── Traces
```

---

# 64. Metrics

Metrics are numerical measurements over time.

Examples:

```text
CPU Usage
Memory Usage
Request Rate
Traffic Patterns
```

---

## Prometheus

The transcript describes Prometheus as a tool used to scrape/collect metrics from workloads.

---

## Grafana

Grafana helps visualize metrics through dashboards.

```text
Workloads
   ↓
Prometheus
   ↓
Metrics
   ↓
Grafana
   ↓
Dashboards
```

---

# 65. Logs

Logs record events.

They help answer:

```text
What happened?
When did it happen?
Why did the service fail?
Why did it restart?
```

Think of logs as a line-by-line story of what happened inside the application/system.

Logging agents can collect logs from workloads and send them to a central platform.

The transcript also mentions Datadog in this context.

---

# 66. Traces

Traces show the journey of a request through a system.

Example:

```text
User Request
     ↓
API Gateway
     ↓
Service A
     ↓
Service B
     ↓
Database
     ↓
Response
```

This becomes especially important in microservices where a single request may pass through many components.

---

# 67. Observability Mental Model

Remember:

```text
Metrics
→ How much / how many?

Logs
→ What happened?

Traces
→ Where did the request go?
```

Together:

```text
Metrics + Logs + Traces
          ↓
      Observability
```

---

# 68. AI Infrastructure

AI is creating new infrastructure workloads for DevOps engineers.

Traditional infrastructure may host:

```text
Web Apps
APIs
Databases
```

AI infrastructure may additionally host:

```text
Training Workloads
Inference Workloads
Models
GPU Workloads
```

---

# 69. GPUs

Training and inference often require GPUs.

GPUs are:

- Expensive
    
- Limited/scarce resources
    

Kubernetes can also orchestrate GPU workloads.

---

# 70. AI Infrastructure Challenges

Model weights can become large artifacts.

Therefore you need to think about:

```text
Storage
Versioning
Networking
GPU Communication
```

But the underlying DevOps fundamentals remain:

```text
Containers
Kubernetes
Networking
Storage
```

The workload has changed, but the foundations remain relevant.

---

# 71. MLOps

**MLOps = Machine Learning Operations**

MLOps extends traditional software delivery concepts to machine-learning systems.

Traditional CI/CD:

```text
Code
 ↓
Build
 ↓
Test
 ↓
Deploy
```

MLOps introduces additional concerns:

```text
Datasets
Models
Experiments
Feature Pipelines
Training
Retraining
Model Deployment
```

---

# 72. MLOps Responsibilities

You may manage:

- Datasets
    
- Model versions
    
- Experiments
    
- Feature pipelines
    
- Automated retraining
    
- Model deployment
    

The overall goal is to deploy and operate ML models reliably.

---

# 73. AIOps

**AIOps = Artificial Intelligence for IT Operations**

Instead of DevOps infrastructure specifically serving AI, AIOps uses AI to help operate IT systems.

AI can help:

```text
Analyze Logs
Collect/Analyze Metrics
Detect Anomalies
Correlate Alerts
Identify Root Causes
```

---

# 74. AI-Assisted Troubleshooting

Traditional:

```text
Incident
 ↓
Engineer searches logs
 ↓
Checks metrics
 ↓
Compares alerts
 ↓
Finds cause
```

AIOps aims to assist with:

```text
Logs + Metrics + Alerts
          ↓
          AI
          ↓
Potential root cause / analysis
```

The engineer still needs foundational knowledge to verify the result.

---

# 75. Model Hosting and Serving

Another infrastructure area is deploying and optimizing inference workloads.

The transcript mentions backend engines such as:

- vLLM
    
- TensorRT-LLM
    
- SGLang
    

The focus includes improving things such as:

```text
Latency
Throughput
GPU Utilization
Cost
```

The overall goal:

> Make AI applications reliable and efficient in production.

---

# 76. AI Does Not Remove the Need for Fundamentals

The video's key message is that AI can reduce repetitive operational work such as:

- Searching logs
    
- Analyzing reports
    
- Troubleshooting assistance
    

But engineers still need to understand:

```text
Linux
Networking
Cloud
Containers
Kubernetes
Observability
```

because AI-generated recommendations must still be verified.

---

# 77. Complete DevOps Workflow

Now connect everything.

```text
Developer
    ↓
Writes Code
    ↓
Git
    ↓
Branch + Pull Request
    ↓
CI
    ↓
Lint + Test + Build
    ↓
DevSecOps Scanning
    ↓
Docker Image
    ↓
Container Registry
    ↓
Deployment Configuration
    ↓
Git
    ↓
GitOps
    ↓
Kubernetes
    ↓
Cloud Infrastructure
    ↓
Running Application
    ↓
Observability
    ↓
Metrics + Logs + Traces
```

Meanwhile:

```text
Terraform / IaC
        ↓
Creates the infrastructure
```

And underneath everything:

```text
Linux
+
Networking
+
Scripting
+
Cloud
```

---

# 78. The Big Picture

Don't think:

```text
Linux
Docker
Git
AWS
Terraform
Kubernetes
Prometheus
```

as unrelated tools.

Think:

```text
             APPLICATION
                  │
                  ▼
                 Git
                  │
                  ▼
                CI/CD
                  │
                  ▼
           Container Image
                  │
                  ▼
              Registry
                  │
                  ▼
             Kubernetes
                  │
                  ▼
                Cloud
                  │
                  ▼
              Production
                  │
                  ▼
            Observability
```

Supporting the system:

```text
Linux + Networking
        ↓
Foundation

Scripting
        ↓
Automation

Terraform
        ↓
Infrastructure Creation

DevSecOps
        ↓
Security

GitOps
        ↓
Deployment State

Metrics + Logs + Traces
        ↓
Production Understanding

AI Infrastructure / MLOps
        ↓
Modern AI Workloads

AIOps
        ↓
AI-assisted Operations
```

---

# 79. What You Actually Need to Learn

## Foundation

-  Linux terminal
    
-  Filesystem
    
-  Processes
    
-  Services
    
-  Logs
    
-  Permissions
    
-  Bash
    
-  Basic Python automation
    

## Networking

-  DNS
    
-  IP addresses
    
-  CIDR
    
-  Subnets
    
-  Ports
    
-  TCP vs UDP
    
-  Firewalls
    
-  Security groups
    
-  Load balancers
    
-  Layer 4 vs Layer 7
    

## Cloud

-  Pick one cloud
    
-  Compute
    
-  Storage
    
-  Networking
    
-  IAM
    
-  Managed databases
    
-  Billing alerts
    

## Git

-  Branches
    
-  Push
    
-  Pull Requests
    
-  Merge
    
-  Feature branching
    
-  Trunk-based development
    

## CI/CD

-  Continuous Integration
    
-  Continuous Delivery
    
-  Continuous Deployment
    
-  Testing
    
-  Build
    
-  Packaging
    
-  Deployment
    
-  Rollback concepts
    

## Docker

-  Docker fundamentals
    
-  Dockerfile
    
-  Images
    
-  Containers
    
-  Registry
    
-  Networking
    
-  Volumes
    

## Kubernetes

-  Pods
    
-  Deployments
    
-  Services
    
-  YAML
    
-  Replicas
    
-  Desired state
    
-  Storage
    
-  Networking
    
-  RBAC
    
-  Debugging
    

## DevSecOps

-  Dependency scanning
    
-  Secret scanning
    
-  Static analysis
    
-  Image scanning
    
-  Shift-left security
    

## Infrastructure as Code

-  Terraform
    
-  Desired state
    
-  State management
    
-  Repeatable environments
    
-  Infrastructure through Git
    

## GitOps

-  Git as source of truth
    
-  Desired vs actual state
    
-  Reconciliation
    
-  Argo CD / Flux
    
-  Rollbacks
    

## Observability

-  Metrics
    
-  Logs
    
-  Traces
    
-  Prometheus
    
-  Grafana
    

## AI + DevOps

-  AI infrastructure
    
-  GPU workloads
    
-  Model serving
    
-  MLOps
    
-  AIOps
    

---

# 80. Final Cheat Sheet

|Concept|Remember It As|
|---|---|
|DevOps|Development + Operations + Automation|
|Linux|Where workloads commonly run|
|Bash/Python|Automate repetitive work|
|Networking|How systems communicate|
|DNS|Name → IP|
|Cloud|Infrastructure exposed through APIs|
|Git|Version and track changes|
|CI|Automatically validate changes|
|Continuous Delivery|Deployment ready, human approves production|
|Continuous Deployment|Automatically deploy to production|
|Docker|Package application + environment|
|Dockerfile|Instructions to build image|
|Image|Packaged application template|
|Container|Running image|
|Registry|Stores container images|
|Kubernetes|Orchestrates containers|
|Pod|Smallest Kubernetes deployment unit|
|Deployment|Manages replicas/updates|
|Service|Stable endpoint for workloads|
|DevSecOps|Security inside delivery workflow|
|Shift Left|Detect security issues earlier|
|IaC|Infrastructure defined as code|
|Terraform|Infrastructure provisioning tool|
|GitOps|Git = deployment source of truth|
|Argo CD / Flux|GitOps reconciliation tools|
|Metrics|Numbers over time|
|Logs|What happened|
|Traces|Request journey|
|Prometheus|Metrics collection|
|Grafana|Metrics visualization|
|MLOps|Operate ML lifecycle|
|AIOps|AI assisting IT operations|
|AI Infrastructure|Infrastructure for AI workloads|

---

# 81. One-Line DevOps Mental Model

> **Linux runs it → Networking connects it → Cloud hosts it → Git tracks it → CI/CD builds and ships it → Docker packages it → Kubernetes operates it at scale → DevSecOps secures it → Terraform creates its infrastructure → GitOps keeps deployment state synchronized → Observability tells you what it is doing → AI/MLOps extends the same foundations to AI workloads.**