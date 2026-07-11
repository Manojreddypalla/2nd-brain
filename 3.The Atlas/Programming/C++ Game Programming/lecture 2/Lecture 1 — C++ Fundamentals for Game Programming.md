# 1. Introduction to C++

## What is C++?

C++ is a **general-purpose, compiled programming language** designed for **high performance** and **low-level hardware control** while still supporting modern programming concepts such as Object-Oriented Programming (OOP), Generic Programming, and Functional Programming.

It was created by **Bjarne Stroustrup** at **Bell Labs** in **1983**.

Originally it was called

> **C with Classes**

because it extended the C language by adding classes and object-oriented features.

---

## Why was C++ created?

C was extremely fast but lacked abstraction.

Large software projects became difficult to maintain.

C++ added

- Classes
    
- Constructors
    
- Encapsulation
    
- Inheritance
    
- Polymorphism
    

while keeping almost all of C's speed.

---

## Why is C++ used in Game Development?

Games require

- High FPS
    
- Direct memory control
    
- Predictable execution
    
- Efficient CPU usage
    

Unlike Python or Java, C++ runs almost directly on the hardware.

Examples

- Unreal Engine
    
- Unity Engine (native engine code)
    
- CryEngine
    
- Source Engine
    
- Frostbite
    

are largely written in C++.

---

# 2. Advantages of C++

### High Performance

C++ programs are compiled into native machine code.

No interpreter.

No virtual machine.

The CPU executes the instructions directly.

---

### Low-Level Memory Access

Programmers can control

- Memory allocation
    
- Memory layout
    
- CPU cache efficiency
    

This is critical for

- Physics engines
    
- Rendering
    
- AI
    
- Networking
    

---

### Huge Standard Library (STL)

Provides ready-made

- Containers
    
- Algorithms
    
- Strings
    
- Smart pointers
    
- File handling
    
- Multithreading
    

instead of writing everything yourself.

---

### Highly Portable

Available on

- Windows
    
- Linux
    
- macOS
    
- PlayStation
    
- Xbox
    
- Nintendo Switch
    

---

### Large Ecosystem

Thousands of libraries already exist.

Examples

- SDL
    
- SFML
    
- OpenGL
    
- Vulkan
    
- DirectX
    
- Bullet Physics
    

---

# 3. Disadvantages

### Complex Syntax

C++ has many language features.

Example

Templates

Pointers

References

Namespaces

Operator overloading

Memory management

---

### Manual Memory Management

Programmer must manage heap memory.

```cpp
new
delete
```

Wrong management leads to

- Memory leaks
    
- Dangling pointers
    
- Crashes
    

---

### Unsafe if Misused

Accessing invalid memory causes

Segmentation Fault

Access Violation

Undefined Behavior

---

### Difficult Compiler Errors

Template errors can produce pages of compiler output.

Modern compilers have improved this significantly.

---

# 4. C vs C++

|C|C++|
|---|---|
|Procedural|Multi-Paradigm|
|Functions|Classes + Functions|
|No OOP|OOP|
|No Templates|Templates|
|Manual abstraction|High abstraction available|

---

# 5. Programming Levels

### High-Level Languages

Example

Python

Java

JavaScript

Easy to learn.

Abstract hardware details.

---

### Low-Level Languages

Assembly

Machine Code

Very close to hardware.

Maximum control.

---

### Mid-Level Language

C++

Offers

High-level abstraction

Low-level hardware access

This is why C++ is often called a **middle-level language**.

---

# 6. Static Typing

C++ is

**Statically Typed**

Every variable has a known type before compilation.

```cpp
int score = 100;
```

Compiler knows

- size
    
- memory layout
    
- valid operations
    

before execution.

---

Python

```python
score = 100
```

Type decided at runtime.

---

Advantages

- Faster
    
- Safer
    
- Better optimization
    

---

# 7. Program Structure

```cpp
#include <iostream>

int main()
{
    std::cout<<"Hello";
    return 0;
}
```

---

Execution starts from

```cpp
main()
```

Every C++ program must have one.

---

# 8. Preprocessor

Runs before compilation.

Processes directives beginning with

```cpp
#
```

Examples

```cpp
#include

#define

#ifdef
```

---

### Include

```cpp
#include <iostream>
```

Copies the entire library into the translation unit.

Provides

```cpp
std::cout

std::cin
```

---

### Macros

```cpp
#define PI 3.14159
```

Compiler replaces every

PI

with

3.14159

before compiling.

---

# 9. main()

```cpp
int main(int argc,char* argv[])
```

---

### argc

Argument Count

Number of command line arguments.

---

### argv

Argument Vector

Array of C-style strings.

---

Example

```
game.exe level1 hard
```

argc

```
3
```

argv

```
argv[0]=game.exe

argv[1]=level1

argv[2]=hard
```

Useful for launching games with options like debug modes or specific levels.

---

# 10. std::cout

Outputs data to the console.

```cpp
std::cout<<"Hello";
```

`<<`

means

Insert into output stream.

---

# 11. Braces & Indentation

Braces define

Scopes

Functions

Loops

Conditionals

Example

```cpp
if(playerAlive)
{
    Shoot();
}
```

Indentation improves readability but does not affect execution.

---

# 12. Standard Template Library (STL)

One of C++'s greatest strengths.

Contains

### Containers

- vector
    
- list
    
- deque
    
- stack
    
- queue
    
- map
    
- set
    
- unordered_map
    

---

### Algorithms

sort

find

reverse

binary_search

max

min

---

### Utilities

string

pair

tuple

filesystem

chrono

---

# 13. Source Files

```
main.cpp

Player.cpp

Enemy.cpp
```

Contain

Definitions

Actual implementations.

---

# 14. Header Files

```
Player.h

Math.h
```

Contain

Declarations

Interfaces.

---

# 15. Declaration vs Definition

Declaration

```cpp
int Add(int,int);
```

Definition

```cpp
int Add(int a,int b)
{
    return a+b;
}
```

---

# 16. Compilation Pipeline

```
.cpp + .h

↓

Preprocessor

↓

Compiler

↓

Object Files (.o)

↓

Linker

↓

Executable (.exe)
```

Each stage has a specific role:

- **Preprocessor:** Expands `#include` and `#define`.
    
- **Compiler:** Checks syntax and converts C++ into object code.
    
- **Linker:** Combines object files and libraries into the final executable.
    

---

# 17. Common `g++` Commands

Preprocess only:

```bash
g++ -E main.cpp
```

Compile to object file:

```bash
g++ -c main.cpp
```

Link object files:

```bash
g++ main.o math.o -o app
```

Compile and link all `.cpp` files:

```bash
g++ *.cpp -o app
```

---

# 18. Classes

A class is a **blueprint** for creating objects.

```cpp
class Point
{
private:
    int m_x;
    int m_y;

public:
    Point(int x,int y);

    int getX();
    int getY();
};
```

### Components

- **Member variables:** Store an object's state (`m_x`, `m_y`).
    
- **Member functions:** Operate on that state (`getX()`, `getY()`).
    
- **Access specifiers:** Control visibility (`private`, `public`).
    

By default, members of a `class` are **private**.

The `m_` prefix is a common naming convention indicating a **member variable**.

---

# 19. Constructors

A constructor initializes an object when it is created.

```cpp
Point::Point(int x,int y)
{
    m_x = x;
    m_y = y;
}
```

Called automatically:

```cpp
Point p(10,20);
```

Constructors ensure objects start in a valid state.

---

# 20. STL Containers

### `std::vector<T>`

Dynamic array.

```cpp
std::vector<int> numbers;
```

- Fast random access (`O(1)`).
    
- Efficient `push_back()`.
    
- Widely used for collections of game objects.
    

---

### `std::set<T>`

Stores **unique**, automatically **sorted** elements.

```cpp
std::set<int> values;
```

Requires the type `T` to support comparison (`operator<`).

---

### `std::map<Key, Value>`

Associates keys with values.

```cpp
std::map<std::string, int> scores;
```

Example:

```text
"Player1" → 1000
"Player2" → 850
```

Useful for configuration data, lookups, and resource management.

---

# 21. Why These Topics Matter for Game Programming

Every topic in this lecture lays the foundation for working with a game engine:

- **Compilation** → Understanding how Unreal builds projects.
    
- **Classes** → Everything in Unreal (Actors, Components, Pawns) is a class.
    
- **Constructors** → Used to initialize game objects.
    
- **STL Containers** → Store enemies, bullets, items, and game state.
    
- **Static Typing** → Enables compiler optimizations for high-performance games.
    
- **Memory Management** → Essential for avoiding leaks and maintaining frame rate.
    

---

## Lecture 1 Summary

By the end of this lecture, you should understand:

- What C++ is and why it dominates game development.
    
- The differences between C and C++.
    
- Static typing and the structure of a basic C++ program.
    
- The roles of the preprocessor, compiler, and linker.
    
- The purpose of header (`.h`) and source (`.cpp`) files.
    
- How classes and constructors model game objects.
    
- The basics of STL containers such as `vector`, `set`, and `map`.
    
- The complete compilation pipeline from source code to executable.
    

These notes cover all the concepts visible in your screenshots while adding the context that connects them to game programming, making them a solid reference for this first lecture.