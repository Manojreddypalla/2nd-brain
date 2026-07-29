
This is the **most important chapter** in Pandas.

If you truly understand **Series**, then **DataFrame** becomes almost obvious.

---

# First, what is a Series?

Think back to Excel.

|Name|Age|Salary|
|---|---|---|
|Alex|21|50000|
|Bob|22|60000|
|Charlie|20|45000|

Pick just one column.

|Age|
|---|
|21|
|22|
|20|

That single column is a **Series**.

A DataFrame is simply multiple Series placed side by side.

```
DataFrame

Name      Age      Salary
 |         |          |
 |         |          |
Series   Series    Series
```

So mentally:

```
Series = One Column

DataFrame = Collection of Series
```

---

# Internal Structure

A Series isn't just values.

It stores **two things**.

```
Index        Value

0 ----------> 21

1 ----------> 22

2 ----------> 20
```

Notice something?

Every value has an **index**.

Internally it's very similar to

```python
{
    0:21,
    1:22,
    2:20
}
```

except optimized using NumPy.

Think of a Series as

```
Index + Values
```

---

# Why not use a Python List?

Python list

```python
ages = [21,22,20]
```

Looks similar.

But lists don't know anything about data.

They don't know

- missing values
    
- statistics
    
- filtering
    
- labels
    
- data type optimizations
    

Series does.

---

# Creating Your First Series

First import pandas.

```python
import pandas as pd
```

Create one.

```python
ages = pd.Series([21,22,20,24])

print(ages)
```

Output

```text
0    21
1    22
2    20
3    24
dtype: int64
```

Let's understand every part.

```
0
1
2
3
```

These are indexes.

```
21
22
20
24
```

These are values.

```
dtype: int64
```

This is the data type.

---

# Why "dtype"?

NumPy introduced the idea of **one data type for the whole array**.

Pandas follows that idea.

```
21
22
20
24

↓

int64
```

Because every value is an integer.

---

# Custom Index

Indexes don't have to be numbers.

```python
ages = pd.Series(
    [21,22,20],
    index=["Alex","Bob","Charlie"]
)

print(ages)
```

Output

```text
Alex       21
Bob        22
Charlie    20
dtype:int64
```

Now the labels are names instead of `0, 1, 2`.

---

# Accessing Data

### By position

```python
print(ages[0])
```

Output

```text
21
```

### By label

```python
print(ages["Alex"])
```

Output

```text
21
```

Both work because the index acts as labels.

---

# Creating from a Dictionary

This is one of the coolest features.

```python
ages = {
    "Alex":21,
    "Bob":22,
    "Charlie":20
}

series = pd.Series(ages)

print(series)
```

Output

```text
Alex       21
Bob        22
Charlie    20
dtype:int64
```

Notice that the dictionary keys automatically become the Series index.

---

# Different Data Types

Integers

```python
pd.Series([1,2,3])
```

Floats

```python
pd.Series([1.5,2.8,7.1])
```

Strings

```python
pd.Series(["Python","Java","C++"])
```

Boolean

```python
pd.Series([True,False,True])
```

Dates

```python
pd.Series(pd.date_range("2026-01-01", periods=5))
```

---

# Useful Properties

Suppose

```python
ages = pd.Series([21,22,20,24])
```

### Values

```python
print(ages.values)
```

```
[21 22 20 24]
```

Returns the underlying NumPy array.

---

### Index

```python
print(ages.index)
```

Output

```
RangeIndex(start=0, stop=4, step=1)
```

---

### Data Type

```python
print(ages.dtype)
```

```
int64
```

---

### Size

```python
print(ages.size)
```

```
4
```

---

### Shape

```python
print(ages.shape)
```

```
(4,)
```

A Series is one-dimensional, so the shape has a single value.

---

# Vectorized Operations

This is where Pandas shines.

Instead of writing a loop:

```python
result = []

for age in ages:
    result.append(age + 1)
```

You simply write:

```python
ages + 1
```

Output

```
22
23
21
25
```

Every element is updated automatically.

---

Multiply everything.

```python
ages * 10
```

Output

```
210
220
200
240
```

Subtract

```python
ages - 5
```

Square

```python
ages ** 2
```

No loops needed.

---

# Filtering

Suppose

```python
ages = pd.Series([18,25,30,17,40])
```

Find everyone older than 20.

```python
ages > 20
```

Output

```text
0    False
1     True
2     True
3    False
4     True
dtype: bool
```

This is called a **Boolean Mask**.

Now use it:

```python
ages[ages > 20]
```

Output

```text
1    25
2    30
4    40
dtype:int64
```

This concept powers almost all filtering in Pandas.

---

# Missing Values

Series can store missing data.

```python
import numpy as np

ages = pd.Series([21,np.nan,20,24])

print(ages)
```

Output

```
0    21
1    NaN
2    20
3    24
dtype: float64
```

Notice something interesting:

The dtype changed from `int64` to `float64`.

That's because the special value `NaN` is represented as a floating-point value in NumPy, so Pandas promotes the entire Series to a compatible type.

---

# Series vs NumPy Array

|NumPy Array|Pandas Series|
|---|---|
|Only values|Values + Index|
|Numerical computing|Data analysis|
|No labels|Labels supported|
|Faster|Slightly slower but much richer|
|Built for mathematics|Built for real-world data|

---

# Dry Run

```python
import pandas as pd

ages = pd.Series(
    [21,22,20],
    index=["Alex","Bob","Charlie"]
)
```

Internally, think of it like this:

```
Index        Value

Alex  ------> 21

Bob   ------> 22

Charlie ---> 20
```

Now:

```python
ages["Bob"]
```

Pandas:

```
Find label

↓

Bob

↓

Return associated value

↓

22
```

Now:

```python
ages + 5
```

Pandas internally applies the operation to every value:

```
21 → 26

22 → 27

20 → 25
```

Result:

```
Alex       26
Bob        27
Charlie    25
```

No explicit loop from you—Pandas performs a **vectorized operation**, which is much faster than iterating in pure Python.

---

# Pattern to Remember

```
Python List
      ↓
NumPy Array
      ↓
Pandas Series
      ↓
Pandas DataFrame
```

- A **list** is just a collection of values.
    
- A **NumPy array** is a fast collection of values with one data type.
    
- A **Series** is a NumPy array **plus an index (labels)**.
    
- A **DataFrame** is a collection of Series sharing the same index.
    

Once this mental model clicks, Chapter 3 (DataFrame) becomes much easier because you'll simply be combining multiple Series into a table.