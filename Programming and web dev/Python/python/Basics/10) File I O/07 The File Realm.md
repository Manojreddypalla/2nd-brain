# 07. The File Realm

# [**#](https://www.codedex.io/intermediate-python/07-the-file-realm#opening-files) Opening Files**

**When handling a file, you have to open it first. 🔑**

**In Python, we use the `open()` function:**

```python
file = open(file_path, mode)

```

**This function returns an object that we can save to a variable like `file`.**

**The `open()` function takes two arguments:**

- **The `file_path`, such as `'filename.txt'`.**
- **The `mode` for how to use the file.**

**There are three modes you can open a file with:**

- **`'r'` for reading a file. 📖**
- **`'w'` for writing to a file. ✍🏼**
- **`'a'` for writing from the end of a file. 📝**

**Note: Be careful with `'w'` mode. It will overwrite any existing file content. Use `'a'` to avoid this.**

## [**#](https://www.codedex.io/intermediate-python/07-the-file-realm#writing-to-files) Writing to Files**

**You can write to a file with various methods. The `write()` method simply writes data to a file:**

```python
file.write('text')

```

**A string is passed to the `write()` method, and written to the file.**

**To write on multiple lines, this method must be used multiple times with the `\n` newline character:**

```python
file.write('This is a line.\n')
file.write('This is the next line.\n')

```

**You can also write multiple lines to a file at once with the `writelines()` method:**

```python
lines = ['This is a line.\n', 'This is the next line.\n']

file.writelines(lines)

```

**The `writelines()` method takes a list of lines and writes them to the file.**

## [**#](https://www.codedex.io/intermediate-python/07-the-file-realm#reading-files) Reading Files**

**To read from files, you have options as well.**

**The `.read()` method lets you read the entire content of a file. This method can be saved to a variable and printed to the terminal:**

```python
file = open('filename.txt', 'r')

content = file.read()

print('Using read():')
print(content)

```

**The `.readline()` method lets you read a file one line at a time:**

```python
line1 = file.readline()  # Read the first line
print(line1, end='')     # Printing the first line

line2 = file.readline()  # Read the second line
print(line2, end='')     # Printing the second line

```

**To print each line on a single line without adding a `'\n'` newline character at the end, we use the `end=''` option in the `print()` function.**

**Note: By default, `print()` ends with a `'\n'` newline character.**

**The `.readlines()` method lets you read all lines in a list:**

```python
lines = file.readlines()

for line in lines:
  print(line, end='')

```

## [**#](https://www.codedex.io/intermediate-python/07-the-file-realm#closing-files) Closing Files**

**In Python, the `.close()` method is used to finish working with a file and free up resources.**

```python
# Opens file to read
file = open('filename.txt', 'r')

# Closes file
file.close()

```

**Always call `.close()` after reading or writing to the file to ensure everything is saved.**

**You can also use a `with` block to open a file, handle it, and close it automatically:**

```python
with open('filename', 'r') as f:
  # handle file here

```

**No `.close()` method necessary!**

## **Instructions**

**✨ Here at Codédex, we are 🤩 big fans of music 🎹. The only thing better than a perfect playlist is a playlist with all your favorite songs. Let's create a Python script that organizes the songs into a .txt file. Let's make a playlist!**

**First, let's define a dictionary to store liked songs:**

```python
liked_songs = {
  'title': 'artist'
}

```

**Next, create a function to display and write liked songs to a file:**

```python
def write_liked_songs_to_file(liked_songs, file_name):

```

- **Open the file in write mode.**
- **Write each song and artist by iterating through the `liked_songs` dictionary.**

**Your liked_songs.txt file should look something like this:**

```
Liked Songs:
Bad Habits by Ed Sheeran
I'm Just Ken by Ryan Gosling
Mastermind by Taylor Swift
Uptown Funk by Mark Ronson ft. Bruno Mars
Ghost by Justin Bieber
```