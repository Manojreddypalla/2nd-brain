# Exploring the Shell

## Verifying the Bash Version

To confirm that Bash is available and check its version, run:

```bash
bash --version
```

## Checking Environment Variables

Environment variables store information about the shell session and the user's environment.

*   **List all environment variables:**
    ```bash
    env
    ```
*   **Print a specific environment variable:**
    ```bash
    echo ${SHELL}
    ```
    *Note: The `${}` syntax is recommended for clarity, but `$SHELL` also works.*

*   **Common Default Environment Variables:**
    *   `BASH_VERSION`: The version of the running Bash shell.
    *   `BASHPID`: The process identifier (PID) of the current Bash process.
    *   `GROUPS`: A list of groups the current user belongs to.
    *   `HOSTNAME`: The name of the host.
    *   `OSTYPE`: The type of operating system.
    *   `PWD`: The current working directory.
    *   `RANDOM`: A random number between 0 and 32,767.
    *   `UID`: The user ID (UID) of the current user.
    *   `SHELL`: The full path to the shell.

## Running Linux Commands

*   **Manual Pages (`man`):** To understand a command's usage and options, use the `man` command.
    ```bash
    man ls
    ```

*   **Argument Syntax:**
    *   **Short-form:** A single dash followed by one or more characters (e.g., `-l`, `-a`). Some commands allow combining arguments (e.g., `ls -la`).
    *   **Long-form:** A double dash followed by a word (e.g., `--all`, `--help`).
