Now we're entering one of the most important chapters in **real-world data analysis**.

# Chapter 8 — Missing Data (`NaN`)

Here's something you'll discover quickly:

> **Perfect datasets don't exist.**

Almost every real dataset has missing values.

Imagine this employee table:

|Name|Age|Salary|Department|
|---|--:|--:|---|
|Alex|21|50000|IT|
|Bob|❌|60000|HR|
|Charlie|23|❌|IT|
|Emma|27|75000|❌|

Those blanks are called **missing values**.

In Pandas, they're usually represented as **`NaN`**.

---

# Why does data become missing?

Many reasons:

- User skipped a form field.
    
- Sensor failed.
    
- Database error.
    
- Data wasn't collected.
    
- Value is unknown.
    

Example:

```text
Hospital Dataset

Patient   Blood Pressure

Alex      120

Bob       NaN

Emma      135
```

Bob's blood pressure wasn't recorded.

---

# Create a Dataset with Missing Values

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "Name": ["Alex", "Bob", "Charlie", "Emma"],
    "Age": [21, np.nan, 23, 27],
    "Salary": [50000, 60000, np.nan, 75000],
    "Department": ["IT", "HR", None, "Finance"]
})

print(df)
```

Output

```text
      Name   Age   Salary Department
0     Alex  21.0  50000.0         IT
1      Bob   NaN  60000.0         HR
2  Charlie  23.0      NaN       None
3     Emma  27.0  75000.0    Finance
```

Notice:

- `NaN`
    
- `None`
    

Both are treated as missing by Pandas.

---

# Detect Missing Values

## `isna()`

```python
df.isna()
```

Output

```text
    Name    Age  Salary Department

0  False  False   False     False

1  False   True   False     False

2  False  False    True      True

3  False  False   False     False
```

Think of it as a **map** showing where data is missing.

---

## Count Missing Values

Instead of looking row by row:

```python
df.isna().sum()
```

Output

```text
Name          0
Age           1
Salary        1
Department    1
```

This is one of the **first commands** analysts run.

---

# Total Missing Values

```python
df.isna().sum().sum()
```

Output

```text
3
```

First `.sum()` counts per column.

Second `.sum()` adds those counts together.

---

# Find Rows with Missing Data

```python
df[df.isna().any(axis=1)]
```

Output

|Name|Age|Salary|Department|
|---|--:|--:|---|
|Bob|NaN|60000|HR|
|Charlie|23|NaN|None|

`axis=1` means:

> Check **across each row**.

---

# Remove Missing Data

## `dropna()`

```python
df.dropna()
```

Output

|Name|Age|Salary|Department|
|---|--:|--:|---|
|Alex|21|50000|IT|
|Emma|27|75000|Finance|

Every row with **any missing value** is removed.

---

## Remove Columns with Missing Data

```python
df.dropna(axis=1)
```

Result:

Only columns with **no missing values** remain.

---

# Remove Only if All Values Are Missing

```python
df.dropna(how="all")
```

This removes a row only if **every column** is missing.

---

# Fill Missing Values

Instead of deleting data, sometimes we replace it.

---

## Replace with Zero

```python
df.fillna(0)
```

Output

```text
Age

NaN

↓

0
```

---

## Replace with Text

```python
df["Department"] = df["Department"].fillna("Unknown")
```

Now

```text
Unknown
```

appears instead of missing.

---

## Replace with Mean

Suppose

```text
Age

21

NaN

23

27
```

Average

```text
(21+23+27)/3

↓

23.67
```

Code

```python
df["Age"] = df["Age"].fillna(df["Age"].mean())
```

This is **very common** before training machine learning models.

---

## Replace with Median

```python
df["Age"] = df["Age"].fillna(df["Age"].median())
```

Median is often better because it's less affected by extreme values.

---

## Replace with Most Frequent Value (Mode)

For categorical data:

```python
df["Department"] = df["Department"].fillna(
    df["Department"].mode()[0]
)
```

Example:

```text
IT
IT
HR
Finance
NaN
```

Gets replaced with

```text
IT
```

because it's the most common value.

---

# Check if Dataset is Clean

```python
df.isna().sum()
```

Output

```text
Name          0
Age           0
Salary        0
Department    0
```

Congratulations—no missing values remain.

---

# Practical Titanic Example

```python
import pandas as pd

df = pd.read_csv("titanic.csv")
```

Check missing values:

```python
df.isna().sum()
```

You'll notice columns like:

```text
Age       177
Cabin     687
Embarked    2
```

Now clean them:

```python
df["Age"] = df["Age"].fillna(df["Age"].median())

df["Embarked"] = df["Embarked"].fillna(
    df["Embarked"].mode()[0]
)

df = df.drop(columns=["Cabin"])
```

Why drop `Cabin`?

Because most values are missing, so filling them wouldn't make much sense.

---

# Why This Matters in Machine Learning

Imagine predicting house prices.

If `Price` is missing:

```
House A → ₹50 lakh

House B → ?

House C → ₹70 lakh
```

A machine learning model can't learn from missing target values.

Similarly, if important features like `Age` or `Income` are missing, many algorithms won't work until you clean the data.

---

# Real-World Thought Process

When you find missing values, ask yourself:

1. **How many are missing?**
    
2. **Is the column important?**
    
3. **Should I drop rows?**
    
4. **Should I drop the entire column?**
    
5. **Should I fill with mean, median, mode, or a constant?**
    

There isn't one correct answer—it depends on the data and the problem.

---

# Cheat Sheet

|Task|Command|
|---|---|
|Find missing values|`df.isna()`|
|Count missing values|`df.isna().sum()`|
|Total missing values|`df.isna().sum().sum()`|
|Drop rows|`df.dropna()`|
|Drop columns|`df.dropna(axis=1)`|
|Fill with 0|`df.fillna(0)`|
|Fill with mean|`df["Age"].fillna(df["Age"].mean())`|
|Fill with median|`df["Age"].fillna(df["Age"].median())`|
|Fill with mode|`df["Dept"].fillna(df["Dept"].mode()[0])`|

---

# 🧪 Practical Challenge

Using the Titanic dataset:

```python
import pandas as pd

df = pd.read_csv("titanic.csv")

# 1. Count missing values in each column.

# 2. Find the total number of missing values.

# 3. Show only rows with missing values.

# 4. Fill missing Age values with the median.

# 5. Fill missing Embarked values with the mode.

# 6. Drop the Cabin column.

# 7. Verify that Age and Embarked no longer contain missing values.
```

---

## 📍Where you are now

You've learned to:

- ✅ Load datasets
    
- ✅ Explore them
    
- ✅ Select rows and columns
    
- ✅ Filter data
    
- ✅ Sort data
    
- ✅ Clean missing values
    

These six skills cover a large part of a data analyst's daily workflow.

**Next up (Chapter 9): String Operations (`.str`)**—you'll learn how to clean and manipulate text columns such as names, emails, cities, and movie titles. This is another skill you'll use constantly with real-world datasets like Netflix and customer records.