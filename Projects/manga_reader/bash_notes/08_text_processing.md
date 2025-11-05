# 8. Text Processing and Parsing

Common tools for manipulating text on the command line.

### `grep` (Global Regular Expression Print)

Filters input line by line based on a pattern.

-   `grep "pattern" file.txt`: Find lines containing "pattern".
-   `grep -i "pattern"`: Case-insensitive search.
-   `grep -v "pattern"`: Invert match (show lines that *don't* contain the pattern).
-   `grep -o "pattern"`: Print only the matched part of the lines.
-   `grep -e "pattern1" -e "pattern2"`: Search for multiple patterns (OR).

**Example: Find all IP addresses in a log file**

```bash
# The -E flag enables extended regular expressions
grep -E -o "(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)" /var/log/auth.log
```

### `awk`

A powerful pattern-scanning and processing language. Excellent for column-based data.

-   `awk '{print $1}' file.txt`: Prints the first field (column) of each line. Fields are space-delimited by default.
-   `awk '{print $1, $NF}' file.txt`: Prints the first and the last field (`NF` is the number of fields).
-   `awk -F',' '{print $1}' file.csv`: Specifies a comma as the field separator.
-   `awk 'NR < 10' file.txt`: Prints the first 9 lines (`NR` is the record/line number).

**Example: Calculate the total size of files**

```bash
ls -l | awk '{total += $5} END {print total}'
```

### `sed` (Stream Editor)

Performs text transformations on an input stream.

-   `sed 's/old/new/g' file.txt`: Substitutes all (`g`) occurrences of "old" with "new". Without `g`, it only replaces the first occurrence on each line.
-   `sed '5,7d' file.txt`: Deletes lines 5 through 7.
-   `sed -n '2,15p' file.txt`: Prints only lines 2 through 15 (`-n` suppresses default output).
-   `sed -i 's/old/new/g' file.txt`: Edits the file in-place (`-i`). **Use with caution.**

**Example: Comment out a line in a config file**

```bash
# This will comment out the line containing 'some_setting=true'
sed -i '/some_setting=true/s/^/#/' /etc/myconfig.conf
```
