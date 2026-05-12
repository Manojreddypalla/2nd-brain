# 01.What is NumPy?

# **1. The Notebook**

## [**#](https://www.codedex.io/numpy/01-the-notebook#what-is-numpy) What is NumPy?**

**We live in a world full of data. It’s everywhere around us.**

**In fact, 328 million terabytes of data are created every day. To put that into perspective, if a byte is an inch, that would be 207,890,118,441 times around Earth... a day.**

**And within the bits & bytes, numbers & datasets, and tables & graphs, mysteries are waiting to be discovered and stories beckon to be told.**

**Welcome to the first chapter of Data Analysis: NumPy! 🔢**

![](https://imgur.com/VuRgh22.gif)

**This is the beginning of our data science journey.**

[**NumPy](https://numpy.org/) (short for "Numerical Python") is a Python library that supports large, multi-dimensional arrays. It also comes with a bunch of math functions that you can use to operate on these arrays.**

**It works well with other data science libraries such as Pandas (data analysis), Matplotlib (data visualization), and scikit-learn (machine learning), which makes NumPy the obvious first stop in the journey.**

**In this chapter, we will learn about the basics of NumPy.**

## [**#](https://www.codedex.io/numpy/02-import#import-numpy) Import NumPy**

**First things first, to access NumPy and its functions, we need to import it at the top of our Python file, like this:**

```python
import numpy

```

**Now, we can use the `numpy` package in the rest of the program. 📦**

**Another common way to import NumPy is:**

```python
import numpy as np

```

**Here, we are importing the `numpy` package and then using the `as` keyword to name it `np` for short. After this point of the program, we can use `np` instead of the full name `numpy`.**

**Yes, it's only three characters shorter, but programmers love efficiency! If you have to type "numpy" over and over again, you would get sick of it, too!**

**Data Analysts and Data Scientists use `np`, so we will follow that in this course.**

## **Instructions**

**In the notebook, import the NumPy package and use the nickname `np`.**

**Now, add this line to see the version number.**

```python
print(np.__version__)

```

**Run the program. What version are you running?**