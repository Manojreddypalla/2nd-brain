# 1D Array Passing in C — GATE Notes

## 1. Passing a 1D Array to a Function

Suppose:

```c
int arr[5] = {10, 20, 30, 40, 50};
```

Call:

```c
fun(arr);
```

When an array is passed to a function, **the entire array is NOT copied**.

`arr` gives the address of its first element:

```text
arr
 ↓
&arr[0]
```

So the function receives a pointer to the first element.

---

## 2. Function Parameter

These are effectively equivalent for a 1D array:

```c
void fun(int *a)
```

```c
void fun(int a[])
```

```c
void fun(int a[5])
```

Inside the function, `a` behaves as:

```c
int *a;
```

The number inside `[]` does **not** make the function receive a copy of that many elements.

---

## 3. What Actually Happens?

```c
int arr[] = {10,20,30};

fun(arr);
```

Think:

```text
arr
 ↓
┌────┬────┬────┐
│ 10 │ 20 │ 30 │
└────┴────┴────┘
  ↑
  │
  a
```

Inside:

```c
void fun(int *a)
```

`a` contains the **address of `arr[0]`**.

Therefore:

```c
a[0] → arr[0]
a[1] → arr[1]
a[2] → arr[2]
```

---

## 4. Array Indexing = Pointer Arithmetic

Very important:

```c
a[i] == *(a + i)
```

Therefore:

```c
a[0] == *a
a[1] == *(a + 1)
a[2] == *(a + 2)
```

This is why a pointer can be used to access the array.

---

## 5. Modifying the Array

```c
void fun(int *a)
{
    a[0] = 100;
}
```

If:

```c
int arr[] = {10,20,30};
fun(arr);
```

then:

```text
Before:
arr → [10][20][30]

After:
arr → [100][20][30]
```

Because `a` points to the **original array memory**.

---

## 6. `a = a + 1` vs `*a = *a + 1`

### Pointer changes

```c
a = a + 1;
```

Only the **local pointer `a` moves**.

```text
Before:

a
↓
[10][20][30]

After:

   a
   ↓
[10][20][30]
```

The original array doesn't move.

### Data changes

```c
*a = *a + 1;
```

The value in the original array changes:

```text
Before:
[10][20][30]

After:
[11][20][30]
```

---

## 7. `sizeof` Trap ⚠️

Outside the function:

```c
int arr[5];

sizeof(arr)
```

→ size of the **entire array**.

But inside:

```c
void fun(int arr[])
{
    sizeof(arr);
}
```

`arr` behaves as a **pointer**, so `sizeof(arr)` gives the **pointer size**, not the array size.

---

## 8. Can C Pass an Array by Value?

**No, not directly.**

```c
void fun(int a[])
```

doesn't create a copy.

If you want a copy, you must explicitly create one and copy the elements.

```c
int b[5];

for(int i = 0; i < 5; i++)
    b[i] = a[i];
```

Then `b` and `a` are independent arrays.

---

# 🔥 GATE Quick Revision

```text
Array passed to function
        ↓
address of first element
        ↓
pointer
```

### Remember:

```c
fun(arr);
```

means effectively:

```text
arr → &arr[0]
```

### Equivalent parameters:

```c
int *a
int a[]
int a[5]
```

### Key identity:

```c
a[i] == *(a + i)
```

### Modify original array:

```c
a[i] = value;
```

✅ Original array changes.

### Move pointer:

```c
a = a + 1;
```

❌ Original array doesn't move.

### Most important line:

> **In C, a 1D array is not passed by value; the function receives a pointer to its first element.**