## 🧭 1️⃣ Check your drives

Run:

```bash
lsblk

```

You’ll see all storage devices and partitions, e.g.:

```
sda ├─sda1
sdb └─sdb1
sdc └─sdc1

```

Identify your target (for example: `/dev/sdd1`).

---

## 🗂 2️⃣ Create a mount point

Make a folder where the drive will appear:

```bash
sudo mkdir -p /media/<your_username>/<folder_name>

```

Example:

```bash
sudo mkdir -p /media/retr0/ZORO

```

---

## 🧩 3️⃣ Mount the drive manually

Mount the partition to that folder:

```bash
sudo mount /dev/sdd1 /media/retr0/ZORO

```

✅ Now you can access files using:

```bash
cd /media/retr0/ZORO
ls

```

---

## ⚙️ 4️⃣ (Optional) Mount automatically at startup

To make it permanent:

### a) Get the UUID:

```bash
sudo blkid /dev/sdd1

```

You’ll get something like:

```
/dev/sdd1: UUID="DA7CFE1A7CFDF15D" TYPE="ntfs"

```

### b) Edit the fstab file:

```bash
sudo nano /etc/fstab

```

### c) Add a line at the end:

For **NTFS drive:**

```
UUID=DA7CFE1A7CFDF15D  /media/retr0/ZORO  ntfs  defaults,auto,nofail,uid=1000,gid=1000,umask=0022  0  0

```

For **EXT4 drive:**

```
UUID=DA7CFE1A7CFDF15D  /media/retr0/ZORO  ext4  defaults  0  2

```

### d) Save and test:

```bash
sudo mount -a

```

If no error appears — it’s perfect 🎯

---

## 🧹 5️⃣ To unmount

When you’re done or want to safely remove:

```bash
sudo umount /media/retr0/ZORO

```

---

## ✅ Summary

| Action        | Command                                   |
| ------------- | ----------------------------------------- |
| View drives   | `lsblk`                                   |
| Create folder | `sudo mkdir -p /media/user/folder`        |
| Mount drive   | `sudo mount /dev/sdXY /media/user/folder` |
| Find UUID     | `sudo blkid /dev/sdXY`                    |
| Edit fstab    | `sudo nano /etc/fstab`                    |
| Test mounts   | `sudo mount -a`                           |
| Unmount       | `sudo umount /media/user/folder`          |