# until

Whereas `while` runs so long as the condition succeeds, `until` runs so long as 
it fails. Listing 2-16 shows the `until` loop syntax.

```bash
until some_condition; do
 # Run some commands until the condition is no longer false.
done
```
*Listing 2-16: An until loop*

Listing 2-17 uses `until` to run some commands until a file’s size is 
greater than zero (meaning it is not empty).

*until_loop.sh*
```bash
#!/bin/bash
FILE="output.txt"
touch "${FILE}"
until [[ -s "${FILE}" ]]; do
 echo "${FILE} is empty..."
 echo "Checking again in 2 seconds..."
 sleep 2
done
echo "${FILE} appears to have some content in it!"
```
*Listing 2-17: Checking a file’s size*

We first create an empty file, then begin a loop that runs until the file is 
no longer empty. Within the loop, we print messages to the terminal. Save 
this file as `until_loop.sh` and run it:

```
$ chmod u+x until_loop.sh
$ ./until_loop.sh
output.txt is empty...
Checking again in 2 seconds...
--snip-
```

At this point, the script has created the file `output.txt`, but it’s an empty 
file. We can check this by using the `du` (disk usage) command:

```
$ du -sb output.txt
0       output.txt
```

Open another terminal and navigate to the location at which your script 
is saved, then append some content to the file so its size is no longer zero:

```
$ echo "until_loop_will_now_stop!" > output.txt
```

The script should exit the loop, and you should see it print the following:

```
output.txt appears to have some content in it!
```