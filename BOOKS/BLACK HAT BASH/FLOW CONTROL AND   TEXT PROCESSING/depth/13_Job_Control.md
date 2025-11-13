# Job Control

As you become proficient in bash, you’ll start to build complex scripts that 
take an hour to complete or must run continuously. Not all scripts need to 
execute in the foreground, blocking execution of other commands. Instead, 
you may want to run certain scripts as background jobs, either because they 
take a while to complete or because their runtime output isn’t interesting 
and you care about only the end result.

Commands that you run in a terminal occupy that terminal until the 
command is finished. These commands are considered foreground jobs. In 
Chapter 1, we used the ampersand character (`&`) to send a command to the 
background. This command then becomes a background job that allows us to 
unblock the execution of other commands.

## Managing the Background and Foreground

To practice working with background and foreground jobs, let’s run a com
mand directly in the terminal and send it to the background:

```
$ sleep 100 &
```

Notice that we can continue working on the terminal while this `sleep` 
command runs for 100 seconds. We can verify that the spawned process is 
running by using the `ps` command:

```
$ ps -ef | grep sleep
user    1827    1752 cons0    19:02:29 /usr/bin/sleep
```

Now that this job is in the background, we can use the `jobs` command 
to see what jobs are currently running:

```
$ jobs
[1]+  Running                 sleep 100 &
```

The output shows that the `sleep` command is in `Running` state and that its 
job ID is 1.

We can migrate the job from the background to the foreground by issu
ing the `fg` command and the job ID:

```
$ fg %1
sleep 100
```

At this point, the `sleep` command is occupying the terminal, since it’s 
running in the foreground. You can press `ctrl-Z` to suspend the process, 
which will produce the following output in the `jobs` table:

```
[1]+  Stopped                 sleep 100
```

To send this job to the background again in a running state, use the `bg` 
command with the job ID:

```
$ bg %1
[1]+ sleep 100 &
```

Here, we supply the job ID of 1.

## Keeping Jobs Running After Logout

Whether you send a job to the background or are running a job in the fore
ground, the process won’t survive if you close the terminal or log out. If you 
close the terminal, the process will receive a `SIGHUP` signal and terminate.

What if we want to keep running a script in the background even after 
we’ve logged out of the terminal window or closed it? To do so, we could start 
a script or command with the `nohup` (no hangup) command prepended:

```
$ nohup ./my_script.sh &
```

The `nohup` command will create a file named `nohup.out` with standard 
output stream data. Make sure you delete this file if you don’t want it on the 
filesystem.

There are additional ways to run background scripts, such as by plug
ging into system and service managers like `systemd`. These managers provide 
additional features, such as monitoring that the process is running, restart
ing it if it isn’t, and capturing failures. We encourage you to read more 
about `systemd` at https://man7.org/linux/man-pages/man1/init.1.html if you have 
such use cases.