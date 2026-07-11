Excellent. Now we reach something you'll use in almost every real dataset.

# Chapter 9 — String Operations (`.str`)

Think about datasets like:

- Netflix (Movie Titles)
    
- Amazon (Product Names)
    
- Employee Database (Names)
    
- Email Lists
    
- Cities
    
- Countries
    

Most datasets contain **text**.

Before analyzing them, you usually need to **clean** that text.

---

# Our Dataset

```python
import pandas as pd

df = pd.DataFrame({
    "Name": [
        "Alex Johnson",
        " Bob Smith ",
        "CHARLIE",
        "Emma Watson",
        "David Brown"
    ],
    "Email": [
        "alex@gmail.com",
        "bob@yahoo.com",
        "charlie@gmail.com",
        "emma@outlook.com",
        "david@gmail.com"
    ]
})

print(df)
```

Notice:

- Extra spaces
    
- Uppercase
    
- Lowercase
    
- Emails
    

Perfect for learning.

---

# The `.str` Accessor

Just like numbers have mathematical operations,

strings have string operations.

Every string method starts with

```python
.str
```

Think

```text
Series

↓

String Series

↓

.str

↓

Operations
```

---

# 1. Convert to Lowercase

```python
df["Name"].str.lower()
```

Output

```text
alex johnson
 bob smith
charlie
emma watson
david brown
```

Useful for making text consistent.

---

# 2. Convert to Uppercase

```python
df["Name"].str.upper()
```

Output

```text
ALEX JOHNSON
BOB SMITH
CHARLIE
```

---

# 3. Capitalize

```python
df["Name"].str.title()
```

Output

```text
Alex Johnson
Bob Smith
Charlie
Emma Watson
```

Very useful for cleaning names.

---

# 4. Remove Extra Spaces

Notice

```text
" Bob Smith "
```

contains spaces.

Use

```python
df["Name"].str.strip()
```

Output

```text
Bob Smith
```

Also available:

```python
.str.lstrip()
```

Left side only.

```python
.str.rstrip()
```

Right side only.

---

# 5. Length of String

```python
df["Name"].str.len()
```

Output

```text
12
9
7
11
12
```

Useful for validation.

Example:

Passwords shorter than 8 characters.

---

# 6. Replace Text

```python
df["Email"].str.replace("gmail.com", "company.com")
```

Output

```text
alex@company.com
```

---

# 7. Check if Text Contains Something

Suppose

Find all Gmail users.

```python
df["Email"].str.contains("gmail")
```

Output

```text
True
False
True
False
True
```

Filter them

```python
df[
    df["Email"].str.contains("gmail")
]
```

---

# 8. Starts With

```python
df["Name"].str.startswith("A")
```

Output

```text
True
False
False
False
False
```

---

# 9. Ends With

```python
df["Email"].str.endswith(".com")
```

---

# 10. Split

Suppose

```text
Alex Johnson
```

Split at the space.

```python
df["Name"].str.split()
```

Output

```text
["Alex","Johnson"]
```

---

Split email

```python
df["Email"].str.split("@")
```

Output

```text
["alex","gmail.com"]
```

---

# 11. Get Part of Split

Suppose

```python
df["Email"].str.split("@").str[1]
```

Output

```text
gmail.com

yahoo.com

outlook.com
```

Now you've extracted the domain.

---

# 12. Count Characters

How many times does "a" appear?

```python
df["Name"].str.count("a")
```

---

# 13. Slice Strings

First four characters.

```python
df["Name"].str[:4]
```

Output

```text
Alex

Bob

CHAR
```

---

# 14. Regex (Simple Example)

Find emails ending with gmail.

```python
df[
    df["Email"].str.contains("gmail")
]
```

Later you'll learn powerful regex patterns.

---

# Practical Netflix Example

Suppose

```python
netflix = pd.read_csv("netflix_titles.csv")
```

Movie titles in uppercase

```python
netflix["title"].str.upper()
```

Find movies containing

```text
Love
```

```python
netflix[
    netflix["title"].str.contains("Love")
]
```

Find TV Shows

```python
netflix[
    netflix["type"].str.contains("TV")
]
```

---

# Practical Titanic Example

Passenger names

```python
df["Name"].str.upper()
```

Passengers whose name starts with "A"

```python
df[
    df["Name"].str.startswith("A")
]
```

---

# Dry Run

Suppose

```python
df["Name"]
```

contains

```text
Alex

Bob

Charlie
```

Now

```python
df["Name"].str.upper()
```

Internally Pandas does something like

```text
Alex

↓

ALEX

Bob

↓

BOB

Charlie

↓

CHARLIE
```

Every row is processed automatically.

No loop written by you.

---

# Real Industry Example

Imagine you're cleaning customer data.

```text
" john doe "

↓

strip()

↓

"john doe"

↓

title()

↓

"John Doe"
```

Or

Emails

```text
abc@gmail.com

↓

Extract domain

↓

gmail.com
```

This is extremely common before analysis.

---

# Cheat Sheet

|Task|Command|
|---|---|
|Lowercase|`.str.lower()`|
|Uppercase|`.str.upper()`|
|Title Case|`.str.title()`|
|Remove spaces|`.str.strip()`|
|Length|`.str.len()`|
|Replace|`.str.replace()`|
|Contains|`.str.contains()`|
|Starts with|`.str.startswith()`|
|Ends with|`.str.endswith()`|
|Split|`.str.split()`|
|Slice|`.str[:5]`|
|Count text|`.str.count()`|

---

# 🧪 Practice Challenge

Using the Titanic dataset:

```python
import pandas as pd

df = pd.read_csv("titanic.csv")

# 1. Convert all passenger names to uppercase.

# 2. Convert all names to title case.

# 3. Find passengers whose names contain "Mr".

# 4. Find passengers whose names contain "Miss".

# 5. Count the length of each passenger's name.

# 6. Show passengers whose names start with "A".

# 7. Extract the first five characters of each name.

# 8. Count how many times "a" appears in each name.
```

---

# 📍 Progress So Far

You've learned:

- ✅ Series
    
- ✅ DataFrame
    
- ✅ Reading Data
    
- ✅ Exploring Data
    
- ✅ Selection (`loc` / `iloc`)
    
- ✅ Filtering
    
- ✅ Sorting
    
- ✅ Missing Data
    
- ✅ String Operations
    

At this point, you're already able to clean and explore many real datasets.

## Next Chapter (Chapter 10)

We'll cover **Date & Time**, where you'll learn:

- `to_datetime()`
    
- Extracting year, month, day
    
- Day of the week
    
- Time differences
    
- Date filtering
    
- Working with timestamps
    

Dates are one of the most important data types in analytics because almost every business dataset has a time component.