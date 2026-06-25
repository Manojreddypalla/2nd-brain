# Python

## What is Python?

Python is a high-level, interpreted, general-purpose programming language designed to prioritize readability and developer productivity.

Created by Guido van Rossum in 1991.

Python is widely used in:

- [[web development]]
- Data Science
- Machine Learning
- Artificial Intelligence
- Automation
- Cybersecurity
- DevOps
- Networking
- Robotics
- Scientific Computing

---

# Why Python Exists

Programming languages exist to communicate instructions to computers.

Earlier languages often required:

```
int a = 10;
```

Python reduces complexity:

```
a = 10
```

The goal is:

- Simplicity
- Readability
- Productivity

---

# How Python Works Internally

Many beginners think:

```
print("Hello")
```

runs directly on the CPU.

Not exactly.

Flow:

```
Python Source Code        ↓Python Compiler        ↓Bytecode (.pyc)        ↓Python Virtual Machine (PVM)        ↓Machine Instructions        ↓CPU
```

---

# Core Philosophy

Python follows:

```
Readability > ClevernessSimple > ComplexExplicit > Implicit
```

You can read the philosophy through:

```
import this
```

---

# Python Ecosystem

Python is not one thing.

Think:

```
Python Language      +Libraries      +Frameworks      +Tools
```

Examples:

|Domain|Library|
|---|---|
|Data Science|Pandas|
|Numerical Computing|NumPy|
|AI/ML|PyTorch|
|Deep Learning|TensorFlow|
|Visualization|Matplotlib|
|Web|Django|
|APIs|FastAPI|
|Automation|Selenium|

---

# Python Fundamentals Roadmap

```
Python│├── Variables├── Data Types├── Operators├── Input/Output├── Control Flow├── Functions├── Data Structures├── Modules├── OOP├── Exceptions├── File Handling├── Iterators├── Generators├── Decorators├── Context Managers├── Concurrency└── Internals
```

---

# Variables

## Intuition

A variable is a label attached to an object in memory.

Example:

```
x = 10
```

Visualization:

```
Memory+------+|  10  |+------+   ↑   x
```

Notice:

```
a = 10b = a
```

Visualization:

```
Memory+------+|  10  |+------+ ↑   ↑ a   b
```

Both names point to the same object.

---

# Dynamic Typing

Python decides types at runtime.

```
x = 10x = "Hello"
```

Allowed because variables hold references to objects.

Visualization:

```
x → Integer Objectx → String Object
```

The reference changes.

---

# Data Types

## Numeric Types

```
intfloatcomplex
```

Examples:

```
a = 10b = 3.14c = 2 + 3j
```

---

## Boolean

```
TrueFalse
```

Used for decisions.

```
is_logged_in = True
```

---

## Strings

Strings are immutable sequences of characters.

```
name = "Python"
```

Memory:

```
P y t h o n0 1 2 3 4 5
```

Operations:

```
name[0]name[-1]name[1:4]
```

Related:

- [[String Slicing]]
- [[Immutability]]

---

# Lists

Most important beginner data structure.

```
nums = [1,2,3]
```

Visualization:

```
Index0 → 11 → 22 → 3
```

Lists are:

- Ordered
- Mutable
- Dynamic

---

## Internal Memory Concept

```
nums = [10,20,30]
```

Internally:

```
nums ↓+----+----+----+| *  | *  | *  |+----+----+----+ ↓    ↓    ↓10   20   30
```

The list stores references.

---

# Tuples

Immutable lists.

```
point = (10,20)
```

Benefits:

- Faster
- Safer
- Hashable

Used in:

- Dictionary keys
- Coordinates
- Fixed records

---

# Dictionaries

One of Python's most important structures.

```
student = {    "name":"John",    "age":20}
```

Internally based on:

```
Hash Tables
```

Related:

- [[Hash Tables]]
- [[Caching]]
- [[JSON]]

---

# Sets

Store unique elements.

```
nums = {1,2,3}
```

Useful for:

```
Duplicate RemovalMembership TestingSet Operations
```

---

# Control Flow

Programs need decisions.

---

## If Statement

```
age = 18if age >= 18:    print("Adult")
```

Flow:

```
Condition    ↓ True? /    \Yes   No ↓      ↓Run   Skip
```

---

## Loops

### For Loop

```
for i in range(5):    print(i)
```

Used when iterations are known.

---

### While Loop

```
while condition:    ...
```

Used when iterations are unknown.

---

# Functions

Functions package reusable behavior.

```
def greet(name):    return f"Hello {name}"
```

Think:

```
Input ↓Processing ↓Output
```

---

# Scope

Variables live in regions.

```
Global ScopeFunction ScopeLocal Scope
```

Example:

```
x = 10def test():    x = 20
```

Different variables.

---

# Modules

Large programs need organization.

```
math.sqrt(25)
```

A module is simply:

```
Python File
```

---

# OOP

Object-Oriented Programming models real-world entities.

---

## Class

Blueprint.

```
class Car:    pass
```

---

## Object

Instance.

```
c = Car()
```

Visualization:

```
Class  ↓Object
```

---

## Pillars

### Encapsulation

Hide implementation details.

### Inheritance

Reuse code.

### Polymorphism

One interface, many behaviors.

### Abstraction

Expose essentials.

---

# Exceptions

Programs fail.

Python handles failures gracefully.

```
try:    x = 10/0except ZeroDivisionError:    print("Error")
```

Flow:

```
Normal Path      ↓Exception?      ↓Handle
```

---

# File Handling

Programs interact with storage.

```
with open("data.txt") as f:    content = f.read()
```

Related:

- [[Operating Systems]]
- [[Filesystems]]

---

# Iterators

An iterator produces values one at a time.

```
iter()next()
```

Mental Model:

```
Book ↓One Page At A Time
```

---

# Generators

Special functions that pause execution.

```
def count():    yield 1    yield 2
```

Useful for:

- Large files
- Streaming
- Pipelines

---

# Decorators

Functions that modify functions.

```
@decoratordef func():    pass
```

Think:

```
Original Function       ↓Wrapper Added       ↓Enhanced Function
```

Used heavily in:

- FastAPI
- Flask
- Django

---

# Context Managers

Manage resources automatically.

```
with open("file.txt") as f:    ...
```

Guarantees cleanup.

---

# Concurrency

Running multiple tasks.

Topics:

- Threading
- Multiprocessing
- AsyncIO

Related:

- [[Operating Systems]]
- [[CPU Scheduling]]

---

# Python and Other Domains

```
Python│├── Data Science│   ├── NumPy│   ├── Pandas│├── AI│   ├── PyTorch│   ├── Transformers│├── DevOps│   ├── Automation│   ├── Scripting│├── Cybersecurity│   ├── Scapy│   ├── Pwntools│├── Robotics│   ├── ROS2│   ├── OpenCV│└── Web    ├── Django    ├── FastAPI
```

---

For your Obsidian vault, I'd make this the **Python Hub Note** and then create separate deep notes for:

- Variables
- Memory Model
- Strings
- Lists
- Dictionaries
- Functions
- OOP
- Iterators
- Generators
- Decorators
- AsyncIO
- Modules & Packages

Those topics are each large enough to deserve their own detailed wiki page. A full deep dive on just "Functions" or "Dictionaries" can easily be 10–20 pages by itself.