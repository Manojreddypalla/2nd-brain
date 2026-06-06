# ✅ **Syntax**

```cpp
#include <array>

array<int, 5> arr = {1, 2, 3, 4, 5};

```

---

# ⭐ 1. **C – Create (Insert/Add elements)**

👉 `std::array` **cannot change size** → no push/pop/insert.

You can only **assign values** at fixed positions.

### Example:

```cpp
array<int, 5> arr;
arr[0] = 10;
arr[1] = 20;
arr.at(2) = 30;

```

**Think:** Writing items into fixed boxes.

---

# ⭐ 2. **R – Read (Access elements)**

### Access by index:

```cpp
cout << arr[1];
cout << arr.at(2);

```

### Traversal:

```cpp
for(int x : arr)
    cout << x << " ";

```

### Get first & last:

```cpp
arr.front();
arr.back();

```

---

# ⭐ 3. **U – Update (Modify element)**

Just overwrite value.

```cpp
arr[0] = 99;
arr.at(3) = 88;

```

---

# ⭐ 4. **D – Delete**

👉 `std::array` **cannot delete elements** (size fixed).

But you can **reset value** to mark it as deleted.

Example:

```cpp
arr[2] = 0;   // or -1, or any "empty" marker

```

If you want real deletion, **use `vector`**.

---

# ⭐ Quick Table (Perfect for Notes)

|CRUD|std::array Support|Method|
|---|---|---|
|**Create**|Yes|assign using `[]` or `at()`|
|**Read**|Yes|`[]`, `at()`, `front()`, `back()`, loop|
|**Update**|Yes|overwrite index|
|**Delete**|❌ No actual delete|Only overwrite/reset value|