# 🧠 Setting Up My Self-Hosted GitLab & Gitea for My Home Lab (and Learning DevOps)

## 📅 Date

**November 8, 2025**

---

## 🚀 Introduction

Today I built a **fully functional self-hosted GitLab and Gitea setup** on my home lab to start learning **DevOps**, **Git hosting**, and **automation** — all locally.

I wanted a private space to experiment with real-world workflows like repositories, CI/CD pipelines, Docker containers, and Git mirroring — without depending on cloud services.

---

## 🏠 My Home Lab Setup

Here’s what my current setup looks like:

|Device|Specs|Role|
|---|---|---|
|💻 **HP OMEN 16 (Windows 11)**|Ryzen 7 + RTX 4060 + 2 TB SSD|Main workstation / Dev machine|
|🖥️ **HP ThinClient T630**|16 GB RAM + 160 GB SSD|Main homelab server (Docker + GitLab)|
|🍎 **Mac Mini (2014)**|Intel i5 + 4 GB RAM + 256 GB SSD|Secondary server (Ubuntu Server + Gitea backup)|

The servers are connected through a local switch and router network, giving me my own mini data-center environment.

---

## 🧩 Installing GitLab (Self-Hosted)

I deployed **GitLab CE** in Docker on my Ubuntu Server.

```bash
sudo docker run -d \\
  --hostname 192.168.1.95 \\
  --publish 443:443 --publish 80:80 --publish 2222:22 \\
  --name gitlab \\
  --restart always \\
  --volume /srv/gitlab/config:/etc/gitlab \\
  --volume /srv/gitlab/logs:/var/log/gitlab \\
  --volume /srv/gitlab/data:/var/opt/gitlab \\
  gitlab/gitlab-ce:latest

```

GitLab started perfectly at [](http://192.168.1.95/)**[http://192.168.1.95](http://192.168.1.95)**.

The web UI allowed me to create projects, push code, and explore CI/CD features locally.

---

## ⚙️ System Performance Tuning

I checked system resources with:

```bash
htop
lsblk

```

Then expanded my LVM root volume to use the full SSD capacity:

```bash
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv

```

This gave my Ubuntu server **~235 GB** of usable storage for Docker containers and Git data.

---

## 💾 Checking SSD Health and Speed

My SSD model: **CONSISTENT M.2 S6 256 GB (SATA III, 6 Gb/s)**

Installed SMART tools:

```bash
sudo apt install smartmontools -y
sudo smartctl -H /dev/sda

```

Result:

```
SMART overall-health self-assessment test result: PASSED

```

Converted LBAs → total writes: only **~27 MB**!

Wear Leveling Count = **100** → SSD is practically brand new.

✅ **Verdict:** Safe for 24×7 server use.

---

## 🧠 Why I Switched from GitLab to Gitea (for Light Use)

GitLab ran great — but it’s heavy.

On my Mac Mini (4 GB RAM), it maxed out memory fast.

So I installed **Gitea** — a lightweight, open-source Git server:

```bash
sudo docker run -d \\
  --name gitea \\
  -p 3000:3000 -p 222:22 \\
  -v /srv/gitea:/data \\
  gitea/gitea:latest

```

Opened it at **[http://192.168.1.95:3000](http://192.168.1.95:3000)**, finished setup in 2 minutes, and it used < 200 MB RAM.

Gitea gives me:

- Git hosting
    
- Pull requests & issues
    
- Web UI
    
- Private LAN access
    
    Perfect for small projects and fast performance.
    

---

## 🔁 GitHub + Local Sync Workflow

I learned that:

- Git doesn’t auto-sync between GitHub and GitLab/Gitea.
- But I can mirror GitHub → GitLab automatically or via cron.

For now, I use **manual pushes**:

```bash
git remote add origin <http://192.168.1.95/user/project.git>
git remote add github <https://github.com/user/project.git>

git push origin main
git push github main

```

So my local GitLab is my **DevOps playground**,

and GitHub is my **public backup + portfolio**.

---

## 🔐 SSD Reliability and Backup Plan

Even though the SSD is 1–1.5 years old (used by my dad earlier), it’s still in excellent health.

Since all my Git repos are already synced to GitHub, I don’t need rsync backups.

I might add rsync later for Docker configs or private data, but not for Git repos.

---

## ⚡ What I Learned Today

✅ How to install and run **GitLab CE** in Docker

✅ Why **Gitea** is a better fit for lightweight setups

✅ How to manage **LVM volumes** and expand storage

✅ How to check **SSD health, wear level, and speed**

✅ How Git remotes and **mirroring** work

✅ That I can now use **local GitLab** to practice DevOps pipelines and push code to **GitHub**

---

## 🧱 Next Steps

- Configure **GitLab CI/CD** runners on Docker
- Learn **GitLab Pipelines** (`.gitlab-ci.yml`)
- Add **Portainer** to manage Docker visually
- Explore **Drone CI** for lightweight builds with Gitea
- Write weekly blog updates documenting my progress

---

## ✍️ Conclusion

Today marks the start of my personal **DevOps learning journey** using a home-built environment.

With GitLab, Gitea, Docker, and a couple of old machines, I now have a **private Git + CI/CD lab** that mirrors what real companies use — right in my room.

Next goal: automate builds, monitor performance, and master continuous deployment 🚀

---

### 🧩 Tags

`#Homelab` `#GitLab` `#Gitea` `#DevOps` `#SelfHosted` `#Linux` `#Docker` `#SSDHealth` `#Learning`

---

Would you like me to format this for **Markdown + code syntax highlighting** (so you can post it directly on your blog or GitHub Pages)?

Or do you want me to style it like a **Medium/Hashnode article** with sections, emojis, and images?