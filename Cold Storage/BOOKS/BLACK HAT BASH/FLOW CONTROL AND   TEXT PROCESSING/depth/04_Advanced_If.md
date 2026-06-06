# Testing Command Success

We can even test the exit code of commands to determine whether they 
were successful (Listing 2-8).

```bash
if command; then
 # command was successful.
fi

if ! command; then
 # command was unsuccessful.
fi
```
*Listing 2-8: Executing commands based on exit code values*

You’ll often find yourself using this technique in bash, as commands 
aren’t guaranteed to succeed. Failures could happen for reasons such 
as these:

*   A lack of the necessary permissions when creating resources
*   An attempt to execute a command that is not available on the operat
ing system
*   The disk being full when downloading a file
*   The network being down while executing network utilities

To see how this technique works, execute the following in your terminal:

```bash
$ if touch test123; then
   echo "OK: file created"
 fi
OK: file created
```

We attempt to create a file. Because the file creation succeeds, we print 
a message to indicate this.

# Checking Subsequent Conditions

If the first `if` condition fails, you can check for other conditions by using 
the `elif` keyword (short for `else if`). To show how this works, let’s write a 
program that checks the arguments passed to it on the command line. 
Listing 2-9 will output a message clarifying whether the argument is a file 
or a directory.

*if_elif.sh*
```bash
#!/bin/bash
USER_INPUT="${1}"
if [[ -z "${USER_INPUT}" ]]; then
 echo "You must provide an argument!"
 exit 1
fi
if [[ -f "${USER_INPUT}" ]]; then
 echo "${USER_INPUT} is a file."
elif [[ -d "${USER_INPUT}" ]]; then
 echo "${USER_INPUT} is a directory."
else
 echo "${USER_INPUT} is not a file or a directory."
fi
```
*Listing 2-9: Using if and elif statements*

We begin with an `if` statement that checks whether the variable `USER`
`_INPUT` is null. This allows us to exit the script early by using `exit 1` if we 
receive no command line arguments from the user. We then begin a second 
`if` condition that uses the file test operator to check whether the input is 
a file. Below this condition, we use `elif` to test whether the argument 
is a directory. This condition won’t be tested unless the file test fails. If 
neither of these conditions is true, the script responds that the argument is 
neither a file nor a directory.