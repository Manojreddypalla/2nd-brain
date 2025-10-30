# system recovery

## Linux System Recovery via Chroot (Interrupted Upgrade)

This procedure is used when a major system upgrade (like a kernel update) is interrupted (e.g., due to a lost SSH connection), causing the system to be unbootable.

---

### Preparation: Accessing the Broken Drive

1. **Connect the non-booting drive** to a working Linux machine.
2. **Identify the root partition** (/), the largest partition containing your broken system (e.g., `/dev/sdd3`):Bash
    
    `lsblk`
    
3. **Set the Mount Point Variable** (use your actual path):Bash
    
    `MNT_POINT="/media/manoj/b6a855cb-330f-4ea0-9352-c43dcc30636d"`
    
4. **Bind Mount Essential Directories:** Mount the live system's core resources (devices, process data, etc.) into the mounted drive's filesystem.Bash
    
    `for i in /dev /dev/pts /proc /sys /run; do sudo mount -B $i $MNT_POINT$i; done`
    

---

### Step 1: Chroot and Repair

Enter the broken system's environment to force the package manager to finish its work.

1. **Enter the Chroot Environment:**Bash
    
    `sudo chroot $MNT_POINT`
    
    *(Your prompt should change to show you're logged in as root in the new environment, e.g., `root@manoj-Retr0:/#`)*
    
2. **Force Configure Packages:** This is the core fix. It runs all interrupted package configuration scripts.Bash
    
    `dpkg --configure -a`
    
3. **Verify/Finalize Upgrade:** Run these to confirm all dependencies are met and no updates are pending.Bash
    
    `apt --fix-broken install
    apt upgrade -y`
    
4. **Update GRUB (Critical for Boot):** Regenerate the bootloader configuration to correctly recognize the newly configured kernel.Bash
    
    `update-grub`
    

---

### Step 2: Cleanup and Disconnect

Exit the chroot and safely unmount all partitions using the "lazy" option to handle busy resources.

1. **Exit Chroot:**Bash
    
    `exit`
    
2. **Lazy Unmount All Resources:** This safely detaches the mounts, even if some processes are still holding references to them (the source of the "target is busy" error).Bash
    
    `for i in /dev/pts /dev /proc /sys /run; do sudo umount -l $MNT_POINT$i; done
    sudo umount -l $MNT_POINT`
    
3. **Final Check:** Use `lsblk` to confirm no mountpoints are left on the external drive (`sdd`).
    
    **→ Your system is now repaired. Reinstall the hard drive and boot normally.**
    

---

### Post-Recovery Housekeeping

After successfully booting the repaired system, run these commands to keep it clean and healthy:

1. **Remove Old Kernels and Unused Packages:**Bash
    
    `sudo apt autoremove`
    
2. **Fix Broken Docker Repository (Optional but Recommended):**
If you see the `xia` repository error again, you need to find the offending file and correct the codename (`xia` is likely outdated).Bash
    
    `grep -r docker /etc/apt/sources.list.d/
    # Once the file is found (e.g., docker.list), edit it:
    # sudo nano /etc/apt/sources.list.d/docker.list`
    
    Change the line containing `xia` to the correct codename (`noble` or `jammy` if you are running Mint 22.x or Ubuntu 24.04).