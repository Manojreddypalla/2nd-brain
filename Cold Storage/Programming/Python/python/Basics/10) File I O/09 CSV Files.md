# 09.  CSV Files

## [**#](https://www.codedex.io/intermediate-python/09-bestsellers#csv-files) CSV Files**

**I don’t think you can handle this. 🔥**

**CSVs... you’ve seen them, you’ve used them, maybe it's complicated between you both. 👀**

**A CSV (comma-separated values) file is a common format for organizing data. Python provides a nifty `csv` module to read and write data in this format.**

**Each row in a CSV file typically represents a record, and the values within a record are separated by commas. This simple structure makes CSV a widely used format for storing and exchanging data between applications.**

## [**#](https://www.codedex.io/intermediate-python/09-bestsellers#reading-csv-files) Reading CSV Files**

**Let’s hit the books. 📚**

**From the `csv` module, we use the `.reader()` method to create a CSV reader object that displays our file content, row by row:**

```python
import csv

# Open the CSV file in 'read' mode with UTF-8 encoding
with open('example.csv', 'r', encoding='utf8') as file:
  # Create a CSV reader object
  csv_reader = csv.reader(file)

  for row in csv_reader:
    print(row)

```

**Sometimes opening CSV files without specifying an encoding can cause decoding errors (especially in IDEs). If you're seeing strange errors when reading the file, try adding `encoding="utf8"` when opening the file. It helps Python properly read characters from the file.**

**The `.reader()` method returns a list of rows where each row is a list of CSV data.**

**With a `for` loop, you can iterate through each row in the reader object.**

## [**#](https://www.codedex.io/intermediate-python/09-bestsellers#writing-to-csv-files) Writing to CSV Files**

**Writing to a CSV file is simple.**

**We use the `.writer()` method to create a CSV writer object, and then use its methods to write rows to the file.**

```python
import csv

# Data to be written to the CSV file
data_to_write = [
  ['Name', 'Age', 'Grade'],
  ['Alice', 25, 'A'],
  ['Bob', 22, 'B'],
  ['Charlie', 28, 'A+']
]

# Open the CSV file in 'write' mode
with open('output.csv', 'w', newline='') as file:
  # Create a CSV writer object
  csv_writer = csv.writer(file)

  # Write the data to the CSV file
  csv_writer.writerows(data_to_write)

```

**The writer object's `.writerows()` method can parse something iterable (like a list of lists!) and write it into the CSV file. In this example there is a `newline` parameter that ensures the results are printed on a new line.**

## **Instructions**

**We've all seen, perhaps bought, one of our favorite products with a "Bestseller" sticker. 🏅**

**Let’s write a program to read a CSV file and find the best-selling book of all time. 🔍**

1. **Download [this CSV file](https://docs.google.com/spreadsheets/d/1NfE3AeQR_8JatDuGcSMysFpa5yr72rD79rkxs05Q9fg/edit?usp=sharing) of all-time bestselling books data!**
2. **Open the Bestseller - Sheet1.csv file in "read" mode.**
3. **Use the CSV reader to navigate through the data and find the book with the highest sales, via the `sales in millions` column.**
4. **Create a new file called bestseller_info.csv with the CSV writer.**
5. **In the new file, use `.writerow()` to add new CSV data.**

**Up next, let’s learn about what to do in the face of the unexpected, we'll be exploring error handling.**

**Help**