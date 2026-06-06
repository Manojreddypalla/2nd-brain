# SMBA

# **Part 1: Connect a Drive in Windows via SMB (Network Share)**

### **Step 1: Know Your Linux SMB Share**

Assume your Linux machine has SMB (Samba) installed and running:

```bash
sudo systemctl status smbd

```

- SMB shares are usually defined in `/etc/samba/smb.conf`.
- Example share for your drive:

```
[NARUTO]
   path = /media/manoj/NARUTO
   browsable = yes
   writable = yes
   guest ok = yes
   read only = no

```

- Restart Samba after editing:

```bash
sudo systemctl restart smbd

```

---

### **Step 2: Find Linux Machine IP**

```bash
ip a

```

- Suppose IP is `192.168.1.50`.

---

### **Step 3: Connect from Windows**

1. Open **File Explorer**.
2. In the address bar, type:

```
\\192.168.1.50\NARUTO

```

1. Press Enter.
2. If prompted, enter your Samba username/password (or leave blank if guest access is enabled).

✅ You should now see the contents of `/media/manoj/NARUTO` in Windows.

---

### **Step 4: Map Network Drive (Optional)**

- Right-click `This PC` → `Map network drive`.
- Choose a drive letter (e.g., `Z:`) → `Folder: \\192.168.1.50\NARUTO` → `Finish`.
- Now it behaves like a normal drive in Windows.

---

# **Part 2: Add a Drive to Samba (SMB) via config**

### **Step 1: Mount Your Drive in Linux**

```bash
sudo mkdir -p /media/manoj/NARUTO
sudo mount /dev/sdb2 /media/manoj/NARUTO

```

---

### **Step 2: Edit Samba Config**

```bash
sudo nano /etc/samba/smb.conf

```

Add a new section at the end:

```
[NARUTO]
   comment = 500GB Drive NARUTO
   path = /media/manoj/NARUTO
   browsable = yes
   writable = yes
   guest ok = yes
   read only = no

```

- `path` → folder you want to share
- `writable = yes` → allows write access
- `guest ok = yes` → allows access without a password (optional)

---

### **Step 3: Restart Samba**

```bash
sudo systemctl restart smbd

```

---

### **Step 4: Test Access**

- From Linux:

```bash
smbclient //localhost/NARUTO -U username

```

- From Windows:

```
\\<Linux_IP>\NARUTO

```

---

### ⚡ **Optional: Automate Mount + Share**

- If you want the drive to **always be shared**, make sure:
    - `/etc/fstab` mounts the drive at boot
    - Samba config has the share
    - Samba service is enabled to start on boot:

```bash
sudo systemctl enable smbd
sudo systemctl enable nmbd

```