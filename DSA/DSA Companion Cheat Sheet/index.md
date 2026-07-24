# 00. Quick Reference ⭐

The page you open every day.

```
Numbers

Characters

Strings

Vector

Map

Set

Priority Queue

Algorithms

Comparator

Lambda

Common Formulae
```

Basically your **Ctrl+F notebook**.

---

# 01. C++ Essentials

Everything about the language.

```
Language

Headers

Input Output

auto

const

reference

pointer

typedef

using

pair

tuple

enum

struct

class
```

Notice I don't separate Numbers, Characters, Strings anymore.

Those become subsections.

---

# 02. STL Toolbox

This becomes your STL encyclopedia.

```
Containers

    vector

    array

    deque

    list

    stack

    queue

    priority_queue

    set

    multiset

    unordered_set

    map

    unordered_map

    bitset

Algorithms

    sort

    reverse

    rotate

    unique

    find

    lower_bound

    upper_bound

    binary_search

    accumulate

    next_permutation

Iterators

Comparators

Lambdas
```

Everything STL lives here.

---

# 03. Algorithms Toolbox

Not DSA algorithms.

Utility algorithms.

```
Searching

Sorting

Binary Search

Math

Bit Tricks

Prefix Sum

Difference Array

Coordinate Compression

Random

Numeric Algorithms
```

Basically

> Small reusable algorithms.

---

# 04. Problem Solving Patterns ⭐

Probably the biggest chapter.

```
Arrays

Two Pointer

Sliding Window

Hashing

Binary Search

Greedy

Stack

Queue

Heap

Linked List

Tree

Graph

Backtracking

Recursion

DP

Trie

Union Find

Bitmask

Math

String
```

Every solved problem links here.

---

# 05. Templates Library ⭐

No explanations.

Only code.

```
DFS

BFS

Binary Search

Trie

DSU

Fenwick

Segment Tree

Lazy Propagation

KMP

LPS

Z Algorithm

Dijkstra

Floyd

Bellman Ford

Topological Sort

Sieve

Fast Power

Matrix Expo
```

---

# 06. Time Complexity

Everything.

```
Containers

Algorithms

Searching

Sorting

Graphs

Trees

Heaps

DP

String Algorithms
```

---

# 07. Common Tricks & Pitfalls ⭐

This becomes GOLD after 300 problems.

Example.

```
Erase Remove Idiom

unique() doesn't erase

reserve vs resize

push_back vs emplace_back

Comparator tricks

Overflow tricks

Integer division

Floating precision

Iterator invalidation

Pass by reference

Move semantics

Prefix Sum tricks

Bit tricks

Coordinate compression

Sentinel node

Dummy node

Modulo tricks
```

These aren't algorithms.

They're tiny things you constantly forget.

---

# 08. Debugging Toolkit

```
Print Vector

Print Matrix

Print Tree

Print Linked List

Debug Macro

Assertions

Common Runtime Errors

Common STL Errors

Overflow Checklist

Segmentation Fault Checklist
```

---

# 09. Personal Discoveries ⭐⭐⭐

The most valuable note.

Every problem adds something.

Example

```
Problem

Largest Number

Learned

Comparator compares only two elements.

Compare a+b with b+a.

Need string conversion.

If first element is 0 return 0.
```

---

# One thing I would add that almost nobody has

Instead of a section called **Decision Trees**, I would put a subsection under **Problem Solving Patterns** called:

```
Choosing the Right Tool
```

Example:

```
Need fast lookup?
        ↓
unordered_map

Need sorted lookup?
        ↓
map

Need kth smallest?
        ↓
Heap
or
nth_element

Need frequency counting?
        ↓
unordered_map

Need unique sorted values?
        ↓
set

Need duplicates?
        ↓
multiset
```

This is far more useful than memorizing syntax because it answers **"What should I use?"**, which is the real interview question.

---

## If I had to keep only **10 root sections for the next 5 years**, I'd choose exactly these:

```
📘 DSA Companion Cheat Sheet
│
├── 00. Quick Reference
├── 01. C++ Essentials
├── 02. STL Toolbox
├── 03. Algorithms Toolbox
├── 04. Problem Solving Patterns
├── 05. Templates Library
├── 06. Time Complexity
├── 07. Common Tricks & Pitfalls
├── 08. Debugging Toolkit
└── 09. Personal Discoveries
```