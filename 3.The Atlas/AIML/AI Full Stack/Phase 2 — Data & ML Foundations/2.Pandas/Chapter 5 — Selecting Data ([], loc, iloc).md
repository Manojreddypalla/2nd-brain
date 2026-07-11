Now we're entering what I call the **heart of Pandas**.

Until now you've only looked at the dataset.

From this chapter onwards, you're actually **asking questions** to the data.

---

# Chapter 5 — Selecting Data (`[]`, `loc`, `iloc`)

This chapter is probably the **most frequently used** part of Pandas.

If someone uses Pandas every day, they use these commands **hundreds of times**.

---

# First, load the dataset

```python
import pandas as pd

df = pd.read_csv("titanic.csv")
```

Let's assume the data looks like this:

|PassengerId|Name|Sex|Age|Fare|
|---|---|---|---|---|
|1|Braund|male|22|7.25|
|2|Cumings|female|38|71.28|
|3|Heikkinen|female|26|7.92|
|4|Allen|male|35|53.10|

---

# There are TWO ways to find data

Think of your classroom.

You can identify a student by:

### Roll Number

```text
1
2
3
4
```

or

### Name

```text
Alex
Bob
Charlie
```

Pandas works the same way.

```text
Position

↓

iloc
```

```text
Label

↓

loc
```

This is the biggest idea of this chapter.

---

# 1. Selecting a Single Column

```python
df["Age"]
```

Output

```text
0    22
1    38
2    26
3    35
```

Notice something?

It returns a **Series**.

Because one column = one Series.

---

# 2. Selecting Multiple Columns

```python
df[["Name","Age"]]
```

Output

|Name|Age|
|---|--:|
|Braund|22|
|Cumings|38|
|Heikkinen|26|

Notice

```python
[
   ["Name","Age"]
]
```

The outer `[]` means "select columns."

The inner `[]` is a **Python list** of column names.

---

# Why two brackets?

This confuses almost everyone.

Imagine

```python
df["Age"]
```

means

> Give me one column.

Now imagine

```python
["Age","Fare"]
```

This is already a Python list.

So Pandas sees

```python
df[
    ["Age","Fare"]
]
```

You're telling it:

> Give me **these columns**.

---

# 3. Selecting Rows using `iloc`

`iloc` means:

> **Integer Location**

Think

```text
0
1
2
3
```

The syntax is:

```python
df.iloc[row]
```

Example

```python
df.iloc[0]
```

Output

First row

---

Second row

```python
df.iloc[1]
```

---

Multiple rows

```python
df.iloc[0:5]
```

Output

Rows

```text
0
1
2
3
4
```

Exactly like Python slicing.

---

# 4. Selecting Columns with `iloc`

Remember

```python
df.iloc[rows, columns]
```

Example

```python
df.iloc[:,0]
```

Meaning

```text
All rows

First column
```

Output

```text
PassengerId
```

---

Second column

```python
df.iloc[:,1]
```

---

First three columns

```python
df.iloc[:,0:3]
```

Output

```text
PassengerId

Name

Sex
```

---

# 5. Selecting Rows and Columns Together

```python
df.iloc[0:5,0:3]
```

Think

```text
Rows

0

↓

4

Columns

0

↓

2
```

Output

|PassengerId|Name|Sex|
|---|---|---|

Only a small rectangle of data.

---

# Visualize `iloc`

Imagine the DataFrame like a matrix.

```text
         0      1      2      3

0

1

2

3

4
```

Now

```python
df.iloc[1:4,0:2]
```

means

```text
Rows

1

2

3

Columns

0

1
```

Like cutting out a rectangle.

---

# 6. `loc`

Now things change.

Instead of numbers,

`loc` uses labels.

Suppose

```python
df.index=[
    "A",
    "B",
    "C",
    "D"
]
```

Now

```python
df.loc["A"]
```

returns

First row.

---

Multiple rows

```python
df.loc["A":"C"]
```

returns

```text
A

B

C
```

Notice something.

Unlike Python slicing,

**the ending label is included.**

---

# `loc` with Columns

```python
df.loc[:,"Name"]
```

means

```text
All rows

Column Name
```

---

Multiple columns

```python
df.loc[:,["Name","Age"]]
```

Output

|Name|Age|
|---|---|

---

# `loc` vs `iloc`

This interview question is asked everywhere.

|`iloc`|`loc`|
|---|---|
|Uses integer positions|Uses labels|
|End index excluded|End label included|
|Like Python lists|Like dictionaries|

Example

```python
df.iloc[0]
```

First row by position.

Example

```python
df.loc["Alex"]
```

Row labeled Alex.

---

# Real Example

```python
import pandas as pd

df = pd.read_csv("titanic.csv")

print(df["Age"])

print(df[["Name","Age"]])

print(df.iloc[0])

print(df.iloc[0:5])

print(df.iloc[0:5,0:3])

print(df.loc[:,["Name","Age"]])
```

---

# Dry Run

Suppose

|Index|Name|Age|
|---|---|---|
|0|Alex|21|
|1|Bob|22|
|2|Sam|20|

Now

```python
df.iloc[1]
```

Pandas thinks:

```text
Go to row number 1

↓

Return everything

↓

Bob
22
```

---

Now

```python
df["Age"]
```

Pandas thinks:

```text
Find column

↓

Age

↓

Return Series
```

---

Now

```python
df.iloc[0:2,0:2]
```

Pandas thinks

```text
Rows

0

1

Columns

0

1
```

Returns

|Name|Age|
|---|---|
|Alex|21|
|Bob|22|

---

# Mental Model

Keep this picture in your head:

```text
                 DataFrame

        Columns →→→→→→→→→

Rows ↓

        ┌──────────────────────┐
        │ Name Age Salary Dept │
        │ Alex 21 50000 IT     │
        │ Bob  22 60000 HR     │
        │ Sam  20 45000 IT     │
        └──────────────────────┘
```

- `df["Age"]` → one column.
    
- `df[["Age","Salary"]]` → multiple columns.
    
- `iloc` → cut by **row/column numbers**.
    
- `loc` → cut by **row/column labels**.
    

---

# 🎯 Practical Challenge

Using the Titanic dataset, try these yourself:

```python
import pandas as pd

df = pd.read_csv("titanic.csv")

# 1. Print only the Name column

# 2. Print Name and Age columns

# 3. Print the first row

# 4. Print the first 10 rows

# 5. Print rows 10 to 20

# 6. Print the first 5 rows and first 3 columns

# 7. Print only the Sex column using loc

# 8. Print Name, Sex, and Fare using loc
```

If you can do these comfortably, you've learned one of the most-used parts of Pandas.

---

## Next Chapter (Chapter 6)

This is where data analysis really begins.

You'll learn to answer questions like:

- "Show me only passengers older than 30."
    
- "Show only females."
    
- "Show only IT employees earning more than ₹50,000."
    

That chapter introduces **filtering with Boolean masks**, which is the foundation of almost every real-world data analysis task.