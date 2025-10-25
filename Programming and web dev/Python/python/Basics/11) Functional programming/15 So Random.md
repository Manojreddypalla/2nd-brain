# 15. So Random

## [**#](https://www.codedex.io/intermediate-python/15-so-random#congrats) Congrats!**

**Welcome to the final exercise in our chapter on functional programming in Python! 🥳**

**Let's recap what we learned:**

- **Pure functions return output with no side effects.**
- **Higher-order functions are used as arguments and/or return values.**
- **`map()`, `filter()`, and `reduce()` can efficiently manipulate data.**
- **List comprehensions can process existing list data in one line.**

**With functional programming, we can write code that is easier to understand, test, and maintain. It encourages us to think about our code in terms of functions and data transformations, exciting stuff.**

**Hope you're ready for a challenge!**

## **Instructions**

**If you’ve ever signed up for Discord or Reddit, you probably got a random username. Odds are those sites are using list comprehensions to generate fantastical names for new users.**

**Let’s generate our usernames with functional programming! Here's some code to get us started:**

```python
import random

prefixes = ['Mystic', 'Golden', 'Dark', 'Shadow', 'Silver']
suffixes = ['storm', 'song', 'fire', 'blade', 'whisper']

def create_fantasy_name(list_1, list_2):
  return random.choice(list_1) + ' ' + random.choice(list_2)

```

**Each name will be a combination of a prefix and a suffix randomly chosen from predefined lists.**

**Under the `suffixes` list, define a `capitalize_suffix()` function that returns a capitalized `name` parameter. Then, use the `map()` function to apply `capitalize_suffix()` to each item in the `suffix` list and store in a variable.**

**Note: Remember to use the `list()` function (e.g., `list(map(capitalize_suffix, suffixes))`).**

**Next, use list comprehensions to generate 10 fantasy names, using `create_fantasy_name()`. Save to a new `random_names` list.**

**Then, define the following pure functions:**

- **`fire_in_name(name)` that returns `True` if `'Fire'` is in `name`.**
- **`concatenate_names(name1, name2)` that returns a string of `name1` + `name2`.**

**Next, do the following:**

- **Use `filter()` and apply `fire_in_name()` to the `random_names` list.**
- **Use `reduce()` and apply `concatenate_names()` to the filtered names.**

**Note: Don't forget that we need the `functools` module to use `reduce()`.**

**Lastly, define one more function, `display_name_info()`, that does the following:**

- **Prints the generated fantasy names with a `for` loop.**
- **Prints the filtered names that include `'Fire'`.**
- **Prints the reduced names.**

**Go ahead and use `display_name_info()`. What names did you get? Feel free to share with your friends on Twitter!**