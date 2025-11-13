# Control Operators

Control operators in Bash are tokens that perform control functions, allowing you to sequence and manage command execution.

## Overview of Control Operators

| Operator | Description                                                                                                                                                                                                                                                               |
| :------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `&`      | **Background a Command:** Sends a command to the background, allowing the script to continue to the next command without waiting for the backgrounded command to finish. Useful for long-running processes.                                                                 |
| `&&`     | **Logical AND:** Executes the second command **only if** the first command successfully completes (returns an exit code of 0).                                                                                                                                                |
| `( )`    | **Command Grouping:** Groups commands so they act as a single unit. Useful for redirecting the output of multiple commands together.                                                                                                                                         |
| `;`      | **List Terminator/Command Separator:** Executes commands sequentially. The second command runs after the first finishes, regardless of the first command's exit status.                                                                                                    |
| `;;`     | **Case Statement Terminator:** Used to terminate patterns in `case` statements.                                                                                                                                                                                             |
| `|`      | **Pipe:** Redirects the standard output (stdout) of the command on the left as the standard input (stdin) to the command on the right.                                                                                                                                     |
| `||`     | **Logical OR:** Executes the second command **only if** the first command fails (returns a non-zero exit code).                                                                                                                                                             |

## Examples

*   **`&` (Backgrounding):**
    ```bash
    echo "Sleeping for 10 seconds..."
    sleep 10 & # Runs sleep in the background
    echo "Creating the file test123"
    touch test123 # This command runs immediately without waiting for sleep
    ```

*   **`&&` (Logical AND):**
    ```bash
    # test123 will only be created if the 'touch test' command succeeds
    touch test && touch test123
    ```

*   **`( )` (Command Grouping):**
    ```bash
    # Groups ls and ps, potentially for redirection to a single output
    (ls; ps)
    ```

*   **`;` (Command Separator):**
    ```bash
    # Runs ls, then ps, then whoami, one after the other
    ls; ps; whoami
    ```

*   **`||` (Logical OR):**
    ```bash
    # The echo command will only run if 'lzl' (a non-existent command) fails
    lzl || echo "the lzl command failed"
    ```
