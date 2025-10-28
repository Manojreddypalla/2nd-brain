# Arithmetic Operators

Bash allows you to perform mathematical operations on integers.

## Available Operators

| Operator | Description           |
| :------- | :-------------------- |
| `+`      | Addition              |
| `-`      | Subtraction           |
| `*`      | Multiplication        |
| `/`      | Division              |
| `%`      | Modulo (remainder)    |
| `+=`     | Increment by a constant |
| `-=`     | Decrement by a constant |

## Methods for Performing Arithmetic Operations

### 1. Using the `let` command

The `let` command takes a variable name and performs an arithmetic calculation to resolve its value.

```bash
let result="4 * 5"
echo ${result}
# Output: 20
```

### 2. Using Double Parentheses Syntax `((expression))`

This is a common and often preferred method for arithmetic operations in Bash.

```bash
result=$((5 * 5))
echo ${result}
# Output: 25
```

### 3. Using the `expr` command

The `expr` command evaluates expressions, which can include arithmetic operations. It's also used for string manipulations.

```bash
result=$(expr 5 + 505)
echo ${result}
# Output: 510
```
*Note: When using `expr`, operators and numbers often need to be separated by spaces.*
