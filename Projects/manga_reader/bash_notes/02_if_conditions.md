# 2. `if` and `elif` Conditions

Executes code based on a condition.

**Syntax:**
```bash
if [[ condition ]]; then
  # Do something if the condition is met.
elif [[ another_condition ]]; then
  # Do something else.
else
  # Do something if no conditions are met.
fi
```

-   `[[ ... ]]`: The modern, preferred bash construct for tests. `[ ... ]` is the older, more portable POSIX standard.
-   `!`: The NOT operator, used to negate a condition. `if [[ ! -f "file.txt" ]]`
-   `elif`: Allows checking for other conditions if the first `if` condition fails.

**Example:**

```bash
#!/bin/bash

read -p "Enter a number: " num

if [[ "$num" -gt 0 ]]; then
    echo "The number is positive."
elif [[ "$num" -lt 0 ]]; then
    echo "The number is negative."
else
    echo "The number is zero."
fi
```
