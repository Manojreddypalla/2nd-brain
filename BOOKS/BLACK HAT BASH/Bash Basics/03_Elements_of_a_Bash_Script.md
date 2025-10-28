# Elements of a Bash Script

## The Shebang Line

*   The first line of a script, it tells the system which interpreter to use.
*   Starts with `#!`.
*   **Standard:** `#!/bin/bash`
*   **Portable:** `#!/usr/bin/env bash` (searches for the bash interpreter in the system's PATH).
*   **Shebang with options:**
    *   `#!/bin/bash -x`: Debug mode. Prints each command before execution.
    *   `#!/bin/bash -r`: Restricted shell mode.

## Comments

*   Lines that are ignored by the interpreter.
*   Start with a hash mark (`#`).
*   Used to explain code, add metadata (author, version), or temporarily disable code.
*   **Single-line comment:**
    ```bash
    # This is a comment
    ```
*   **Multi-line comment:**
    ```bash
    # This is a
    # multi-line comment.
    ```

## Commands

*   Scripts are fundamentally a series of commands that are executed sequentially.
*   Example:
    ```bash
    #!/bin/bash
    echo "Hello World!"
    ```

## Execution

There are two primary ways to run a bash script:

1.  **Make it executable:**
    *   Set the execute permission for the user:
        ```bash
        chmod u+x your_script.sh
        ```
    *   Run the script from the current directory:
        ```bash
        ./your_script.sh
        ```
2.  **Use the `bash` interpreter directly:**
    *   This method does not require the script to have a shebang line or execute permissions.
        ```bash
        bash your_script.sh
        ```

## Debugging

*   **Dry Run (`-n`):**
    *   Checks for syntax errors without executing the script.
        ```bash
        bash -n your_script.sh
        ```
*   **Verbose Mode (`-x`):**
    *   Prints each command and its arguments as they are executed.
        ```bash
        bash -x your_script.sh
        ```
*   **Using `set` for targeted debugging:**
    *   You can turn verbose mode on and off within the script to debug specific sections.
        ```bash
        #!/bin/bash
        echo "This part is not debugged."
        
        set -x  # Turn on debugging
        # ... code to be debugged ...
        set +x  # Turn off debugging
        
        echo "This part is also not debugged."
        ```
