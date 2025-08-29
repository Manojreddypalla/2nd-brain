**This is manoj today im trying kasm in cloud as i learned we can use it for free in azure and other cloud too , 

now with this i can learn about lot of things only through tab on the go and do partical 

# Kasm Workspaces Installation Guide

This document provides a detailed guide for installing **Kasm Workspaces** on a new Linux virtual machine, focusing on the specific steps and troubleshooting for common issues.

---

## 1. Manually Create a Swap File
The Kasm installer requires a swap file for stability and may fail if one is not detected. This section details how to manually create a swap file on your main drive.

**Create an 8GB swap file on your main drive (`/`):**
```bash
sudo fallocate -l 8G /swapfile
```

**Secure the file permissions so only the root user can access it:**
```bash
sudo chmod 600 /swapfile
```

**Format the file as a Linux swap area:**
```bash
sudo mkswap /swapfile
```

**Enable the newly created swap file:**
```bash
sudo swapon /swapfile
```

**Make the swap file permanent by adding it to your `/etc/fstab` file:**
```bash
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

**Verify the swap is active and listed:**
```bash
swapon --show
```

---

## 2. Run the Kasm Installer
After successfully creating the swap file, you can now run the installer from the `kasm_release` directory.  
The script will detect the swap space and continue the installation process.

**Command:**
```bash
sudo ./install.sh
```

---

## 3. Optional: Install a GUI for a Desktop Experience
If you prefer a graphical interface for your Kasm sessions, you can install a lightweight desktop environment like **MATE** along with a remote desktop server.

**Install the MATE Desktop Environment:**
```bash
sudo apt update
sudo apt install mate-desktop-environment
```

**Install XRDP (Remote Desktop Protocol) Server:**
```bash
sudo apt install xrdp
```

**Configure XRDP to use the MATE desktop:**
```bash
echo mate-session | sudo tee ~/.xsession
```

**Restart the XRDP service to apply the new settings:**
```bash
sudo systemctl restart xrdp
```



and it is not working as expected gonna troubleshoot later 

---
