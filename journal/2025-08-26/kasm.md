# 🚀 Kasm Workspaces Setup Diary

## 📌 Day 1: Installing Kasm on Linux

### Problem 1: Missing file during install
```bash
wget https://kasm-static-content.s3.amazonaws.com/kasm_release_1.14.0.0fd4c7.tar.gz
```
❌ Error:
```
404 Not Found
```

✅ Solution:  
Found latest release from [Kasm official downloads](https://www.kasmweb.com/downloads).  
Downloaded:
```bash
wget https://kasm-static-content.s3.amazonaws.com/kasm_release_1.17.0.7f020d.tar.gz
tar -xvzf kasm_release_1.17.0.7f020d.tar.gz
cd kasm_release
sudo ./install.sh
```

---

### Problem 2: No Swap detected
Kasm warned:
```
Your system does not have a Swap file or partition...
```

✅ Solution:  
Since system has **4GB RAM**, created **8GB swap file** on NVMe SSD.

---

### Problem 3: Kasm not accessible after install
Tried accessing:
```
https://<server-ip>
```
❌ Could not connect.

✅ Solution:  
Checked running containers:
```bash
sudo docker ps
```
Confirmed all Kasm containers running.  
Realized Kasm defaults to **port 443** → had to use:
```
https://192.168.1.90/
```

---

### Problem 4: Workspace persistence (saving files)
Initially, files created in Kasm workspaces were **not saved** after logout.

✅ Solution:  
Used Docker volume mounts:
1. Edited `/opt/kasm/current/docker/docker-compose.override.yml`
2. Added:
```yaml
services:
  kasm_agent:
    volumes:
      - /media/manoj/NARUTO:/home/kasm-user/Persistent/NARUTO
```

---

### Problem 5: `KASM_UID` error when using `docker compose`
```
error while interpolating services.kasm_guac.user: required variable KASM_UID is missing a value
```

✅ Solution:  
Instead of plain `docker compose`, used Kasm’s built-in scripts that load env vars:

```bash
cd /opt/kasm/current
./bin/stop
./bin/start
```

---

## 🎉 Success
- Kasm running on Linux host  
- Workspaces accessible via browser + Android  
- Able to launch heavy apps like **Blender** and **VS Code**  
- Persistent storage linked to my NAS folder `/media/manoj/NARUTO`  

---

## 🔮 Next Steps
- Expand mount to full `/media/manoj/` instead of single folder  
- Try multi-user access  
- Benchmark performance with Blender streaming  

