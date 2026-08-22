# 📌 Binary Search — Quick Revision Notes (Obsidian)

## 💡 Intuition

- Works only on **sorted data**.
    
- Compare with the **middle** element.
    
- Eliminate **half** of the search space every step.
    
- Time Complexity: **O(log n)**
    

---

# Standard Template

```cpp
int low = 0, high = n - 1;

while (low <= high) {
    int mid = low + (high - low) / 2;

    if (arr[mid] == target)
        return mid;
    else if (arr[mid] < target)
        low = mid + 1;
    else
        high = mid - 1;
}

return -1;
```

---

# Safe Mid Calculation

```cpp
mid = low + (high - low) / 2;
```

✅ Prevents integer overflow.

---

# Three Cases

```text
arr[mid] == target
→ Found

arr[mid] < target
→ Search Right
→ low = mid + 1

arr[mid] > target
→ Search Left
→ high = mid - 1
```

---

# When to Use Binary Search?

- ✅ Sorted array/vector
    
- ✅ Search an element
    
- ✅ Find first/last occurrence
    
- ✅ Find insertion position
    
- ✅ Monotonic search space (Binary Search on Answer)
    

---

# Common Variants

|Problem|Idea|
|---|---|
|Normal Binary Search|Find target|
|Lower Bound|First element **>= target**|
|Upper Bound|First element **> target**|
|First Occurrence|On match, move left|
|Last Occurrence|On match, move right|
|Search Insert Position|Lower Bound|

---

# Complexity

|Operation|Complexity|
|---|---|
|Time|**O(log n)**|
|Space|**O(1)**|

---

# Common Mistakes

❌ Using Binary Search on an unsorted array.

❌ Using:

```cpp
while (low < high)
```

for standard Binary Search (may miss the last element).

✅ Use:

```cpp
while (low <= high)
```

---

❌ Overflow:

```cpp
mid = (low + high) / 2;
```

✅ Safe:

```cpp
mid = low + (high - low) / 2;
```

---

# Recognition Pattern

```text
Sorted?
      │
      ▼
Can discard half after one comparison?
      │
      ▼
Use Binary Search
```

---

## ⭐ Memory Trick

> **Less → Go Right (`low = mid + 1`)**  
> **Greater → Go Left (`high = mid - 1`)**  
> **Equal → Found**