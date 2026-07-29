Here's an **all-in-one NumPy practice file** that covers everything from Chapter 21. Save it as `numpy_practice.py` and run each section.

```python
import numpy as np

print("=" * 60)
print("NUMPY CHAPTER 21 PRACTICE")
print("=" * 60)

# -------------------------------------------------
# 1. Create a 5x5 matrix of zeros
# -------------------------------------------------
print("\n1. 5x5 Zeros Matrix")
zeros = np.zeros((5, 5), dtype=int)
print(zeros)

# -------------------------------------------------
# 2. Identity Matrix
# -------------------------------------------------
print("\n2. 4x4 Identity Matrix")
identity = np.eye(4, dtype=int)
print(identity)

# -------------------------------------------------
# 3. Random Matrix
# -------------------------------------------------
print("\n3. Random 3x3 Matrix")
random_matrix = np.random.randint(1, 101, (3, 3))
print(random_matrix)

# -------------------------------------------------
# 4. Reshape
# -------------------------------------------------
print("\n4. Reshape")
arr = np.arange(12)
print(arr.reshape(3, 4))

# -------------------------------------------------
# 5. Transpose
# -------------------------------------------------
print("\n5. Transpose")
matrix = np.arange(1, 10).reshape(3, 3)
print(matrix)
print("Transpose:")
print(matrix.T)

# -------------------------------------------------
# 6. Crop Center
# -------------------------------------------------
print("\n6. Crop Center 50x50")
image = np.random.randint(0, 256, (100, 100), dtype=np.uint8)
crop = image[25:75, 25:75]
print(crop.shape)

# -------------------------------------------------
# 7. Brightness
# -------------------------------------------------
print("\n7. Brightness +40")
bright = np.clip(image + 40, 0, 255)
print(bright)

# -------------------------------------------------
# 8. Reverse Every Row
# -------------------------------------------------
print("\n8. Reverse Rows")
matrix = np.arange(1, 17).reshape(4, 4)
print(matrix)
print(matrix[:, ::-1])

# -------------------------------------------------
# 9. Maximum & Index
# -------------------------------------------------
print("\n9. Maximum")
arr = np.array([5, 20, 8, 40, 30])

print("Maximum:", arr.max())
print("Index:", arr.argmax())

# -------------------------------------------------
# 10. Even Numbers
# -------------------------------------------------
print("\n10. Even Numbers")
print(arr[arr % 2 == 0])

# -------------------------------------------------
# 11. Matrix Multiplication
# -------------------------------------------------
print("\n11. Matrix Multiplication")

A = np.array([
    [1, 2],
    [3, 4]
])

B = np.array([
    [5, 6],
    [7, 8]
])

print(A @ B)

# -------------------------------------------------
# 12. Normalize Image
# -------------------------------------------------
print("\n12. Normalize")
image = np.random.randint(0, 256, (4, 4), dtype=np.uint8)
normalized = image / 255.0

print(image)
print(normalized)

# -------------------------------------------------
# 13. Mean Per Column
# -------------------------------------------------
print("\n13. Mean Per Column")

matrix = np.arange(1, 10).reshape(3, 3)
print(matrix.mean(axis=0))

# -------------------------------------------------
# 14. Mean Per Row
# -------------------------------------------------
print("\n14. Mean Per Row")

print(matrix.mean(axis=1))

# -------------------------------------------------
# 15. Remove Duplicates
# -------------------------------------------------
print("\n15. Unique")

arr = np.array([1, 2, 2, 3, 3, 4, 5, 5])

print(np.unique(arr))

# -------------------------------------------------
# 16. Challenge Matrix
# -------------------------------------------------
print("\n16. Challenge")

arr = np.arange(1, 26).reshape(5, 5)

print(arr)

print("\nCenter 3x3")
print(arr[1:4, 1:4])

print("\nFirst Column")
print(arr[:, 0])

print("\nLast Row")
print(arr[-1])

print("\nReverse Rows")
print(arr[::-1])

print("\nReverse Columns")
print(arr[:, ::-1])

print("\nNumbers > 15")
print(arr[arr > 15])

print("\nRow Sum")
print(arr.sum(axis=1))

print("\nColumn Mean")
print(arr.mean(axis=0))

# -------------------------------------------------
# 17. Checkerboard
# -------------------------------------------------
print("\n17. Checkerboard")

checker = np.zeros((10, 10), dtype=int)

checker[::2, ::2] = 1
checker[1::2, 1::2] = 1

print(checker)

# -------------------------------------------------
# BONUS
# -------------------------------------------------
print("\n18. Rotate")

print(np.rot90(arr))

print("\n19. Flip Left-Right")

print(np.fliplr(arr))

print("\n20. Flip Up-Down")

print(np.flipud(arr))

print("\n21. Flatten")

print(arr.flatten())

print("\n22. Reshape")

print(arr.reshape(25))

print("\n23. Random Shuffle")

nums = np.arange(10)
np.random.shuffle(nums)
print(nums)

print("\n24. Sort")

print(np.sort(nums))

print("\n25. Statistics")

print("Sum      :", arr.sum())
print("Mean     :", arr.mean())
print("Min      :", arr.min())
print("Max      :", arr.max())
print("Std      :", arr.std())
print("Variance :", arr.var())

print("\nFinished! 🎉")
```

### ⭐ This one file practices almost everything you've learned:

- ✅ Creating arrays
    
- ✅ Reshape
    
- ✅ Indexing
    
- ✅ Slicing
    
- ✅ Transpose
    
- ✅ Matrix multiplication
    
- ✅ Boolean masking
    
- ✅ Broadcasting (`+40`)
    
- ✅ Aggregation
    
- ✅ Statistics
    
- ✅ Random module
    
- ✅ Image operations
    
- ✅ Sorting
    
- ✅ Unique values
    
- ✅ Checkerboard challenge
    
- ✅ Reverse arrays
    
- ✅ Normalize data
    

This is enough practice before moving on to the last few chapters. After Chapter 25, we'll do the comprehensive review you asked for, connecting all these concepts together.