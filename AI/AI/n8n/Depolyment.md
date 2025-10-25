# Depolyment

### ## Your Accomplishment ✅

You successfully deployed a private, self-hosted **n8n automation server** using **Docker** on your **Linux Mint** machine. It's securely accessible from anywhere through your **Tailscale** network.

---

### ## Your Final `docker-compose.yml` Configuration

This is the complete and correct configuration file located at `~/n8n/docker-compose.yml`. It includes all the fixes we applied.

```
version: '3.8'

# This section defines the services (containers) that make up your application.
services:
  n8n:
    # Use the latest official n8n image.
    image: n8nio/n8n:latest
    
    # This policy ensures the container restarts automatically unless you manually stop it.
    restart: unless-stopped
    
    # Maps port 5678 on your server to port 5678 inside the container.
    ports:
      - "5678:5678"
      
    # Environment variables to configure n8n.
    environment:
      # Sets the timezone to prevent errors. I've set it based on your location.
      - GENERIC_TIMEZONE=Asia/Kolkata
      
      # Allows access over HTTP without a domain. KEEP THIS for your local setup.
      - N8N_SECURE_COOKIE=false
      
      # The following variables fix the deprecation warnings you saw in the logs.
      - DB_SQLITE_POOL_SIZE=10
      - N8N_RUNNERS_ENABLED=true
      - N8N_BLOCK_ENV_ACCESS_IN_NODE=false

    # Mounts a managed Docker volume to persist your n8n data (workflows, credentials).
    volumes:
      - n8n_data:/home/node/.n8n

# This top-level key defines the managed Docker volumes.
volumes:
  n8n_data:
    # An empty driver means Docker will use the default local driver.
```

# This top-level key defines the managed Docker volumes.

volumes:
n8n_data:
# An empty driver means Docker will use the default local driver.

### ## Your Essential Command Cheat Sheet 🚀

These are the key commands for managing your setup, all run from the `~/n8n` directory on your server.

**Managing Your n8n Service:**

- **Start n8n in the background:** `docker-compose up -d`
- **Stop n8n:** `docker-compose down`
- **Restart n8n:** `docker-compose restart`
- **Update n8n to the latest version:** `docker-compose pull && docker-compose up -d`

**Checking Status & Logs:**

- **Check if n8n is running:** `docker-compose ps`
- **View n8n logs for errors:** `docker-compose logs n8n`

**Server Management:**

- **Connect to your server remotely:** `ssh manoj@<your-tailscale-ip>`
- **Check firewall status:** `sudo ufw status`

---

### ## Key Troubleshooting Fixes You Performed

You solved several common issues to get this working:

1. **Firewall Block:** Your `ufw` firewall was active. You fixed this by allowing the n8n port with `sudo ufw allow 5678`.
2. **File Permissions:** The n8n container couldn't write to its data folder. You fixed this by changing the folder ownership with `sudo chown -R 1000:1000 ./n8n-data`.
3. **Secure Cookie Error:** You resolved the `http://` access error by adding the `N8N_SECURE_COOKIE=false` environment variable, which is safe to do because your Tailscale network is already encrypted.

---

### ## How to Access Your n8n

You can access your n8n instance from any device that is also on your Tailscale network by going to:

**`http://<your-tailscale-ip>:5678`**