
## 🧠 **Introduction: My Fear of Cloud Git Platforms**

I’ve been using GitHub for a while, like most developers.

But one question always stayed in my mind —

> “What if one day GitHub bans my account or deletes my repos?”

All my code, projects, and notes are there. That’s _my_ work, but it’s stored on _their_ servers.

I started realizing how dependent I was on centralized platforms.

That’s when I decided:

👉 _It’s time to self-host my own Git server._

---

## 🧰 **Step 1: My Home Lab Setup**

Before I started, I already had my small home lab — a simple but powerful setup I call **OMLAB**.

|Device|Specs|OS|
|---|---|---|
|💻 HP OMEN 16|Ryzen 7 7000 + RTX 4060|Windows 11 (Main Laptop)|
|🖥️ Mac Mini (2014)|Intel i5, 4 GB RAM|Ubuntu Server|
|🧱 HP ThinClient T630|16 GB RAM, 256 GB SSD|Ubuntu Server|

All are connected to a single **network switch** → **router**, with my laptop accessing them over LAN.

I use this home lab for experiments like Docker, servers, and self-hosting tools.

---

## 🧩 **Step 2: Choosing to Self-Host GitLab**

I researched self-hosted Git alternatives like:

- Gitea ☕
- Gogs
- GitLab CE / EE 🦊

I went with **GitLab (Enterprise Edition)** because it gives a full suite:

✅ web UI,

✅ issue tracking,

✅ pipelines (CI/CD),

✅ and built-in Docker support.

---

## ⚙️ **Step 3: Installing GitLab Using Docker**

I didn’t want to mess with manual packages.

So I used Docker — cleaner, isolated, and easily removable.

Here’s what I ran on my **Ubuntu Server** (Mac Mini):

```bash
sudo docker run -d \\
  --hostname 192.168.1.95 \\
  -p 80:80 -p 443:443 -p 2222:22 \\
  --name gitlab \\
  --restart always \\
  --volume "$HOME/Self Hosted/Git Lab/config":/etc/gitlab \\
  --volume "$HOME/Self Hosted/Git Lab/logs":/var/log/gitlab \\
  --volume "$HOME/Self Hosted/Git Lab/data":/var/opt/gitlab \\
  gitlab/gitlab-ee:latest

```

After that, I checked the container status:

```bash
docker ps

```

Output:

```
CONTAINER ID   IMAGE                     STATUS          PORTS
51e3529fcddd   gitlab/gitlab-ee:latest   Up (healthy)    0.0.0.0:80->80/tcp, 0.0.0.0:2222->22/tcp

```

GitLab was alive 🟢

---

## 🌐 **Step 4: Accessing GitLab**

Then I opened my browser on my main laptop and went to:

👉 `http://192.168.1.95`

I saw the **GitLab login page** — hosted right inside my home network!

To get the admin password:

```bash
sudo docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password

```

Example output:

```
Password: z4KNlO895tejcDj1qpNRwq2ZpmaF1QbgYmCyZuRUsX4=

```

I logged in with:

- **Username:** root
- **Password:** (the one above)

And there it was — my **own private GitLab instance**, hosted entirely inside my LAN.

---

## 💾 **Step 5: Understanding Data Storage**

The best part about self-hosting is **knowing where your data actually lives.**

GitLab’s data was all stored here on my host machine:

```
~/Self Hosted/Git Lab/
├── config/   → Configuration files (gitlab.rb, secrets)
├── data/     → Repositories, database, uploads
└── logs/     → Service logs

```

Inside `data/git-data/repositories/` were my actual Git repositories:

```
~/Self Hosted/Git Lab/data/git-data/repositories/root/

```

That’s where every commit and branch I made was saved physically on my disk.

---

## 🧠 **Step 6: My Learnings**

While setting up GitLab, I learned about:

- Docker networking and port mapping
- Persistent storage using Docker volumes
- GitLab’s internal services (Redis, PostgreSQL, Gitaly)
- How GitLab stores repositories using hashed paths

And I realized — **this is what real control feels like.**

No third-party server, no fear of losing my code.

---

## ⚙️ **Step 7: Why I’m Moving to Gitea (Next Step)**

After using GitLab for a while, I noticed:

- It eats a _lot_ of RAM (3–4 GB minimum)
- Takes 10–15 minutes to boot
- Overkill for my solo use

So now, I’m moving to **Gitea**, a much lighter Git server that runs in seconds, perfect for personal and small team setups.

---

## 💡 **Conclusion**

Hosting your own Git server isn’t just about saving money —

It’s about _owning your data_ and understanding how things actually work behind the scenes.

My journey from being **scared of losing GitHub data** to **running GitLab on my own server** changed how I see version control.

I’m no longer dependent on cloud companies. My code, my rules.

---

### 🧾 **Keywords / Tags**

`#SelfHosting #GitLab #HomeLab #Docker #DevOps #Gitea #Linux`