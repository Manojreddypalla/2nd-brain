You're right. I added general C knowledge instead of **extracting the PPT**. Let's keep this **strictly to what is written/shown in L-8.pdf**, with no extra concepts, no GATE additions, and no questions.

# Array — L-8 PPT Notes

## 1. Array

- Array is a data structure which can represent a collection of data items which have the **same data type**:
    
    - `float`
        
    - `int`
        
    - `char`
        
    - etc.
        
- All the data items constituting the group share the **same name**.
    

Example:

```c
int x[10];
```

→ collection of 10 integers.

---

## 2. Accessing Array Elements

- Individual elements are accessed by specifying the **index**.
    
- Example:
    

```c
int x[10];
```

Elements:

```text
x[0]
x[1]
x[2]
...
x[9]
```

- `x` is a **10-element one-dimensional array**.
    
- Array elements are stored in **contiguous memory**.
    

---

## 3. Example

```c
int number[5];
```

Elements:

```text
number[0]
number[1]
number[2]
number[3]
number[4]
```

Values can be assigned individually:

```c
number[0] = 35;
number[1] = 40;
number[2] = 20;
number[3] = 57;
number[4] = 19;
```

Result:

```text
number[0] → 35
number[1] → 40
number[2] → 20
number[3] → 57
number[4] → 19
```

---

# 4. Array Initialization

### 1.

```c
int myArray[10] = {5, 5, 5, 5, 5, 5, 5, 5, 5, 5};
```

All elements are initialized to `5`.

### 2.

```c
int myArray[10] = {1, 2};
```

Initialized as:

```text
1 2 0 0 0 0 0 0 0 0
```

### 3.

```c
int myArray[10] = {0};
```

All elements are `0`.

### 4.

```c
static int myArray[10];
```

All elements are `0`.

---

## 5. Local vs Global Initialization

```c
int myArray[10];
```

- Local → all values are **garbage**
    
- Global → all values are **zero**
    

---

## 6. Selective Initialization

For:

```c
int a[10] = {0, 0, 1, 0, 2};
```

The **3rd and 5th elements** are:

```text
a[2] = 1
a[4] = 2
```

Everything else is zero.

So:

```text
a = {0, 0, 1, 0, 2, 0, 0, 0, 0, 0}
```

---

## 7. Array Initialization

```c
int a[10] = {0};
```

---

# 8. Character Array Initialization

In case of a **character array**, the default value is the **null character `'\0'`**.

ASCII value:

```text
'\0' → 0
```

### 1.

```c
char t[5] = {'a', 'b', 'c', 'd'};
```

Initialized to:

```text
'a' 'b' 'c' 'd' '\0'
```

### 2.

```c
char t[4] = {'a', 'b', 'c', 'd'};
```

Initialized to:

```text
'a' 'b' 'c' 'd'
```

### 3.

```c
char s[5] = "abcd";
```

Initialized to:

```text
'a' 'b' 'c' 'd' '\0'
```

**Short form of list**

### 4.

```c
char s[4] = "abcd";
```

Initialized to:

```text
'a' 'b' 'c' 'd'
```

**Short form of string**

---

# 9. Dimension Not Specified

If dimension is not specified:

> The compiler will deduce the dimension from the initializer list.

Example:

```c
int myArray[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
```

Dimension is determined from the initializer list.

Another example:

```c
char s[] = "abcd";
```

Equivalent to:

```c
char s[5] = "abcd";
```

because:

```text
a b c d \0
```

---

# 10. Character Array — Size Difference

```c
char c[] = {'a', 'b', 'c', 'd'};
```

Size of `c`:

```text
4 characters
```

```c
char c[] = "abcd";
```

Size of `c`:

```text
5 characters
```

because:

```text
a b c d \0
```

---

## Final PPT-Only Revision

```text
ARRAY
│
├── Collection of same data type
├── Same name
├── Individual elements → index
├── Contiguous memory
│
├── int x[10]
│   └── 10-element one-dimensional array
│
├── Initialization
│   ├── {5,5,5,...}
│   ├── {1,2} → remaining 0
│   ├── {0} → all 0
│   └── static array → all 0
│
├── int myArray[10];
│   ├── local → garbage
│   └── global → zero
│
├── Character array
│   └── default → '\0' (ASCII 0)
│
├── Dimension omitted
│   └── compiler deduces dimension
│
└── "abcd"
    ├── char s[5] → a b c d '\0'
    └── char s[4] → a b c d
```

**That's it — only the Array material actually shown in the PPT, no additional array/pointer theory.**