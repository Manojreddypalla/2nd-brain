## 🧠 Overview

You installed **code-server** — an open-source version of VS Code that runs on your server and can be accessed from any web browser.

You did this to manage and develop your **blog project** (`~/Self Hosted/blog`) directly on your Ubuntu server through a browser, without needing a local VS Code installation.

---

## ⚙️ Step-by-Step Process

### 🧩 Step 1: Installed code-server

You downloaded and installed it using the official installation script:

```bash
curl -fsSL <https://code-server.dev/install.sh> | sh

```

✅ What this did:

- Detected your OS (Ubuntu 22.04)
- Downloaded the `.deb` package from GitHub
- Installed it into `/usr/lib/code-server/`
- Registered the **systemd service** `code-server@$USER`

---

### 🧱 Step 2: Enabled and started the service

You started code-server as a background service that launches automatically on boot:

```bash
sudo systemctl enable --now code-server@$USER

```

✅ This made `code-server`:

- Start immediately
- Auto-start whenever your Ubuntu server reboots

---

### 📄 Step 3: Checked service status

You confirmed it was running using:

```bash
systemctl status code-server@$USER

```

You saw logs like:

```
HTTP server listening on <http://127.0.0.1:8080>
Authentication is enabled

```

That meant it was working — but only accessible **locally**.

---

### ⚙️ Step 4: Modified configuration

You opened the config file:

```bash
nano ~/.config/code-server/config.yaml

```

Originally it was:

```yaml
bind-addr: 127.0.0.1:8080
auth: password
password: 9807472411d9176ad32b194f
cert: false

```

You edited it to:

```yaml
bind-addr: 0.0.0.0:9000
auth: password
password: 9807472411d9176ad32b194f
cert: false

```

✅ What changed:

- `0.0.0.0` → allows **LAN access** (not just localhost)
- `9000` → changed port from 8080 to 9000
- `cert: false` → no HTTPS (you’ll add later if needed)

---

### 🔄 Step 5: Restarted the service

You applied your new settings:

```bash
sudo systemctl restart code-server@$USER

```

and verified:

```bash
systemctl status code-server@$USER

```

It showed:

```
HTTP server listening on <http://0.0.0.0:9000>

```

---

### 🔐 Step 6: Opened the port in firewall

You checked if the firewall was active and allowed the new port:

```bash
sudo ufw status
sudo ufw allow 9000/tcp

```

✅ This ensured that other devices on your LAN can connect to port `9000`.

---

### 🌐 Step 7: Accessed from another device

You got your Ubuntu machine’s IP:

```bash
hostname -I

```

Example: `192.168.1.105`

Then you opened on your **Windows laptop or browser**:

```
<http://192.168.1.105:9000>

```

Entered your password (`9807472411d9176ad32b194f`) and logged into the **VS Code web interface** 🎉

---

## 🧰 Result

✅ You now have:

- A fully functional **VS Code** running on your **Ubuntu server**
- Accessible through browser over LAN
- Configured to run on port **9000**
- Connected to your **blog project** directory (`~/Self Hosted/blog`)

---

## 🚀 What’s Next

Here are a few optional upgrades:

1. **Secure Access (HTTPS)** → use Cloudflare Tunnel (no port forwarding needed)
2. **Auto-deploy blog changes** → connect GitHub repo + CI/CD
3. **Run code-server on your ThinClient** → so both servers have web IDEs