# 12.Higher Order Functions

## [**#](https://www.codedex.io/intermediate-python/12-hola#higher-order-functions) Higher Order Functions**

**You are being inducted into a higher order! 🙌**

**In functional programming, higher-order functions can take other functions as arguments or return functions as results. Like Swiss Army knives, higher-order functions are versatile and efficient. They enable us to create powerful and reusable Python code.**

## [**#](https://www.codedex.io/intermediate-python/12-hola#example) Example**

**Consider a scenario where you have a list of numbers, and you want to perform the same operation on each number. Don't fret about the "what ifs" and trust the process. Higher-order functions are here to help!**

```python
# Define the Higher-order function
def apply_operation(operation, numbers):
  result = []
  for num in numbers:
    result.append(operation(num))
  return result

# Example operation
def double(x):
  return x * 2

# List of numbers
numbers_list = [1, 2, 3, 4, 5]

# Using the higher-order function
doubled_numbers = apply_operation(double, numbers_list)

# Displaying the outcomes
print('Original Numbers:', numbers_list)
print('Doubled Numbers:', doubled_numbers)

```

**This will return:**

```
Original Numbers:[1, 2, 3, 4, 5]
Doubled Numbers:[2, 4, 6, 8, 10]

```

**In this example:**

- **`apply_operation()` is our higher-order function.**
- **A function takes another function (`double()`) as an argument and applies it to each item in `numbers_list`.**
- **The result is a fast and customizable way to transform the numbers based on the chosen operation.**

## **Instructions**

**If you're on Duolingo, this exercise is for you! 🦉**

**Let's use higher-order functions to translate some common phrases from different languages.**

**First, define a `translator()` function that takes a single `language` parameter.**

**Inside the function, add a `translations` dictionary that contains translations for common phrases in different languages:**

```python
translations = {
  'spanish': {'hello': 'hola', 'goodbye': 'adiós', 'thank you': 'gracias'},
  'french': {'hello': 'bonjour', 'goodbye': 'au revoir', 'thank you': 'merci'},
  'italian': {'hello': 'ciao', 'goodbye': 'arrivederci', 'thank you': 'grazie'}
}

```

**Next, define an inner `translate_word()` function that takes a `word` parameter and returns a translation if the `word` exists in a specific language.**

**Return the inner `translate_word()` function from the outer `translator()` function.**

**Finally, let's test our `translator()` function with different languages:**

```python
translate_to_spanish = translator('spanish')
print(translate_to_spanish('hello')) # Output: hola

```

**Have fun, maybe try a new language!**