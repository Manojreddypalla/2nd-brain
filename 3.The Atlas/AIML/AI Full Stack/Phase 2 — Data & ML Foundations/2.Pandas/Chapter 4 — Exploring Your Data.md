Awesome. Now we move to what I consider the **first real Pandas chapter**.

Everything before this was just getting data into memory.

Now we actually start **working with it**.

---

# Chapter 4 — Exploring Your Data

Imagine your manager sends you:

```text
sales_2026.csv
```

You have **no idea**:

- How many rows?
    
- What columns?
    
- Missing values?
    
- Data types?
    
- Is Salary text or number?
    
- Is Date actually a date?
    

You don't immediately start analyzing.

You **inspect** the dataset first.

Think of it like entering a new city.

You don't start driving randomly.

You first look at the map.

---

# Our Dataset

Suppose

```python
import pandas as pd

df = pd.read_csv("titanic.csv")
```

Everything below assumes you've loaded it.

---

# 1. head()

Shows the first few rows.

```python
df.head()
```

Default

```text
First 5 rows
```

You can change it.

```python
df.head(10)
```

Shows first 10 rows.

Imagine

```text
891 rows
```

Instead of printing all of them,

Pandas gives you just a preview.

---

# 2. tail()

Exactly opposite.

```python
df.tail()
```

Shows

```text
Last 5 rows
```

Useful for checking whether

- reading completed
    
- sorting worked
    
- appending worked
    

---

# 3. shape

Probably the second command everyone runs.

```python
df.shape
```

Output

```text
(891,12)
```

Meaning

```text
Rows

↓

891

Columns

↓

12
```

Notice

No parentheses.

Not

```python
df.shape()
```

Because `shape` is an **attribute**, not a function.

---

# Function vs Attribute

This confuses beginners.

## Function

Needs parentheses.

```python
df.head()
```

Because it performs an action.

---

## Attribute

Just stores information.

```python
df.shape
```

Think

```text
Car

↓

Color

↓

Blue
```

You don't call

```python
car.color()
```

Same idea.

---

# 4. columns

```python
df.columns
```

Output

```text
PassengerId

Survived

Age

Sex

Fare
...
```

Internally

```text
Index([
PassengerId,
Survived,
Age,
...
])
```

This tells you every column name.

---

# 5. index

```python
df.index
```

Output

```text
RangeIndex

0

↓

890
```

Meaning

```text
Row labels

0

1

2

...

890
```

---

# 6. dtypes

Shows data type of every column.

```python
df.dtypes
```

Output

```text
PassengerId    int64

Age            float64

Name           object

Fare           float64
```

Very useful.

Sometimes numbers accidentally become text.

You'll catch that here.

---

# 7. info()

One of the most useful commands.

```python
df.info()
```

Example

```text
RangeIndex: 891 entries

12 columns

Age float64

Name object

Fare float64

Cabin object
```

It tells you

- rows
    
- columns
    
- data types
    
- missing values
    
- memory usage
    

This is usually the **first diagnostic command** after loading a dataset.

---

# Why is info() so useful?

Imagine

```text
Age

21

23

NaN

25

NaN
```

You might think there are

```text
891 Ages
```

But

```python
df.info()
```

might show

```text
Age

714 non-null
```

Meaning

```text
177 values missing
```

Now you know cleaning is needed.

---

# 8. describe()

Statistics in one command.

```python
df.describe()
```

Example

```text
Age

count

714

mean

29.7

std

14.5

min

0.42

max

80
```

You instantly know

- average
    
- spread
    
- smallest value
    
- largest value
    

No calculations required.

---

# 9. describe(include="object")

By default

```python
df.describe()
```

only shows numbers.

Want text?

```python
df.describe(include="object")
```

Now

```text
Sex

count

891

unique

2

top

male

freq

577
```

Very useful.

---

# 10. Sample

Sometimes

```python
head()
```

is misleading.

Use

```python
df.sample(5)
```

Random rows.

Every run changes.

Great for large datasets.

---

# Practical Example

```python
import pandas as pd

df = pd.read_csv("titanic.csv")

print(df.head())

print(df.tail())

print(df.shape)

print(df.columns)

print(df.index)

print(df.dtypes)

print(df.info())

print(df.describe())

print(df.sample(5))
```

---

# Mental Model

Whenever you receive **any** dataset:

```text
CSV
        │
        ▼
read_csv()
        │
        ▼
DataFrame
        │
        ▼
head()
tail()
shape
columns
info()
describe()
sample()
```

These commands answer the first questions:

- **What does the data look like?** → `head()`, `tail()`
    
- **How big is it?** → `shape`
    
- **What fields exist?** → `columns`
    
- **What types are they?** → `dtypes`
    
- **Are there missing values?** → `info()`
    
- **What are the basic statistics?** → `describe()`
    
- **What do random records look like?** → `sample()`
    

---

# 🧪 Practical Exercise

Download the **Titanic dataset** and run **only** these commands:

```python
import pandas as pd

df = pd.read_csv("titanic.csv")

print(df.head())
print(df.tail())
print(df.shape)
print(df.columns)
print(df.index)
print(df.dtypes)
print(df.info())
print(df.describe())
print(df.describe(include="object"))
print(df.sample(5))
```

Don't worry about understanding every output yet. Just observe what each command returns.

---

## 🔜 Next Chapter (Chapter 5)

This is where Pandas starts feeling powerful.

You'll learn:

- Selecting one column
    
- Selecting multiple columns
    
- Selecting rows
    
- `loc`
    
- `iloc`
    
- Row slicing
    
- Column slicing
    

This chapter is where you'll begin querying data the way professionals do.