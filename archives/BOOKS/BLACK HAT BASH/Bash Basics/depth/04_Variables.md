# Variables

Variables are names assigned to memory locations that hold values. In Bash, all variables are considered character strings, meaning they are untyped. However, Bash allows for arithmetic operations and array manipulation if the variable content consists solely of numbers.

## Naming Rules for Bash Variables

*   Can include alphanumeric characters.
*   Cannot start with a number.
*   Can contain an underscore (`_`).
*   Cannot contain whitespace.

## Assigning and Accessing Variables

*   **Assignment:** Use the equal sign (`=`) without any whitespace around it.
    ```bash
    book="black hat bash"
    ```
*   **Accessing (Expansion):**
    *   Using curly braces: `${variable}` (recommended for clarity, especially when concatenating with other text).
        ```bash
        echo "This book's name is ${book}"
        # Output: This book's name is black hat bash
        ```
    *   Using only a dollar sign: `$variable`
        ```bash
        echo "This book's name is $book"
        # Output: This book's name is black hat bash
        ```
*   **Command Substitution:** Store the output of a command into a variable using `$(command)`.
    ```bash
    root_directory=$(ls -ld /)
    echo "${root_directory}"
    # Example Output: drwxr-xr-x 1 user user 0 Feb 13 20:12 /
    ```

## Unassigning Variables

*   Use the `unset` command to remove a variable.
    ```bash
    book="Black Hat Bash"
    unset book
    echo "${book}" # Will print an empty line
    ```

## Scoping Variables

*   **Global Variables:** Accessible throughout the entire script.
    ```bash
    PUBLISHER="No Starch Press"
    ```
*   **Local Variables:** Declared within a function using the `local` keyword, making them accessible only within that function.
    ```bash
    print_name(){
      local name="Black Hat Bash"
      echo "${name} by ${PUBLISHER}"
    }
    print_name
    echo "Variable ${name} will not be printed because it is a local variable."
    # Output:
    # Black Hat Bash by No Starch Press
    # Variable  will not be printed because it is a local variable.
    ```
