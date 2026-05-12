# commands for wi-fi connection

### **Using `nmcli` (NetworkManager CLI – recommended)**

Most Linux Mint versions come with NetworkManager, so you can use `nmcli`:

```bash
# List available Wi-Fi networks
nmcli dev wifi list

# Connect to a Wi-Fi network
nmcli dev wifi connect "SSID_NAME" password "YOUR_PASSWORD"

```

Example:

```bash
nmcli dev wifi connect "Home_Wifi" password "mypassword123"

```

If successful, you’ll be connected immediately.