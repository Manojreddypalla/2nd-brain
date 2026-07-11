Excellent. Now we reach a topic that many beginners misuse.

# Chapter 13 — `apply()`, `map()`, `applymap()` & `lambda`

This chapter answers one question:

> **"What if Pandas doesn't already have the operation I need?"**

For example,

Suppose your manager says:

> Increase salary by **12%**

Easy.

```python
df["Salary"] * 1.12
```

But then they say:

> If salary > 50,000, deduct 10%.
> 
> Otherwise deduct 5%.

Now the built-in operators aren't enough.

That's where **apply()** comes in.

---

# First Rule

Don't immediately think

```python
apply()
```

Always ask yourself

> **Can Pandas already do this?**

For example

Wrong

```python
df["Age"].apply(lambda x: x + 1)
```

Better

```python
df["Age"] + 1
```

Vectorized operations are faster.

Use `apply()` only when your logic is **custom**.

---

# Mental Model

Imagine

```text
Salary

50000

60000

70000
```

Pandas will do

```text
50000

↓

Your Function

↓

Result

60000

↓

Your Function

↓

Result

70000

↓

Your Function

↓

Result
```

It sends **each value** to your function.

---

# Our Dataset

```python
import pandas as pd

df = pd.DataFrame({
    "Name":["Alex","Bob","Charlie"],
    "Salary":[50000,70000,90000]
})
```

---

# Method 1 — apply()

Suppose

Give everyone a ₹5,000 bonus.

First create a function.

```python
def bonus(salary):
    return salary + 5000
```

Apply it.

```python
df["Salary"].apply(bonus)
```

Output

```text
55000

75000

95000
```

---

# Visualize

```text
50000

↓

bonus()

↓

55000

70000

↓

bonus()

↓

75000

90000

↓

bonus()

↓

95000
```

---

# Same Thing Using lambda

Instead of

```python
def bonus(salary):
    return salary + 5000
```

Write

```python
lambda salary: salary + 5000
```

Then

```python
df["Salary"].apply(
    lambda salary: salary + 5000
)
```

Same output.

---

# What is lambda?

A lambda is simply

> **A function without a name.**

Normal function

```python
def square(x):
    return x*x
```

Lambda

```python
lambda x: x*x
```

Exactly the same.

---

# Conditional Logic

Suppose

If salary

Greater than 60,000

↓

Tax = 20%

Else

↓

Tax = 10%

```python
def tax(salary):
    if salary > 60000:
        return salary * 0.8
    else:
        return salary * 0.9
```

Now

```python
df["Salary"].apply(tax)
```

Output

|Salary|
|--:|
|45000|
|56000|
|72000|

---

# Using lambda for Conditions

```python
df["Salary"].apply(
    lambda x: x*0.8 if x>60000 else x*0.9
)
```

One line.

---

# apply() on Rows

Until now

```python
df["Salary"]
```

was one column.

Sometimes you need multiple columns.

Example

|Math|Science|
|--:|--:|
|80|90|
|70|85|

Need

```text
Total
```

Use

```python
df["Total"] = df.apply(
    lambda row: row["Math"] + row["Science"],
    axis=1
)
```

Notice

```python
axis=1
```

means

> Process one **row** at a time.

---

# axis=0 vs axis=1

Remember this forever.

```text
axis=0

↓

Columns
```

```text
axis=1

↓

Rows
```

Visual

```text
        Columns

↓

Age

Salary

Department

Rows →

Alex

Bob

Charlie
```

---

# map()

Suppose

```text
Gender

M

F

M
```

Need

```text
Male

Female

Male
```

Use

```python
df["Gender"].map({
    "M":"Male",
    "F":"Female"
})
```

Output

```text
Male

Female

Male
```

Very common.

---

# apply() vs map()

|map|apply|
|---|---|
|One column|One row or one column|
|Mostly replacing values|Custom calculations|

---

# applymap()

Suppose

|A|B|
|---|---|
|1|2|
|3|4|

Need

Multiply every cell by 10.

```python
df.applymap(
    lambda x: x*10
)
```

Output

|A|B|
|--:|--:|
|10|20|
|30|40|

**Note:** In newer versions of Pandas, `applymap()` has been superseded for some use cases. You'll still see it in tutorials, but for element-wise operations on a Series, prefer vectorized operations or `map()` where appropriate.

---

# Real Titanic Example

Suppose

Create Age Category.

```python
df["AgeGroup"] = df["Age"].apply(
    lambda age:
        "Child"
        if age < 18
        else "Adult"
)
```

Output

|Age|AgeGroup|
|--:|---|
|12|Child|
|35|Adult|

---

# Real Netflix Example

Suppose

Movie duration

```text
90 min

150 min
```

Need

Long

Short

```python
netflix["Length"] = netflix["duration"].apply(
    lambda x:
        "Long"
        if "150" in str(x)
        else "Short"
)
```

---

# Dry Run

Suppose

```python
df["Salary"].apply(
    lambda x:x+5000
)
```

Internally

```text
50000

↓

lambda

↓

55000

70000

↓

lambda

↓

75000

90000

↓

lambda

↓

95000
```

Then Pandas builds a new Series.

---

# SQL Comparison

This is where SQL becomes different.

SQL has

```sql
CASE

WHEN

ELSE
```

Example

```sql
CASE

WHEN Salary > 60000

THEN 'High'

ELSE 'Low'
```

Pandas

```python
df["Salary"].apply(
    lambda x:
        "High"
        if x>60000
        else "Low"
)
```

Very similar logic.

---

# Cheat Sheet

|Task|Command|
|---|---|
|Apply function|`.apply()`|
|Replace values|`.map()`|
|Anonymous function|`lambda`|
|Row-wise apply|`axis=1`|
|Column-wise apply|`axis=0`|
|Element-wise DataFrame|`applymap()`|

---

# Practice Challenge

Using the Titanic dataset:

```python
import pandas as pd

df = pd.read_csv("titanic.csv")
```

Try these:

1. Create a new column `AgeGroup`:
    
    - Age < 18 → `"Child"`
        
    - Otherwise → `"Adult"`
        
2. Create a new column `FareCategory`:
    
    - Fare > 50 → `"High"`
        
    - Otherwise → `"Low"`
        
3. Convert the `Sex` column:
    
    - `"male"` → `"M"`
        
    - `"female"` → `"F"`
        
4. Create a column `DoubleFare` by multiplying `Fare` by 2.
    
5. If you create a small DataFrame with only numeric values, try multiplying every value by 10.
    

---

# 🔥 A Rule Used by Experienced Pandas Users

Whenever you think:

> **"I need a loop."**

Stop and ask:

1. **Can I use a vectorized operation?** (`+`, `-`, `*`, `/`, comparisons)
    
2. **Can I use a built-in Pandas method?**
    
3. **Can I use `map()`?**
    
4. **Only then use `apply()`.**
    

That order leads to cleaner and faster code.

---

## Next Chapter (Chapter 14)

This is the final major Pandas concept:

# **Pivot Tables**

If `groupby()` gives you summaries, **pivot tables** let you reshape those summaries into report-friendly tables—very similar to Excel Pivot Tables.

By the end of that chapter, you'll know enough Pandas to tackle the **Netflix**, **Titanic**, and **Sales** projects from scratch.