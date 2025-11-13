# Bash Customizations for Penetration Testers

As penetration testers, we often follow standard workflows for all ethical 
hacking engagements, whether they are consulting work, bug bounty hunt
ing, or red teaming. We can optimize some of this work with a few bash tips 
and tricks.

## Placing Scripts in Searchable Paths

Bash searches for programs within directories defined by the `PATH` environ
ment variable. Commands such as `ls` are always available to you because sys
tem and user binaries are located in directories that are part of the `PATH`.

To see your `PATH`, run this command:

```
$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

The output might look different, depending on your operating system.

When you write a bash script, place it in a directory such as `/usr/local/
bin`, which, as you can see, is part of the `PATH`. If you don’t do this, you have a 
few other options available:

*   Call the script directly, using the full path.
*   Change the directory to the one in which your script lives and execute 
it from there.
*   Use aliases (shown in the next section).
*   Add paths to the `PATH` environment variable.

The benefit of placing the script in a searchable path is that you can 
simply call it by its name. You don’t have to provide the full path or have the 
terminal be in the same directory.

## Shortening Commands with Aliases

When you find yourself frequently using a long Linux command, you can 
use an `alias` to map the command to a shorter custom name that will save 
you time when you need to run it.

For example, imagine that you often use Nmap (discussed in Chapter 4) 
with special parameters to scan for all 65,535 ports on a given IP address:

```
nmap -vv -T4 -p- -sV --max-retries 5 localhost
```

This command is quite hard to remember. With aliases, we can make it 
more accessible on the command line or to our scripts. Here, we assign the 
command to the alias `quicknmap`:

```
$ alias quicknmap="nmap -vv -T4 -p- -sV --max-retries 5 localhost"
```

Now we can run the aliased command by using the name of the alias:

```
$ quicknmap
Starting Nmap ( https://nmap.org ) at 02-21 22:32 EST
--snip-
PORT    STATE SERVICE
631/tcp open  ipp
```

You can even assign an alias to your own scripts:

```
$ alias helloworld="bash ~/scripts/helloworld.sh"
```

Aliases aren’t permanent, but they can be. In the next section, you’ll 
learn how to use bash profiles to make permanent changes to your shell.

## Customizing the ~/.bashrc Profile

We can use the `~/.bashrc` file to load functions, variables, and just about any 
other custom bash code we desire into a new bash session. For example, we 
can create variables containing information we’ll frequently need to access, 
such as the IP address of a vulnerable host we’re testing.

We could append the following to the end of the `~/.bashrc` file, for 
instance. These lines define a few custom variables and save our aliased 
Nmap command:

```
VULN_HOST=1.0.0.22
VULN_ROUTER=10.0.0.254
alias quicknmap="nmap -vv -T4 -p- -sV --max-retries 5 localhost"
```

The next time you open a terminal, you’ll be able to access these values. 
Make these new values available immediately by using the `source` command 
to reimport the `~/.bashrc` file:

```
$ source ~/.bashrc
$ echo ${VULN_HOST}
1.0.0.22
$ echo ${VULN_ROUTER}
10.0.0.254
```

Now you can use these variables even after you close the terminal and 
start a new session.

## Importing Custom Scripts

Another way to introduce changes to your bash session is to create a dedi
cated script that contains pentesting-related customizations and then have 
the `~/.bashrc` file import it by using the `source` command. To achieve this, cre
ate a `~/.pentest.sh` file containing your new logic and then make a one-time 
modification to `~/.bashrc` to import `pentest.sh` at the end of the file:

```
source ~/.pentest.sh
```

Note that you can also source a bash file by using the `. ` (dot) command:

```
. ~/.pentest.sh
```

This command provides an alternative to `source`.

## Capturing Terminal Session Activity

Penetration testing often involves having dozens of terminals open simulta
neously, all running many tools that can produce a lot of output. When we 
find something of interest, we may need some of that output as evidence for 
later. To avoid losing track of an important piece of information, we can use 
some clever bash.

The `script` command allows us to capture terminal session activity. One 
approach is to load a small bash script that uses `script` to save every session 
to a file for later inspection. The script might look like Listing 2-27.

```bash
#!/bin/bash
FILENAME=$(date +%m_%d_%Y_%H:%M:%S).log
if [[ ! - d ~/sessions ]]; then
 mkdir ~/sessions
fi
# Starting a script session
if [[ - z $SCRIPT ]]; then
 export SCRIPT="/home/kali/sessions/${FILENAME}"
 script - q - f "${SCRIPT}"
fi
```
*Listing 2-27: Saving terminal activity to a file*

Having `~/.bashrc` load this script, as shown earlier, will result in the cre
ation of the `~/sessions` directory, containing each terminal session capture in 
a separate file. The recording stops when you enter `exit` in the terminal or 
close the terminal window.