### Step-by-Step Fix for Static IP (Modern Netplan Format)

1. Open your config file again:
    
    ```bash
    sudo nano /etc/netplan/00-installer-config.yaml
    
    ```
    
2. Replace everything with this (adjust `enp3s0f0` and gateway/DNS if needed):
    
    ```yaml
    network:
      version: 2
      ethernets:
        enp3s0f0:
          dhcp4: false
          addresses:
            - 192.168.1.90/24
          routes:
            - to: default
              via: 192.168.1.1
          nameservers:
            addresses:
              - 8.8.8.8
              - 1.1.1.1
    
    ```
    
    ✅ **Explanation**
    
    - `dhcp4: false` → disables auto IP from router.
    - `addresses:` → sets your static IP.
    - `routes:` → replaces old `gateway4`.
    - `nameservers:` → DNS servers (Google & Cloudflare here).
3. Save (`Ctrl+O`, `Enter`) and exit (`Ctrl+X`).
    
4. Apply changes:
    
    ```bash
    sudo netplan apply
    
    ```
    
5. Check:
    
    ```bash
    ip a
    
    ```
    
    or
    
    ```bash
    ping 8.8.8.8 -c 4
    
    ```
    

---

If everything works, your machine now has:

- **Static IP:** 192.168.1.90
- **Gateway:** 192.168.1.1
- **DNS:** Google + Cloudflare
- **No deprecated warnings**