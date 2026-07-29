# Chapter 7 — Sorting & Ranking

Think about Google Search.

When you search:

```text
Best laptops
```

Google doesn't give results randomly.

It sorts them by relevance.

Amazon sorts by:

- Price
    
- Rating
    
- Popularity
    

Netflix sorts by:

- Release Date
    
- Trending
    
- Rating
    

Sorting is everywhere.

---

# Our Dataset

```python
import pandas as pd

employees = {
    "Name": ["Alex", "Bob", "Charlie", "Emma", "David"],
    "Age": [21, 25, 32, 28, 40],
    "Department": ["IT", "HR", "IT", "Finance", "IT"],
    "Salary": [50000, 60000, 85000, 75000, 120000]
}

df = pd.DataFrame(employees)

print(df)
```

Output

|Name|Age|Department|Salary|
|---|--:|---|--:|
|Alex|21|IT|50000|
|Bob|25|HR|60000|
|Charlie|32|IT|85000|
|Emma|28|Finance|75000|
|David|40|IT|120000|

---

# 1. Sort by One Column

Suppose HR asks:

> Sort employees by salary.

```python
df.sort_values(by="Salary")
```

Output

|Name|Salary|
|---|--:|
|Alex|50000|
|Bob|60000|
|Emma|75000|
|Charlie|85000|
|David|120000|

Notice

Original DataFrame is **not changed**.

---

# Why?

Pandas usually returns a **new DataFrame**.

```
Original DataFrame
        │
sort_values()
        │
        ▼
New Sorted DataFrame
```

If you want to keep it

```python
df = df.sort_values(by="Salary")
```

---

# 2. Descending Order

Highest salary first.

```python
df.sort_values(
    by="Salary",
    ascending=False
)
```

Output

|Name|Salary|
|---|--:|
|David|120000|
|Charlie|85000|
|Emma|75000|
|Bob|60000|
|Alex|50000|

---

# 3. Sort by Age

```python
df.sort_values(by="Age")
```

---

# 4. Sort by Multiple Columns

Suppose

You want

```
Department

↓

Salary
```

Meaning

Inside every department,

sort employees by salary.

```python
df.sort_values(
    by=["Department","Salary"]
)
```

Imagine

```
Finance

↓

75000

HR

↓

60000

IT

↓

50000

↓

85000

↓

120000
```

---

# Different Order for Each Column

Suppose

Departments

```
A → Z
```

Salary

```
Highest → Lowest
```

```python
df.sort_values(
    by=["Department","Salary"],
    ascending=[True,False]
)
```

Notice

`ascending` becomes a list.

One value per column.

---

# 5. Sort by Index

Remember every DataFrame has an index.

```
0

1

2

3
```

Suppose

You changed it.

```python
df.index = [
    "EMP3",
    "EMP1",
    "EMP5",
    "EMP2",
    "EMP4"
]
```

Now

```python
df.sort_index()
```

Output

```
EMP1

EMP2

EMP3

EMP4

EMP5
```

---

# 6. Largest Values

Imagine

100,000 employees.

Need

Top 10 salaries.

Don't sort everything.

Use

```python
df.nlargest(
    3,
    "Salary"
)
```

Output

|Name|Salary|
|---|--:|
|David|120000|
|Charlie|85000|
|Emma|75000|

Much faster than sorting the whole DataFrame.

---

# 7. Smallest Values

```python
df.nsmallest(
    2,
    "Salary"
)
```

Output

|Name|Salary|
|---|--:|
|Alex|50000|
|Bob|60000|

---

# 8. Rank

Suppose

```python
df["Salary Rank"] = df["Salary"].rank(
    ascending=False
)
```

Output

|Name|Salary|Rank|
|---|--:|--:|
|Alex|50000|5|
|Bob|60000|4|
|Emma|75000|3|
|Charlie|85000|2|
|David|120000|1|

Useful in reports.

---

# Sorting Strings

Sort names alphabetically.

```python
df.sort_values(
    by="Name"
)
```

Output

```
Alex

Bob

Charlie

David

Emma
```

---

# Practical Titanic Examples

Highest fare.

```python
df.sort_values(
    by="Fare",
    ascending=False
)
```

Oldest passengers.

```python
df.sort_values(
    by="Age",
    ascending=False
)
```

Top five oldest.

```python
df.nlargest(
    5,
    "Age"
)
```

Youngest five.

```python
df.nsmallest(
    5,
    "Age"
)
```

---

# Dry Run

Suppose

|Name|Salary|
|---|--:|
|Alex|50000|
|Bob|70000|
|Charlie|60000|

Now

```python
df.sort_values("Salary")
```

Internally Pandas does something like:

```
50000

60000

70000
```

Then moves the **entire rows**, not just the salary column.

Result

|Name|Salary|
|---|--:|
|Alex|50000|
|Charlie|60000|
|Bob|70000|

This is important.

Pandas **never separates** a value from its row.

---

# Real Industry Example

Suppose you're analyzing Amazon orders.

Your manager asks:

> Show the **10 most expensive orders**.

```python
orders.nlargest(10, "Price")
```

Or

> Show the **lowest-rated products**.

```python
products.nsmallest(10, "Rating")
```

Or

> Sort customers by city and then by purchase amount.

```python
customers.sort_values(
    by=["City", "Purchase"]
)
```

These are everyday analysis tasks.

---

# Cheat Sheet

|Task|Command|
|---|---|
|Sort ascending|`df.sort_values("Salary")`|
|Sort descending|`df.sort_values("Salary", ascending=False)`|
|Sort multiple columns|`df.sort_values(["Dept","Salary"])`|
|Sort index|`df.sort_index()`|
|Top N|`df.nlargest(5, "Salary")`|
|Bottom N|`df.nsmallest(5, "Salary")`|
|Rank values|`df["Salary"].rank()`|

---

# 🧪 Practical Challenge

Using the Titanic dataset, try these:

```python
import pandas as pd

df = pd.read_csv("titanic.csv")

# 1. Sort passengers by Age.

# 2. Sort passengers by Fare (highest first).

# 3. Show the 10 oldest passengers.

# 4. Show the 10 youngest passengers.

# 5. Sort by Pclass and then Fare.

# 6. Show the 5 highest fares.

# 7. Show the 5 smallest fares.

# 8. Rank passengers by Fare.
```

---

## Where we are now

You've learned to:

- ✅ Load data
    
- ✅ Explore it
    
- ✅ Select rows and columns
    
- ✅ Filter records
    
- ✅ Sort and rank
    

At this point, you can already answer a lot of business questions.

The next chapter is where you learn to deal with something that **every real dataset has**:

> **Missing Data (NaN values)**

If you don't know how to handle missing values, your machine learning models and analyses can produce incorrect results. That's why Chapter 8 is one of the most practical parts of Pandas.