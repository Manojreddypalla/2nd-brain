
# Dev Diary — Switching Git to SSH on Linux
**Date:** August 29, 2025  
**Author:** Manoj Reddy Palla

---

## Why I wrote this
I migrated my GitHub workflow from HTTPS to **SSH** on Linux. This diary captures every step, the mistakes I made, how I fixed them, and the final working commands. Future‑me (and anyone else) can reuse this as a setup + troubleshooting guide.

---

## TL;DR (Cheat Sheet)
```bash
# 1) Create SSH key (no sudo; press Enter at the file prompt)
ssh-keygen -t ed25519 -C "manojreddypalla@gmail.com"

# 2) Start agent & add key
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 3) Copy public key & add to GitHub → Settings → SSH and GPG keys
cat ~/.ssh/id_ed25519.pub

# 4) Test
ssh -T git@github.com

# 5) Use SSH URL
git clone git@github.com:Manojreddypalla/icnsiet.git

# 6) One-time identity (required for commits)
git config --global user.name "Manoj Reddy Palla"
git config --global user.email "manojreddypalla@gmail.com"
```
---

## Full Story (What happened)

### 1) First attempts — I used `sudo` and a custom filename
- I ran:
  ```bash
  sudo ssh-keygen -t ed25519 -C "manojreddypalla@gmail.com"
  # then typed a custom filename: 1379
  ```
- Result: keys **1379** and **1379.pub** got created at the filesystem root **/** and owned by **root**.
- When I tried again as a normal user (still in `/`) and typed `1379`, I got:
  ```
  Saving key "1379" failed: Permission denied
  ```

**Lesson:** SSH keys are **per-user**. Don’t use `sudo`. Save to the **default path** (`~/.ssh/id_ed25519`) by just pressing **Enter** at the prompt. Also, normal users can’t write in `/`.

**Fix I did:**
```bash
# clean up wrong files (needed sudo because they were owned by root)
sudo rm /1379 /1379.pub

# go home, try again (no sudo), accept defaults
cd ~
ssh-keygen -t ed25519 -C "manojreddypalla@gmail.com"
# "Enter file in which to save the key (/home/manoj/.ssh/id_ed25519):"  → pressed Enter
```

✅ Output confirmed:
```
Your identification has been saved in /home/manoj/.ssh/id_ed25519
Your public key has been saved in /home/manoj/.ssh/id_ed25519.pub
```

---

### 2) Load key into ssh-agent and add to GitHub
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
# prompted for passphrase (if set): entered it → "Identity added"
```

Then I copied the **public** key and pasted it in **GitHub → Settings → SSH and GPG keys → New SSH key**:
```bash
cat ~/.ssh/id_ed25519.pub
# looked like: ssh-ed25519 AAAA... manojreddypalla@gmail.com
```

**Tested the connection:**
```bash
ssh -T git@github.com
# First time: accepted the host fingerprint → success message
```

---

### 3) Using the repo over SSH
SSH URL for my repo:
```
git@github.com:Manojreddypalla/icnsiet.git
```

- **Cloning fresh:**
  ```bash
  git clone git@github.com:Manojreddypalla/icnsiet.git
  cd icnsiet
  ```

- **If I had cloned via HTTPS earlier (not needed today, but future note):**
  ```bash
  git remote set-url origin git@github.com:Manojreddypalla/icnsiet.git
  ```

---

### 4) Git errors I hit and how I fixed them

**Error A — `fatal: not a git repository`**
- I ran `git add .` in `/media/manoj/NARUTO` (not inside a repo).  
- **Fix:** Clone the repo (or `cd` into it) **first**:
  ```bash
  cd ~
  git clone git@github.com:Manojreddypalla/icnsiet.git
  cd icnsiet
  ```

**Error B — `Author identity unknown`**
- First commit failed because Git didn’t know my name/email.  
- **Fix (one-time global config):**
  ```bash
  git config --global user.name "Manoj Reddy Palla"
  git config --global user.email "manojreddypalla@gmail.com"
  ```
- After this:
  ```bash
  git add .
  git commit -m "initial"
  git push
  ```

---

## Notes & Best Practices
- **Never share** your private key (`~/.ssh/id_ed25519`). Share only the `.pub` file.
- Use a **passphrase** on your key for extra security (ssh-agent remembers it per session).
- If you ever see the "permission denied" error again during keygen, check:
  - Current directory (`pwd`), who owns the file, and whether you used `sudo` by mistake.
- Keep the **SSH URL** habit: it starts with `git@github.com:` (not `https://`).

---

## What I’d do next
- Add this diary to the repo as `JOURNAL.md` (or append to `README.md`).
- Configure a credential helper/agent auto-start if needed.
- (Optional) Add a second key for another machine and label them clearly in GitHub.

---

## End State
- SSH key generated and stored in `~/.ssh/id_ed25519`.
- Key loaded into `ssh-agent`.
- Public key added to GitHub.
- Repo cloned via SSH and able to **pull/push** without passwords.
```

