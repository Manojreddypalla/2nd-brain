# 5. Functions

Reusable blocks of code.

**Defining a function:**
```bash
function_name(){
  # code to execute
  echo "Hello from a function"
}
```

**Calling a function:**
```bash
function_name
```

**Arguments:** Functions receive arguments positionally (`$1`, `$2`, etc.), just like scripts.
```bash
print_args(){
  echo "First: ${1}, Second: ${2}"
}
print_args "Hello" "World"
```

**Returning Values:** Use the `return` keyword to return a numeric exit status (0 for success, 1-255 for failure). If `return` is not used, the function returns the exit status of the last command executed.

**Example of returning a value:**

```bash
#!/bin/bash

# Function to add two numbers
add() {
    return $(($1 + $2))
}

add 5 10
result=$?

echo "The sum is: $result"
```
