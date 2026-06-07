# 🔥 **1. Connecting to MySQL**

### Command:

```bash
sudo mysql

```

This opens the MySQL shell as root.

---

# 🔥 **2. Showing Databases**

```sql
SHOW DATABASES;

```

---

# 🔥 **3. Creating a Database**

```sql
CREATE DATABASE testing;

```

---

# 🔥 **4. Selecting a Database**

```sql
USE testing;

```

---

# 🔥 **5. Creating a Table**

You made a simple table:

```sql
CREATE TABLE mm (
    id INT,
    name VARCHAR(255),
    region VARCHAR(255),
    roast VARCHAR(255)
);

```

And a complex one:

```sql
CREATE TABLE Avengers (
    avenger_id INT AUTO_INCREMENT PRIMARY KEY,
    real_name VARCHAR(100) NOT NULL,
    hero_name VARCHAR(100) NOT NULL,
    species VARCHAR(50),
    main_power VARCHAR(200),
    power_level INT CHECK (power_level BETWEEN 1 AND 100),
    weapon VARCHAR(100),
    status ENUM('Active', 'Retired', 'Dead', 'Unknown'),
    origin VARCHAR(100),
    joined_year YEAR,
    affiliation VARCHAR(100),
    last_seen DATE
);

```

### Interview Concept:

- `PRIMARY KEY` → unique row
- `AUTO_INCREMENT` → auto-numbering
- `NOT NULL` → value required
- `CHECK` → validates a value
- `ENUM` → fixed allowed values

---

# 🔥 **6. Insert Values**

```sql
INSERT INTO mm VALUES (1, "manoj", "hun", "fry");

```

For Avengers:

```sql
INSERT INTO Avengers (real_name, hero_name, species, main_power, power_level, weapon, status, origin, joined_year, affiliation)
VALUES (...);

```

### Interview Concept:

- Use column names when inserting
- Avoid relying on column order

---

# 🔥 **7. Selecting Data**

Basic:

```sql
SELECT * FROM mm;

```

Condition:

```sql
SELECT * FROM Avengers WHERE origin = 'USA';

```

Negation:

```sql
SELECT * FROM Avengers WHERE NOT origin = 'USA';

```

Sorting:

```sql
ORDER BY power_level ASC;

```

**Remember:**

❌ `ascending` → invalid

✔ `ASC` or `DESC`

---

# 🔥 **8. Describe a Table**

```sql
DESCRIBE Avengers;

```

Shows:

- Field names
- Types
- NULL allowed or not
- Key
- Default
- Extra (like auto_increment)

---

# 🔥 **9. Update Values**

```sql
UPDATE Avengers
SET beard = TRUE
WHERE real_name = "Thor Odinson";

```

Key concepts:

- `UPDATE` modifies rows
- Always use `WHERE` to avoid updating the entire table

---

# 🔥 **10. Altering a Table**

Add a column:

```sql
ALTER TABLE Avengers ADD beard BOOLEAN;

```

Interview concepts:

- `ALTER TABLE` can add, drop or modify columns
- This is schema evolution

---

# 🔥 **11. Deleting a Table**

```sql
DROP TABLE Avengers;

```

❗ Irreversible

❗ Removes all data

---

# 🔥 **12. Common SQL Errors You Encountered**

### ✔ Mistake 1: Missing Column Types

```sql
name varchar   ❌ wrong
name varchar(255) ✔ correct

```

### ✔ Mistake 2: Wrong SELECT Syntax

```sql
select * from table mm; ❌
select * from mm; ✔

```

### ✔ Mistake 3: Wrong NOT operator

```sql
origin not = "USA"; ❌
NOT origin = "USA"; ✔
origin <> "USA"; ✔ (better)

```

### ✔ Mistake 4: ORDER BY ascending

```sql
ORDER BY power_level ascending; ❌
ORDER BY power_level ASC; ✔

```

---

# 🔥 **13. Important Interview Topics Covered by Your Practice**

### **Basics**

✔ CREATE DATABASE

✔ CREATE TABLE

✔ INSERT

✔ SELECT

✔ UPDATE

✔ WHERE

✔ ORDER BY

### **Schema Design**

✔ Data types

✔ ENUM

✔ CHECK constraint

✔ AUTO_INCREMENT

✔ PRIMARY KEY

### **DDL**

✔ ALTER TABLE

✔ DROP TABLE

### **Querying**

✔ Filtering

✔ Sorting

✔ Selecting specific columns

---

# ⭐ **14. Most Important SQL Queries (Using Avengers Table)**

### 1️⃣ Get all Avengers:

```sql
SELECT * FROM Avengers;

```

### 2️⃣ Get only USA Avengers:

```sql
SELECT * FROM Avengers WHERE origin = 'USA';

```

### 3️⃣ Get non-USA Avengers:

```sql
SELECT * FROM Avengers WHERE origin <> 'USA';

```

### 4️⃣ Sort by power:

```sql
SELECT * FROM Avengers ORDER BY power_level DESC;

```

### 5️⃣ Update beard:

```sql
UPDATE Avengers SET beard = TRUE WHERE real_name = 'Thor Odinson';

```

---

# 🚀 **Interview Ready Summary (Super Short)**

- `CREATE DATABASE`, `CREATE TABLE`, `ALTER TABLE`, `DROP TABLE`
- `INSERT`, `SELECT`, `UPDATE`
- Filtering using `WHERE`
- Sorting using `ORDER BY`
- Checking schema using `DESCRIBE`
- Using constraints (`PRIMARY KEY`, `CHECK`, `ENUM`, `NOT NULL`)
- Negation: `<>`, `!=`, `NOT`
- Boolean values: TRUE / FALSE
- SQL errors usually come from syntax or missing parentheses

You are already performing **real SQL**, not beginner level.