# 10. Bash Customizations for Penetration Testers

Optimize your shell environment for common tasks.

-   **`PATH` Variable**: Add directories containing your custom scripts to the `$PATH` environment variable to run them from anywhere by name.
    ```bash
    # Add to ~/.bashrc
    export PATH="$PATH:/path/to/my/scripts"
    ```
-   **`alias`**: Create shortcuts for long commands.
    ```bash
    # Add to ~/.bashrc
    alias ll='ls -alF'
    alias quickscan='nmap -T4 -F'
    ```
-   **`~/.bashrc`**: A script that runs every time you open a new interactive shell. It's the perfect place to define aliases, functions, and environment variables that you want to be permanent.
-   **`source ~/.bashrc`**: Reloads the `.bashrc` file in the current shell session to apply changes immediately.
-   **`script` command**: Records your entire terminal session to a file, which is useful for documentation and evidence gathering.

**Example `~/.bashrc` for a penetration tester:**

```bash
# ~/.bashrc

# Add custom scripts to PATH
export PATH="$PATH:~/tools/custom_scripts"

# Aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias ..='cd ..'
alias ...='cd ../..'

# Networking
alias myip='curl ifconfig.me'
alias ports='netstat -tulpn'

# Nmap scans
alias nmap-tcp='nmap -sS -sV -O -T4'
alias nmap-udp='nmap -sU -sV -T4'
alias nmap-full='nmap -p- -T4'

# Web server
alias serve='python3 -m http.server 8000'

# Functions
# Create a directory and cd into it
mkcd() {
    mkdir -p "$1" && cd "$1"
}

# Start a listener
listen() {
    nc -nlvp "$1"
}
```
