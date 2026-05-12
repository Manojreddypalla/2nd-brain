# 06. File Handling

# [**#](https://www.codedex.io/intermediate-python/06-dear-diary#file-handling) File Handling**

**In data analysis, we can use Python to work with other files!**

**File input/output (I/O) handling lets us interact with external files for various purposes, including:**

- **✏️ Saving information to a file by writing to it.**
- **🔍 Reading from a file to use its information.**
- **🔧 Maintaining your computer's file system.**

**We will dive into the practical aspects of file handling, including common operations, and handling different formats like CSV. 🧑‍💻**

## [**#](https://www.codedex.io/intermediate-python/06-dear-diary#files-in-python) Files in Python**

**Note: Throughout this chapter, you will need a separate code editor like VS Code to run these examples.**

**The `open()` method is your gateway to handling files in Python!**

```python
# This opens example file for writing
file = open('example.txt', 'w')

```

**In this example, you specify the filename and how you want to handle it (e.g., reading or writing). The `open()` method can create a file or open a file depending on the conditions.**

**We can write to this example `file` object with the `.write()` method:**

```python
file.write('Hello, World! 🌎')

```

**If you run the code above, a new example.txt file with the text is created on your computer!**

## **Instructions**

**Diaries can contain anything and everything. Some have even won awards. 🤫 Regardless of what your writing aspirations may be, let’s make a diary. Your secret is safe with us (your local storage 🔐).**

**Let’s start writing!**

- **Use the `open()` method to create a new diaries.txt file.**
- **Fill your new diary with entries or secrets. 🤐**
- **Each entry should be separated by a new line.**