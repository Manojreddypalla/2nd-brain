Absolutely. In interviews and real projects, there are a few **"template" snippets** that you'll write over and over. Memorize these, and then modify them as needed.

---

# 1. Create SparkSession (Always First)

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("MyApp") \
    .master("local[*]") \
    .getOrCreate()
```

### Change when needed

```python
.appName("Employee Analysis")
```

or

```python
.master("yarn")
```

for clusters.

---

# 2. Read CSV

```python
df = spark.read \
    .option("header", "true") \
    .option("inferSchema", "true") \
    .csv("employees.csv")
```

For interview, this is enough.

---

# 3. Read JSON

```python
df = spark.read.json("employees.json")
```

---

# 4. Read Parquet

```python
df = spark.read.parquet("employees.parquet")
```

---

# 5. Basic Inspection

```python
df.show()

df.printSchema()

df.columns

df.count()

df.describe().show()
```

---

# 6. Select Columns

```python
df.select("name", "salary").show()
```

---

# 7. Filter Rows

```python
df.filter(df.salary > 50000).show()
```

or

```python
df.where(df.age > 25).show()
```

---

# 8. Multiple Conditions

```python
df.filter(
    (df.age > 25) &
    (df.salary > 50000)
).show()
```

OR

```python
df.filter(
    (df.department == "IT") |
    (df.department == "HR")
).show()
```

---

# 9. Add New Column

```python
from pyspark.sql.functions import col

df = df.withColumn(
    "bonus",
    col("salary") * 0.10
)
```

---

# 10. Rename Column

```python
df = df.withColumnRenamed(
    "salary",
    "monthly_salary"
)
```

---

# 11. Drop Column

```python
df = df.drop("age")
```

---

# 12. Sort

Ascending

```python
df.orderBy("salary").show()
```

Descending

```python
from pyspark.sql.functions import desc

df.orderBy(desc("salary")).show()
```

---

# 13. Group By

```python
from pyspark.sql.functions import avg

df.groupBy("department") \
  .agg(avg("salary")) \
  .show()
```

---

# 14. Count

```python
from pyspark.sql.functions import count

df.groupBy("department") \
  .agg(count("*")) \
  .show()
```

---

# 15. Multiple Aggregations

```python
from pyspark.sql.functions import *

df.groupBy("department").agg(
    avg("salary"),
    max("salary"),
    min("salary"),
    sum("salary"),
    count("*")
).show()
```

---

# 16. Join

```python
df.join(
    dept_df,
    on="department_id",
    how="inner"
).show()
```

Other joins

```python
how="left"

how="right"

how="outer"

how="cross"
```

---

# 17. Remove Duplicates

```python
df.dropDuplicates().show()
```

Specific column

```python
df.dropDuplicates(["email"])
```

---

# 18. Distinct Values

```python
df.select("department").distinct().show()
```

---

# 19. Cache

```python
df.cache()
```

Persist

```python
from pyspark import StorageLevel

df.persist(StorageLevel.MEMORY_AND_DISK)
```

---

# 20. Repartition

```python
df = df.repartition(8)
```

---

# 21. Coalesce

```python
df = df.coalesce(2)
```

---

# 22. Broadcast Join

```python
from pyspark.sql.functions import broadcast

df.join(
    broadcast(dept_df),
    "department_id"
).show()
```

---

# 23. SQL Query

```python
df.createOrReplaceTempView("employees")

result = spark.sql("""
SELECT name, salary
FROM employees
WHERE salary > 50000
""")

result.show()
```

---

# 24. Write CSV

```python
df.write \
  .mode("overwrite") \
  .csv("output/")
```

---

# 25. Write Parquet

```python
df.write \
  .mode("overwrite") \
  .parquet("output/")
```

---

# 26. Explain Execution Plan

```python
df.explain()
```

---

# 27. Count Rows

```python
rows = df.count()

print(rows)
```

---

# 28. Show Top 10

```python
df.show(10)
```

---

# 29. Collect

```python
rows = df.collect()

for row in rows:
    print(row)
```

⚠️ Avoid on very large datasets.

---

# 30. UDF Template

```python
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

def convert(name):
    return name.upper()

upper_udf = udf(convert, StringType())

df = df.withColumn(
    "NAME",
    upper_udf(df.name)
)
```

---

# 31. Window Function

```python
from pyspark.sql.window import Window
from pyspark.sql.functions import row_number

window = Window.partitionBy("department") \
               .orderBy("salary")

df = df.withColumn(
    "rank",
    row_number().over(window)
)
```

---

# 32. Common Imports (Cheat Sheet)

Instead of importing one by one, many developers use:

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import *
from pyspark.sql.types import *
from pyspark.sql.window import Window
```

This gives access to most commonly used functions like:

- `col()`
    
- `lit()`
    
- `when()`
    
- `avg()`
    
- `sum()`
    
- `count()`
    
- `max()`
    
- `min()`
    
- `desc()`
    
- `asc()`
    
- `broadcast()`
    
- `concat()`
    
- `regexp_replace()`
    
- `current_date()`
    
- `current_timestamp()`
    

---

# The 90% Interview Workflow

This is the pattern you'll use for most interview questions:

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import *

# Create Spark session
spark = SparkSession.builder \
    .appName("Demo") \
    .master("local[*]") \
    .getOrCreate()

# Read data
df = spark.read \
    .option("header", "true") \
    .option("inferSchema", "true") \
    .csv("employees.csv")

# Inspect data
df.printSchema()
df.show()

# Transform data
result = (
    df.filter(col("salary") > 50000)
      .select("name", "department", "salary")
      .groupBy("department")
      .agg(avg("salary").alias("avg_salary"))
      .orderBy(desc("avg_salary"))
)

# Display results
result.show()

# Save output
result.write.mode("overwrite").parquet("output/")
```

If you understand and can adapt this template, you'll be comfortable with the majority of entry-level PySpark interview coding tasks.