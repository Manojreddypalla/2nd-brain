# How to Expose Ollama to Your Local Network

### By default, Ollama only listens for requests on `localhost` (`127.0.0.1`), which means it can only be accessed from the same machine. This guide will walk you through the steps to make it accessible to other devices on your local network or a Tailscale network.

### **Step 1: Configure Ollama to Listen for Network Connections**

The first step is to tell Ollama to accept connections from any IP address, not just `localhost`. This is done by setting an environment variable.

1. **Open PowerShell as an Administrator**: Right-click your Start button and select "PowerShell (Admin)".
2. **Set the Environment Variable**: Run the following command to permanently set `OLLAMA_HOST` to `0.0.0.0`. This special IP address tells a program to listen on all available network interfaces.PowerShell
    
    `[System.Environment]::SetEnvironmentVariable('OLLAMA_HOST', '0.0.0.0', [System.EnvironmentVariableTarget]::Machine)`
    
3. **Restart the Ollama Service (Crucial)**: The change will not take effect until you completely restart Ollama.
    - Find the Ollama icon in your system tray (bottom-right).
    - Right-click it and select **"Quit Ollama"**.
    - Restart Ollama from your Start Menu.

---

### **Step 2: Add a Firewall Rule on the Host Machine**

Your computer's firewall will block incoming connections by default. You need to create a rule to allow other devices to communicate with Ollama on its port, `11434`.

1. **Open Advanced Firewall Settings**: Search for "Windows Defender Firewall with Advanced Security" in the Start Menu.
2. **Create a New Inbound Rule**:
    - Click on **"Inbound Rules"** on the left, then **"New Rule..."** on the right.
    - **Rule Type**: Select **Port**.
    - **Protocol and Ports**: Select **TCP** and enter **11434** for "Specific local ports".
    - **Action**: Select **Allow the connection**.
    - **Profile**: Keep all network profiles (Domain, Private, Public) checked.
    - **Name**: Give the rule a memorable name, like "Ollama Access", and click **Finish**.

---

### **Step 3: Verify the Configuration**

Now, you need to confirm that Ollama is listening correctly and that the firewall is not blocking it.

1. **Check if Ollama is Listening on the Network**:
    - Open a normal Command Prompt or PowerShell on the same Windows machine.
    - Run the command: `netstat -an | findstr "11434"`
    - The output **must show `0.0.0.0:11434`**. If it still shows `127.0.0.1:11434`, repeat Step 1, ensuring you restart Ollama properly.
        
        `# Correct output
        TCP    0.0.0.0:11434          0.0.0.0:0              LISTENING`
        
2. **Find Your Host Machine's IP Address**:
    - You'll need this IP to connect from other devices.
    - For a **regular local network**, run `ipconfig` and find the "IPv4 Address" (e.g., `192.168.1.123`).
    - For a **Tailscale network**, run `tailscale ip -4` to get your `100.x.x.x` address.
3. **Test the Connection from Another Device**:
    - On another computer or server on the same network (like your Linux n8n server), use the `curl` command:Bash
        
        `# Replace the IP with your Ollama machine's IP
        curl http://100.111.138.41:11434`
        
    - A successful connection will return the message: `Ollama is running`.

---

### ## 🚀 Using the Exposed Ollama API in n8n

Once you've confirmed the connection works, you can use it in any n8n workflow with the **HTTP Request** node.

- **Method**: `POST`
- **URL**: `http://<YOUR_OLLAMA_IP>:11434/api/generate` (e.g., `http://100.111.138.41:11434/api/generate`)
- **Body Content Type**: `JSON`
- **Body**:JSON
    
    `{
      "model": "llama3",
      "prompt": "This is a test from my n8n workflow!",
      "stream": false
    }`
    

How to Expose Ollama to Your Local Network