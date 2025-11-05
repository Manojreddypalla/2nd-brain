# Linking Conditions

So far, we’ve used `if` to check whether a single condition is met. But as with 
most programming languages, we can also use the OR (`||`) and AND (`&&`) 
operators to check for multiple conditions at once.

For example, what if we want to check that a file exists and that its size 
is greater than zero? Listing 2-6 does so.

```bash
#!/bin/bash
echo "Hello World!" > file.txt
if [[ -f "file.txt" ]] && [[ -s "file.txt" ]]; then
 echo "The file exists and its size is greater than zero."
fi
```
*Listing 2-6: Using AND to chain two file test conditions*

This code writes content to a file, then checks whether that file exists 
and whether its size is greater than zero. Both conditions have to be met in 
order for the `echo` command to be executed. If either returns false, nothing 
will happen.

To demonstrate an OR condition, Listing 2-7 checks whether a variable 
is either a file or a directory.

```bash
#!/bin/bash
DIR_NAME="dir_test"
mkdir "${DIR_NAME}"
if [[ -f "${DIR_NAME}" ]] || [[ -d "${DIR_NAME}" ]]; then
 echo "${DIR_NAME} is either a file or a directory."
fi
```
*Listing 2-7: Chaining two file test conditions by using OR*

This code first creates a directory, then uses an `if` condition with the 
OR (`||`) operator to check whether the variable is a file (`-f`) or a directory 
(`-d`). The second condition should evaluate to true, and the `echo` command 
should execute.