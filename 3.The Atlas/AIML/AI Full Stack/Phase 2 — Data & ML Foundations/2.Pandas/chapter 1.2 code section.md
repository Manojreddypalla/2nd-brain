Absolutely. Now that you know **what these data sources are**, let's learn **how to load each one into Pandas**.

> **Important pattern to remember:** Every source → `read_*()` → **DataFrame**

---

# 1. CSV (Most Common)

Imagine this file:

```text
employees.csv

Name,Age,Salary
Alex,21,50000
Bob,22,60000
Charlie,20,45000
```

### Read CSV

```python
import pandas as pd

df = pd.read_csv("employees.csv")

print(df)
```

Output:

```text
      Name  Age  Salary
0     Alex   21   50000
1      Bob   22   60000
2  Charlie   20   45000
```

Save it back:

```python
df.to_csv("new_file.csv", index=False)
```

---

# 2. Excel

Suppose

```text
employees.xlsx
```

Read it:

```python
import pandas as pd

df = pd.read_excel("employees.xlsx")
```

If there are multiple sheets:

```python
df = pd.read_excel("employees.xlsx", sheet_name="January")
```

Save:

```python
df.to_excel("output.xlsx", index=False)
```

---

# 3. JSON

Example JSON

```json
[
    {
        "Name":"Alex",
        "Age":21
    },
    {
        "Name":"Bob",
        "Age":22
    }
]
```

Read:

```python
import pandas as pd

df = pd.read_json("employees.json")
```

Write:

```python
df.to_json("output.json")
```

---

# 4. SQL Database

Suppose you're using SQLite.

```python
import sqlite3
import pandas as pd

connection = sqlite3.connect("company.db")

df = pd.read_sql(
    "SELECT * FROM employees",
    connection
)

print(df)
```

You can query only the rows you need:

```python
df = pd.read_sql(
    "SELECT name, salary FROM employees WHERE salary > 50000",
    connection
)
```

---

# 5. Parquet

Read:

```python
import pandas as pd

df = pd.read_parquet("employees.parquet")
```

Write:

```python
df.to_parquet("output.parquet")
```

---

# 6. HTML Table (Bonus)

Suppose a webpage contains a table.

```python
import pandas as pd

tables = pd.read_html("https://example.com")

df = tables[0]
```

`read_html()` returns a **list of DataFrames** because a webpage may contain multiple tables.

---

# 7. XML (Bonus)

```python
import pandas as pd

df = pd.read_xml("employees.xml")
```

---

# 8. Clipboard (Very Useful)

Copy a table from Excel.

Then:

```python
import pandas as pd

df = pd.read_clipboard()
```

Instant DataFrame.

To copy it back:

```python
df.to_clipboard(index=False)
```

---

# 9. Creating a DataFrame Yourself

You don't always need a file.

```python
import pandas as pd

df = pd.DataFrame({
    "Name": ["Alex", "Bob", "Charlie"],
    "Age": [21, 22, 20],
    "Salary": [50000, 60000, 45000]
})

print(df)
```

---

# 10. Reading from a Python Dictionary

```python
import pandas as pd

data = {
    "Name": ["Alex", "Bob"],
    "Age": [21, 22]
}

df = pd.DataFrame(data)
```

---

# 11. Reading from a List

```python
import pandas as pd

data = [
    ["Alex", 21],
    ["Bob", 22]
]

df = pd.DataFrame(
    data,
    columns=["Name", "Age"]
)
```

---

# The Pattern You'll Notice

Every input becomes a **DataFrame**.

```text
CSV ----------\
Excel ---------\
JSON -----------> DataFrame
SQL -----------/
Parquet -------/
Dictionary ----/
List ---------/
```

Once you have a DataFrame, **the source no longer matters**. Whether the data came from a CSV, a database, or an API, you'll use the same Pandas operations to inspect, filter, sort, group, and analyze it.

---

# Methods Worth Memorizing

|Data Source|Read|Write|
|---|---|---|
|CSV|`pd.read_csv()`|`df.to_csv()`|
|Excel|`pd.read_excel()`|`df.to_excel()`|
|JSON|`pd.read_json()`|`df.to_json()`|
|SQL|`pd.read_sql()`|`df.to_sql()`|
|Parquet|`pd.read_parquet()`|`df.to_parquet()`|
|HTML|`pd.read_html()`|—|
|XML|`pd.read_xml()`|`df.to_xml()`|
|Clipboard|`pd.read_clipboard()`|`df.to_clipboard()`|

---

## One important note

Don't try to memorize all these functions immediately. In practice, you'll use:

- **`read_csv()`** ⭐⭐⭐⭐⭐
    
- **`read_excel()`** ⭐⭐⭐⭐☆
    
- **`read_json()`** ⭐⭐⭐⭐☆
    
- **`read_sql()`** ⭐⭐⭐⭐⭐
    

These four account for the vast majority of day-to-day work. The others are useful to know exist, and you can look them up when you need them. Next, in **Chapter 2**, we'll build the foundation of every DataFrame by understanding **Series**.