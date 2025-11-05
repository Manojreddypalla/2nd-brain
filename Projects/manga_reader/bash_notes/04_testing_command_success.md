# 4. Testing Command Success

You can use the exit code of a command directly as a condition. An exit code of `0` means success.

```bash
if command; then
  # command was successful.
fi

if ! command; then
  # command was unsuccessful.
fi
```

**Example:**

```bash
#!/bin/bash

# Check if a user exists in /etc/passwd
read -p "Enter a username to check: " user

if grep -q "^$user:" /etc/passwd; then
    echo "User $user exists."
else
    echo "User $user does not exist."
fi
```
