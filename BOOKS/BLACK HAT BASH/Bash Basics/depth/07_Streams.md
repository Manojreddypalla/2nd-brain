# Streams

Streams are communication channels between a program and its environment, represented by file descriptors. Bash has three standard data streams:

## Standard Data Streams

| Stream Name        | Description                       | File Descriptor Number |
| :----------------- | :-------------------------------- | :--------------------- |
| Standard Input     | Data coming into a program as input | `0`                    |
| Standard Output    | Data coming out of a program      | `1`                    |
| Standard Error     | Errors coming out of a program    | `2`                    |

## Illustrating Streams

When you run commands or scripts, output is typically sent to `stdout` (your terminal screen). Error messages are sent to `stderr`.

**Example:**

```bash
# This command will create directory1 and directory2.
# The third attempt to create directory1 will fail, sending an error to stderr.
mkdir directory1 directory2 directory1
# Output to stderr: mkdir: cannot create directory 'directory1': File exists

# This command will list the created directories, sending its output to stdout.
ls -l
# Output to stdout:
# total 1
# drwxr-xr-x 1 user user   0 Feb 17 09:45 directory1
# drwxr-xr-x 1 user user   0 Feb 17 09:45 directory2
```

You will learn how to redirect these streams in the "Redirection Operators" section.
