# format a drive

# **Document: Formatting and Using a 500 GB Drive on Linux**

## **1. Identify the Drive**

List all connected drives and their mount points:

```bash
lsblk

```

Sample output:

```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda      8:0    0   1.8T  0 disk
└─sda1   8:1    0   1.8T  0 part /media/manoj/ZORO
sdb      8:16   0 465.8G  0 disk
└─sdb2   8:18   0 465.3G  0 part /media/manoj/0e5ccc15-3f74-466a-8d7f-703f24d026b1

```

**Observation:** The 500 GB drive is `/dev/sdb2`.

---

## **2. Unmount the Drive (if currently mounted)**

Move out of the mount point:

```bash
cd ~

```

Unmount the drive:

```bash
sudo umount -l /dev/sdb2

```

*Note: `-l` performs a lazy unmount if the target is busy.*

---

## **3. Format the Drive**

### Option A: **ext4 (Linux-native)**

```bash
sudo mkfs.ext4 /dev/sdb2 -L NARUTO

```

- `L NARUTO` sets the partition label.
- `lost+found` folder will be created automatically.

### Option B: **NTFS (Windows/SMB-compatible)**

```bash
sudo mkfs.ntfs -f /dev/sdb2 -L NARUTO

```

---

## **4. Create Mount Point**

```bash
sudo mkdir -p /media/manoj/NARUTO

```

---

## **5. Mount the Drive**

```bash
sudo mount /dev/sdb2 /media/manoj/NARUTO

```

Test:

```bash
ls /media/manoj/NARUTO

```

- Should show only `lost+found` if ext4, or empty if NTFS.

---

## **6. (Optional) Set Auto-Mount at Boot**

1. Find UUID:

```bash
sudo blkid /dev/sdb2

```

Sample output:

```
UUID="d0dcf630-f41a-4d2a-8ff6-6cf07545d21b" TYPE="ext4"

```

1. Edit `/etc/fstab`:

```bash
sudo nano /etc/fstab

```

1. Add a line (replace UUID and type as needed):

**For ext4:**

```
UUID=d0dcf630-f41a-4d2a-8ff6-6cf07545d21b  /media/manoj/NARUTO  ext4  defaults  0  2

```

**For NTFS:**

```
UUID=XXXX-XXXX  /media/manoj/NARUTO  ntfs-3g  defaults  0  0

```

1. Test fstab:

```bash
sudo mount -a

```

---

## **7. Fix SMB/Windows Write Permissions (if needed)**

### Option 1: Give full permissions (less secure)

```bash
sudo chmod -R 777 /media/manoj/NARUTO

```

### Option 2: Change ownership to SMB user (more secure)

```bash
sudo chown -R nobody:nogroup /media/manoj/NARUTO

```

- Replace `nobody:nogroup` with your SMB user/group.

### Option 3: Use NTFS instead of ext4

- If you need Windows clients to write over SMB frequently.

---

## **8. Summary**

- **Drive:** `/dev/sdb2` (500 GB)
- **Mount Point:** `/media/manoj/NARUTO`
- **Filesystem:** ext4 (or NTFS if Windows-compatible)
- **Auto-mount:** configured via `/etc/fstab`
- **SMB Access:** adjust permissions using `chmod` or `chown`

✅ Now the drive is ready for Linux use and optionally for network access via SMB.