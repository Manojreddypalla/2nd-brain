# L-10 — Pages 35–47: Passing 1D Array to Function

## 1. Passing a 1D Array to a Function

Suppose:

```c
int a[5] = {1,2,3,4,5};
```

When we pass:

```c
fun(a);
```

we are **not copying the entire array** into the function.

What gets passed is essentially:

```text
address of a[0]
```

So:

```text
a
↓
&a[0]
```

---

## 2. These three function parameters are equivalent

```c
void fun(int a) { }      // ❌ not array parameter

void fun(int *a) { }     // ✅

void fun(int a[]) { }    // ✅

void fun(int a[10]) { }  // ✅
```

For a **1D array parameter**, these are treated equivalently:

```c
void fun(int *a)
void fun(int a[])
void fun(int a[10])
```

The array size written inside `[]` in the function parameter **does not make a copy of that many elements**.

### 🧠 Remember

```text
fun(a)
  ↓
address of a[0]
  ↓
int *a inside function
```

---

# 3. Why does this work?

Suppose:

```c
int a[5] = {1,2,3,4,5};
fun(a);
```

Imagine:

```text
main():

a:
┌───┬───┬───┬───┬───┐
│ 1 │ 2 │ 3 │ 4 │ 5 │
└───┴───┴───┴───┴───┘
 100 104 108 112 116
  ↑
  a
```

Inside `fun`:

```c
void fun(int *p)
```

`p` receives:

```text
p = address of a[0]
p = 100
```

So both refer to the same array:

```text
main:  a ──────┐
               ↓
            [1][2][3][4][5]
               ↑
function: p ───┘
```

---

# 4. Important consequence: Changes affect original array

Example:

```c
void fun(int *a)
{
    a[0] = 100;
}

int main()
{
    int arr[3] = {10,20,30};
    fun(arr);
}
```

Inside `fun`:

```text
a[0] = 100
```

changes the **original array**.

Why?

Because `a` points to the original array's memory.

```text
arr ──────┐
          ↓
       [10][20][30]
        ↑
        a

after a[0] = 100

       [100][20][30]
```

---

# 5. Very important: `a = a + 1`

The lecture uses:

```c
void fun(int *a)
{
    a = a + 1;
}
```

Here `a` is a **local pointer variable** inside `fun`.

So:

```text
a = a + 1
```

changes where the **local pointer** points.

It does **not** shift the original array.

### Think:

```text
Original:
arr → [10][20][30]

Inside function:
a → [10][20][30]

a = a + 1

a → [20][30]
```

But `arr` in `main` still points to:

```text
arr → [10][20][30]
```

---

# 6. `a = a + 1` vs `*a = *a + 1`

This distinction is **GATE-important**.

### Case 1

```c
a = a + 1;
```

Changes the **pointer**.

```text
a
↓
moves to next element
```

### Case 2

```c
*a = *a + 1;
```

Changes the **value being pointed to**.

Example:

```text
Before:

a → [10][20][30]

*a = *a + 1

After:

a → [11][20][30]
```

---

# 7. Pointer arithmetic on arrays

If:

```c
int a[] = {10,20,30,50};
int *p = a;
```

then:

```c
p
```

points to:

```text
10
```

and:

```c
p + 1
```

points to:

```text
20
```

because pointer arithmetic automatically accounts for the size of the pointed-to type.

For an `int *`:

```text
p + 1 → next int
```

not simply "next byte."

---

# 8. Array parameter doesn't preserve array size

Consider:

```c
void fun(int a[5])
```

Inside the function, `a` behaves as a pointer:

```c
int *a
```

Therefore:

```c
sizeof(a)
```

inside the function gives the **size of a pointer**, not the size of the original array.

### ⚠️ GATE trap

```c
int arr[5];

sizeof(arr)
```

→ size of entire array.

But:

```c
void fun(int arr[])
{
    sizeof(arr);
}
```

→ size of pointer.

---

# 9. The array itself vs array parameter

### In `main`

```c
int a[5];
```

`a` represents an array object.

```text
a:
[ element ][ element ][ element ][ element ][ element ]
```

### In function

```c
void fun(int a[])
```

`a` is adjusted to a pointer parameter.

```text
a → address of first element
```

This distinction is crucial.

---

# 10. Array indexing is pointer-based

Inside:

```c
void fun(int *a)
```

you can write:

```c
a[0]
a[1]
a[2]
```

because:

```text
a[i] ≡ *(a + i)
```

So:

```c
a[0] = *(a + 0)
a[1] = *(a + 1)
a[2] = *(a + 2)
```

This is the connection between **arrays and pointers**.

---

# 11. Example from the lecture

Suppose:

```c
int arr[] = {10,0,1,3,5};

fun(arr);
```

and:

```c
void fun(int a[])
{
    a = a + 1;
}
```

Initially:

```text
a
↓
10   0   1   3   5
```

After:

```c
a = a + 1;
```

the local `a` points to:

```text
     a
     ↓
10   0   1   3   5
```

Then:

```c
a[0]
```

would refer to `0`, not `10`.

The original array itself hasn't moved.

---

# 12. Array parameter forms

These are equivalent for a 1D array parameter:

```c
void fun(int *a)
void fun(int a[])
void fun(int a[5])
void fun(int a[100])
```

The number inside `[]` does **not control the actual array size passed**.

The important thing is:

```text
function receives → pointer to first element
```

---

# 13. Can we modify the array inside the function?

Yes.

```c
void fun(int *a)
{
    a[0] = 99;
}
```

Because:

```text
a → original array
```

Therefore:

```text
a[0] = 99
```

modifies the original array.

But:

```c
a = a + 1;
```

only changes the **local pointer**.

---

# 14. What exactly gets passed?

For:

```c
int arr[5] = {1,2,3,4,5};

fun(arr);
```

Think:

```text
arr
 ↓
&arr[0]
 ↓
pointer passed to function
```

So:

```c
void fun(int *p)
```

receives:

```text
p = &arr[0]
```

---

# 15. GATE Mental Model 🔥

Whenever you see:

```c
int arr[] = {...};
fun(arr);
```

immediately draw:

```text
arr
 ↓
[ a[0] ][ a[1] ][ a[2] ][ a[3] ]
   ↑
   p
```

Then ask:

### Is the code changing:

**A. Pointer?**

```c
p = p + 1;
```

→ pointer moves.

### B. Data?

```c
*p = *p + 1;
```

→ array element changes.

### C. Another element?

```c
p[2] = 50;
```

→ `arr[2]` changes.

This simple classification will save you a LOT of time in pointer questions.

---

# ⚡ Pages 35–47 — Fast Revision

```text
1D array passed to function
        ↓
address of first element
        ↓
pointer
```

### Equivalent parameter forms

```c
void fun(int *a)
void fun(int a[])
void fun(int a[10])
```

### Inside function

```text
a[i] == *(a+i)
```

### Modify data

```c
*a = ...
a[i] = ...
```

→ modifies original array.

### Move local pointer

```c
a = a + 1;
```

→ only local pointer changes.

### Key GATE trap

```text
array in main
    ≠
array parameter inside function
```

The latter behaves as a **pointer**.

The lecture's final page for this section summarizes exactly that: **what gets passed is a pointer to the array's first element**, and the parameter name can be used on the left side of assignment inside the called function.

**One-line memory hook:**

> **Array → function = address; pointer moves ≠ array moves.**