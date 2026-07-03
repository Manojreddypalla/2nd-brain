Absolutely. Here's a **clean, beginner-friendly note** covering everything you've done so far with PostgreSQL. You can paste this into Obsidian or your notes.

---

# PostgreSQL Notes – Part 1 (Setup & `psql` Basics)

# What is PostgreSQL?

PostgreSQL is an **open-source Relational Database Management System (RDBMS)**.

It stores data in:

```text
Database
    ↓
Schema
    ↓
Tables
    ↓
Rows & Columns
```

Example:

```text
PostgreSQL Server
│
├── MyDatabase
│      ├── customers
│      └── orders
│
├── salesdb
│      └── sales
│           ├── customers
│           ├── employees
│           ├── products
│           ├── orders
│           └── ordersarchive
│
└── postgres
```

---

# PostgreSQL Server vs psql

### PostgreSQL Server

- Stores data
    
- Runs continuously as a service
    
- Executes SQL queries
    

### psql

- Command Line Client
    
- Used to communicate with PostgreSQL Server
    

```text
You
 │
 ▼
psql
 │
 ▼
PostgreSQL Server
 │
 ▼
Database
```

---

# Starting PostgreSQL

Open PostgreSQL shell

```bash
sudo -u postgres psql
```

### Explanation

```bash
sudo
```

Run command with elevated privileges.

```bash
-u postgres
```

Run the command as Linux user **postgres**.

```bash
psql
```

Launch PostgreSQL CLI.

---

# Running a SQL Script

```bash
sudo -u postgres psql -f /path/to/file.sql
```

### Explanation

```text
sudo
    ↓
Become Linux user "postgres"
    ↓
Start psql
    ↓
Open SQL file
    ↓
Execute every SQL statement
```

---

# Prompt Meanings

### Ready for a new command

```text
postgres=#
```

or

```text
MyDatabase=#
```

Means:

> PostgreSQL is waiting for a new SQL statement.

---

### Waiting for more input

```text
postgres-#
```

Means:

> Previous SQL statement is incomplete.

Example

Wrong

```sql
SELECT *
```

Prompt becomes

```text
postgres-#
```

Complete it

```sql
FROM customers;
```

---

Cancel unfinished query

```
Ctrl + C
```

---

# Quitting PostgreSQL

```sql
\q
```

---

# Clearing Screen

Linux Terminal

```
Ctrl + L
```

or

```bash
clear
```

Inside psql

```
Ctrl + L
```

---

# Listing Databases

```sql
\l
```

or

```sql
\list
```

Example

```text
MyDatabase
postgres
salesdb
template0
template1
```

---

# Switching Database

```sql
\c database_name
```

Example

```sql
\c MyDatabase
```

or

```sql
\c salesdb
```

Prompt changes

```text
MyDatabase=#
```

or

```text
salesdb=#
```

---

# Current Database

```sql
SELECT current_database();
```

Example

```text
 current_database
------------------
 MyDatabase
```

---

# Current User

```sql
SELECT current_user;
```

Example

```text
 postgres
```

---

# PostgreSQL Version

```sql
SELECT version();
```

---

# Schemas

List schemas

```sql
\dn
```

Example

```text
public
sales
```

---

# Tables

## List tables

```sql
\dt
```

---

## List tables in a schema

```sql
\dt sales.*
```

---

## List all tables

```sql
\dt *.*
```

---

# Describe Table

```sql
\d table_name
```

Example

```sql
\d customers
```

Detailed information

```sql
\d+ customers
```

---

# Why "relation does not exist"

Suppose you're connected to

```text
postgres=#
```

Running

```sql
SELECT * FROM customers;
```

may fail because **customers** exists in **MyDatabase**, not in **postgres**.

Solution

```sql
\c MyDatabase
```

Then

```sql
SELECT * FROM customers;
```

works.

Always check

```sql
SELECT current_database();
```

---

# Executing SQL Files inside psql

```sql
\i filename.sql
```

Example

```sql
\i /media/manoj/ZORO/DATA/postgres/init-postgres-mydatabase.sql
```

---

# Pager

If output shows

```text
~
~
~
(END)
```

It is using **less**.

Quit pager

```
q
```

Disable pager

```sql
\pset pager off
```

Enable pager

```sql
\pset pager on
```

---

# Most Useful psql Commands

|Command|Description|
|---|---|
|`\l`|List databases|
|`\c database`|Connect to database|
|`\dt`|List tables|
|`\dt schema.*`|List tables in schema|
|`\dt *.*`|List all tables|
|`\dn`|List schemas|
|`\d table`|Describe table|
|`\d+ table`|Detailed table info|
|`\i file.sql`|Execute SQL file|
|`\pset pager off`|Disable pager|
|`\?`|Show psql commands|
|`\h SELECT`|SQL help|
|`\q`|Quit psql|

---

# SQL Commands Learned

View all rows

```sql
SELECT * FROM customers;
```

View specific columns

```sql
SELECT first_name, score
FROM customers;
```

Current database

```sql
SELECT current_database();
```

Current user

```sql
SELECT current_user;
```

Version

```sql
SELECT version();
```

---

# PostgreSQL Hierarchy

```text
Linux Machine
        │
        ▼
PostgreSQL Server
        │
        ├── Database
        │      │
        │      ├── Schema
        │      │      │
        │      │      ├── Tables
        │      │      │      │
        │      │      │      ├── Rows
        │      │      │      └── Columns
```

---

# Mental Checklist Before Writing SQL

Whenever you open PostgreSQL, ask yourself:

✅ Which PostgreSQL server am I connected to?

```bash
sudo -u postgres psql
```

↓

✅ Which database am I using?

```sql
SELECT current_database();
```

↓

✅ Which schema contains my table?

```sql
\dn
```

↓

✅ What tables are available?

```sql
\dt
```

or

```sql
\dt schema.*
```

↓

✅ Describe the table

```sql
\d table_name
```

↓

✅ Query the data

```sql
SELECT * FROM table_name;
```

---

## 🚀 What's Next?

The next topic should be **SQL Fundamentals**, where you'll start learning the language itself:

1. `SELECT`
    
2. `FROM`
    
3. `WHERE`
    
4. `ORDER BY`
    
5. `LIMIT`
    
6. `DISTINCT`
    
7. Aliases (`AS`)
    

These form the foundation of almost every SQL query you'll write, and they're the perfect next step now that you're comfortable navigating PostgreSQL.