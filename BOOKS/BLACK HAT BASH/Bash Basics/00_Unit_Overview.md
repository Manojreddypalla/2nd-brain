# Chapter 1: Bash Basics - Unit Overview

This unit provides a foundational understanding of Bash scripting, essential for penetration testers and security practitioners. It covers setting up your Bash development environment, exploring the shell's functionalities, and diving deep into the core syntax elements required to write effective scripts.

## Key Topics Covered:

*   **Environmental Setup:** Learn how to access the Bash shell on various operating systems (Linux, macOS, Windows via WSL/Cygwin) and set up a suitable text editor for script development.
*   **Exploring the Shell:** Understand how to verify your Bash version, inspect environment variables that define your shell's context, and execute fundamental Linux commands with various argument syntaxes.
*   **Elements of a Bash Script:** Discover the basic building blocks of a script, including the crucial shebang line, how to use comments for documentation, structuring commands, executing scripts, and essential debugging techniques.
*   **Variables:** Grasp the concept of variables in Bash, their naming conventions, how to assign and access values (including command substitution), unassign them, and differentiate between global and local variable scopes.
*   **Arithmetic Operators:** Explore how to perform mathematical operations on integers using different methods like the `let` command, double parentheses `$(())` syntax, and the `expr` command.
*   **Arrays:** Learn to create and manipulate single-dimension arrays, including adding, accessing, deleting, and modifying elements, which are useful for handling collections of data.
*   **Streams:** Understand the three standard data streams (Standard Input, Standard Output, Standard Error) and their file descriptor numbers, which are fundamental for controlling data flow.
*   **Control Operators:** Familiarize yourself with operators like `&`, `&&`, `()`, `;`, `|`, and `||` that control the sequence and conditions of command execution within scripts.
*   **Redirection Operators:** Master how to redirect standard streams using operators like `>`, `>>`, `&>`, `&>>`, `<`, `<<`, and `|` to manage input/output to and from files and other commands.
*   **Positional Arguments:** Learn to make your scripts dynamic by accepting command-line arguments, understanding special variables like `$0`, `$#`, `$@`, and `$*` for accessing script name, argument count, and individual arguments.
*   **Input Prompting:** Discover how to create interactive scripts using the `read` command to prompt users for input and store their responses in variables.
*   **Exit Codes:** Understand the importance of exit codes (0 for success, non-zero for failure) for determining command success and controlling script flow, including how to check and set them.

This unit lays the groundwork for writing more complex and powerful Bash scripts for various tasks, particularly in offensive security engagements.
