# Full step-by-step guide

### 0) Assumptions

- You’re on the machine shown (`retr0@retr0`) and have `cloudflared` installed and in PATH.
    
- You control a domain on Cloudflare (e.g. `example.com`) and can add DNS records in the Cloudflare dashboard. Replace `blog.example.com` below with the hostname you want.
    

---

### 1) Lock down the credentials file

Make the file readable only by root/user that runs cloudflared:

`sudo chown retr0:retr0 /home/retr0/.cloudflared/6fb1cdc0-1c4d-40bd-b663-452d25aab541.json chmod 600 /home/retr0/.cloudflared/6fb1cdc0-1c4d-40bd-b663-452d25aab541.json`

### 2) Create a `config.yml` for cloudflared

Create `/etc/cloudflared/config.yml` (system-wide) or `~/.cloudflared/config.yml` (user). Example (system-wide):

```yaml
tunnel: 6fb1cdc0-1c4d-40bd-b663-452d25aab541
credentials-file: /home/retr0/.cloudflared/6fb1cdc0-1c4d-40bd-b663-452d25aab541.json

ingress:
  - hostname: blog.example.com
    service: <http://localhost:8080>   # change to where your app listens
  - hostname: "*.blog.example.com"
    service: http_status:404
  - service: http_status:404

```

Adjust `hostname` and `service` to match your app (e.g., `http://127.0.0.1:3000`).

Save file and ensure cloudflared can read it:

```bash
sudo mkdir -p /etc/cloudflared
sudo cp /home/retr0/.cloudflared/6fb1cdc0-1c4d-40bd-b663-452d25aab541.json /etc/cloudflared/
sudo chown root:root /etc/cloudflared/*
sudo chmod 600 /etc/cloudflared/*
# if you moved the JSON, update the path in config.yml accordingly

```

_(Optional: you can keep the credentials in the user dir and set `credentials-file` to that path instead.)_