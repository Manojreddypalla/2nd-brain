# break and continue

Loops can run forever or until a condition is met. But we can also exit a 
loop at any point by using the `break` keyword. This keyword provides an 
alternative to the `exit` command, which would cause the entire script, not 
just the loop, to exit. Using `break`, we can leave the loop and advance to the 
next code block (Listing 2-22).

```bash
#!/bin/bash
while true; do
 echo "in the loop"
 break
done
echo "This code block will be reached."
```
*Listing 2-22: Breaking from a loop*

In this case, the last `echo` command will be executed.

The `continue` statement is used to jump to the next iteration of a loop. 
We can use it to skip a certain value in a sequence. To illustrate this, let’s 
create three empty files so we can iterate through them:

```
$ touch example_file1 example_file2 example_file3
```

Next, our `for` loop will write content to each file, excluding the first 
one, `example_file1`, which the loop will leave empty (Listing 2-23).

```bash
#!/bin/bash
for file in example_file*; do
 if [[ "${file}" == "example_file1" ]]; then
   echo "Skipping the first file"
   continue
 fi
 echo "${RANDOM}" > "${file}"
done
```
*Listing 2-23: Skipping an element in a for loop*

We start a `for` loop with the `example_file*` glob, which will expand to 
match the names of all files starting with `example_file` in the directory where 
the script runs. As a result, the loop should iterate over all three files we 
created earlier. Within the loop, we use string comparison to check whether 
the filename is equal to `example_file1` because we want to skip this file and 
not make any changes to it. If the condition is met, we use the `continue` 
statement to proceed to the next iteration, leaving the file unmodified. 
Later in the loop, we use the `echo` command with the environment variable 
`${RANDOM}` to generate a random number and write it into the file.

Save this script as `for_loop_continue.sh` and execute it in the same direc
tory as the three files:

```
$ chmod u+x for_loop_continue.sh
$ ./for_loop_continue.sh
Skipping the first file
```

If you examine the files, you should see that the first file is empty, while 
the other two contain a random number as a result of the script echoing 
the value of the `${RANDOM}` environment variable into them.