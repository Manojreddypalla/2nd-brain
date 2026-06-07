# Exit Codes

Bash commands and scripts return an exit code (also known as exit status or return code) upon completion. This code indicates whether the execution was successful or if an error occurred.

## Understanding Exit Codes

*   Exit codes range from 0 to 255.
*   **`0`**: Indicates **success**. This is the standard for successful execution.
*   **Non-zero (1-255)**: Indicates **failure** or an error.
    *   **`1`**: Generally indicates a general error.
    *   **`126`**: Command found but is not executable.
    *   **`127`**: Command not found.
    *   Other non-zero codes often have specific meanings defined by the command or script itself.

## Checking Exit Codes

The special variable `$?` holds the exit code of the most recently executed foreground command or script.

**Example Script (`exit_codes.sh`):**

```bash
#!/bin/bash
# Experimenting with exit codes

# Attempt a successful command (ls)
ls -l > /dev/null # Redirect output to /dev/null to silence it
echo "The exit code of the ls command was: $?"

# Attempt a non-existent command (lzl)
lzl 2> /dev/null # Redirect stderr to /dev/null to silence error message
echo "The exit code of the non-existing lzl command was: $?"
```

**How to Run:**

```bash
chmod u+x exit_codes.sh
./exit_codes.sh
```

**Output:**

```
The exit code of the ls command was: 0
The exit code of the non-existing lzl command was: 127
```

*   **Warning:** Using `/dev/null` to silence output should be done with caution, as it can hide important error messages. For critical operations, consider redirecting output to a dedicated log file instead.

## Setting a Script's Exit Code

You can explicitly set the exit code of your own Bash script using the `exit` command followed by the desired code number.

**Example Script (`set_exit_code.sh`):**

```bash
#!/bin/bash
# Sets the exit code of the script to be 223

echo "Exiting with exit code: 223"
exit 223
```

**How to Run and Check:**

```bash
chmod u+x set_exit_code.sh
./set_exit_code.sh
echo $?
# Output: 223
```

Exit codes are fundamental for controlling the logical flow of scripts, especially when chaining commands or making decisions based on the success or failure of previous operations.
