# if Conditions

In bash, we can use an `if` condition to execute code only when a certain 
condition is met. Listing 2-1 shows its syntax.

```bash
if [[ condition ]]; then
 # Do something if the condition is met.
else
 # Do something if the condition is not met.
fi
```
*Listing 2-1: The structure of an if statement*

We start with the `if` keyword, followed by a test condition between 
double square brackets (`[[ ]]`). We then use the `;` character to separate the 
`if` keyword from the `then` keyword, which allows us to introduce a block of 
code that runs only if the condition is met.

Next, we use the `else` keyword to introduce a fallback code block that 
runs if the condition is not met. Note that `else` is optional, and you may not 
always need it. Finally, we close the `if` condition with the `fi` keyword (which 
is `if` inversed).

> **NOTE** In some operating systems, such as those often used in containers, the default shell 
might not necessarily be bash. To account for these cases, you may want to use single 
square brackets (`[...]`) rather than double to enclose your condition. This use of sin
gle brackets meets the Portable Operating System Interface standard and should work 
on almost any Unix derivative, including Linux.

Let’s see an `if` condition in practice. Listing 2-2 uses an `if` condition to 
test whether a file exists and, if not, creates it.

*test_if_file_exists.sh*
```bash
#!/bin/bash
FILENAME="flow_control_with_if.txt"
if [[ -f "${FILENAME}" ]]; then
 echo "${FILENAME} already exists."
 exit 1
else
 touch "${FILENAME}"
fi
```
*Listing 2-2: An if condition to test for the existence of a file*

We first create a variable named `FILENAME` containing the name of the 
file we need. This saves us from having to repeat the filename in the code. 
We then introduce the `if` statement, which includes a condition that uses 
the `-f` file test operator to test for the existence of the file. If this condition 
is true, we use `echo` to print to the screen a message explaining that the file 
already exists and then use the status code 1 (failure) to exit the program. 
In the `else` block, which will execute only if the file does not exist, we create 
the file by using the `touch` command.

> **NOTE** You can download this chapter’s scripts from https://github.com/dolevf/Black-Hat-Bash/blob/master/ch02.

Save the file and execute it. You should see the `flow_control_with_if.txt` 
file in your current directory when you run `ls`.

Listing 2-3 shows a different way of achieving the same outcome: it uses 
the NOT operator (`!`) to check whether a directory doesn’t exist and, if it 
doesn’t, creates it. This example has fewer lines of code and eliminates the 
need for an `else` block altogether.

```bash
#!/bin/bash
FILENAME="flow_control_with_if.txt"
if [[ ! -f "${FILENAME}" ]]; then
 touch "${FILENAME}"
fi
```
*Listing 2-3: Using a negative check to test file existence*

Let’s explore `if` conditions that use some of the other kinds of test 
operators we’ve covered. Listing 2-4 shows a string comparison test. It tests 
whether two variables are equal by performing string comparison with the 
equal-to operator (`==`).

*string_comparison.sh*
```bash
#!/bin/bash
VARIABLE_ONE="nostarch"
VARIABLE_TWO="nostarch"
if [[ "${VARIABLE_ONE}" == "${VARIABLE_TWO}" ]]; then
 echo "They are equal!"
else
 echo "They are not equal!"
fi
```
*Listing 2-4: Comparing two string variables*

The script will compare the two variables, both of which have the value 
`nostarch`, and print `They are equal!` by using the `echo` command.

Next is an integer comparison test, which takes two integers and checks 
which one is the larger number (Listing 2-5).

*integer_comparison.sh*
```bash
#!/bin/bash
VARIABLE_ONE="10"
VARIABLE_TWO="20"
if [[ "${VARIABLE_ONE}" -gt "${VARIABLE_TWO}" ]]; then
 echo "${VARIABLE_ONE} is greater than ${VARIABLE_TWO}."
else
 echo "${VARIABLE_ONE} is less than ${VARIABLE_TWO}."
fi
```
*Listing 2-5: Comparing integers*

We create two variables, `VARIABLE_ONE` and `VARIABLE_TWO`, and assign them 
values of 10 and 20, respectively. We then use the `-gt` operator to compare 
the two values and print the result based on an integer comparison.