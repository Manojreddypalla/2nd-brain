
# Fixing GRUB Boot Prompt Issue (Linux Mint / Ubuntu)

**Date**: 2025-10-28
**Author**: Manoj Reddy
**System**: Linux Mint (Ubuntu-based)
**Issue**: System stopped at GRUB shell and booted only after typing `exit`.

---

## 🧠 Problem Overview

At startup, the system was dropping to the GRUB prompt:

```shell
grub>
```

Typing `exit` would continue booting to the OS. This indicates that GRUB was installed, but the UEFI boot order or EFI configuration was incorrect.

---

## 🧾 Diagnosis

Logged into the system using SSH and verified GRUB installation and EFI entries.

### Commands:

```bash
sudo grub-install /dev/sda
sudo update-grub
sudo efibootmgr -v
```

---

## 🧰 Commands Used

### 1. Reinstall GRUB (UEFI mode)

```bash
sudo grub-install /dev/sda
```

✅ **Output**: “Installation finished. No error reported.”

### 2. Update GRUB Configuration

```bash
sudo update-grub
```

✅ Detected all Linux kernels and Windows boot entries.

### 3. Check EFI Boot Entries

```bash
sudo efibootmgr -v
```

Output showed:
```
BootOrder: 0001,0080
Boot0001* Ubuntu
Boot0000* Windows Boot Manager
Boot0080* Mac OS X
```

---

## ⚙️ Permanent Fix

### Step 1: Correct Boot Order

```bash
sudo efibootmgr -o 0001,0000,0080
```

Sets Ubuntu (GRUB) first, then Windows.

### Step 2: Remove Unused EFI Entries

```bash
sudo efibootmgr -b 0080 -B
sudo efibootmgr -b 0081 -B
```

Removes leftover macOS entries.

### Step 3: Verify

```bash
sudo efibootmgr
```

**Expected**:
```
BootOrder: 0001,0000
```

### Step 4: Reboot

```bash
sudo reboot
```

✅ System now boots directly to GRUB or OS.

---

## 🧩 Optional — Hide GRUB Menu

If you want the OS to boot directly without showing the GRUB menu:

```bash
sudo nano /etc/default/grub
```

Change:
```
GRUB_TIMEOUT=0
GRUB_TIMEOUT_STYLE=hidden
```

Then update:
```bash
sudo update-grub
```

---

## 🧱 Optional — Fallback EFI Path (if issue repeats)

If the system ever reverts to the `grub>` prompt again:

```bash
sudo cp /boot/efi/EFI/ubuntu/shimx64.efi /boot/efi/EFI/Boot/bootx64.efi
```

This ensures BIOS finds GRUB even if it ignores the `BootOrder`.

---

## ✅ Result

- System boots directly without manual `exit`.
- GRUB and EFI entries are clean and consistent.
- Dual-boot (Windows + Linux) works correctly.

---

## 🧰 Backup Tip (Recommended)

Create a backup of your working EFI configuration:

```bash
sudo efibootmgr -v > ~/efi-backup.txt
```

To restore later, you can manually recreate entries using the file.

---

## 🧩 Summary

| Step | Action | Command |
| :--- | :--- | :--- |
| 1 | Reinstall GRUB | `sudo grub-install /dev/sda` |
| 2 | Update GRUB | `sudo update-grub` |
| 3 | Fix Boot Order | `sudo efibootmgr -o 0001,0000` |
| 4 | Remove old entries | `sudo efibootmgr -b 0080 -B` |
| 5 | Reboot | `sudo reboot` |
| 6 | (Optional) Hide GRUB | Edit `/etc/default/grub` |
| 7 | (Optional) Fallback path | `Copy shimx64.efi` |

---

✅ **Status**: Boot fixed and stable
🕒 **Last Verified**: 2025-10-28
📘 **Tags**: #linux #boot #grub #efi #fix #obsidian
