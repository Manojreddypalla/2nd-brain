Perfect. Now we reach **the single most important feature in Pandas**.

# Chapter 11 — GroupBy

If I had to choose **one Pandas feature** that every data analyst must know, it would be **`groupby()`**.

Most business questions are actually GroupBy questions.

---

# Imagine You're the CEO

You have this data:

|Employee|Department|Salary|
|---|---|--:|
|Alex|IT|50000|
|Bob|HR|60000|
|Charlie|IT|80000|
|Emma|Finance|70000|
|David|IT|90000|

Now you ask:

> **"What is the average salary in each department?"**

Notice something.

You don't want individual employees anymore.

You want **one result per department**.

This is exactly what `groupby()` does.

---

# The Mental Model

Always remember these **three steps**:

```text
Split
   ↓
Apply
   ↓
Combine
```

This is the entire philosophy of `groupby()`.

### Step 1: Split

Split the data into groups.

```text
IT
Alex
Charlie
David

HR
Bob

Finance
Emma
```

---

### Step 2: Apply

Do some operation on each group.

Example:

```text
Average Salary
```

IT

```text
50000
80000
90000

↓

73333
```

HR

```text
60000

↓

60000
```

Finance

```text
70000

↓

70000
```

---

### Step 3: Combine

Return one table.

|Department|Average Salary|
|---|--:|
|Finance|70000|
|HR|60000|
|IT|73333|

That's GroupBy.

---

# Basic Syntax

```python
df.groupby("Department")
```

This only creates the groups.

Nothing is calculated yet.

---

# Mean

```python
df.groupby("Department")["Salary"].mean()
```

Output

```text
Department

Finance    70000

HR         60000

IT         73333
```

---

# Sum

```python
df.groupby("Department")["Salary"].sum()
```

Output

```text
Finance    70000

HR         60000

IT        220000
```

---

# Count

How many employees?

```python
df.groupby("Department")["Employee"].count()
```

Output

```text
Finance    1

HR         1

IT         3
```

---

# Maximum

Highest salary in each department.

```python
df.groupby("Department")["Salary"].max()
```

Output

```text
Finance    70000

HR         60000

IT         90000
```

---

# Minimum

```python
df.groupby("Department")["Salary"].min()
```

---

# Multiple Calculations

Instead of writing

```python
mean()

max()

min()

count()
```

separately

use

```python
df.groupby("Department")["Salary"].agg(
    ["mean", "max", "min", "count"]
)
```

Output

|Department|Mean|Max|Min|Count|
|---|--:|--:|--:|--:|
|Finance|70000|70000|70000|1|
|HR|60000|60000|60000|1|
|IT|73333|90000|50000|3|

---

# Group by Multiple Columns

Suppose

|Department|Gender|Salary|
|---|---|--:|
|IT|Male|50000|
|IT|Female|70000|
|HR|Female|60000|

Now

```python
df.groupby(
    ["Department", "Gender"]
)["Salary"].mean()
```

Output

```text
Department Gender

HR Female

60000

IT Female

70000

IT Male

50000
```

---

# Reset Index

Notice

```python
df.groupby("Department")["Salary"].mean()
```

returns Department as the index.

If you want a normal DataFrame

```python
df.groupby(
    "Department"
)["Salary"].mean().reset_index()
```

Output

|Department|Salary|
|---|--:|
|Finance|70000|
|HR|60000|
|IT|73333|

You'll use `reset_index()` a lot.

---

# Real Titanic Example

Average age by gender.

```python
df.groupby("Sex")["Age"].mean()
```

Output

```text
female

27.9

male

30.7
```

---

Passengers in each class.

```python
df.groupby("Pclass")["PassengerId"].count()
```

Output

```text
1

216

2

184

3

491
```

---

Average fare by passenger class.

```python
df.groupby("Pclass")["Fare"].mean()
```

---

# Real Netflix Example

Suppose

```text
Genre
```

exists.

Movies per genre.

```python
netflix.groupby("listed_in")["title"].count()
```

---

Average release year.

```python
netflix.groupby("type")["release_year"].mean()
```

---

# Real Sales Example

|Month|Sales|
|---|--:|
|Jan|100|
|Jan|200|
|Feb|300|
|Feb|500|

Now

```python
sales.groupby("Month")["Sales"].sum()
```

Output

|Month|Sales|
|---|--:|
|Jan|300|
|Feb|800|

Exactly what businesses do every day.

---

# `value_counts()`

Sometimes you only want counts.

Instead of

```python
df.groupby("Department")["Department"].count()
```

write

```python
df["Department"].value_counts()
```

Output

```text
IT

3

HR

1

Finance

1
```

Much shorter.

---

# Dry Run

Suppose

|Dept|Salary|
|---|--:|
|IT|50|
|HR|60|
|IT|80|
|Finance|70|
|IT|90|

Now

```python
df.groupby("Dept")["Salary"].mean()
```

Internally

Split

```text
IT

50

80

90
```

HR

```text
60
```

Finance

```text
70
```

Apply

```text
Mean
```

IT

```text
73.3
```

HR

```text
60
```

Finance

```text
70
```

Combine

|Dept|Mean|
|---|--:|
|Finance|70|
|HR|60|
|IT|73.3|

---

# SQL Comparison

This is where your SQL knowledge helps a lot.

|SQL|Pandas|
|---|---|
|`GROUP BY Department`|`groupby("Department")`|
|`AVG(Salary)`|`.mean()`|
|`SUM(Salary)`|`.sum()`|
|`COUNT(*)`|`.count()`|
|`MAX(Salary)`|`.max()`|
|`MIN(Salary)`|`.min()`|

Example:

**SQL**

```sql
SELECT Department,
AVG(Salary)
FROM employees
GROUP BY Department;
```

**Pandas**

```python
df.groupby("Department")["Salary"].mean()
```

Almost the same logic.

---

# Cheat Sheet

|Task|Command|
|---|---|
|Group|`groupby()`|
|Mean|`.mean()`|
|Sum|`.sum()`|
|Count|`.count()`|
|Max|`.max()`|
|Min|`.min()`|
|Multiple stats|`.agg([...])`|
|Frequency|`.value_counts()`|
|Convert back to DataFrame|`.reset_index()`|

---

# 🧪 Practice Challenge (Titanic)

Try these on the Titanic dataset:

```python
import pandas as pd

df = pd.read_csv("titanic.csv")

# 1. Average age by Sex

# 2. Average Fare by Pclass

# 3. Number of passengers in each Pclass

# 4. Maximum Fare by Pclass

# 5. Minimum Age by Sex

# 6. Average Age and Fare by Sex

# 7. Count passengers by Embarked

# 8. Use agg() to get mean, max, and min Fare for each Pclass
```

---

# 🎯 What you've learned so far

At this point, you can already perform most everyday data analysis tasks:

- ✅ Read data
    
- ✅ Explore data
    
- ✅ Select rows/columns
    
- ✅ Filter
    
- ✅ Sort
    
- ✅ Handle missing values
    
- ✅ Clean strings
    
- ✅ Work with dates
    
- ✅ Group and aggregate
    

The next chapter is **Merge & Join**, where you'll learn how to combine multiple DataFrames—the Pandas equivalent of SQL `JOIN`s. This is another essential skill because real-world data is often spread across multiple tables rather than stored in one large file.