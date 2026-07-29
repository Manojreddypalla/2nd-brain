# 📘 NumPy Like an Engineer

# Chapter 5 — Indexing (The Superpower of NumPy)

> **Goal:** Learn how to access exactly the data you want.

If you master indexing, then **slicing, cropping images, selecting datasets, and machine learning preprocessing** become much easier.

---

# Learning Objectives

You'll learn:

- Access single elements
    
- Negative indexing
    
- 2D indexing
    
- 3D indexing
    
- Row selection
    
- Column selection
    
- Common mistakes
    

---

# Big Idea

A NumPy array is like an Excel sheet.

```text
        Columns
        0   1   2

Rows 0  1   2   3
     1  4   5   6
     2  7   8   9
```

Always remember:

```python
array[row, column]
```

👉 **Rows first, Columns second**

---

# Create an Example

```python
import numpy as np

arr = np.array([
    [1,2,3],
    [4,5,6],
    [7,8,9]
])
```

Visualize it

```text
        C0  C1  C2

R0      1   2   3

R1      4   5   6

R2      7   8   9
```

---

# 1. Access One Element

Syntax

```python
arr[row, column]
```

Example

```python
print(arr[0,0])
```

Output

```text
1
```

Because

```text
Row 0

Column 0
```

---

Example

```python
print(arr[1,2])
```

Look at the table

```text
        C0 C1 C2

R0      1  2  3

R1      4  5 [6]

R2      7  8  9
```

Output

```text
6
```

---

# 2. Negative Indexing

Negative means

> Start counting from the end.

Example

```python
print(arr[-1,-1])
```

Last row

Last column

Output

```text
9
```

---

```python
print(arr[-1,0])
```

Last row

First column

Output

```text
7
```

---

Think

```text
0 1 2

↓

-3 -2 -1
```

---

# 3. Access an Entire Row

```python
print(arr[0])
```

Output

```text
[1 2 3]
```

NumPy assumes

```python
arr[0,:]
```

means

```text
Row 0

All Columns
```

---

Second row

```python
print(arr[1])
```

Output

```text
[4 5 6]
```

---

# 4. Access an Entire Column

This is where beginners get confused.

```python
print(arr[:,0])
```

Let's decode it.

```text
:

↓

Everything
```

So

```python
arr[:,0]
```

means

```text
All Rows

Column 0
```

Highlight

```text
      C0 C1 C2

R0   [1] 2  3

R1   [4] 5  6

R2   [7] 8  9
```

Output

```text
[1 4 7]
```

---

Second column

```python
arr[:,1]
```

Output

```text
[2 5 8]
```

---

Third column

```python
arr[:,2]
```

Output

```text
[3 6 9]
```

---

# Easy Trick

Whenever you see

```python
:
```

Read it as

> **Everything**

Examples

```python
arr[:,0]
```

↓

Everything in column 0

---

```python
arr[1,:]
```

↓

Everything in row 1

---

```python
arr[:,:]
```

↓

Everything

---

# 5. Modify Elements

```python
arr[0,0] = 100

print(arr)
```

Output

```text
100 2 3

4   5 6

7   8 9
```

---

Entire row

```python
arr[1] = [10,20,30]
```

Output

```text
1 2 3

10 20 30

7 8 9
```

---

Entire column

```python
arr[:,1] = 0
```

Output

```text
1 0 3

4 0 6

7 0 9
```

---

# 6. Indexing in 3D

Suppose

```python
cube = np.array([
[
 [1,2],
 [3,4]
],

[
 [5,6],
 [7,8]
]
])
```

Shape

```text
(2,2,2)
```

Think

```text
Layer

↓

Row

↓

Column
```

---

Visual

```text
Layer 0

1 2

3 4

---------

Layer 1

5 6

7 8
```

---

Access

```python
cube[0,1,1]
```

Means

```text
Layer 0

↓

Row 1

↓

Column 1
```

Output

```text
4
```

---

Another

```python
cube[1,0,1]
```

Output

```text
6
```

---

General Rule

```python
array[
Layer,
Row,
Column
]
```

---

# 7. Images

Images are NumPy arrays.

Example

```python
image.shape
```

```text
(720,1280,3)
```

Meaning

```text
Height

↓

Width

↓

RGB
```

Pixel

```python
image[100,200]
```

Output

```text
[255 120 80]
```

Meaning

```text
Red

Green

Blue
```

---

Single color

```python
image[100,200,0]
```

Only

```text
Red
```

---

# Common Mistakes

## Wrong

```python
arr[3]
```

If the array has only 3 rows

```text
0

1

2
```

Row 3 doesn't exist.

You'll get

```text
IndexError
```

---

## Wrong

```python
arr[0,5]
```

Column 5 doesn't exist.

---

## Wrong

Confusing row and column.

Many beginners think

```python
arr[0,1]
```

means

```text
Column 0

Row 1
```

❌ No.

Always

```text
Row

↓

Column
```

---

# Real Examples

### Student Marks

```python
marks = np.array([
    [90,85,88],
    [70,60,75],
    [95,92,91]
])
```

Second student's marks

```python
marks[1]
```

Output

```text
70 60 75
```

---

Math marks

```python
marks[:,0]
```

Output

```text
90

70

95
```

---

# Cheat Sheet

|Syntax|Meaning|
|---|---|
|`arr[0]`|First row|
|`arr[-1]`|Last row|
|`arr[:,0]`|First column|
|`arr[:,-1]`|Last column|
|`arr[1,2]`|One element|
|`arr[-1,-1]`|Last element|
|`arr[:,:]`|Entire array|

---

# Practice

```python
arr = np.arange(1,17).reshape(4,4)
```

It looks like

```text
1   2   3   4

5   6   7   8

9  10  11  12

13 14  15  16
```

Find:

1. First element
    
2. Last element
    
3. Second row
    
4. Third column
    
5. Last row
    
6. First column
    
7. Element **11**
    
8. Replace **16** with **100**
    

---

# Mental Model

Whenever you index a NumPy array, imagine a cursor moving through a grid:

```text
array[row, column]

       ↓
   Pick the row first

       ↓
   Then move to the column
```

For 3D arrays:

```text
array[layer, row, column]

        ↓
     Choose the layer

        ↓
      Choose the row

        ↓
    Choose the column
```

Once this clicks, you'll be ready for the next step.

---

# 🚀 Next Chapter: Slicing

Indexing gets **one item**.

```python
arr[2, 3]
```

Slicing gets **many items**.

```python
arr[1:4, 2:5]
```

This is how you crop images, extract datasets, and work efficiently with large arrays. It's one of the most important skills in NumPy.