# 3. Linking Conditions

Combine multiple conditions in a single `if` statement.

-   **AND (`&&`)**: Both conditions must be true.
    ```bash
    if [[ -f "file.txt" ]] && [[ -s "file.txt" ]]; then
      echo "File exists and is not empty."
    fi
    ```
-   **OR (`||`)**: At least one condition must be true.
    ```bash
    if [[ -f "item" ]] || [[ -d "item" ]]; then
      echo "Item is a file or a directory."
    fi
    ```

**Combined Example:**

```bash
#!/bin/bash

read -p "Enter your age: " age
read -p "Are you a student? (y/n): " student

if [[ "$age" -ge 18 && "$student" == "y" ]] || [[ "$age" -lt 18 ]]; then
    echo "You are eligible for a discount."
else
    echo "You are not eligible for a discount."
fi
```
