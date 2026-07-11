# STEP 2 — Pandas Mastery

# Chapter 3 — DataFrame

This is where Pandas becomes powerful.

Remember the mental model from Chapter 2:

```text
Series = One Column

DataFrame = Many Series
```

Everything in Pandas revolves around the **DataFrame**.

---

# What is a DataFrame?

Imagine an Excel sheet.

|Name|Age|Salary|
|---|--:|--:|
|Alex|21|50000|
|Bob|22|60000|
|Charlie|20|45000|

This entire table is a **DataFrame**.

Internally:

```text
            DataFrame

 ┌────────────────────────────┐
 │ Name │ Age │ Salary        │
 ├────────────────────────────┤
 │ Alex │ 21  │ 50000         │
 │ Bob  │ 22  │ 60000         │
 │ Sam  │ 20  │ 45000         │
 └────────────────────────────┘

      ↓      ↓       ↓

   Series  Series  Series
```

Each **column** is a separate Series.

---

# Internal Structure

Let's visualize it.

```python
df = pd.DataFrame({
    "Name": ["Alex","Bob","Sam"],
    "Age": [21,22,20],
    "Salary": [50000,60000,45000]
})
```

Internally imagine:

```text
DataFrame

Columns
│
├── Name
│      Alex
│      Bob
│      Sam
│
├── Age
│      21
│      22
│      20
│
└── Salary
       50000
       60000
       45000
```

Notice something.

Each column is just a **Series**.

---

# Creating a DataFrame

## Method 1 (Most Common)

```python
import pandas as pd

df = pd.DataFrame({
    "Name": ["Alex","Bob","Charlie"],
    "Age": [21,22,20],
    "Salary": [50000,60000,45000]
})

print(df)
```

Output

```text
      Name  Age  Salary
0     Alex   21   50000
1      Bob   22   60000
2  Charlie   20   45000
```

---

## Method 2 — List of Lists

```python
data = [
    ["Alex",21,50000],
    ["Bob",22,60000],
    ["Sam",20,45000]
]

df = pd.DataFrame(
    data,
    columns=["Name","Age","Salary"]
)
```

---

## Method 3 — List of Dictionaries

Very common when working with APIs.

```python
data = [
    {"Name":"Alex","Age":21},
    {"Name":"Bob","Age":22},
    {"Name":"Sam","Age":20}
]

df = pd.DataFrame(data)
```

---

## Method 4 — From Series

```python
names = pd.Series(["Alex","Bob","Sam"])
ages = pd.Series([21,22,20])

df = pd.DataFrame({
    "Name": names,
    "Age": ages
})
```

This shows the connection between Series and DataFrame.

---

# DataFrame Anatomy

Take this table.

```text
      Name    Age   Salary

0     Alex    21    50000

1     Bob     22    60000

2     Sam     20    45000
```

Let's label every part.

```text
          Columns
      ↓      ↓      ↓

     Name   Age   Salary

Rows

0

1

2
```

A DataFrame has:

- Rows
    
- Columns
    
- Index
    
- Values
    
- Data Types
    

---

# Accessing a Column

```python
print(df["Age"])
```

Output

```text
0    21
1    22
2    20
dtype:int64
```

Notice something?

The result is **not** a DataFrame.

It is a **Series**.

---

# Multiple Columns

```python
df[["Name","Salary"]]
```

Output

```text
      Name   Salary

0     Alex   50000

1     Bob    60000

2     Sam    45000
```

Notice the double brackets.

```python
["Age"]        # Series

[["Age"]]      # DataFrame
```

Why?

Because:

```text
One column

↓

Series

Many columns

↓

DataFrame
```

---

# Adding a New Column

```python
df["Bonus"] = [5000,6000,4500]
```

Now

```text
Name

Age

Salary

Bonus
```

Pandas creates a new Series internally.

---

# Modifying a Column

```python
df["Salary"] = df["Salary"] + 5000
```

Result

```text
55000

65000

50000
```

No loops.

---

# Deleting a Column

```python
df.drop(columns=["Bonus"])
```

Or

```python
df = df.drop(columns=["Bonus"])
```

---

# DataFrame Properties

Suppose

```python
print(df)
```

---

## Shape

```python
df.shape
```

Output

```text
(3,3)
```

Meaning

```text
3 rows

3 columns
```

---

## Columns

```python
df.columns
```

Output

```text
Index(['Name','Age','Salary'])
```

---

## Index

```python
df.index
```

Output

```text
RangeIndex(0,3)
```

---

## Data Types

```python
df.dtypes
```

Output

```text
Name       object

Age        int64

Salary     int64
```

Notice every column has its own dtype.

---

## Values

```python
df.values
```

Output

```text
[
['Alex',21,50000],

['Bob',22,60000],

['Sam',20,45000]
]
```

Returns the underlying NumPy array.

---

# Size

```python
df.size
```

Output

```text
9
```

Because

```text
3 × 3 = 9 values
```

---

# Renaming Columns

```python
df.columns = [
    "Employee",
    "Age",
    "Income"
]
```

---

# Custom Index

Instead of

```text
0

1

2
```

You can do

```python
df.index = [
    "E01",
    "E02",
    "E03"
]
```

Now

```text
E01

E02

E03
```

become row labels.

---

# Vectorized Operations

Increase everyone's salary.

```python
df["Salary"] = df["Salary"] + 10000
```

No loop.

Internally

```text
50000

↓

60000

60000

↓

70000
```

---

# Dry Run

Imagine

```python
df = pd.DataFrame({
    "Age":[21,22,20]
})
```

Internally

```text
DataFrame

↓

Series

↓

21

22

20
```

Now

```python
df["Age"] + 5
```

Pandas

```text
21 → 26

22 → 27

20 → 25
```

Creates a new Series and assigns it back if requested.

---

# DataFrame vs Series

|Series|DataFrame|
|---|---|
|One column|Multiple columns|
|One dimension|Two dimensions|
|Has index|Has rows and columns|
|Represents one feature|Represents an entire dataset|

---

# Mental Model

This is the picture I want you to remember:

```text
CSV / Excel / SQL
        │
        ▼
     DataFrame
        │
 ┌──────┼────────┐
 ▼      ▼        ▼
Name    Age    Salary
 │       │        │
Series Series  Series
```

Everything starts as a **DataFrame**, and when you select a single column, you're simply taking out one **Series**.

---

# What's Next?

Now that you know **how data is stored**, the next step is learning **how to inspect it**.

In **Chapter 4**, you'll learn the first commands every data analyst runs after loading a dataset:

- `head()`
    
- `tail()`
    
- `info()`
    
- `describe()`
    
- `shape`
    
- `columns`
    
- `index`
    
- `dtypes`
    

These are your "look around before touching anything" tools. In real projects, this is almost always the first thing you do after `pd.read_csv()`.



