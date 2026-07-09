# NumPy Chapter 6 – Slicing

> **Goal:** Learn how to extract parts of an array without writing loops.

---

# Mental Model

Indexing → Pick one item

```python
arr[2,1]
```

Output

```
8
```

---

Slicing → Pick multiple items

```python
arr[1:3,0:2]
```

Output

```
[[4 5]
 [7 8]]
```

Think of slicing as **cutting a rectangle** from an array.

---

# Example Array

```python
import numpy as np

arr = np.arange(1,26).reshape(5,5)

print(arr)
```

```
      C0 C1 C2 C3 C4

R0     1  2  3  4  5

R1     6  7  8  9 10

R2    11 12 13 14 15

R3    16 17 18 19 20

R4    21 22 23 24 25
```

---

# Slice Syntax

```python
array[start:end]
```

Read it as

```
Start

↓

Go until

↓

Stop BEFORE end
```

Important

```
End index is NOT included.
```

---

Example

```python
arr[1:4]
```

Means

```
Rows

1

2

3
```

NOT row 4.

---

# Row Slicing

First two rows

```python
arr[:2]
```

Output

```
Rows 0 and 1
```

---

Last two rows

```python
arr[-2:]
```

Output

```
Rows 3 and 4
```

---

Middle rows

```python
arr[1:4]
```

Output

```
Rows 1,2,3
```

---

# Column Slicing

First column

```python
arr[:,0]
```

---

First two columns

```python
arr[:,:2]
```

Output

```
Columns

0

1
```

---

Last column

```python
arr[:,-1]
```

---

Last two columns

```python
arr[:,-2:]
```

---

# Rectangle Selection

General syntax

```python
array[
rows,
columns
]
```

Example

```python
arr[1:4,1:4]
```

Rows

```
1
2
3
```

Columns

```
1
2
3
```

Selected region

```
7   8   9

12 13 14

17 18 19
```

---

# Entire Array

```python
arr[:,:]
```

Means

```
All rows

All columns
```

---

# Every Other Row

```python
arr[::2]
```

Output

```
Rows

0

2

4
```

---

Every Other Column

```python
arr[:,::2]
```

Output

```
Columns

0

2

4
```

---

# Step Size

General syntax

```python
start:end:step
```

Example

```python
arr[::3]
```

Rows

```
0

3
```

---

Columns

```python
arr[:,::3]
```

Columns

```
0

3
```

---

# Reverse

Entire array backwards

```python
arr[::-1]
```

Rows become

```
4

3

2

1

0
```

---

Reverse columns

```python
arr[:,::-1]
```

Output

```
5 4 3 2 1

10 9 8 7 6

...
```

---

Reverse both

```python
arr[::-1,::-1]
```

Everything flips.

---

# Crop an Image

Images are arrays.

Suppose

```python
image.shape

(720,1280,3)
```

Crop the center

```python
crop = image[
200:500,
300:900
]
```

Meaning

Rows

```
200 → 499
```

Columns

```
300 → 899
```

You just extracted a smaller image.

---

# RGB Images

Shape

```
(H,W,3)
```

Crop while keeping colors

```python
crop = image[
100:300,
200:400,
:
]
```

Last `:`

Means

```
Keep all RGB channels.
```

---

# Real Examples

Students

```python
marks = np.array([
[90,80,70],
[60,50,40],
[95,91,88],
[77,66,55]
])
```

First two students

```python
marks[:2]
```

---

Only Math marks

```python
marks[:,0]
```

---

Last two students

```python
marks[-2:]
```

---

# Common Mistakes

Wrong

```python
arr[1:5]
```

Thinking

```
1

2

3

4

5
```

Wrong.

Actually

```
1

2

3

4
```

Remember

```
End is excluded.
```

---

Wrong

```python
arr[5:]
```

If row 5 doesn't exist

Output

```
Empty array
```

NOT an error.

---

# Cheat Sheet

| Code | Meaning |
|------|---------|
| `arr[:]` | Everything |
| `arr[:3]` | First 3 rows |
| `arr[3:]` | From row 3 onward |
| `arr[-2:]` | Last 2 rows |
| `arr[:,0]` | First column |
| `arr[:,-1]` | Last column |
| `arr[:,1:3]` | Columns 1 and 2 |
| `arr[1:4,2:5]` | Rectangle |
| `arr[::-1]` | Reverse rows |
| `arr[:,::-1]` | Reverse columns |
| `arr[::-1,::-1]` | Reverse everything |
| `arr[::2]` | Every second row |
| `arr[:,::2]` | Every second column |

---

# Memory Trick

```
array[
rows,
columns
]
```

Each side follows

```
start : end : step
```

Example

```python
arr[1:4,2:5]
```

Read it like English

```
Rows

1 to before 4

AND

Columns

2 to before 5
```

---

# Practice

```python
arr = np.arange(1,26).reshape(5,5)
```

```
1   2   3   4   5
6   7   8   9  10
11 12  13 14 15
16 17  18 19 20
21 22  23 24 25
```

Find:

1. First 2 rows
2. Last 2 rows
3. Middle 3×3
4. First 3 columns
5. Last 2 columns
6. Every other row
7. Every other column
8. Reverse rows
9. Reverse columns
10. Reverse the entire matrix

---

# Key Takeaways

✅ Slicing returns multiple elements.

✅ Syntax

```python
start:end:step
```

✅ End index is excluded.

✅ `:` means everything.

✅ Slicing is used for image cropping, dataset selection, and feature extraction.

✅ Master slicing before moving to Views vs Copies.