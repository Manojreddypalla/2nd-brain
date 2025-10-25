# 14. List Comprehensions

## [**#](https://www.codedex.io/intermediate-python/14-mental-math#list-comprehensions) List Comprehensions**

**If loops make you dizzy, let me introduce the art of list comprehension. Imagine transforming complex loops into elegant one-liners, making your code more readable and efficient. 😎**

**List comprehensions can transform data, filter elements, and even create nested lists. In functional programming, they let us transform list data concisely and expressively.**

## [**#](https://www.codedex.io/intermediate-python/14-mental-math#syntax) Syntax**

**Here is how we write list comprehensions in Python:**

```python
[expression for item in dataset if condition]

```

**Let's break down each piece, from left to right:**

1. **The `expression` is applied to each `item`.**
2. **A `for..in` statement loops through each `item`.**
3. **An optional `if condition` can filter out items.**

**The `dataset` can be something iterable like a list or tuple. List comprehensions do not change the original list.**

**Note: A list comprehension works with `for` loops but not `while` loops.**

**Let's see list comprehensions in action:**

```python
# Original approach using a loop
numbers = [1, 2, 3, 4, 5]
squares = []
for num in numbers:
  squares.append(num ** 2)

# Using a list comprehension to square each number
squared_numbers = [num ** 2 for num in numbers]

# Displaying the outcomes
print('Original Numbers:', numbers)
print('Squared Numbers:', squared_numbers)

```

- **The expression `num ** 2`, calculates the square of each number.**
- **The item: `num`, represents each number in the `numbers` list.**
- **The data set: `numbers` is the original list of numbers.**

**In this example, we used a list comprehension to create a new list containing the squares of each number in the original list. It's a concise and elegant way to achieve the same result as using a loop, but with less code.**

## **Instructions**

[**Mental math](https://en.wikipedia.org/wiki/Mental_calculation) can bring up mixed feelings. A core math concept is that even numbers are integers divisible by 2.**

**Let’s practice using list comprehensions to create a list of even numbers from the following list of integers:**

```python
numbers = [57, 10, 82, 36, 89, 46, 13, 23, 92, 48]

```

- **Create a list comprehension that filters the even numbers.**
- **Print the original range and the list of even numbers.**

**Your outcome should look like this:**

```
Original Numbers: [57, 10, 82, 36, 89, 46, 13, 23, 92, 48]
Even Numbers: [10, 82, 36, 46, 92, 48]

```

**Bonus: create and print a list of odd numbers**