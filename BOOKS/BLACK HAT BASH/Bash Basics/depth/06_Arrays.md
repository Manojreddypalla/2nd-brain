# Arrays

Bash allows you to create single-dimension arrays, which are indexed collections of elements. Array indices start at zero. Arrays are useful for iterating over multiple strings and performing actions on each.

## Creating and Accessing Arrays

*   **Creating an array:**
    ```bash
    IP_ADDRESSES=(192.168.1.1 192.168.1.2 192.168.1.3)
    ```
*   **Printing all elements:** Use `[*]` or `[@]` to represent all elements.
    ```bash
    echo "${IP_ADDRESSES[*]}"
    # Output: 192.168.1.1 192.168.1.2 192.168.1.3
    ```
*   **Printing a specific element:** Use the index number (0-based).
    ```bash
    echo "${IP_ADDRESSES[0]}"
    # Output: 192.168.1.1
    ```

## Deleting Array Elements

*   Use the `unset` command with the array name and the index of the element to delete.
    ```bash
    IP_ADDRESSES=(192.168.1.1 192.168.1.2 192.168.1.3)
    unset IP_ADDRESSES[1] # Deletes 192.168.1.2
    echo "${IP_ADDRESSES[*]}"
    # Output: 192.168.1.1 192.168.1.3
    ```

## Modifying Array Elements

*   Assign a new value to a specific index.
    ```bash
    IP_ADDRESSES=(192.168.1.1 192.168.1.2 192.168.1.3)
    IP_ADDRESSES[0]="192.168.1.10" # Replaces the first element
    echo "${IP_ADDRESSES[*]}"
    # Output: 192.168.1.10 192.168.1.2 192.168.1.3
    ```
