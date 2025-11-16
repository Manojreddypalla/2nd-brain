**Node.js (npm run dev)** project to run **automatically**, even after reboot, and keep running in background like a real server?

The **correct and easiest** way:

# ✅ Use **PM2**

PM2 is a process manager for Node.js that:

- runs your app in background
- restarts it if it crashes
- auto-starts on reboot
- gives logs
- works on Linux servers (Ubuntu etc.)

---

# 🚀 **Step 1 — Install PM2**

```bash
sudo npm install -g pm2

```

---

# 🚀 **Step 2 — Start your project**

Go to your project folder:

```bash
cd /path/to/your/project

```

Then run:

```bash
pm2 start "npm run dev" --name myapp

```

This will keep your development server running 24/7.

---

# 🚀 **Step 3 — Enable auto-start on reboot**

```bash
pm2 startup

```

Then copy-paste the command it prints.

Finally:

```bash
pm2 save

```

---

# 🚀 **Step 4 — Useful PM2 commands**

|Command|Meaning|
|---|---|
|`pm2 list`|show all running apps|
|`pm2 logs myapp`|view logs|
|`pm2 restart myapp`|restart app|
|`pm2 stop myapp`|stop app|
|`pm2 delete myapp`|remove app|

---

# 🎉 Done

Your `npm run dev` project will run **automatically**, forever, even after reboot.