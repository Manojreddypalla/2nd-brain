# Positional Arguments

Bash scripts can accept arguments passed to them on the command line. These are called positional arguments or parameters. They are crucial for making scripts dynamic and reusable, allowing them to modify their behavior based on external input without altering the script's source code.

## Accessing Positional Arguments

Arguments are accessed using special variables: `$0`, `$1`, `$2`, and so on. The number represents the order in which the argument was provided.

*   **`$0`**: Represents the name of the script itself.
*   **`$1`, `$2`, `$3`, ...**: Represent the first, second, third, and subsequent arguments passed to the script.

**Example Script (`ping_with_arguments.sh`):**

```bash
#!/bin/bash
# This script will ping any address provided as an argument.

SCRIPT_NAME="${0}" # Stores the script's name
TARGET="${1}"      # Stores the first argument

echo "Running the script ${SCRIPT_NAME}"
echo "Pinging the target: ${TARGET}"
ping "${TARGET}"
```

**How to Run:**

```bash
chmod u+x ping_with_arguments.sh
./ping_with_arguments.sh nostarch.com
```

**Output:**

```
Running the script ping_with_arguments.sh...
Pinging the target: nostarch.com...
PING nostarch.com (104.20.120.46) 56(84) bytes of data.
64 bytes from 104.20.120.46 (104.20.120.46): icmp_seq=1 ttl=57 time=6.89 ms
... (ping output) ...
```

## Special Variables for All Arguments and Argument Count

*   **`$#`**: The total number of positional arguments passed to the script.
*   **`$*`**: All positional arguments, treated as a single string. When enclosed in double quotes (`"$*"`), it expands into a single word.
*   **`$@`**: All positional arguments, where each argument is individually quoted. When enclosed in double quotes (`"$@"`), it expands into separate words, which is generally preferred for iterating over arguments.

**Example Script (`show_args.sh`):**

```bash
#!/bin/bash
echo "The arguments are: $@"
echo "The total number of arguments is: $#"

echo "--- Iterating with \"$@\" ---"
for arg in "$@"; do
   echo "Individual arg: ${arg}"
done

echo "--- Iterating with \"$*\" ---"
for arg in "$*"; do
   echo "Single string arg: ${arg}"
done
```

**How to Run:**

```bash
chmod u+x show_args.sh
./show_args.sh "hello world" argument3
```

**Output:**

```
The arguments are: hello world argument3
The total number of arguments is: 2
--- Iterating with "$@" ---
Individual arg: hello world
Individual arg: argument3
--- Iterating with "$*" ---
Single string arg: hello world argument3
```
