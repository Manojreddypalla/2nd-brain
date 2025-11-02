## Step 4: If still broken, manually fix DNS temporarily

Edit the resolver:

```bash
sudo nano /etc/resolv.conf

```

Add:

```
nameserver 8.8.8.8
nameserver 1.1.1.1

```

- Disable auto-DNS management:
    
    ```bash
    sudo chattr +i /etc/resolv.conf
    
    ```
    
    (Prevents it from being overwritten — can undo with `sudo chattr -i /etc/resolv.conf`.)
    
- Or set DNS permanently in:
    
    - `/etc/netplan/*.yaml`
    - or NetworkManager GUI