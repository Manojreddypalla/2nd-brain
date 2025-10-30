# mint random crashes

# Linux Mint Crashes on Mac Mini 2014: Diagnosis and Fix

---

## 1. Problem Description

**Issue:**

Linux Mint running on a Mac Mini 2014 would randomly freeze or crash during normal usage.

**Symptoms:**

- Sudden system freeze.
- ACPI kernel errors in logs.
- Errors related to hardware power management.

---

## 2. Diagnosis

**Steps Taken:**

1. Checked logs with:
    
    ```bash
    journalctl -p 3 -xb
    
    ```
    
    Observed ACPI errors:
    
    ```
    ACPI Error: No handler for Region [CMS0] ...
    ACPI Error: AE_NOT_EXIST, during \_SB.PCI0._INI execution
    
    ```
    
2. Verified system stability was affected by ACPI.
3. Noted Mac Mini’s hardware relies on ACPI for power management, which was causing instability.

---

## 3. Solution Implemented

**Fix:**

Disabled ACPI support by editing GRUB configuration.

**Steps:**

1. Open GRUB configuration:
    
    ```bash
    sudo nano /etc/default/grub
    
    ```
    
2. Edit the line:
    
    ```
    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
    
    ```
    
    to:
    
    ```
    GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi=off"
    
    ```
    
3. Update GRUB:
    
    ```bash
    sudo update-grub
    
    ```
    
4. Reboot the system:
    
    ```bash
    sudo reboot
    
    ```
    

**Effects:**

- Random crashes stopped.
- System became stable under load.

**Side Effects:**

- No automatic sleep/hibernate.
- No ACPI fan control.
- Some hardware features might not work fully.
- Slightly higher power usage.

These side effects were acceptable because the goal was a stable **24/7 server**.

---

## 4. Stability Verification

**Testing Method:**

1. Installed stress testing tool:
    
    ```bash
    sudo apt install stress -y
    
    ```
    
2. Ran a stress test:
    
    ```bash
    stress --cpu 4 --io 2 --vm 2 --vm-bytes 128M --timeout 600
    
    ```
    
3. Monitored logs:
    
    ```bash
    watch -n 5 "journalctl -p 3 -b -n 20"
    
    ```
    

**Result:**

- No ACPI errors appeared.
- System remained stable.

---

## 5. Remaining Issues

Post-fix, some unrelated service errors remain:

- `dhcpd` configuration issues.
- `nginx.service` failure.
- `jellyfin.service` failure.
- Disk mount timeout errors.

These are unrelated to the crash and require separate configuration fixes.

---

## 6. Conclusion

By disabling ACPI (`acpi=off`), Linux Mint on Mac Mini 2014 became **stable for long-term use**, resolving random crash issues. This was verified with stress tests and log monitoring.

---

### Commands Summary for Future Reference:

```bash
# Check recent errors
journalctl -p 3 -xb

# Edit GRUB to disable ACPI
sudo nano /etc/default/grub
# Add acpi=off to GRUB_CMDLINE_LINUX_DEFAULT
sudo update-grub
sudo reboot

# Install stress test
sudo apt install stress -y

# Run stress test
stress --cpu 4 --io 2 --vm 2 --vm-bytes 128M --timeout 600

# Monitor logs
watch -n 5 "journalctl -p 3 -b -n 20"

```

---

**End of Document**