# Chapter 8 — Templates (Generic Programming)

> **Goal:** Understand why templates exist, how the compiler uses them, and how `IntArray` becomes a reusable `Array<T>`.

---

# The Problem

We built an array for integers.

```cpp
class IntArray
{
    int* m_Data;
};
```

It works great.

```cpp
IntArray arr(5);
```

But what if tomorrow you need

```cpp
float
```

Instead of integers.

You can't do this.

```cpp
IntArray arr(5);

arr[0] = 3.14f;
```

Because

```cpp
m_Data
```

stores

```cpp
int*
```

Not

```cpp
float*
```

---

# Solution 1 (Bad)

Create another class.

```cpp
class FloatArray
{
    float* m_Data;
};
```

Now you need

```cpp
DoubleArray

CharArray

StringArray

PersonArray

StudentArray
```

Soon you'll have

```text
IntArray.cpp

FloatArray.cpp

DoubleArray.cpp

StringArray.cpp

...
```

Every class is **99% identical**.

Only the data type changes.

This is code duplication.

---

# Why Copying Code Is Bad

Suppose your class has a bug.

```cpp
class IntArray
{
    // Bug here
};
```

Now you fix it.

But...

You also have

```cpp
FloatArray

DoubleArray

StringArray

StudentArray
```

You must fix the bug

everywhere.

This is exactly what templates were invented to solve.

---

# The Big Idea

Instead of writing

```cpp
int
```

or

```cpp
float
```

we write

```cpp
T
```

Think of

```cpp
T
```

as a placeholder.

Like a blank space.

```
Array< _____ >
```

Later

the compiler fills the blank.

---

# What Does T Mean?

Most people say

> "T means Type."

That's true.

But mentally,

I prefer this.

Think

```text
T

↓

Unknown Data Type
```

The compiler doesn't know yet

whether it's

```text
int

float

double

string

Person

Enemy

Monster

Anything
```

It waits until you use the class.

---

# Template Syntax

```cpp
template<typename T>

class Array
{

};
```

Let's understand every word.

---

## template

This tells the compiler

> "I'm creating a blueprint."

Not an actual class.

Just instructions.

---

## typename

Means

> "The next thing is a type."

Older C++ also allows

```cpp
template<class T>
```

These are almost identical.

Modern code often uses

```cpp
typename
```

because it's slightly clearer.

---

## T

Simply the name

of the placeholder.

It could have been

```cpp
template<typename Banana>
```

Then

```cpp
Banana
```

would replace

```cpp
T
```

People use

```cpp
T
```

by convention.

---

# Converting IntArray

Old version

```cpp
class IntArray
{
private:

    int* m_Data;
};
```

New version

```cpp
template<typename T>

class Array
{
private:

    T* m_Data;
};
```

Notice

Only one word changed.

```text
int

↓

T
```

Now

the class no longer cares

what type it stores.

---

# Constructor

Old

```cpp
m_Data = new int[size];
```

New

```cpp
m_Data = new T[size];
```

Why?

Because

sometimes

```text
T

↓

int
```

Sometimes

```text
T

↓

float
```

Sometimes

```text
T

↓

std::string
```

The compiler decides.

---

# Operator[]

Old

```cpp
int& operator[](size_t index)
{
    return m_Data[index];
}
```

New

```cpp
T& operator[](size_t index)
{
    return m_Data[index];
}
```

Nothing else changes.

Because

```text
T

↓

Whatever type

the user requested.
```

---

# How the Compiler Thinks

Suppose you write

```cpp
Array<int> numbers(5);
```

The compiler says

> Oh.

Here

```text
T

=

int
```

So it creates

```cpp
class Array
{
    int* m_Data;
};
```

Automatically.

---

Now

```cpp
Array<float> values(5);
```

Compiler says

Now

```text
T

=

float
```

So it creates

```cpp
class Array
{
    float* m_Data;
};
```

Notice

You never wrote

```cpp
FloatArray
```

The compiler did.

---

# Visual Representation

Think of templates

like a cookie cutter.

The template

```
Cookie Cutter

↓

Makes

Chocolate Cookie

Vanilla Cookie

Strawberry Cookie
```

The cutter never changes.

Only the dough changes.

Templates work exactly the same way.

```
Template

↓

Compiler

↓

Array<int>

Array<float>

Array<string>
```

---

# Memory Example

Suppose

```cpp
Array<int> numbers(3);
```

Memory

```text
Stack

numbers

↓

m_Data

↓

5000

Heap

5000

↓

10

20

30
```

Now

```cpp
Array<float> prices(3);
```

Memory

```text
Stack

prices

↓

7000

Heap

7000

↓

1.5

2.7

9.8
```

Exactly the same class.

Different type.

---

# Complete Class

```cpp
template<typename T>
class Array
{
private:
    T* m_Data;
    size_t m_Size;

public:

    Array(size_t size)
        : m_Size(size),
          m_Data(new T[size])
    {
    }

    ~Array()
    {
        delete[] m_Data;
    }

    T& operator[](size_t index)
    {
        return m_Data[index];
    }

    const T& operator[](size_t index) const
    {
        return m_Data[index];
    }

    size_t size() const
    {
        return m_Size;
    }
};
```

---

# Using It

```cpp
Array<int> numbers(3);

numbers[0] = 10;
numbers[1] = 20;
numbers[2] = 30;
```

---

Another object

```cpp
Array<float> prices(3);

prices[0] = 19.5f;
prices[1] = 12.7f;
prices[2] = 7.2f;
```

Same class.

Different types.

---

Even

```cpp
Array<std::string> names(2);

names[0] = "Luffy";
names[1] = "Zoro";
```

Still works.

---

# Important Point

Templates **do not exist at runtime**.

This surprises many beginners.

The compiler generates real classes **during compilation**.

For example:

```cpp
Array<int> a;
Array<float> b;
```

The compiler effectively generates something like:

```cpp
class Array_int
{
    int* m_Data;
};

class Array_float
{
    float* m_Data;
};
```

These aren't the actual names, but this mental model is accurate enough to understand what happens.

At runtime, there's no "template" anymore—only normal compiled classes.

---

# Why the STL Uses Templates

Think about `std::vector`.

If templates didn't exist, the C++ Standard Library would need:

```text
VectorInt

VectorFloat

VectorDouble

VectorString

VectorPerson

VectorEnemy

...
```

Instead, it provides

```cpp
std::vector<T>
```

You choose the type.

The compiler creates the correct version.

---

# Common Mistakes

### ❌ Thinking `T` is a variable

No.

```cpp
T
```

is a **type**, not a value.

Wrong

```cpp
T = 10;
```

Correct thinking

```text
T

↓

int
```

or

```text
T

↓

float
```

---

### ❌ Thinking Templates Slow Programs

They don't.

Templates are resolved at **compile time**.

The generated code is often just as fast as handwritten code for that type.

---

### ❌ Believing Only Classes Use Templates

Functions can also use templates.

Example:

```cpp
template<typename T>
T max(T a, T b)
{
    return (a > b) ? a : b;
}
```

Now the compiler can generate:

- `max(int, int)`
    
- `max(float, float)`
    
- `max(double, double)`
    

from a single implementation.

---

# Mental Model

Imagine you're designing a bottle factory.

Without templates:

- One machine makes only water bottles.
    
- Another machine makes only juice bottles.
    
- Another machine makes only soda bottles.
    

Lots of duplicated machinery.

With templates:

You build **one adjustable machine**.

You tell it:

```
Today

↓

Make glass bottles
```

Tomorrow

```
Make plastic bottles
```

The machine stays the same.

Only the material changes.

That's exactly what templates do.

---

# Key Takeaways

- Templates let you write code once and use it with many types.
    
- `T` is a placeholder for a type.
    
- The compiler replaces `T` with the actual type during compilation.
    
- Templates eliminate duplicated code.
    
- `std::vector`, `std::array`, `std::optional`, `std::unique_ptr`, and most of the STL are built using templates.
    
- Templates have **no runtime cost** because they are resolved at compile time.
    

---

## We've Finished the Live Coding Section 🎉

You've now covered the concepts from the live coding part of the lecture:

1. ✅ Variable Addresses
    
2. ✅ Arrays & Memory Layout
    
3. ✅ Pointers
    
4. ✅ Dynamic Memory (`new` / `delete`)
    
5. ✅ RAII
    
6. ✅ Building `IntArray`
    
7. ✅ Operator Overloading (`operator[]`)
    
8. ✅ Templates
    

One thing I would add to your notes later (not covered deeply in this lecture) is the **Rule of Three / Rule of Five**. Once you have a destructor that owns dynamic memory, copying your class becomes dangerous unless you define proper copy/move behavior. That's the natural next topic before moving on to smart pointers and `std::vector`.