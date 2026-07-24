# MOST IMPORTANT ARRAY RULE

```
Arrays CANNOT be passed by value in C/C++
```

This is MASSIVELY important.

---

# WHY?

Because arrays are huge.

Copying entire arrays every function call is expensive.

So compiler automatically converts array parameter into pointer.

---

# Example

```
voidfun(intarr[])
{
}
```

Actually becomes:

```
voidfun(int *arr)
{
}
```

---

# HUGE CONCEPT

```
Array parameter = pointer
```

inside functions.

---

# EXAMPLE

```
#include<iostream>
usingnamespacestd;

voidfun(intarr[],intn)
{
arr[0] =100;
}

intmain()
{
intA[5] = {1,2,3,4,5};

fun(A,5);

cout<<A[0];
}
```

Output:

```
100
```

---

# WHY DID ORIGINAL ARRAY CHANGE?

Because array passed by address.

Function directly accesses original memory.

---

# MEMORY VISUALIZATION

```
A:
1000 -> 1
1004 -> 2
1008 -> 3
```

Function parameter:

```
arr = 1000
```

Pointer to original array.

---

# IMPORTANT

Inside function:

```
arr[0]
```

really means:

```
*(arr+0)
```

---

# WHY ARRAY SIZE IS PASSED SEPARATELY

```
voidfun(intarr[],intn)
```

Because:

```
array decays into pointer
```

and pointer does NOT know size.

---

# THIS FAILS

```
sizeof(arr)
```

inside function.

Because:

```
arr is pointer now
```

NOT actual array.

---

# ARRAY RETURN FROM FUNCTION

---

# WRONG WAY

```
intarr[5];

returnarr;
```

BAD if local array.

Because local memory dies after function ends.

---

# CORRECT WAY

Use heap:

```
int*create()
{
int *p =newint[5];

returnp;
}
```

---

# WHY THIS WORKS

Heap memory survives function end.

Local stack variables do not.

---

# VERY IMPORTANT DSA CONNECTION

Arrays passed as pointers leads directly to:

```
pointer arithmetic
dynamic arrays
linked lists
memory traversal
```