# 02. Tuples

## [**#](https://www.codedex.io/intermediate-python/02-find-my-friends#tuples) Tuples**

**Tuples are ordered collections that cannot be changed once created. They are ideal for storing fixed data that shouldn't change (e.g., coordinates, RGB color values).**

**While lists can change, tuples cannot. This means tuples are immutable.**

**However, you can replace one tuple with another and have duplicates within a tuple.**

## [**#](https://www.codedex.io/intermediate-python/02-find-my-friends#syntax) Syntax**

**Lists and tuples have a few things in common:**

- **They can store multiple items in a single variable.**
- **Their values are separated by commas.**

**However, tuples use a different syntax:**

```python
tuple_example = (1, '2', True)
navy_blue = (0, 0, 128)

```

**Tuples are defined with or without parentheses and their items can be a mix of any data type. They can have one or more items.**

**If there is only one item, make sure there is a comma `,` beside it:**

```python
# valid tuple
t1 = ('a',)

# valid tuple
t2 = 'a',

# NOT a tuple
t3 = ('a')

```

## [**#](https://www.codedex.io/intermediate-python/02-find-my-friends#creating-and-accessing-tuples) Creating and Accessing Tuples**

**For example, a DNA sequence would be stored as a tuple since it contains very important values that should not be changed and should not change when referenced without.**

```python
my_dna = ('GCT', 'AGC', 'AGG', 'TAA', 'ACT', 'CAT')

```

**You can access tuple items the same way you do with lists... using the index! If we want to get the 3rd item in our `my_dna` tuple:**

```python
print(my_dna[2]) # Output: AGG

```

**Because tuples are an ordered collection, you can look up their items by index.**

## [**#](https://www.codedex.io/intermediate-python/02-find-my-friends#using-tuples) Using Tuples**

**Tuples are also unpackable which allows you to easily turn them into variables. Most commonly, tuples are used as return values allowing you to easily work with large data sets.**

```python
full_name = ('Ada', 'Lovelace')

first_name = full_name[0]

print(first_name) # Output: Ada

```

**Tuples can also be combined:**

```python
# Combining tuples
t1 = 'a',
t2 = 'b',
t3 = t1 + t2

print(t3)  # Output: ('a', 'b')

```

## **Instructions**

**Thanks to the internet, we can connect with friends across the world. 🧑‍🤝‍🧑**

**When you and your friends are scattered across different cities, sharing locations is something that can help you feel closer. 🫶**

**Let's use [latitude and longitude 🌐](https://www.latlong.net/) to create tuples of our friends locations!**

- **Create a tuple for the city you are in.**
- **Create 3 tuples for your friends, each with the latitude and longitude of the cities they live in.**
- **Print the locations of all your friends.**

**Which of your friends is the furthest away?**

**Hit "Complete" to feel closer than ever with your friends. 🥰**

**Bonus: See if you can combine all the tuples into one tuple.**