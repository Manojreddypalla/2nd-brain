# 03.index and slicing

## **Indexing**

**In Python lists, we access individual items using the bracket notation. In NumPy arrays, we can access individual arrays the same way!**

**The following array contains the coffee intake in a work week. ☕️**

```python
coffee = np.array([3, 2, 1, 0, 1])

```

- **The element at index 0 is `3`.**
- **The element at index 1 is `2`.**
- **The element at index 2 is `1`.**
- **The element at index 3 is `0`.**
- **The element at index 4 is `1`.**

**To output each of the elements, we can use the `name[index]` syntax:**

```python
print(coffee[0])   # Output: 3
print(coffee[1])   # Output: 2
print(coffee[2])   # Output: 1
print(coffee[3])   # Output: 0
print(coffee[4])   # Output: 1

```

## [**#](https://www.codedex.io/numpy/04-population#slicing) Slicing**

**Same thing with slicing! We can slice NumPy arrays the same way as Python lists.**

**Slicing is where we can access certain parts of a sequence. We can get multiple elements by specifying where to start slicing and where to end like `name[start : end]`.**

**For example:**

```python
coffee = np.array([3, 2, 1, 0, 1])

print(coffee[0:2])    # Output: [3 2]
print(coffee[2:])     # Output: [1 0 1]
print(coffee[-2:])    # Output: [0 1]

```

**It starts from the `start` index (inclusive) and ends before the `end` index (non-inclusive).**