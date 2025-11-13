# Text Processing and Parsing

One of the most common things you’ll find yourself doing in bash is pro
cessing text. You can parse text on the command line by running one-off 
commands, or use a script to store parsed data in a variable that you can act 
on in some way. Both approaches are important to many scenarios.

To test the commands in this section on your own, download the sample 
logfile from https://github.com/dolevf/Black-Hat-Bash/blob/master/ch02/log.txt. 
This file is space-separated, and each segment represents a specific data 
type, such as the client’s source IP address, timestamp, HyperText Transfer 
Protocol (HTTP) method, HTTP path, HTTP User Agent field, HTTP sta
tus code, and more.

## Filtering with grep

The `grep` command is one of the most popular Linux commands out there 
today. We use `grep` to filter out information of interest from streams. At its 
most basic form, we can use it as shown in Listing 2-26.

```
$ grep "35.237.4.214" log.txt
```
*Listing 2-26: Filtering for a specific string from a file*

This `grep` command will read the file and extract any lines containing 
the IP address `35.237.4.214` from it.

We can even use `grep` for multiple patterns simultaneously. The follow
ing backslash pipe (`\|`) acts as an OR condition:

```
$ grep "35.237.4.214\|13.66.139.0" log.txt
```

Alternatively, we could use multiple `grep` patterns with the `-e` argument 
to accomplish the same thing:

```
$ grep -e "35.237.4.214" -e "13.66.139.0" log.txt
```

As you learned in Chapter 1, we can use the pipe (`|`) command to pro
vide one command’s output as the input to another. In the following exam
ple, we run the `ps` command and use `grep` to filter out a specific line. The `ps` 
command lists the processes on the system:

```
$ ps | grep TTY
```

By default, `grep` is case sensitive. We can make our search case insensi
tive by using the `-i` flag:

```
$ ps | grep -i tty
```

We can also use `grep` with the `-v` argument to exclude lines containing a 
certain pattern:

```
$ grep -v "35.237.4.214" log.txt
```

To print only the matched pattern, and not the entire line at which the 
matched pattern was found, use `-o`:

```
$ grep -o "35.237.4.214" log.txt
```

The command also supports regular expressions, anchoring, group
ing, and much more. Use the `man grep` command to read more about its 
capabilities.

## Filtering with awk

The `awk` command is a data processing and extraction Swiss Army knife. You 
can use it to identify and return specific fields from a file. To see how `awk` 
works, take another close look at our logfile. What if we need to print just 
the IP addresses from this file? This is easy to do with `awk`:

```
$ awk '{print $1}' log.txt
```

The `$1` represents the first field of every line in the file where the IP 
addresses are. By default, `awk` treats spaces or tabs as separators or delimiters.

Using the same syntax, we can print additional fields, such as the time
stamps. The following command filters the first three fields of every line in 
the file:

```
$ awk '{print $1,$2,$3}' log.txt
```

Using similar syntax, we can print the first and last field simultaneously. 
In this case, `NF` represents the last field:

```
$ awk '{print $1,$NF}' log.txt
```

We can also change the default delimiter. For example, if we had a file 
separated by commas (that is, a CSV, or comma-separated values file), rather 
than by spaces or tabs, we could pass `awk` the `-F` flag to specify the type 
of delimiter:

```
$ awk -F',' '{print $1}' example_csv.txt
```

We can even use `awk` to print the first 10 lines of a file. This emulates 
the behavior of the `head` Linux command; `NR` represents the total number of 
records and is built into `awk`:

```
$ awk 'NR < 10' log.txt
```

You’ll often find it useful to combine `grep` and `awk`. For example, you 
might want to first find the lines in a file containing the IP address 
`42.236.10.117` and then print the HTTP paths requested by this IP:

```
$ grep "42.236.10.117" log.txt | awk '{print $7}'
```

The `awk` command is a superpowerful tool, and we encourage you to dig 
deeper into its capabilities by running `man awk` for more information.

## Editing Streams with sed

The `sed` (stream editor) command takes actions on text. For example, it can 
replace the text in a file, modify the text in a command’s output, and even 
delete selected lines from files.

Let’s use `sed` to replace any mentions of the word `Mozilla` with the word 
`Godzilla` in the `log.txt` file. We use its `s` (substitution) command and `g` (global) 
command to make the substitution across the whole file, rather than to just 
the first occurrence:

```
$ sed 's/Mozilla/Godzilla/g' log.txt
```

This will output the modified version of the file but won’t change 
the original version. You can redirect the output to a new file to save 
your changes:

```
$ sed 's/Mozilla/Godzilla/g' log.txt > newlog.txt
```

We could also use `sed` to remove any whitespace from the file with the `/ //` 
syntax, which will replace whitespace with nothing, removing it from the 
output altogether:

```
$ sed 's/ //g' log.txt
```

If you need to delete lines of a file, use the `d` command. In the following 
command, `1d` deletes (`d`) the first line (`1`):

```
$ sed '1d' log.txt
```

To delete the last line of a file, use the dollar sign (`$`), which represents 
the last line, along with `d`:

```
$ sed '$d' log.txt
```

You can also delete multiple lines, such as lines 5 and 7:

```
$ sed '5,7d' log.txt
```

Finally, you can print (`p`) specific line ranges, such as lines 2 through 15:

```
$ sed -n '2,15 p' log.txt
```

When you pass `sed` the `-i` argument, it will make the changes to the file 
itself rather than create a modified copy:

```
$ sed -i '1d' log.txt
```

This rich utility can do a whole lot more. Use the `man sed` command to 
find additional ways to use `sed`.