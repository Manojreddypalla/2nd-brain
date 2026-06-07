# 16. Test Run

## [**#](https://www.codedex.io/intermediate-python/16-test-run#unit-tests) Unit Tests**

**Building off our experience with testing, let's learn how to write basic unit tests in Python.**

**Unit testing involves taking smaller pieces of a large program and checking that the code behaves as expected under various scenarios. A unit of code can be as simple as a small function or as complex as a large class.**

## [**#](https://www.codedex.io/intermediate-python/16-test-run#unittest-framework) unittest Framework**

**In Python, the `unittest` framework is a built-in module. It helps developers validate the behavior of their code, piece by piece. By isolating and testing each unit independently, we can verify their functionality and behavior.**

**A unittest can be as simple as testing that values are the same. We use `.assertEqual` in unittest to ensure two elements are equal to each other**

```python
assertEqual(a, b)
## a == b

```

**For this chapter, we will be running the examples and exercises on a local text editor, like VS Code.**

**To use the `unittest` framework in a new Python file, we must first import it:**

```python
import unittest

```

**Note: Check out our [project tutorial](https://www.codedex.io/projects/set-up-your-local-environment-in-python) for help with setting up VS Code for Python!**

## **Instructions**

**Let’s test a simple `add_numbers()` function that takes two numbers as input and returns their sum.**

**First, open your local editor and create a new test_addition.py file. Then, copy/paste the code below:**

```python
import unittest

def add(a, b):
  return a + b

class TestAddition(unittest.TestCase):
  # Define the first unit test
  def test_add(self):
    result = add(3, 4)

    # Define the expected result
    expected_result = 7
    self.assertEqual(result, expected_result)

if __name__ == '__main__':
  unittest.main()

```

**To run this code, open a terminal and run `python3 test_addition.py`.**

**What happens when you press Enter?**

**Happy testing! 🧪**