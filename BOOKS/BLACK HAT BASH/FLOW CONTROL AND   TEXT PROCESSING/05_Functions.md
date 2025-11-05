# Functions

Functions help us reuse blocks of code so we can avoid repeating them. They 
allow us to run multiple commands and other bash code simultaneously 
by simply entering the function’s name. To define a new function, enter a 
name for it, followed by parentheses. Then place the code you would like 
the function to run within curly brackets (Listing 2-10).

```bash
#!/bin/bash
say_name(){
 echo "Black Hat Bash"
}
```
*Listing 2-10: Defining a function*

Here, we define a function called `say_name()` that executes a single `echo` 
command. To call a function, simply enter its name:

```bash
say_name
```

If the function is not called, the commands within it won’t run.

## Returning Values

Like commands and their exit statuses, functions can return values by 
using the `return` keyword. If there is no `return` statement, the function will 
return the exit code of the last command it ran. For example, the function 
in Listing 2-11 returns a different value based on whether the current user 
is root.

*check_root_function.sh*
```bash
#!/bin/bash
# This function checks if the current user ID equals zero.
check_if_root(){
if [[ "${EUID}" -eq "0" ]]; then
   return 0
 else
   return 1
 fi
}

if check_if_root; then
 echo "User is root!"
else
 echo "User is not root!"
fi
```
*Listing 2-11: An if condition to test whether a function returned true or false*

We define the `check_if_root()` function. Within this function, we use 
an `if` condition with an integer comparison test, accessing the environ
ment variable `EUID` to get the effective running user’s ID and checking 
whether it equals 0. If so, the user is root, and the function returns 0; if 
not, it returns 1. Next, we call the `check_if_root` function and check if it 
returned 0, which means the user is root. Otherwise, we print that the 
user is not root.

Bash scripts that perform privileged actions often check whether the 
user is root before attempting to install software, create users, delete groups, 
and so on. Attempting to perform privileged actions on Linux without 
the necessary privileges will result in errors, so this check helps handle 
these cases.

## Accepting Arguments

In Chapter 1, we covered the passing of arguments to commands on the 
command line. Functions can also take arguments by using the same syn
tax. For example, the function in Listing 2-12 prints the first three argu
ments it receives.

```bash
#!/bin/bash
print_args(){
 echo "first: ${1}, second: ${2}, third: ${3}"
}
print_args No Starch Press
```
*Listing 2-12: A function with arguments*

To call a function with arguments, enter its name and the arguments 
separated by spaces. Save this script as `function_with_args.sh` and run it:

```bash
$ chmod u+x function_with_args.sh
$ ./function_with_args.sh
first: No, second: Starch, third: Press
```

You should see output similar to that shown here.