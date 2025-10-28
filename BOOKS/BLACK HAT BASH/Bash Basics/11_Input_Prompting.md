# Input Prompting

While some Bash scripts take arguments during execution, others may need to interactively ask the user for information. The `read` command is used for this purpose, allowing the script to pause and wait for user input, which can then be assigned to variables.

## Using the `read` Command

The `read` command reads a line from standard input and splits it into fields. These fields are then assigned to one or more variables.

**Example Script (`input_prompting.sh`):**

```bash
#!/bin/bash
# Takes input from the user and assigns it to variables

echo "What is your first name?"
read -r firstname # Reads input into the 'firstname' variable

echo "What is your last name?"
read -r lastname  # Reads input into the 'lastname' variable

echo "Your first name is ${firstname} and your last name is ${lastname}"
```

*   The `-r` option to `read` prevents backslash escapes from being interpreted. This is generally a good practice to ensure that the input is treated literally.

**How to Run:**

```bash
chmod u+x input_prompting.sh
./input_prompting.sh
```

**Interactive Session:**

```
What is your first name?
John
What is your last name?
Doe
Your first name is John and your last name is Doe
```
