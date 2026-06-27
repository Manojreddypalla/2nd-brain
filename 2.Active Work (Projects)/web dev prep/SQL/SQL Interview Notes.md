# SQL Interview Notes

# SQL

**Full Form:** Structured Query Language

## What is SQL?

SQL is a language used to communicate with **Relational Databases (RDBMS)**.

Examples

- MySQL
    
- PostgreSQL
    
- SQLite
    
- Oracle Database
    
- Microsoft SQL Server
    

Purpose

- Store data
    
- Retrieve data
    
- Update data
    
- Delete data
    

---

# Database

A database is an organized collection of data.

Example

College Database

```text
Students

ID   Name   Branch

1    John   CSE

2    Alice  IT
```

---

# Table

A table stores related data.

Example

```text
Employees

ID  Name   Salary

1   John   50000

2   Alice  60000
```

Rows = Records

Columns = Fields

---

# Row (Record)

Represents one complete entry.

```text
1   John   50000
```

---

# Column (Field)

Represents one attribute.

```text
ID

Name

Salary
```

---

# Primary Key ⭐⭐⭐⭐⭐

Uniquely identifies each row.

Properties

- Unique
    
- Cannot be NULL
    
- One primary key per table
    

Example

```text
EmployeeID
```

---

# Foreign Key ⭐⭐⭐⭐⭐

Links one table to another.

Example

```text
Orders

CustomerID
```

CustomerID refers to

```text
Customers

CustomerID
```

Purpose

Maintain relationships between tables.

---

# SQL Categories

## DDL

**Full Form:** Data Definition Language

Commands

```sql
CREATE

ALTER

DROP

TRUNCATE
```

Used to define database structure.

---

## DML

**Full Form:** Data Manipulation Language

Commands

```sql
INSERT

UPDATE

DELETE
```

Used to modify data.

---

## DQL

**Full Form:** Data Query Language

Command

```sql
SELECT
```

Retrieve data.

---

## DCL

**Full Form:** Data Control Language

Commands

```sql
GRANT

REVOKE
```

Manage permissions.

---

## TCL

**Full Form:** Transaction Control Language

Commands

```sql
COMMIT

ROLLBACK

SAVEPOINT
```

Manage transactions.

---

# CREATE TABLE

```sql
CREATE TABLE Students (

id INT PRIMARY KEY,

name VARCHAR(50),

age INT

);
```

---

# INSERT

```sql
INSERT INTO Students

VALUES

(1,'John',20);
```

Insert one row.

---

# SELECT ⭐⭐⭐⭐⭐

Retrieve data.

```sql
SELECT *

FROM Students;
```

Specific columns

```sql
SELECT

name,

age

FROM Students;
```

---

# WHERE ⭐⭐⭐⭐⭐

Filter records.

```sql
SELECT *

FROM Students

WHERE age > 18;
```

Operators

```text
=

>

<

>=

<=

!=

<>
```

---

# ORDER BY ⭐⭐⭐⭐⭐

Sort results.

Ascending

```sql
ORDER BY salary ASC;
```

Descending

```sql
ORDER BY salary DESC;
```

---

# LIMIT

Restrict number of rows.

```sql
SELECT *

FROM Students

LIMIT 5;
```

---

# DISTINCT

Remove duplicates.

```sql
SELECT DISTINCT department

FROM Employees;
```

---

# UPDATE

Modify existing records.

```sql
UPDATE Students

SET age=22

WHERE id=1;
```

---

# DELETE

Delete records.

```sql
DELETE

FROM Students

WHERE id=1;
```

---

# Aggregate Functions ⭐⭐⭐⭐⭐

COUNT

```sql
SELECT COUNT(*)

FROM Employees;
```

SUM

```sql
SELECT SUM(salary)

FROM Employees;
```

AVG

```sql
SELECT AVG(salary)

FROM Employees;
```

MAX

```sql
SELECT MAX(salary)

FROM Employees;
```

MIN

```sql
SELECT MIN(salary)

FROM Employees;
```

---

# GROUP BY ⭐⭐⭐⭐⭐

Groups rows.

Example

```sql
SELECT

department,

COUNT(*)

FROM Employees

GROUP BY department;
```

---

# HAVING ⭐⭐⭐⭐☆

Filters grouped data.

```sql
SELECT

department,

COUNT(*)

FROM Employees

GROUP BY department

HAVING COUNT(*) > 5;
```

---

# WHERE vs HAVING

|WHERE|HAVING|
|---|---|
|Before grouping|After grouping|
|Individual rows|Groups|

---

# LIKE

Pattern matching.

Starts with A

```sql
WHERE name LIKE 'A%'
```

Ends with A

```sql
WHERE name LIKE '%A'
```

Contains

```sql
WHERE name LIKE '%oh%'
```

---

# IN

```sql
WHERE department

IN ('HR','IT')
```

---

# BETWEEN

```sql
WHERE salary

BETWEEN 50000 AND 100000;
```

---

# NULL

Check NULL

```sql
IS NULL
```

Check NOT NULL

```sql
IS NOT NULL
```

---

# JOINS ⭐⭐⭐⭐⭐

Very important.

---

## INNER JOIN

Returns matching rows.

```sql
SELECT *

FROM Orders

INNER JOIN Customers

ON Orders.customer_id = Customers.id;
```

---

## LEFT JOIN

Returns all rows from left table.

Matching rows from right.

---

## RIGHT JOIN

Returns all rows from right table.

---

## FULL OUTER JOIN

Returns all rows.

(Not supported directly in MySQL.)

---

# Join Visualization

```text
Customers

ID Name

1 John

2 Alice



Orders

CustomerID Product

1 Laptop

1 Mouse
```

INNER JOIN

```text
John Laptop

John Mouse
```

---

# Normalization ⭐⭐⭐⭐☆

Purpose

Reduce data redundancy.

Common Forms

1NF

2NF

3NF

Interview

Usually knowing

"Normalization removes duplicate data."

is sufficient.

---

# Index ⭐⭐⭐⭐☆

Purpose

Speed up searching.

Without Index

```text
Scan

Every Row
```

With Index

```text
Jump

Directly
```

Tradeoff

- Faster SELECT
    
- Slightly slower INSERT/UPDATE
    

---

# Transactions ⭐⭐⭐⭐☆

Group multiple operations.

Example

```text
Withdraw

↓

Deposit

↓

Commit
```

If something fails

```text
Rollback
```

---

# ACID Properties ⭐⭐⭐⭐⭐

## A

Atomicity

All or Nothing.

---

## C

Consistency

Database remains valid.

---

## I

Isolation

Transactions don't interfere.

---

## D

Durability

Committed data survives crashes.

---

# COMMIT

Save changes.

```sql
COMMIT;
```

---

# ROLLBACK

Undo changes.

```sql
ROLLBACK;
```

---

# Views

Virtual table.

```sql
CREATE VIEW EmployeeView

AS

SELECT

name,

salary

FROM Employees;
```

---

# Constraints

PRIMARY KEY

UNIQUE

NOT NULL

CHECK

DEFAULT

FOREIGN KEY

---

# SQL Execution Order (Interview Favorite)

Even though you write:

```sql
SELECT name
FROM Employees
WHERE salary > 50000
GROUP BY department
HAVING COUNT(*) > 1
ORDER BY name;
```

SQL processes it roughly as:

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

# Interview Differences

## DELETE vs TRUNCATE vs DROP

|DELETE|TRUNCATE|DROP|
|---|---|---|
|Deletes selected rows|Removes all rows|Deletes entire table|
|Can use WHERE|No WHERE|Removes table structure|

---

## PRIMARY KEY vs UNIQUE

|PRIMARY KEY|UNIQUE|
|---|---|
|One per table|Multiple allowed|
|Cannot be NULL|Can allow NULL (depends on DBMS)|

---

## CHAR vs VARCHAR

|CHAR|VARCHAR|
|---|---|
|Fixed length|Variable length|

---

## WHERE vs HAVING

|WHERE|HAVING|
|---|---|
|Before GROUP BY|After GROUP BY|

---

# Full Forms

SQL → Structured Query Language

DDL → Data Definition Language

DML → Data Manipulation Language

DQL → Data Query Language

DCL → Data Control Language

TCL → Transaction Control Language

RDBMS → Relational Database Management System

ACID → Atomicity Consistency Isolation Durability

---

# Frequently Asked Interview Questions

1. What is SQL?
    
2. Difference between SQL and NoSQL?
    
3. What is a Primary Key?
    
4. What is a Foreign Key?
    
5. Difference between WHERE and HAVING?
    
6. Difference between DELETE, TRUNCATE, and DROP?
    
7. What are Joins?
    
8. What is Normalization?
    
9. What is an Index?
    
10. Explain ACID properties.
    
11. Difference between CHAR and VARCHAR?
    
12. What is a Transaction?
    
13. Write a query to find the second highest salary. _(Very common coding question.)_
    

---

# Revision Checklist ✅

-  SQL Basics
    
-  Database & Tables
    
-  Primary Key
    
-  Foreign Key
    
-  DDL / DML / DQL / DCL / TCL
    
-  CREATE
    
-  INSERT
    
-  SELECT
    
-  WHERE
    
-  ORDER BY
    
-  LIMIT
    
-  DISTINCT
    
-  UPDATE
    
-  DELETE
    
-  Aggregate Functions
    
-  GROUP BY
    
-  HAVING
    
-  LIKE
    
-  IN
    
-  BETWEEN
    
-  NULL
    
-  INNER JOIN
    
-  LEFT JOIN
    
-  RIGHT JOIN
    
-  Normalization
    
-  Index
    
-  Transactions
    
-  ACID
    
-  Constraints
    
-  Views