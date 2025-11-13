# ✅ **Kasm Workspaces – Easy Installation Guide**

## **📌 What is Kasm?**

Kasm lets you run **browser-based desktops and apps** like Linux Desktop, Windows-like desktops, VS Code, etc., all inside your browser. Perfect for home labs.

---

# 🔧 **Step 1 — Update Your Server**

Run these commands:

`sudo apt update && sudo apt upgrade -y`

---

# 🔧 **Step 2 — Install Required Tools**

`sudo apt install curl wget -y`

---

### ✅ Download and install using that exact file

```bash
cd /tmp
wget <https://kasm-static-content.s3.amazonaws.com/kasm_release_1.17.0.7f020d.tar.gz>
tar -xzf kasm_release_1.17.0.7f020d.tar.gz
cd kasm_release_1.17.0.7f020d
sudo ./install.sh

```