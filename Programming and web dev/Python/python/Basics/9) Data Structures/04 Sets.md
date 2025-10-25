# 04.  Sets

# [**#](https://www.codedex.io/intermediate-python/04-fruit-cart#sets) Sets**

**We've now arrived at the fascinating realm of sets! 🪄**

**Sets are collections of unique items with no duplicates.**

- **They are unordered collection of distinct elements.**
- **Each item in a set must be unique.**

**Unlike lists or tuples, sets do not have an inherent order, making them perfect for scenarios where the sequence of the elements is not important.**

**Sets are useful when dealing with unique elements and performing tasks such as:**

- **Working with a database of user profiles, each with a unique ID.**
- **Looking up only information with a unique tag attached to it.**
- **Searching for something and filtering out the results by category.**

**This makes sets great for finding unique items or eliminating duplicates.**

## [**#](https://www.codedex.io/intermediate-python/04-fruit-cart#creating-sets) Creating Sets**

**Let's start by creating sets. You can define a set using curly braces `{` `}` or the `set()` constructor. Each set item is separated by a comma:**

```
set_example = {val1, val2, val3}

```

**Like dictionaries, the items in sets are in no particular order. This makes them handy for times when the order of items isn't important.**

**Let's set one up (pun intended):**

```python
fruits = {'🍎 apple', '🍌 banana', '🍊 orange'}

```

**The `fruits` set contains three items, in no certain order.**

**Note: Dictionaries and sets can be made with curly braces but set do not use key-value pairs.**

## [**#](https://www.codedex.io/intermediate-python/04-fruit-cart#using-sets) Using Sets**

**Sets support various methods that allow us to combine sets, find common items, or filtering out items. This makes sets powerful for data manipulation.**

**Here are some of those methods in action:**

```python
set1 = {1, 2, 3, 4}
set2 = {3, 4, 5, 6}

# Union of sets
union_result = set1.union(set2)

# Intersection of sets
intersection_result = set1.intersection(set2)

# Difference of sets
difference_result = set1.difference(set2)

print(union_result)
print(intersection_result)
print(difference_result)

# Output:
# {1, 2, 3, 4, 5, 6}
# {3, 4}
# {1, 2}

```

**There are three methods in particular:**

- **`.union()`: Combines two or more sets and returns a new set.**
- **`.intersection()`: Returns a set of items common to two or more sets.**
- **`.difference()`: Returns a set of items found only in the calling set.**

## [**#](https://www.codedex.io/intermediate-python/04-fruit-cart#methods) Methods**

**There are more set methods for common operations, like adding and removing elements:**

```python
my_set = {1, 2, 3}

my_set.add(4)
print(my_set) # Output: {1, 2, 3, 4}

my_set.remove(2)
print(my_set) # Output: {1, 3, 4}

```

**The `.add()` method adds one new item to a set. If that item already exists, nothing happens.**

**The `.remove()` method removes an item from a set if it exists.**

## **Instructions**

**With sets, you are one step closer to becoming a Python Data Manipulator. 🧗**

**🫐🍇🍌 Let’s go back to fruits! 🍓🍒🍎**

**Grocery shopping is great until you forget what was on your list. 😥**

**Before you head out, your best friend ask you to pick up some fruit for her too. Let's combine the lists!**

- **Create two sets representing your favorite fruits and your best friend's favorite fruits.**
- **Print the union of the two sets would look like.**
- **Print the intersection of the two sets.**

**Have fun with it, check if the same fruit is `in` both sets or see the `<difference>` in both sets.**

**Remember: tomatoes are fruits! 🍅**

**After finishing, hit "Complete" to get started on the final exercise!**