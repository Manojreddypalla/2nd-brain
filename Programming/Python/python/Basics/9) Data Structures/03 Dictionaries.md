# 03. Dictionaries

# [**#](https://www.codedex.io/intermediate-python/03-met-museum#dictionaries) Dictionaries**

**Let's explore another data structure in Python: dictionaries!**

**Dictionaries are unordered collections that allow you to store and access data with `key`:`value` pairs.**

```
# single line
dictionary = {key: value}

# multi-line
dictionary = {
  key1: value1,
  key2: value2,
  key3: value3
}

```

**The `key`:`value` pairs are comma-separated, surrounded by curly braces `{` `}`.**

**With lists and tuples, we access their items by index. But with dictionaries, we access their items by key. 🔑**

```python
laptop = {
  'brand': 'Apple',
  'model': 'Macbook Pro',
  'size': 14,
  'year': 2023
}

```

**Each key in a dictionary must be unique and associated with a specific value. This pairing allows for efficient retrieval of data. Keys must also be immutable, such as a string.**

**To access the value associated with a specific key in a dictionary, you use `[` `]` brackets with the key inside.**

```python
print(laptop['model'])
# Output: Macbook Pro

```

**Consider a scenario where you are managing a database of artifacts or people, each with unique information.**

**Let's define a dictionary!**

```python
# Creating a dictionary
student_info = {'name': 'Alice', 'age': 9, 'grade': 'A'}

# Accessing dictionary elements
print('Name:', student_info['name'])
print('Age:', student_info['age'])
print('Grade:', student_info['grade'])

# Modifying dictionary elements
student_info['age'] = 10
print('Updated Age:', student_info['age'])

```

## [**#](https://www.codedex.io/intermediate-python/03-met-museum#dictionary-methods) Dictionary Methods**

**Dictionaries come equipped with various methods to streamline common tasks. This includes `.keys()`, `.values()`, and `items()`:**

```python
# Dictionary methods
print('Keys:', student_info.keys())
print('Values:', student_info.values())
print('Items:', student_info.items())

```

**This prints the following:**

```
Keys: dict_keys(['name', 'age', 'grade'])
Values: dict_values(['Alice', 9, 'A'])
Items: dict_items([('name', 'Alice'), ('age', 9), ('grade', 'A')])

```

- **`.keys()` returns just the keys from a dictionary.**
- **`.values()` returns just the values.**
- **`.items()` returns a list of the key-value pairs as tuples.**

**As you continue your Python journey, keep in mind the ways dictionaries can simplify your code and optimize your programs.**

## **Instructions**

**🍎 Welcome to NYC! 🗽**

**As one of the cultural capitals of the world, NYC is the home of the Met Museum.**

**It has one of the biggest art collections in the world! 🗺️**

**Let’s catalog an artifact from the museum! 🖼️ 👩‍🎨**

**First, [search the Met Museum site](https://www.metmuseum.org/art/collection/search) for your favorite artifact. Scroll to "Artwork Details" and let's start cataloging.**

**On the Python editor, create a dictionary with the information of your artifact, including:**

- **`artist`**
- **`period`**
- **`date`**

**Lastly, print the dictionary. What do you see? What if we just want to print the keys, or the values?**