# How to Add an SSH Key for GitHub on Windows

---

This guide will walk you through the complete process of generating a new SSH key and adding it to your GitHub account on a Windows machine. Using an SSH key is a secure way to connect to GitHub without needing to enter your username and password every time you push or pull.

### Prerequisites

- You must have **Git for Windows** installed, as this guide uses the **Git Bash** terminal that comes with it.

---

### Step 1: Open Git Bash and Check for Existing Keys

First, you need to check if you already have an SSH key. It's good practice to avoid creating new keys if you already have one you can use.

1. Open the **Git Bash** terminal.
2. Enter the following command to see if you have any existing keys:Bash
    
    `ls -al ~/.ssh`
    
3. Look for a file pair like `id_ed25519` and `id_ed25519.pub`. If you see these files, you already have a key and can skip to **Step 3**. If not, or if you want to create a new one, proceed to the next step.

---

### Step 2: Generate a New SSH Key

If you don't have a key, you'll need to generate one. We'll use the `Ed25519` algorithm, which is modern and secure.

1. In Git Bash, run the following command. **Make sure to replace the email with the email address associated with your GitHub account.**Bash
    
    `ssh-keygen -t ed25519 -C "your_email@example.com"`
    
2. The terminal will ask you where to save the file. Simply press **Enter** to accept the default location.
    
    `> Enter file in which to save the key (c/Users/YOUR_USERNAME/.ssh/id_ed25519):`
    
    If it says the file already exists (like in your screenshot), you can type `y` and press **Enter** to overwrite it with a new key.
    
3. Next, it will ask you to enter a passphrase. This is an optional password for your SSH key that adds an extra layer of security.
    
    `> Enter passphrase (empty for no passphrase):`
    
    You can either type a secure passphrase and press **Enter**, or just press **Enter** to have no passphrase. You will be asked to confirm it.
    

After this, you will see a confirmation that your key has been saved.

---

### Step 3: Add Your SSH Key to the ssh-agent

The `ssh-agent` is a background program that keeps your key ready and handles authentication for you.

1. Start the agent for your current terminal session:Bash
    
    `eval "$(ssh-agent -s)"`
    
    You should see a message like `Agent pid 12345`.
    
2. Add your newly created private key to the agent:Bash
    
    `ssh-add ~/.ssh/id_ed25519`
    
    If you created a passphrase in the previous step, it will prompt you to enter it here.
    

---

### Step 4: Add Your Public Key to Your GitHub Account

Now, you need to give the **public** part of your key to GitHub so it knows to trust your computer.

1. Copy the public key to your clipboard. This command reads the `.pub` file and pipes it directly to your Windows clipboard.Bash
    
    `cat ~/.ssh/id_ed25519.pub | clip`
    
2. Open your browser and navigate to the SSH keys section of your GitHub settings: [**github.com/settings/keys**](https://github.com/settings/keys)
3. Click the green **"New SSH key"** button.
4. In the **"Title"** field, give your key a descriptive name that you'll recognize, like "My Windows Laptop" or "Work PC".
5. Click inside the **"Key"** text box and **paste** your key (`Ctrl + V`). The text you paste should start with `ssh-ed25519...` and end with your email address.
6. Click **"Add SSH key"** to save it.

---

### Step 5: Test Your Connection

Finally, let's make sure everything is working correctly by attempting to connect to GitHub.

1. In your Git Bash terminal, run:Bash
    
    `ssh -T git@github.com`
    
2. The first time you connect, you might see a warning about the authenticity of the host. This is normal.
    
    `> The authenticity of host 'github.com (IP_ADDRESS)' can't be established.
    > ED25519 key fingerprint is SHA256:....
    > Are you sure you want to continue connecting (yes/no/[fingerprint])?`
    
    Type **`yes`** and press **Enter**.
    
3. If everything was successful, you will see a welcome message with your username:
    
    > Hi Manojreddypalla! You've successfully authenticated, but GitHub does not provide shell access.
    > 

You are all set! You can now use `git push`, `git pull`, and other Git commands to interact with your remote repositories securely and without typing your password.