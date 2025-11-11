# 🧠 Notes — Self-Hosting GitLab, Gitea, and Jellyfin with Cloudflare Tunnel (No Port Forwarding)

---

## ⚙️ 1. Objective

You wanted to **self-host multiple apps (GitLab, Gitea, Jellyfin)** inside your home lab **without relying on ISP port forwarding**.

Your goals:

- Fully self-hosted, local-first setup
- Secure remote access via HTTPS
- Use your own domain (`manojblogs.live`)
- Isolate and manage services via Docker or native installs

---

## 🧩 2. Home Lab Setup

|Device|OS|Role|
|---|---|---|
|🖥️ HP Omen 16|Windows 11|Main workstation|
|🧱 Mac Mini (2014)|Ubuntu Server|Git servers (GitLab, Gitea)|
|💻 HP ThinClient T630|Ubuntu Server|Lightweight Docker host|

**Networking:**

- All connected via a managed switch and router
- No external port forwarding (ISP restriction)
- Used **Cloudflare Tunnel** for secure inbound traffic

---

## ☁️ 3. Domain + Cloudflare DNS

- Registered domain: `manojblogs.live`
- Managed via **Cloudflare DNS**
- Set **A records** to Cloudflare proxy (orange cloud ☁️ enabled)
- Each subdomain points to different local service:
    - `gitlab.manojblogs.live` → GitLab
    - `gitea.manojblogs.live` → Gitea
    - `media.manojblogs.live` → Jellyfin

---

## 🔐 4. Cloudflare Tunnel (cloudflared)

You installed and configured **Cloudflare Tunnel** to expose local services securely over HTTPS.

### 🧰 Installation

```bash
sudo mkdir -p /etc/cloudflared
curl -fsSL <https://pkg.cloudflare.com/gpg> | sudo gpg --dearmor -o /usr/share/keyrings/cloudflare-main.gpg
echo "deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] <https://pkg.cloudflare.com/cloudflared> $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/cloudflare-main.list
sudo apt update && sudo apt install cloudflared -y

```

### 🔑 Login to Cloudflare

```bash
cloudflared tunnel login

```

→ Opened a browser window to authenticate with Cloudflare.

### 🏗️ Create tunnels

For Gitea:

```bash
cloudflared tunnel create gitea-lab

```

For Jellyfin:

```bash
cloudflared tunnel create jellyfin-tunnel

```

### ⚙️ Example config file

**`/etc/cloudflared/config.yml`**

```yaml
tunnel: f2392f89-5f17-43a7-8a8b-61da841cde23
credentials-file: /home/retr0/.cloudflared/f2392f89-5f17-43a7-8a8b-61da841cde23.json

ingress:
  - hostname: media.manojblogs.live
    service: <http://localhost:8096>
  - service: http_status:404

```

### 🚀 Run tunnel manually

```bash
cloudflared tunnel run jellyfin-tunnel

```

---

## 🔁 5. Auto-Start Cloudflared at Boot

You created a systemd service to make the tunnel run automatically:

**`/etc/systemd/system/cloudflared.service`**

```
[Unit]
Description=cloudflared Tunnel Service
After=network-online.target
Wants=network-online.target

[Service]
TimeoutStartSec=15
Type=notify
ExecStart=/usr/bin/cloudflared --no-autoupdate --config /etc/cloudflared/config.yml tunnel run
Restart=on-failure
RestartSec=5s

[Install]
WantedBy=multi-user.target

```

Enable it:

```bash
sudo systemctl enable cloudflared
sudo systemctl start cloudflared

```

---

## 🐙 6. Deploying Gitea (Docker)

### Docker Compose file

**`docker-compose.yml`**

```yaml
version: "3"
services:
  gitea:
    image: gitea/gitea:latest
    container_name: gitea
    restart: always
    environment:
      - USER_UID=1000
      - USER_GID=1000
    ports:
      - "6060:3000"
      - "2222:22"
    volumes:
      - ./gitea:/data

```

Run:

```bash
docker compose up -d

```

Then configure Cloudflare Tunnel to forward:

```yaml
- hostname: git.manojblogs.live
  service: <http://localhost:6060>

```

---

## 🎬 7. Deploying Jellyfin (Native Install)

You chose a **native installation** for Jellyfin (not Docker).

```bash
sudo apt install apt-transport-https
sudo mkdir -p /etc/apt/keyrings
curl -fsSL <https://repo.jellyfin.org/ubuntu/jellyfin_team.gpg.key> | sudo gpg --dearmor -o /etc/apt/keyrings/jellyfin.gpg
echo "deb [signed-by=/etc/apt/keyrings/jellyfin.gpg] <https://repo.jellyfin.org/ubuntu> $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/jellyfin.list
sudo apt update
sudo apt install jellyfin -y

```

Default access:

🔗 `http://localhost:8096`

(Or via Cloudflare: `https://media.manojblogs.live`)

---

## ⚙️ 8. Jellyfin Network Config Fix

Edited:

```bash
sudo nano /etc/jellyfin/network.xml

```

Key settings:

```xml
<EnableHttps>false</EnableHttps>
<EnableRemoteAccess>true</EnableRemoteAccess>
<EnableIPv4>true</EnableIPv4>
<EnableIPv6>false</EnableIPv6>
<PublicHttpPort>8096</PublicHttpPort>

```

Then restart:

```bash
sudo systemctl restart jellyfin

```

---

## 🧠 9. GitLab + GitHub Mirror Backup

You mirrored your GitHub repositories to your self-hosted GitLab:

```bash
git remote add gitlab <http://192.168.1.95/root/docker-compose-files.git>
git push -u gitlab main

```

🔐 GitLab authentication via **Personal Access Token**

(URL: `http://192.168.1.95/-/profile/personal_access_tokens`)

You also configured dual-push mirroring:

```bash
git remote set-url --add origin <http://192.168.1.95/root/docker-compose-files.git>

```

Now:

```bash
git push origin main

```

pushes to **both GitHub and GitLab** 🚀

---

## 🧱 10. System Resilience Notes

During testing, you hit temporary errors:

- `input/output error` → filesystem temporarily went read-only (HDD aging)
- Fix: `sudo mount -o remount,rw /` and eventual restart

→ Moved critical services to SSD (fast & healthy)

---

## 🔒 11. Security Setup Summary

✅ Local-only by default

✅ HTTPS via Cloudflare

✅ No open ports to ISP

✅ Automatic startup with systemd

✅ Enforced remote access policies per app

---

## 🧩 12. Optional Enhancements

|Goal|Tool|
|---|---|
|Reverse proxy & cert management|Nginx Proxy Manager|
|Automatic DNS updates for dynamic IP|Cloudflare DDNS|
|VPN access to your LAN|Tailscale|
|Monitoring|Grafana + Prometheus|
|Offsite backup of configs|Git push mirror|

---

## ✅ 13. Final Results

|App|Access|Deployment|
|---|---|---|
|**GitLab**|`https://gitlab.manojblogs.live`|Docker|
|**Gitea**|`https://git.manojblogs.live`|Docker|
|**Jellyfin**|`https://media.manojblogs.live`|Native|
|**Cloudflare Tunnel**|Secure HTTPS access|Systemd autostart|

---

## ✍️ 14. Summary Line

> You built a fully self-hosted cloud, using your home lab + Cloudflare Tunnel — no port forwarding, fully HTTPS, with mirrored Git backups and secure remote access.