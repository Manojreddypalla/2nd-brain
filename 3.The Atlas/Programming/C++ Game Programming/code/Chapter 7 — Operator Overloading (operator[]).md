Perfect. Now we're entering one of the coolest parts of C++. Everything we've built so far was preparing for this.

We'll first learn **Operator Overloading**, then **Templates**, because that's also the order your lecture follows (`Operator[]` then templating).

---

# Chapter 7 — Operator Overloading (`operator[]`)

> **Goal:** Understand how C++ lets you make your own classes behave like built-in types.

---

# The Problem

Our current class looks something like this.

```cpp
class IntArray
{
private:
    int* m_Data;
    size_t m_Size;

public:
    IntArray(size_t size);
    ~IntArray();
};
```

Suppose we want to store a value.

How should we do it?

Option 1

```cpp
arr.set(0, 10);
```

Option 2

```cpp
arr.get(0);
```

This works...

But it doesn't feel like an array.

Real arrays use

```cpp
arr[0]
```

Can we make our own class support this syntax?

**Yes.**

That's called **Operator Overloading**.

---

# What is Operator Overloading?

Every operator in C++ is actually a function.

When you write

```cpp
a + b
```

The compiler internally thinks

```cpp
a.operator+(b)
```

(or a non-member overload, depending on the operator).

Similarly,

```cpp
arr[2]
```

becomes

```cpp
arr.operator[](2);
```

The compiler simply rewrites the syntax.

---

# What is an Operator?

Operators are symbols that perform operations.

Examples

```cpp
+
-
*
/
[]
()
==
<
>
=
++
--
<<
>>
```

Normally,

these work only for built-in types.

```cpp
int a = 10;
int b = 20;

a + b;
```

works because

the compiler already knows

how to add integers.

But

```cpp
IntArray arr(5);

arr[2];
```

doesn't compile

because the compiler has no idea

what

```cpp
[]
```

means for

```cpp
IntArray
```

So we teach it.

---

# The Syntax

```cpp
ReturnType operator[](Parameter)
{
}
```

For our array

```cpp
int& operator[](size_t index)
{
    return m_Data[index];
}
```

Let's understand every piece.

---

# Why `operator[]`?

The keyword

```cpp
operator
```

tells the compiler

> "I'm defining how this operator behaves."

The symbol

```cpp
[]
```

identifies

which operator.

So

```cpp
operator[]
```

means

> "Define the behavior of square brackets."

---

# Why `size_t index`?

Suppose we write

```cpp
arr[3]
```

The compiler transforms it into

```cpp
arr.operator[](3);
```

Therefore

```text
3

↓

Parameter

↓

index
```

So

inside the function

```cpp
index == 3
```

---

# Returning the Element

Suppose

```cpp
m_Data

↓

7000
```

Memory

```text
7000 → 10

7004 → 20

7008 → 30

7012 → 40
```

If

```cpp
index == 2
```

then

```cpp
m_Data[index]
```

means

```cpp
m_Data[2]
```

which becomes

```cpp
*(m_Data + 2)
```

Address

```text
7000

+

2 × 4

=

7008
```

Read value

```text
30
```

Return it.

---

# Why Return `int&`?

This is one of the most important questions.

Many beginners write

```cpp
int operator[](size_t index)
```

instead of

```cpp
int&
```

Let's see why that's wrong.

---

## Returning by Value

Suppose

```cpp
int operator[](size_t index)
{
    return m_Data[index];
}
```

Memory

```text
Heap

10

20

30
```

Now

```cpp
int x = arr[1];
```

The function returns

```text
20
```

A copy is created.

Memory

```text
Heap

20

↓

Copy

↓

x
```

Works fine.

But now

```cpp
arr[1] = 99;
```

What happens?

You're trying to assign

to a temporary copy.

That doesn't modify the original array.

---

## Returning by Reference

Instead

```cpp
int& operator[](size_t index)
{
    return m_Data[index];
}
```

Now

the function returns

the actual object.

Not a copy.

Memory

```text
Heap

Address 7004

↓

20
```

Returned

```text
Reference

↓

Same Integer
```

Now

```cpp
arr[1]=99;
```

becomes

```cpp
m_Data[1]=99;
```

Original memory changes.

---

# Visual Memory

Before

```text
Heap

10

20

30
```

Code

```cpp
arr[1]=99;
```

After

```text
Heap

10

99

30
```

Exactly what we wanted.

---

# Dry Run

Suppose

```cpp
arr[2]=50;
```

Compiler transforms

```cpp
arr.operator[](2)=50;
```

Now

inside

```cpp
operator[]
```

```cpp
return m_Data[2];
```

returns

```text
Reference

↓

Heap Element
```

Assignment

```cpp
=50;
```

changes

the original memory.

---

# Why Can We Assign?

This is because

the function returns

```cpp
int&
```

Remember

a reference

is simply

another name

for an existing object.

So

```cpp
arr[2]
```

becomes

another name

for

```cpp
m_Data[2]
```

Therefore

```cpp
arr[2]=50;
```

is identical to

```cpp
m_Data[2]=50;
```

---

# Reading Still Works

Suppose

```cpp
std::cout<<arr[2];
```

Compiler

```cpp
arr.operator[](2)
```

returns

```cpp
int&
```

Printing

a reference

prints

the integer.

Output

```text
50
```

---

# Const Overload

Suppose

```cpp
const IntArray arr(5);
```

Now

```cpp
arr[2]=50;
```

should NOT compile.

Why?

Because

the object is const.

We shouldn't modify it.

So we add another function.

```cpp
const int& operator[](size_t index) const
{
    return m_Data[index];
}
```

Notice

There are two

```cpp
const
```

keywords.

---

First

```cpp
const int&
```

means

```text
Reference

↓

Cannot modify value
```

Second

```cpp
... const
```

means

```text
This function

doesn't modify

the object.
```

These are different.

---

# Two Versions

Normal Object

```cpp
IntArray arr;
```

Uses

```cpp
int&
```

Const Object

```cpp
const IntArray arr;
```

Uses

```cpp
const int&
```

The compiler automatically chooses the correct overload.

---

# Bounds Checking

Our operator currently does

```cpp
return m_Data[index];
```

What if

```cpp
index == 100
```

for

```cpp
IntArray(5)
```

Memory

```text
Heap

10

20

30

40

50
```

Accessing

```cpp
m_Data[100]
```

is **Undefined Behavior**.

A safer implementation might check:

```cpp
if(index >= m_Size)
    throw std::out_of_range("Index out of bounds");
```

The real `std::vector` provides both:

- `operator[]` → fast, no bounds checking.
    
- `at()` → checks bounds and throws an exception.
    

This is a deliberate design choice balancing performance and safety.

---

# Complete Implementation

```cpp
class IntArray
{
private:
    int* m_Data;
    size_t m_Size;

public:
    IntArray(size_t size)
        : m_Size(size),
          m_Data(new int[size])
    {
    }

    ~IntArray()
    {
        delete[] m_Data;
    }

    int& operator[](size_t index)
    {
        return m_Data[index];
    }

    const int& operator[](size_t index) const
    {
        return m_Data[index];
    }
};
```

Now

```cpp
IntArray arr(3);

arr[0] = 10;
arr[1] = 20;
arr[2] = 30;

std::cout << arr[1];
```

Output

```text
20
```

The class now behaves almost exactly like a built-in array.

---

# Why This Is Powerful

Operator overloading lets your own classes feel natural.

Imagine if `std::vector` worked like this:

```cpp
numbers.set(0,10);

numbers.set(1,20);

numbers.get(0);
```

Instead,

thanks to operator overloading,

we write

```cpp
numbers[0]=10;

numbers[1]=20;

std::cout<<numbers[0];
```

Much cleaner.

This is why C++ allows operator overloading—not to be clever, but to make user-defined types feel like built-in ones.

---

# Common Mistakes

## ❌ Returning by Value

```cpp
int operator[](size_t index)
```

Creates a copy.

Assignments like

```cpp
arr[2] = 50;
```

won't modify the original array.

---

## ❌ Returning a Reference to a Local Variable

```cpp
int& foo()
{
    int x = 10;
    return x;
}
```

`x` is destroyed when the function returns.

The returned reference dangles.

Never return references to local variables.

---

## ❌ Forgetting the `const` Overload

If you only define

```cpp
int& operator[](size_t index)
```

then indexing a `const IntArray` won't compile.

Providing both overloads is the standard C++ pattern.

---

# Mental Model

Think of `operator[]` as giving you a **window** into the array.

If it returns a **copy**, you're looking at a photograph.

Changing the photograph doesn't change the real object.

If it returns a **reference**, you're looking through an open window.

Anything you do affects the actual object inside.

---

# Key Takeaways

- Operators in C++ are functions.
    
- `arr[i]` becomes `arr.operator[](i)`.
    
- Returning `int&` allows modification of the original element.
    
- Returning by value creates a copy.
    
- `const` overloads allow read-only access to const objects.
    
- `operator[]` usually doesn't perform bounds checking; safer alternatives like `at()` can.
    

---

## Next Chapter

We'll take `IntArray` one step further and make it work with **any type**, not just `int`.

We'll transform it into:

```cpp
Array<int>

Array<float>

Array<double>

Array<std::string>
```

using **templates**, one of the most powerful features of C++. This is exactly how the Standard Library builds generic containers like `std::vector<T>`.