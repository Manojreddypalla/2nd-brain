Excellent. 🎉 This is the **last major Pandas concept**.

# Chapter 14 — Pivot Tables

If `groupby()` summarizes data,

**Pivot Tables summarize and reshape data** into a report.

If you've ever used **Excel Pivot Tables**, this is the same idea.

---

# Imagine You're a Manager

You have sales data.

|Salesperson|Product|Sales|
|---|---|--:|
|Alex|Laptop|1000|
|Alex|Phone|500|
|Bob|Laptop|1200|
|Bob|Phone|800|
|Charlie|Laptop|900|

Your manager asks:

> "Show me total sales for each salesperson."

Easy with `groupby()`.

But then asks:

> "I want salespeople as rows, products as columns."

That's where **pivot tables** shine.

---

# Step 1 — Create Data

```python
import pandas as pd

sales = pd.DataFrame({
    "Salesperson": ["Alex", "Alex", "Bob", "Bob", "Charlie"],
    "Product": ["Laptop", "Phone", "Laptop", "Phone", "Laptop"],
    "Sales": [1000, 500, 1200, 800, 900]
})

print(sales)
```

Output

|Salesperson|Product|Sales|
|---|---|--:|
|Alex|Laptop|1000|
|Alex|Phone|500|
|Bob|Laptop|1200|
|Bob|Phone|800|
|Charlie|Laptop|900|

---

# Step 2 — Basic Pivot Table

```python
pivot = sales.pivot_table(
    index="Salesperson",
    values="Sales",
    aggfunc="sum"
)

print(pivot)
```

Output

|Salesperson|Sales|
|---|--:|
|Alex|1500|
|Bob|2000|
|Charlie|900|

---

# What happened?

Think of it like:

```text
Salesperson

↓

Group Data

↓

Sum Sales

↓

Report
```

Looks similar to `groupby()`.

---

# Step 3 — Add Columns

Now make Products into columns.

```python
pivot = sales.pivot_table(
    index="Salesperson",
    columns="Product",
    values="Sales",
    aggfunc="sum"
)

print(pivot)
```

Output

|Salesperson|Laptop|Phone|
|---|--:|--:|
|Alex|1000|500|
|Bob|1200|800|
|Charlie|900|NaN|

Now it's a report.

---

# Visualize

Original

```text
Alex
Laptop
1000

Alex
Phone
500

Bob
Laptop
1200
```

↓

Pivot

```text
          Laptop   Phone

Alex      1000     500

Bob       1200     800
```

Notice how one column (`Product`) became multiple columns.

---

# Step 4 — Fill Missing Values

Charlie never sold phones.

So Pandas shows

```text
NaN
```

Replace it.

```python
pivot = sales.pivot_table(
    index="Salesperson",
    columns="Product",
    values="Sales",
    aggfunc="sum",
    fill_value=0
)

print(pivot)
```

Output

|Salesperson|Laptop|Phone|
|---|--:|--:|
|Alex|1000|500|
|Bob|1200|800|
|Charlie|900|0|

---

# Step 5 — Different Aggregations

Average

```python
sales.pivot_table(
    index="Salesperson",
    values="Sales",
    aggfunc="mean"
)
```

Maximum

```python
sales.pivot_table(
    index="Salesperson",
    values="Sales",
    aggfunc="max"
)
```

Count

```python
sales.pivot_table(
    index="Salesperson",
    values="Sales",
    aggfunc="count"
)
```

Exactly like SQL aggregate functions.

---

# Step 6 — Multiple Aggregations

```python
sales.pivot_table(
    index="Salesperson",
    values="Sales",
    aggfunc=["sum", "mean", "max"]
)
```

Output

|Salesperson|Sum|Mean|Max|
|---|--:|--:|--:|
|Alex|1500|750|1000|
|Bob|2000|1000|1200|

---

# Step 7 — Multiple Indexes

```python
sales.pivot_table(
    index=["Salesperson", "Product"],
    values="Sales",
    aggfunc="sum"
)
```

Output

```text
Alex
   Laptop
   Phone

Bob
   Laptop
   Phone
```

---

# Real Titanic Example

Average fare by passenger class.

```python
import pandas as pd

df = pd.read_csv("titanic.csv")

df.pivot_table(
    index="Pclass",
    values="Fare",
    aggfunc="mean"
)
```

Output

|Pclass|Fare|
|--:|--:|
|1|84.15|
|2|20.66|
|3|13.67|

---

Average age by Sex.

```python
df.pivot_table(
    index="Sex",
    values="Age",
    aggfunc="mean"
)
```

---

Average fare by Sex and Passenger Class.

```python
df.pivot_table(
    index="Sex",
    columns="Pclass",
    values="Fare",
    aggfunc="mean"
)
```

Output

|Sex|1|2|3|
|---|--:|--:|--:|
|Female|106|21|16|
|Male|67|19|12|

This is much easier to read than a grouped result.

---

# Pivot Table vs GroupBy

This is an interview favorite.

### GroupBy

Returns grouped data.

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

### Pivot Table

Returns a report.

```python
df.pivot_table(
    index="Sex",
    values="Age"
)
```

Looks more like Excel.

---

# SQL Comparison

SQL

```sql
SELECT
Department,
AVG(Salary)
FROM Employee
GROUP BY Department;
```

Pandas GroupBy

```python
df.groupby("Department")["Salary"].mean()
```

Pivot

```python
df.pivot_table(
    index="Department",
    values="Salary",
    aggfunc="mean"
)
```

Same result.

But Pivot can easily create **cross-tab reports** with rows and columns.

---

# Cheat Sheet

|Task|Command|
|---|---|
|Pivot|`pivot_table()`|
|Rows|`index=`|
|Columns|`columns=`|
|Values|`values=`|
|Aggregation|`aggfunc=`|
|Fill missing|`fill_value=`|

---

# 🧪 Practical Challenge (Titanic)

```python
import pandas as pd

df = pd.read_csv("titanic.csv")
```

Try these:

Average Fare by Passenger Class.

```python
index="Pclass"
```

---

Average Age by Sex.

```python
index="Sex"
```

---

Average Fare by Sex and Passenger Class.

```python
index="Sex"

columns="Pclass"
```

---

Count passengers by Embarked.

---

Maximum Fare by Passenger Class.

---

# 🏆 Congratulations!

You've completed the core Pandas topics.

Here's the roadmap you've covered:

|Chapter|Topic|
|---|---|
|1|Introduction|
|2|Series|
|3|DataFrame|
|4|Exploring Data|
|5|Selection (`loc`, `iloc`)|
|6|Filtering|
|7|Sorting|
|8|Missing Data|
|9|String Operations|
|10|Date & Time|
|11|GroupBy|
|12|Merge & Concat|
|13|Apply & Map|
|14|Pivot Tables|

---

## 🚀 What comes next?

Now comes the most valuable part—not more Pandas functions, but **using them together**.

We'll build three end-to-end projects:

1. **Titanic Analysis** – Explore survival rates, passenger demographics, fares, and missing data.
    
2. **Netflix Analysis** – Analyze content types, release years, countries, genres, and ratings.
    
3. **Sales Dashboard** – Clean data, aggregate metrics, create pivot tables, and generate business insights.
    

This is where everything you've learned starts to feel like real data analysis rather than isolated Pandas commands.