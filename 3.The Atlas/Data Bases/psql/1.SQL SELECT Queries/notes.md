Here's a **one-page SQL cheat sheet** you can keep beside you while practicing.

# SQL Fundamentals Cheat Sheet

## Basic Query Structure

```sql
SELECT columns
FROM table
WHERE condition
GROUP BY column
HAVING condition
ORDER BY column
LIMIT n;
```

---

# SELECT

Choose columns.

```sql
SELECT * FROM customers;
```

```sql
SELECT first_name, country
FROM customers;
```

---

# FROM

Choose the table.

```sql
SELECT *
FROM customers;
```

---

# WHERE

Filter rows.

```sql
SELECT *
FROM customers
WHERE country = 'USA';
```

Operators

```text
=
!=
<>
>
<
>=
<=
```

Logical Operators

```sql
AND
OR
NOT
```

Example

```sql
SELECT *
FROM customers
WHERE country='USA'
AND score>500;
```

---

# ORDER BY

Sort data.

Ascending (Default)

```sql
SELECT *
FROM customers
ORDER BY score;
```

Descending

```sql
SELECT *
FROM customers
ORDER BY score DESC;
```

---

# GROUP BY

Group similar rows.

```sql
SELECT country,
COUNT(*)
FROM customers
GROUP BY country;
```

---

# HAVING

Filter groups.

```sql
SELECT country,
COUNT(*)
FROM customers
GROUP BY country
HAVING COUNT(*) > 1;
```

---

# DISTINCT

Remove duplicates.

```sql
SELECT DISTINCT country
FROM customers;
```

---

# LIMIT (PostgreSQL)

Return first N rows.

```sql
SELECT *
FROM customers
LIMIT 3;
```

Top 2 highest scores

```sql
SELECT *
FROM customers
ORDER BY score DESC
LIMIT 2;
```

---

# Aggregate Functions

```sql
COUNT(*)
SUM(column)
AVG(column)
MAX(column)
MIN(column)
```

Examples

```sql
SELECT COUNT(*) FROM customers;

SELECT AVG(score) FROM customers;

SELECT MAX(score) FROM customers;
```

---

# Execution Order

Although you write:

```sql
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
LIMIT
```

PostgreSQL executes:

```text
FROM
 ↓
WHERE
 ↓
GROUP BY
 ↓
HAVING
 ↓
SELECT
 ↓
ORDER BY
 ↓
LIMIT
```

---

# Practice Queries (Your Database)

Show all customers

```sql
SELECT * FROM customers;
```

Names only

```sql
SELECT first_name
FROM customers;
```

Customers from Germany

```sql
SELECT *
FROM customers
WHERE country='Germany';
```

Customers with score > 500

```sql
SELECT *
FROM customers
WHERE score>500;
```

Highest scores first

```sql
SELECT *
FROM customers
ORDER BY score DESC;
```

Unique countries

```sql
SELECT DISTINCT country
FROM customers;
```

Number of customers

```sql
SELECT COUNT(*)
FROM customers;
```

Average score per country

```sql
SELECT country,
AVG(score)
FROM customers
GROUP BY country;
```

Countries with more than one customer

```sql
SELECT country,
COUNT(*)
FROM customers
GROUP BY country
HAVING COUNT(*)>1;
```

Top 3 customers

```sql
SELECT *
FROM customers
ORDER BY score DESC
LIMIT 3;
```

---

# SQL Thinking Pattern

Whenever you write a query, think:

```text
Which table?
        ↓
FROM

Which rows?
        ↓
WHERE

Need groups?
        ↓
GROUP BY

Filter groups?
        ↓
HAVING

Which columns?
        ↓
SELECT

Need sorting?
        ↓
ORDER BY

Need only a few rows?
        ↓
LIMIT
```

---

## 🎯 Rule to Remember

- **SELECT** → What do I want?
    
- **FROM** → Where is it?
    
- **WHERE** → Which rows?
    
- **GROUP BY** → Combine similar rows.
    
- **HAVING** → Filter groups.
    
- **ORDER BY** → Sort.
    
- **LIMIT** → Show only a few rows.
    

If you master just these seven clauses, you'll be able to write the majority of basic and intermediate SQL queries. The next big milestone is learning **JOINs**, which let you combine data from multiple tables like `customers` and `orders`.