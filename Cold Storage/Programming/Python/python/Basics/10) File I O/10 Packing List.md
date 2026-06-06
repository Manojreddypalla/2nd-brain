# 10. Packing List

## [**#](https://www.codedex.io/intermediate-python/10-packing-list#congrats) Congrats!**

**Nice work on getting to the end of the chapter! 🔥**

**Let’s recap what we learned:**

- **File handling in Python, including how to read and write files.**
- **Using the the `.read()` , `.write()`, and `.seek()` methods to interact with file contents.**
- **Working with CSV file data using the `csv` module.**

**As a refresher, here is what file handling in Python looks like:**

```python
with open('filename.txt', 'r') as file:
  try:
    # Work with files here
  except Error:
    # Catch errors here

```

## **Instructions**

**Moving day is almost here! As you gather your belongings, the thought of packing everything can feel overwhelming. 😥**

**Using what we learned about files in Python, let’s make a program to help manage your packing list.**

**First, in a .py file, import the `csv` module. Then, paste the following:**

```python
data = [
  ['Item', 'Quantity'],
  ['Blender', 2],
  ['Posters', 30],
  ['Shoes', 2]
]

```

**We're gonna use this `data` to make a CSV file!**

**Next, create a `try`-`except` statement. For the `try` block:**

- **Try opening a `packing_list.csv` file in `'r'` "read" mode.**
- **Create a reader object for the CSV file.**
- **Loop through and `print()` each row in the object .**

**For the `except` block:**

- **Specify the `FileNotFoundError`.**
- **Print "Packing list file not found. Creating a new one."**
- **Open a new `packing_list.csv` file in `'w'` "write" mode.**
- **Create a writer object of the CSV file.**
- **Use the object's `.writerows()` method to process the `data` variable.**

**At this point, go ahead and save your .py file and run your program once.**

**Look in your file system and you should see a new CSV file in the same folder as this file.**

**Nice, you did it! Go ahead and add more things to your packing list and share with your friends to have them double check!**

**Happy packing! 📦**