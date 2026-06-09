Since your goal is **DevOps + homelab + self-hosting**, don't learn every Ansible feature upfront. Learn it in stages and build something useful at each stage.

# Phase 0: Prerequisites (1-2 Days)

Before Ansible, make sure you're comfortable with:

- Linux commands
    
- SSH
    
- SSH keys
    
- Package managers (`apt`, `dnf`)
    
- Basic YAML
    

Know how to:

```bash
ssh user@server
ssh-copy-id user@server
```

Understand YAML:

```yaml
name: nginx

packages:
  - git
  - curl
```

---

# Phase 1: First Contact with Ansible (2-3 Days)

### Goal

Control one server from another machine.

Learn:

- What is Ansible?
    
- Control Node vs Managed Node
    
- Inventory
    
- Ad-hoc Commands
    
- Modules
    

Commands:

```bash
ansible all -m ping
ansible all -m command -a "hostname"
ansible all -m apt -a "update_cache=yes"
```

Project:

- Connect HP OMEN → ThinClient
    

By the end:

```text
Laptop
  |
Ansible
  |
ThinClient
```

should work.

---

# Phase 2: Playbooks (3-4 Days)

### Goal

Stop typing commands manually.

Learn:

- Playbooks
    
- Tasks
    
- Hosts
    
- become: yes
    

Example:

```yaml
---
- hosts: servers
  become: yes

  tasks:
    - name: Install git
      apt:
        name: git
        state: present
```

Project:

Create:

```text
install-basic-tools.yml
```

that installs:

- git
    
- curl
    
- htop
    
- vim
    

on all servers.

---

# Phase 3: Variables & Inventory Management (2-3 Days)

### Goal

Avoid hardcoding values.

Learn:

- Variables
    
- Host variables
    
- Group variables
    

Example:

```yaml
package_name: docker.io
```

Use:

```yaml
name: "{{ package_name }}"
```

Project:

Create:

```text
web_servers
db_servers
storage_servers
```

groups in inventory.

---

# Phase 4: Loops and Conditions (3-4 Days)

### Goal

Reduce repetition.

Instead of:

```yaml
- install git
- install curl
- install vim
```

Use:

```yaml
loop:
  - git
  - curl
  - vim
```

Learn:

- loop
    
- when
    
- register
    

Project:

Install 10 packages using a loop.

---

# Phase 5: Templates (3-5 Days)

### Goal

Generate configuration files automatically.

Learn:

- Jinja2
    
- Templates
    

Example:

```jinja2
server_name={{ hostname }}
```

Project:

Generate:

```text
motd
nginx.conf
docker configs
```

automatically.

This is where Ansible starts feeling powerful.

---

# Phase 6: Roles (1 Week)

### Goal

Organize automation professionally.

Structure:

```text
roles/

docker/
nginx/
users/
monitoring/
```

Learn:

- tasks
    
- handlers
    
- vars
    
- templates
    

Project:

Create:

```text
docker-role
```

that installs Docker anywhere.

---

# Phase 7: Docker Automation (1 Week)

This is the most useful phase for your homelab.

Learn:

- docker_container
    
- docker_network
    
- docker_volume
    

Project:

Deploy:

- Nginx
    
- Portainer
    
- n8n
    
- Uptime Kuma
    

with Ansible.

One command:

```bash
ansible-playbook deploy.yml
```

should deploy everything.

---

# Phase 8: Multi-Server Homelab Automation (1 Week)

Your setup is ideal for this.

Machines:

```text
HP OMEN
ThinClient
Mac Mini
```

Project:

Create:

```text
homelab.yml
```

Tasks:

- Update systems
    
- Configure SSH
    
- Install Docker
    
- Create users
    
- Configure firewall
    
- Deploy containers
    

Now you're using Ansible the way many DevOps engineers do.

---

# Phase 9: Secrets Management (3-4 Days)

Learn:

- Ansible Vault
    

Store:

```text
Passwords
API Keys
Tokens
```

encrypted.

Example:

```bash
ansible-vault encrypt secrets.yml
```

---

# Phase 10: CI/CD Integration (Advanced)

Learn:

- GitLab CI
    
- GitHub Actions
    
- Jenkins
    

Flow:

```text
Git Push
   |
Pipeline
   |
Ansible
   |
Servers Updated
```

Now infrastructure updates become automatic.

---

# Phase 11: Infrastructure as Code Ecosystem

After Ansible, learn how it fits with:

```text
Terraform
     ↓
Creates VMs

Ansible
     ↓
Configures VMs

Docker
     ↓
Runs Apps

Kubernetes
     ↓
Orchestrates Apps
```

This is the classic DevOps stack.

---

# Final Homelab Project

Build a complete automated setup:

```text
GitLab
   |
GitLab CI
   |
Ansible
   |
-----------------
| ThinClient    |
| Mac Mini      |
-----------------
```

One pipeline should:

1. Pull code
    
2. Run tests
    
3. Deploy containers
    
4. Update configs
    
5. Restart services
    

---

For **your current level**, I'd focus only on:

✅ Phase 0  
✅ Phase 1  
✅ Phase 2  
✅ Phase 3  
✅ Phase 7

first.

You can become useful with Ansible in a homelab after just those phases. Roles, Vault, CI/CD, and advanced features can come later once you're already automating real servers.