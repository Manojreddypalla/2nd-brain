# while

In bash, `while` loops allow you to run a code block until a test returns a suc
cessful exit status code. You might use them in penetration testing to con
tinuously perform a port scan on a network and pick up any new hosts that 
join the network, for example.

Listing 2-13 shows the syntax of a `while` loop.

```bash
while some_condition; do
 # Run commands while the condition is true.
done
```
*Listing 2-13: A while loop*

This loop starts with the keyword `while`, followed by an expression that 
describes the condition. We then surround the code to be executed with 
the `do` and `done` keywords, which define the start and end of the code block.

You can use `while` loops to run a chunk of code infinitely by using `true` 
as the condition; because `true` always returns a successful exit code, the 
code will always run. Let’s use a `while` loop to repeatedly print a command 
to the screen. Save Listing 2-14 to a file named `basic_while.sh` and run it.

```bash
#!/bin/bash
while true; do
 echo "Looping..."
 sleep 2
done
```
*Listing 2-14: Repeatedly running a command at two-second intervals*

You should see the following output:

```
$ chmod u+x basic_while.sh
$ ./basic_while.sh
Looping...
Looping...
--snip-
```

Next, let’s write a more sophisticated `while` loop that runs until it finds 
a specific file on the filesystem (Listing 2-15). Use `ctrl-C` to stop the code 
from executing at any point.

*while_loop.sh*
```bash
#!/bin/bash
SIGNAL_TO_STOP_FILE="stoploop"
while [[ ! -f "${SIGNAL_TO_STOP_FILE}" ]]; do
 echo "The file ${SIGNAL_TO_STOP_FILE} does not yet exist..."
 echo "Checking again in 2 seconds..."
 sleep 2
done
echo "File was found! Exiting..."
```
*Listing 2-15: File monitoring*

At 1, we define a variable representing the name of the file for which 
the `while` loop 2 checks, using a file test operator. The loop won’t exit until 
the condition is satisfied. Once the file is available, the loop will stop, and the 
script will continue to the `echo` command 3. Save this file as `while_loop.sh` 
and run it:

```
$ chmod u+x while_loop.sh
$ ./while_loop.sh
The file stoploop does not yet exist...
Checking again in 2 seconds...
--snip-
```

While the script is running, open a second terminal in the same direc
tory as the script and create the `stoploop` file:

```
$ touch stoploop
```

Once you’ve done so, you should see the script break out of the loop 
and print the following:

```
File was found! Exiting...
```

We can use `while` loops to monitor for filesystem events, such as file cre
ations or deletions, or when a process starts. This may come in handy if an 
application is suffering from a vulnerability we can only temporarily abuse. 
For example, consider an application that runs daily at a particular hour 
and checks whether the file `/tmp/update.sh` exists; if it does, the application 
executes it as the root user. Using a `while` loop, we can monitor when that 
application has started and then create the file just in time so our com
mands are executed by that application.