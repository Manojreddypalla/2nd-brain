# 13. Transformation

## [**#](https://www.codedex.io/intermediate-python/13-grammys#transformation) Transformation**

**Unlike Princess and The Frog, when it comes to transforming data in Python no amphibians are involved. 🐸**

**In Python, we can transform data using three powerful functions: `map()`, `filter()`, and `reduce()`. These functions allow us to manipulate, select, and aggregate data efficiently. Let's explore each one!**

### [**##](https://www.codedex.io/intermediate-python/13-grammys#map) map()**

**Let's look at the `map()` function first:**

```python
map(function, data)

```

**`map()` applies the given `function` to each element in the `data` and returns a new list. It's perfect for when you need to perform the same operation on every item in a list, tuple, or data set.**

**Here's an example of `map()` being used to divide each number in a list by 2:**

```python
def divide_by_2(x):
  return x / 2

halved_numbers = map(divide_by_2, [1, 2, 3, 4, 5])

print(list(halved_numbers))
# Output: [0.5, 1.0, 1.5, 2.0, 2.5]

```

**Note: In order to get the actual list, we need to use the `list()` function. This also goes for the `filter()` function, which we'll cover next.**

### [**##](https://www.codedex.io/intermediate-python/13-grammys#filter) filter()**

**Next up is the `filter()` function:**

```python
filter(function, data)

```

**Here, we select elements from the `data` that satisfy the given condition specified by the `function` and return a new list. It helps extract specific items from a dataset based on the criteria you define. The input function in `filter()` must return a boolean.**

**Say we want to extract only even numbers from a list:**

```python
def filter_even(x):
  return x % 2 == 0

even_numbers = filter(filter_even, [1, 2, 3, 4, 5])

print(list(even_numbers)) # Output: [2, 4]

```

**In this example, the function will go through the list of number and if `x % 2 ==0` the function `filter_even` is true it will print the number.**

### [**##](https://www.codedex.io/intermediate-python/13-grammys#reduce) reduce()**

**Lastly, there's the `reduce()` function:**

```python
from functools import reduce
reduce(function, data, initial)

```

**Note: Unlike the previous two functions, we must import `reduce()` from the [`functools`](https://docs.python.org/3/library/functools.html) module.**

**`reduce()` takes a collection of iterable `data` and combines its elements into a single value via a `function`. An optional `initial` value can be used, as well.**

**This function is ideal for tasks like summing up a list of numbers.**

**`reduce()` Example:**

```python
from functools import reduce

def multiply(x, y):
  return x * y

product = reduce(multiply, [1, 2, 3, 4, 5])

print(product) # Output: 120

```

**After importing `reduce()` from `functools`, we successively multiply each number in the list with a `multiply()` function we defined separately. This results in a final, cumulative `product` of 120.**

## **Instructions**

**For many, music is so personal. We have created a `playlist` of songs that have won the “Song of the Year” 🏆 [Grammy award](https://en.wikipedia.org/wiki/Grammy_Awards) in the past 5 years.**

**Let's play around with the data!**

```python
# List of songs with their durations (in minutes)
playlist = [('What Was I Made For?', 3.42), ('Just Like That', 5.05), ('Song 3', 6.55), ('Leave The Door Open', 4.02), ('I Can\'t Breath', 4.47), ('Bad Guy', 3.14)]

```

**First, use the `filter()` function to pick out the songs that are longer than 5 minutes (i.e., `5.00`).**

**Next, use `map()` to convert all the durations of the songs in your playlist from minutes to seconds.**

**Lastly, add up the total playtime of the `playlist` with `reduce()`.**

**Since `map()`, `filter()`, and `reduce()` all use function parameters, it may be helpful to define separate named functions for them:**

- **A `longer_than_five_minutes()` function for `filter()`.**
- **A `minutes_to_seconds()` function for `map()`.**
- **An `add_durations()` function for `reduce()`.**