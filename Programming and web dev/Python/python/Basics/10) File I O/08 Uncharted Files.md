# 08. Uncharted Files

# [**#](https://www.codedex.io/intermediate-python/08-uncharted-files#file-operations) File Operations**

**Next, we're going to explore the world of file operations! 🚀**

**There are various ways to navigate, modify, and manage files in Python.**

**It's essential to understand the key file operations when handling files. 🔑**

**Enter the `try`-`finally` statement:**

```python
try:
  file = open('example.txt', 'r')
  # Perform operations on the file
finally:
  file.close()  # Ensures file close even if an exception occurs

```

**The `try`-`finally` statement is made up of two blocks:**

- **The `try` block with the code that we attempt to run.**
- **The `finally` block that always closes the file to finish the statement.**

**Python also allows us to interact with files through operations by moving the file cursor and truncating files.**

```python
file_path = 'example.txt'

try:
  file = open(file_path, 'r')
  # Perform operations on the file
finally:
  file.close()
# Reading from a file using read() method
with open('example.txt', 'r') as file:
  content = file.read()
  print(content)

```

## [**#](https://www.codedex.io/intermediate-python/08-uncharted-files#file-cursor) File Cursor**

**The file cursor is necessary for reading or modifying specific parts of the file. Methods like `.seek()` move the cursor to a desired location within the file.**

```python
# Moving the file cursor using seek()
with open('example.txt', 'r') as file:
  file.seek(10)  # Move to the 10th byte
  content = file.read()

```

**The `.truncate()` method allows us to modify file size. This is useful when we want to remove or reset content beyond a certain point. We can trim or extend the file size as needed.**

```python
# Truncating a file
with open('example.txt', 'a') as file:
  file.truncate(20)  # Limit the file size to 20 bytes

```

## **Instructions**

**We’ve all sent a message we knew we shouldn’t. 🫣**

**Thankfully, technology is here to save the day! 🌟**

**Let's use `.seek()` and `.truncate()` to simulate unsending a message within a text file.**

**First, create a new `sent_message.txt` file and write a secret message to it:**

```python
sent_message = 'Hey there! This is a secret message.'

with open('sent_message.txt', 'w') as file:
  file.write(sent_message)

```

**Read the message and use `.seek()` to move the cursor to the beginning (0):**

```python
with open('sent_message.txt', 'r+') as file:
  # Read the sent message from the file
  original_message = file.read()

  # Move the cursor to the beginning of the file
  file.seek(0)

```

**Note: `r+` in the `open()` function is used to indicate both reading and writing are allowed in the file.**

```python
# Modify the message to simulate unsending
unsent_message = 'This message has been unsent.'

```

- **Use `.truncate()` to reset the content to the length of the unsend message.**
- **Save the new message to the file.**
- **Print your messages!**

**Note: `seek(0)` is used to move the cursor to the beginning of the file before modifying the message.**

**Then, `truncate()` is used to adjust the file size to match the length of the new message. This simulates the act of unsending the message within the file.**

### **Help**

When you use **`.truncate(n)`**, Python **cuts the file after `n` bytes**, and the rest of the data is **lost permanently**.

### Example of data loss:

```python
with open("data.txt", "w+") as f:
    f.write("Python is awesome!")

    # Keep only first 6 characters
    f.truncate(6)

    f.seek(0)
    print(f.read())   # Output: Python

```

Here:

- Original content = `Python is awesome!`
- After `.truncate(6)` → only `"Python"` remains. The `" is awesome!"` part is gone forever.

⚠️ So `.truncate()` is **destructive** (like cutting a rope — you can’t get back what’s chopped off).

👉 If you want to keep original data **safe**, you should first make a copy before truncating.

Do you want me to also show you a **safe way** to shorten or overwrite a file **without losing the original**?