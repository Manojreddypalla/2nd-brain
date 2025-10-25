# Resolving SSH Key Conflicts

# **Title: Resolving SSH Key Conflicts for Multiple GitHub Accounts on Windows**

## **1. Problem Overview**

While trying to commit or clone a GitHub repository on a Windows machine, the user experienced repeated errors:

```
Warning: Identity file C:Usersmanoj.sshid_ed25519_college not accessible
Can't open user config file NUL: No such file or directory
fatal: Could not read from remote repository.

```

However, testing the SSH key with:

```
ssh -T git@github.com

```

worked correctly:

```
Hi Manojreddypalla! You've successfully authenticated, but GitHub does not provide shell access.

```

**Observation:** Git was trying to use a **non-existent SSH key** (`id_ed25519_college`) even though the main key was correctly set up.

---

## **2. Cause Analysis**

1. **Old Environment Variable: `GIT_SSH_COMMAND`**
    - At some point, the user tried to configure Git to use a secondary “college” SSH key by running:
        
        ```bash
        setx GIT_SSH_COMMAND "ssh -i C:\Users\manoj\.ssh\id_ed25519_college -F NUL"
        
        ```
        
    - This sets a **system-wide environment variable** that forces Git to always use the college key, even if it doesn’t exist.
2. **Missing SSH config file / misconfiguration**
    - The SSH config file was either missing, misnamed (`config.txt` instead of `config`), or contained references to the college key.
3. **Attempting Git commands in CMD instead of Git Bash**
    - Commands like `eval` or `$GIT_SSH_COMMAND` only work in Git Bash or Unix-like shells. Running them in CMD causes errors, confusing the setup further.
4. **Git still referencing old key**
    - Even after generating a new main key and successfully authenticating with GitHub, Git continued attempting the **college key** because of the `GIT_SSH_COMMAND` environment variable.

---

## **3. Step-by-Step Resolution**

### **Step 1: Remove the old environment variable**

In Git Bash:

```bash
unset GIT_SSH_COMMAND

```

- This clears the college key reference for the current session.

To remove it permanently (system-wide):

```bash
setx GIT_SSH_COMMAND ""

```

- Close all terminals after running this.

---

### **Step 2: Verify SSH configuration**

1. Ensure the config file exists and is named correctly:

```bash
ls ~/.ssh/config

```

1. If missing, create it:

```bash
nano ~/.ssh/config

```

Content:

```
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes

```

- Remove any references to `id_ed25519_college`.

---

### **Step 3: Start ssh-agent and add the main key**

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

```

- Verify output: `Identity added: /c/Users/manoj/.ssh/id_ed25519 (manojreddypalla@gmail.com)`

---

### **Step 4: Test SSH connection**

```bash
ssh -T git@github.com

```

Expected:

```
Hi Manojreddypalla! You've successfully authenticated, but GitHub does not provide shell access.

```

---

### **Step 5: Clone or push repositories**

```bash
git clone git@github.com:Manojreddypalla/icnsiet2025.git

```

- Git now correctly uses your **main SSH key**.
- No warnings about `id_ed25519_college` appear.

---

## **4. Key Lessons Learned**

1. **System-wide environment variables can override local settings**.
    - `GIT_SSH_COMMAND` was the main cause.
2. **SSH config files must be correctly named** (`config`, not `config.txt`) and contain valid paths.
3. **Use Git Bash for SSH and Git commands on Windows**.
4. **Adding multiple SSH keys for different accounts** requires proper host aliases in the config file, otherwise Git will fail to authenticate.

---

## **5. Final Notes**

- Current setup uses **only the main GitHub account** (`manojreddypalla@gmail.com`).
- Old “college” key references are completely removed.
- Git SSH workflow is now clean, reliable, and ready for cloning, committing, and pushing without errors.