Awesome. This chapter is one of the biggest "aha!" moments in Pandas.

# Chapter 10 — Date & Time (`datetime`)

Imagine you're working at Amazon.

Every order has a date.

|Order ID|Date|Amount|
|---|---|--:|
|101|2026-01-15|500|
|102|2026-02-20|1200|
|103|2026-02-25|700|

Your manager asks:

- 📅 How many orders were placed in **February**?
    
- 📅 Total sales in **2026**?
    
- 📅 Orders placed on **Monday**?
    
- 📅 Average sales **per month**?
    

Without datetime support, these questions are hard.

With Pandas, they're easy.

---

# Step 1 — Dates Are Often Just Strings

Suppose you load a CSV.

```python
import pandas as pd

df = pd.DataFrame({
    "Date": [
        "2026-01-15",
        "2026-02-20",
        "2026-03-10"
    ]
})

print(df.dtypes)
```

Output

```text
Date    object
```

Pandas thinks it's **text**, not a date.

---

# Step 2 — Convert to Date

```python
df["Date"] = pd.to_datetime(df["Date"])
```

Now check again.

```python
print(df.dtypes)
```

Output

```text
Date    datetime64[ns]
```

Now Pandas understands it's a real date.

---

# Step 3 — Extract the Year

```python
df["Date"].dt.year
```

Output

```text
2026
2026
2026
```

Notice something new:

```python
.dt
```

Just like:

- `.str` → String operations
    
- `.dt` → Date operations
    

---

# Step 4 — Month

```python
df["Date"].dt.month
```

Output

```text
1
2
3
```

---

# Step 5 — Month Name

```python
df["Date"].dt.month_name()
```

Output

```text
January
February
March
```

Much nicer for reports.

---

# Step 6 — Day

```python
df["Date"].dt.day
```

Output

```text
15
20
10
```

---

# Step 7 — Day of Week

```python
df["Date"].dt.day_name()
```

Output

```text
Thursday
Friday
Tuesday
```

Now you can answer:

> Which day gets the most orders?

---

# Step 8 — Weekday Number

```python
df["Date"].dt.weekday
```

Output

```text
0 → Monday
1 → Tuesday
2 → Wednesday
...
6 → Sunday
```

---

# Step 9 — Extract Everything

```python
df["Year"] = df["Date"].dt.year
df["Month"] = df["Date"].dt.month_name()
df["Day"] = df["Date"].dt.day
```

Now your DataFrame becomes

|Date|Year|Month|Day|
|---|---|---|--:|
|2026-01-15|2026|January|15|
|2026-02-20|2026|February|20|

---

# Step 10 — Filter by Date

Only February.

```python
df[
    df["Date"].dt.month == 2
]
```

Only year 2026.

```python
df[
    df["Date"].dt.year == 2026
]
```

---

# Step 11 — Compare Dates

Orders after February 1.

```python
df[
    df["Date"] > "2026-02-01"
]
```

Notice

You're comparing dates directly.

---

# Step 12 — Current Date

```python
pd.Timestamp.now()
```

Output

```text
2026-07-11 ...
```

Useful for logs and reports.

---

# Step 13 — Difference Between Dates

```python
df = pd.DataFrame({
    "Start": pd.to_datetime([
        "2026-01-01",
        "2026-01-10"
    ]),
    "End": pd.to_datetime([
        "2026-01-05",
        "2026-01-20"
    ])
})

df["Days"] = df["End"] - df["Start"]

print(df)
```

Output

|Start|End|Days|
|---|---|---|
|Jan 1|Jan 5|4 days|
|Jan10|Jan20|10 days|

This is how companies calculate:

- Delivery time
    
- Project duration
    
- Employee experience
    
- Subscription length
    

---

# Step 14 — Sort by Date

```python
df.sort_values("Date")
```

Oldest to newest.

Newest first

```python
df.sort_values(
    "Date",
    ascending=False
)
```

---

# Practical Sales Dataset

Suppose

|Date|Sales|
|---|--:|
|2026-01-10|500|
|2026-02-15|700|
|2026-02-28|900|

Total February sales

```python
feb = df[
    df["Date"].dt.month == 2
]

print(feb["Sales"].sum())
```

---

# Practical Netflix Dataset

Suppose the dataset has

```text
date_added
```

Convert

```python
netflix["date_added"] = pd.to_datetime(
    netflix["date_added"]
)
```

Movies added in 2020

```python
netflix[
    netflix["date_added"].dt.year == 2020
]
```

Movies added in December

```python
netflix[
    netflix["date_added"].dt.month == 12
]
```

---

# Practical Titanic

Titanic itself doesn't have dates, so datetime isn't very useful there. That's why business datasets like **Sales**, **Orders**, or **Netflix** are better examples.

---

# Dry Run

Suppose

```text
Date

2026-01-15

2026-02-20

2026-03-10
```

You execute

```python
df["Date"].dt.month
```

Internally Pandas reads each datetime object:

```text
2026-01-15

↓

Month = 1

2026-02-20

↓

Month = 2

2026-03-10

↓

Month = 3
```

and returns a new Series:

```text
0    1
1    2
2    3
```

---

# Cheat Sheet

|Task|Command|
|---|---|
|Convert to datetime|`pd.to_datetime()`|
|Year|`.dt.year`|
|Month|`.dt.month`|
|Month name|`.dt.month_name()`|
|Day|`.dt.day`|
|Day name|`.dt.day_name()`|
|Weekday|`.dt.weekday`|
|Filter by year|`.dt.year == 2026`|
|Filter by month|`.dt.month == 2`|
|Current timestamp|`pd.Timestamp.now()`|
|Date difference|`End - Start`|

---

# 🧪 Practice Challenge

Create this DataFrame:

```python
import pandas as pd

sales = pd.DataFrame({
    "Date": [
        "2026-01-15",
        "2026-02-10",
        "2026-02-25",
        "2026-03-12"
    ],
    "Sales": [500, 700, 1200, 900]
})
```

Then:

1. Convert `Date` to datetime.
    
2. Extract the year.
    
3. Extract the month number.
    
4. Extract the month name.
    
5. Extract the day name.
    
6. Show only February sales.
    
7. Sort the data by date.
    
8. Show only sales after **2026-02-01**.
    

---

# 📍You're almost at the most important Pandas chapter

So far you've learned how to:

- Load data
    
- Explore it
    
- Select it
    
- Filter it
    
- Sort it
    
- Clean missing values
    
- Work with strings
    
- Work with dates
    

The next chapter is **GroupBy**.

If I had to choose **one feature** that defines Pandas, it would be `groupby()`.

It answers questions like:

- "Average salary by department."
    
- "Total sales by month."
    
- "Highest-rated movie by genre."
    
- "Survival rate by passenger class."
    

Once you understand `groupby()`, you'll start thinking like a data analyst rather than someone just manipulating tables.