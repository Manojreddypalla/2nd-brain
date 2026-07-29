# Chapter 6 — Filtering Data (Boolean Indexing)

## The Mental Model

Imagine Excel.

You click the filter button.

```
Age ▼
```

Then choose

```
✓ Age > 30
```

Excel hides the other rows.

Pandas does exactly the same thing with code.

---

## Our Dataset

```python
import pandas as pd

employees = {
    "Name": ["Alex", "Bob", "Charlie", "Emma", "David"],
    "Age": [21, 25, 32, 28, 40],
    "Department": ["IT", "HR", "IT", "Finance", "IT"],
    "Salary": [50000, 60000, 85000, 75000, 120000]
}

df = pd.DataFrame(employees)
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

# Step 1 — A Simple Condition

```python
df["Age"] > 25
```

Output

```text
0    False
1    False
2     True
3     True
4     True
dtype: bool
```

Wait...

Where's the data?

There isn't any.

Pandas first creates something called a **Boolean Mask**.

---

## What is a Boolean Mask?

Think of it as a filter.

```
Age

21

25

32

28

40
```

Condition

```
Age > 25
```

Internally

```
21  → False

25  → False

32  → True

28  → True

40  → True
```

That's the mask.

---

# Step 2 — Use the Mask

```python
df[df["Age"] > 25]
```

Output

|Name|Age|Department|Salary|
|---|--:|---|--:|
|Charlie|32|IT|85000|
|Emma|28|Finance|75000|
|David|40|IT|120000|

Think of it like this:

```
DataFrame

↓

Boolean Mask

↓

Keep only True rows
```

---

# Why Two `df`s?

This is the most confusing line in Pandas:

```python
df[df["Age"] > 25]
```

Break it down.

First

```python
df["Age"] > 25
```

creates

```
False

False

True

True

True
```

Then

```python
df[
    mask
]
```

means

> Keep only rows where the mask is `True`.

It's exactly the same as writing:

```python
mask = df["Age"] > 25

df[mask]
```

Much easier to understand.

---

# Equal To

```python
df[df["Department"] == "IT"]
```

Output

Only IT employees.

Notice:

```python
==
```

means comparison.

Not

```python
=
```

which means assignment.

---

# Greater Than

```python
df[df["Salary"] > 70000]
```

---

# Less Than

```python
df[df["Age"] < 30]
```

---

# Greater Than or Equal

```python
df[df["Salary"] >= 75000]
```

---

# Less Than or Equal

```python
df[df["Age"] <= 25]
```

---

# Not Equal

```python
df[df["Department"] != "HR"]
```

Everything except HR.

---

# Multiple Conditions (AND)

Suppose you want

> IT employees earning more than ₹60,000.

```python
df[
    (df["Department"] == "IT") &
    (df["Salary"] > 60000)
]
```

Output

Charlie

David

---

## Why Parentheses?

Python sees

```python
&
```

as an operator.

Each comparison must be completed first.

Correct

```python
(df["Age"] > 25)
```

Wrong

```python
df["Age"] > 25 &
```

Without parentheses, Python doesn't know how to group the operations.

---

# Multiple Conditions (OR)

Suppose

> IT OR Finance.

```python
df[
    (df["Department"] == "IT") |
    (df["Department"] == "Finance")
]
```

---

# NOT

```python
df[
    ~(df["Department"] == "HR")
]
```

The `~` operator means "invert the condition."

---

# isin()

Instead of writing

```python
(df["Department"] == "IT") |
(df["Department"] == "HR")
```

Use

```python
df[
    df["Department"].isin(["IT", "HR"])
]
```

Much cleaner.

---

# between()

Instead of

```python
(df["Age"] >= 20) &
(df["Age"] <= 30)
```

Write

```python
df[
    df["Age"].between(20,30)
]
```

Very common.

---

# Filter Specific Columns

Want only names of IT employees?

```python
df.loc[
    df["Department"] == "IT",
    ["Name","Salary"]
]
```

Output

|Name|Salary|
|---|--:|
|Alex|50000|
|Charlie|85000|
|David|120000|

Notice the pattern:

```
Rows

↓

Condition

Columns

↓

Name

Salary
```

---

# Practical Example with Titanic

Passengers older than 50.

```python
df[df["Age"] > 50]
```

Only females.

```python
df[df["Sex"] == "female"]
```

Passengers paying more than ₹100.

```python
df[df["Fare"] > 100]
```

Female passengers older than 30.

```python
df[
    (df["Sex"] == "female") &
    (df["Age"] > 30)
]
```

First-class OR second-class passengers.

```python
df[
    df["Pclass"].isin([1,2])
]
```

---

# Dry Run

Suppose

|Name|Age|
|---|--:|
|Alex|21|
|Bob|25|
|Charlie|32|

Now

```python
df["Age"] > 25
```

Pandas creates

```
21

↓

False

25

↓

False

32

↓

True
```

Then

```python
df[mask]
```

becomes

```
False

↓

Discard

False

↓

Discard

True

↓

Keep
```

Final result

|Name|Age|
|---|--:|
|Charlie|32|

---

# Cheat Sheet

|Operation|Example|
|---|---|
|Equal|`df[df["Age"] == 20]`|
|Greater|`df[df["Age"] > 20]`|
|Less|`df[df["Age"] < 20]`|
|AND|`(cond1) & (cond2)`|
|OR|`(cond1) \| (cond2)`|
|NOT|`~(condition)`|
|In List|`isin([...])`|
|Between|`between(a,b)`|

---

# Real-World Thinking

Imagine you're a data analyst at Amazon.

Your manager asks:

- "Show orders above ₹50,000."
    
- "Show orders from Hyderabad."
    
- "Show customers who bought laptops."
    
- "Show premium users from India."
    

Every single one of these is **just filtering**.

That's why this is one of the most important Pandas skills.

---

# 🧪 Practice Challenge (Titanic)

Try solving these yourself:

```python
import pandas as pd

df = pd.read_csv("titanic.csv")

# 1. Show only female passengers

# 2. Show passengers older than 30

# 3. Show passengers younger than 18

# 4. Show passengers who paid more than 50

# 5. Show female passengers older than 30

# 6. Show first or second class passengers

# 7. Show passengers whose age is between 20 and 40

# 8. Show only Name, Age, and Fare for passengers older than 40
```

If you can solve these without looking at the notes, you're already using Pandas the way it's used in real data analysis.

---

## Next Chapter (Chapter 7)

We'll learn **Sorting & Ranking**:

- `sort_values()`
    
- `sort_index()`
    
- `nlargest()`
    
- `nsmallest()`
    
- Ranking data
    
- Sorting by multiple columns
    

This is how you answer questions like:

- "Who are the top 10 highest-paid employees?"
    
- "What are the cheapest products?"
    
- "Which movies have the highest ratings?"