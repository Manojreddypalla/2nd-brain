# Test Operators

Bash lets us selectively execute commands when certain conditions of inter
est are met. We can use test operators to craft a wide variety of conditions, 
such as whether one value equals another value, whether a file is of a cer
tain type, or whether one value is greater than another. We often rely on 
such tests to determine whether to continue running a block of code, so 
being able to construct them is fundamental to bash programming.

Bash has multiple kinds of test operators. File test operators allow us to 
perform tests against files on the filesystem, such as checking whether a file 
is executable or whether a certain directory exists. Table 2-1 shows a short 
list of the available tests.

**Table 2-1: File Test Operators**

| Operator | Description                               |
| :------- | :---------------------------------------- |
| -d       | Checks whether the file is a directory    |
| -r       | Checks whether the file is readable       |
| -x       | Checks whether the file is executable     |
| -w       | Checks whether the file is writable       |
| -f       | Checks whether the file is a regular file |
| -s       | Checks whether the file size is greater than zero |

You can find the full list of file test operators at https://ss64.com/bash/test.html or by running the `man test` command.

String comparison operators allow us to perform tests related to strings, 
such as testing whether one string is equal to another. Table 2-2 shows the 
string comparison operators.

**Table 2-2: String Comparison Operators**

| Operator | Description                                               |
| :------- | :-------------------------------------------------------- |
| =        | Checks whether a string is equal to another string        |
| ==       | Synonym of = when used within `[[ ]]` constructs         |
| !=       | Checks whether a string is not equal to another string    |
| <        | Checks whether a string comes before another string (in alphabetical order) |
| >        | Checks whether a string comes after another string (in alphabetical order) |
| -z       | Checks whether a string is null                           |
| -n       | Checks whether a string is not null                       |

Integer comparison operators allow us to perform checks on integers, such 
as whether an integer is less than or greater than another. Table 2-3 shows 
the available operators.

**Table 2-3: Integer Comparison Operators**

| Operator | Description                                               |
| :------- | :-------------------------------------------------------- |
| -eq      | Checks whether a number is equal to another number        |
| -ne      | Checks whether a number is not equal to another number    |
| -ge      | Checks whether a number is greater than or equal to another number |
| -gt      | Checks whether a number is greater than another number    |
| -lt      | Checks whether a number is less than another number       |
| -le      | Checks whether a number is less than or equal to another number |

Let’s use these operators in flow-control mechanisms to decide what 
code to run next.