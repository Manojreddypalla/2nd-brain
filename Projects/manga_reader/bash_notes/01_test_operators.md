# 1. Test Operators

Bash provides operators to test various conditions.

### File Test Operators

Used to perform tests against files on the filesystem.

| Operator | Description                               |
| :------- | :---------------------------------------- |
| `-d`     | Checks whether the file is a directory    |
| `-r`     | Checks whether the file is readable       |
| `-x`     | Checks whether the file is executable     |
| `-w`     | Checks whether the file is writable       |
| `-f`     | Checks whether the file is a regular file |
| `-s`     | Checks whether the file size is > 0       |

**Examples:**

```bash
# Check if /etc/passwd is a regular file
if [[ -f "/etc/passwd" ]]; then
    echo "/etc/passwd is a regular file."
fi

# Check if /etc is a directory
if [[ -d "/etc" ]]; then
    echo "/etc is a directory."
fi

# Check if a file is not empty
if [[ -s "/etc/passwd" ]]; then
    echo "/etc/passwd is not empty."
fi
```

### String Comparison Operators

Used for tests related to strings.

| Operator | Description                                       |
| :------- | :------------------------------------------------ |
| `=`      | Checks for string equality                        |
| `==`     | Synonym of `=` within `[[ ]]`                      |
| `!=`     | Checks for string inequality                      |
| `<`      | Checks if a string comes before another (alpha)   |
| `>`      | Checks if a string comes after another (alpha)    |
| `-z`     | Checks if a string is null (empty)                |
| `-n`     | Checks if a string is not null                    |

**Examples:**

```bash
# Check if two strings are equal
str1="hello"
str2="world"
if [[ "$str1" == "$str2" ]]; then
    echo "Strings are equal."
else
    echo "Strings are not equal."
fi

# Check if a string is empty
str3=""
if [[ -z "$str3" ]]; then
    echo "String is empty."
fi
```

### Integer Comparison Operators

Used for checks on integers.

| Operator | Description                               |
| :------- | :---------------------------------------- |
| `-eq`    | Equal to                                  |
| `-ne`    | Not equal to                              |
| `-ge`    | Greater than or equal to                  |
| `-gt`    | Greater than                              |
| `-lt`    | Less than                                 |
| `-le`    | Less than or equal to                     |

**Examples:**

```bash
# Check if two numbers are equal
num1=10
num2=20
if [[ "$num1" -eq "$num2" ]]; then
    echo "Numbers are equal."
else
    echo "Numbers are not equal."
fi

# Check if a number is greater than another
if [[ "$num2" -gt "$num1" ]]; then
    echo "$num2 is greater than $num1."
fi
```
