# Chapter 12 — Merge, Join & Concat

If `groupby()` is like SQL's **GROUP BY**,

then **`merge()` is SQL's JOIN**.

This chapter is extremely important because **real-world data is rarely stored in one table**.

---

# The Problem

Imagine you're building an e-commerce dashboard.

You have two tables.

### Customers

|CustomerID|Name|
|---|---|
|1|Alex|
|2|Bob|
|3|Charlie|

### Orders

|CustomerID|Product|
|---|---|
|1|Laptop|
|2|Phone|
|1|Keyboard|

Question:

> **Which customer bought which product?**

You cannot answer that from one table alone.

You must combine them.

---

# SQL Thinking

```sql
SELECT *
FROM Customers
JOIN Orders
ON Customers.CustomerID = Orders.CustomerID;
```

Pandas does the same thing.

---

# merge()

```python
import pandas as pd

customers = pd.DataFrame({
    "CustomerID": [1,2,3],
    "Name": ["Alex","Bob","Charlie"]
})

orders = pd.DataFrame({
    "CustomerID": [1,2,1],
    "Product": ["Laptop","Phone","Keyboard"]
})

result = pd.merge(
    customers,
    orders,
    on="CustomerID"
)

print(result)
```

Output

|CustomerID|Name|Product|
|---|---|---|
|1|Alex|Laptop|
|1|Alex|Keyboard|
|2|Bob|Phone|

Charlie doesn't appear because he has no orders.

---

# Visualize What's Happening

Customers

```text
1 → Alex

2 → Bob

3 → Charlie
```

Orders

```text
1 → Laptop

2 → Phone

1 → Keyboard
```

Pandas matches the common key.

```text
CustomerID

↓

1

↓

Alex

↓

Laptop

Keyboard
```

---

# Why `on="CustomerID"`?

This tells Pandas

> **"Match rows using this column."**

Think of it as the common key between tables.

---

# Types of Merge

Exactly like SQL.

---

## 1. Inner Join (Default)

Only matching records.

```python
pd.merge(customers, orders, on="CustomerID")
```

Result

|CustomerID|Name|Product|
|---|---|---|
|1|Alex|Laptop|
|1|Alex|Keyboard|
|2|Bob|Phone|

Charlie disappears.

---

## 2. Left Join

Keep everything from the left table.

```python
pd.merge(
    customers,
    orders,
    on="CustomerID",
    how="left"
)
```

Output

|CustomerID|Name|Product|
|---|---|---|
|1|Alex|Laptop|
|1|Alex|Keyboard|
|2|Bob|Phone|
|3|Charlie|NaN|

Charlie stays.

---

## 3. Right Join

Keep everything from the right table.

```python
pd.merge(
    customers,
    orders,
    how="right",
    on="CustomerID"
)
```

---

## 4. Outer Join

Keep everything.

```python
pd.merge(
    customers,
    orders,
    how="outer",
    on="CustomerID"
)
```

Nothing gets removed.

---

# Merge on Different Column Names

Sometimes

Customers

```text
CustomerID
```

Orders

```text
Cust_ID
```

Now

```python
pd.merge(
    customers,
    orders,
    left_on="CustomerID",
    right_on="Cust_ID"
)
```

---

# Concat

Merge combines tables **horizontally**.

Concat combines them **vertically** (or horizontally if requested).

Imagine

January Sales

|Month|Sales|
|---|--:|
|Jan|100|
|Jan|200|

February Sales

|Month|Sales|
|---|--:|
|Feb|300|
|Feb|500|

Now

```python
pd.concat([jan, feb])
```

Output

|Month|Sales|
|---|--:|
|Jan|100|
|Jan|200|
|Feb|300|
|Feb|500|

Think

```text
Stack

↓

One on top of another
```

---

# Horizontal Concat

```python
pd.concat(
    [df1, df2],
    axis=1
)
```

Instead of stacking rows,

it adds columns.

---

# Join

Pandas also has

```python
df.join()
```

It's mainly used when two DataFrames already share the same index.

For beginners, remember:

> **Most of the time, you'll use `merge()`.**

---

# Real Example

Imagine

### Employee Table

|ID|Name|
|---|---|
|1|Alex|
|2|Bob|

### Salary Table

|ID|Salary|
|---|--:|
|1|50000|
|2|70000|

Merge

```python
employees.merge(
    salary,
    on="ID"
)
```

Output

|ID|Name|Salary|
|---|---|--:|
|1|Alex|50000|
|2|Bob|70000|

---

# SQL Comparison

|SQL|Pandas|
|---|---|
|`INNER JOIN`|`merge()`|
|`LEFT JOIN`|`merge(..., how="left")`|
|`RIGHT JOIN`|`merge(..., how="right")`|
|`FULL OUTER JOIN`|`merge(..., how="outer")`|
|`UNION ALL`|`concat()`|

---

# Dry Run

Suppose

Customers

```text
1 Alex

2 Bob
```

Orders

```text
1 Laptop

2 Phone
```

Pandas internally does:

```text
CustomerID = 1

↓

Find CustomerID = 1

↓

Combine

↓

Alex Laptop
```

Then repeats for every matching key.

---

# Cheat Sheet

|Task|Command|
|---|---|
|Merge|`pd.merge()`|
|Inner Join|`how="inner"`|
|Left Join|`how="left"`|
|Right Join|`how="right"`|
|Outer Join|`how="outer"`|
|Merge different keys|`left_on`, `right_on`|
|Stack rows|`pd.concat()`|
|Add columns|`pd.concat(axis=1)`|

---

# 🧪 Practice Challenge

```python
import pandas as pd

students = pd.DataFrame({
    "ID": [1,2,3],
    "Name": ["Alex","Bob","Charlie"]
})

marks = pd.DataFrame({
    "ID": [1,2,3],
    "Marks": [85,90,78]
})
```

Try:

1. Merge the two DataFrames.
    
2. Add another student with no marks and perform a left join.
    
3. Create two monthly sales DataFrames and combine them using `concat()`.
    
4. Try an outer join and observe the `NaN` values.
    

---

# 📍Progress Check

At this point you've learned about **90% of the Pandas API you'll use regularly**:

- ✅ Series
    
- ✅ DataFrame
    
- ✅ Reading/Writing Data
    
- ✅ Exploring Data
    
- ✅ Selecting (`loc` / `iloc`)
    
- ✅ Filtering
    
- ✅ Sorting
    
- ✅ Missing Data
    
- ✅ String Operations
    
- ✅ Date & Time
    
- ✅ GroupBy
    
- ✅ Merge & Concat
    

The remaining core topics are:

- **Chapter 13:** `apply()`, `map()`, `lambda`
    
- **Chapter 14:** Pivot Tables
    
- **Chapter 15:** Real Projects (Netflix, Titanic, Sales)
    

These chapters focus on combining everything you've learned into practical data analysis workflows.