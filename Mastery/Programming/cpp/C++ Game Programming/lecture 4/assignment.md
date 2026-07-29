
From the screenshots, I can reconstruct the assignment almost completely. Here's what you're expected to build.

---

# Assignment Overview

**Language:** C++

**Graphics Library:** SFML

**Goal:**

Write a program that **reads a configuration file** and automatically creates shapes inside an SFML window.

Instead of hardcoding circles and rectangles, **everything comes from `config.txt`.**

---

# Final Program

Your program should display something like this:

- Open an SFML window
    
- Read window size
    
- Read font
    
- Read every shape
    
- Draw them
    
- Write the shape's name in its center
    
- Make every shape move
    
- Make every shape bounce when it hits a wall
    

Exactly like the final screenshots.

---

# Step 1

Read

```
Window W H
```

Example

```
Window 800 600
```

Create

```cpp
sf::RenderWindow window(
    sf::VideoMode(800,600),
    "Assignment 1"
);
```

---

# Step 2

Read the font

Example

```
Font fonts/tech.ttf 18 255 255 255
```

Meaning

```
Font
↓

File
↓

Size
↓

R

G

B
```

Load

```cpp
sf::Font font;
font.loadFromFile("fonts/tech.ttf");
```

Store

```
Font size = 18

Text color = White
```

---

# Step 3

Read every remaining line.

There are only two possible shape types.

## Circle

Format

```
Circle
Name
X
Y
SX
SY
R
G
B
Radius
```

Example

```
Circle CGreen
100
100
-0.03
0.02
0
255
0
50
```

Meaning

```
Type = Circle

Name = CGreen

Position = (100,100)

Velocity = (-0.03 , 0.02)

Color = Green

Radius = 50
```

---

## Rectangle

Format

```
Rectangle
Name
X
Y
SX
SY
R
G
B
Width
Height
```

Example

```
Rectangle RRed
200
200
0.1
0.15
255
0
0
50
25
```

Means

```
Rectangle

Name

Position

Velocity

Color

Width

Height
```

---

# Step 4

Store every shape.

The assignment hint literally recommends

```cpp
std::vector
```

You'll probably have something like

```cpp
std::vector<CircleShapeData>
```

and

```cpp
std::vector<RectangleShapeData>
```

or

```cpp
std::vector<Entity>
```

if you design it more cleanly.

---

# Step 5

Every frame

For every shape

```
position += velocity
```

Example

```
x += sx

y += sy
```

---

# Step 6

Bounce

When a shape reaches a wall

Reverse its speed.

Left wall

```
sx *= -1;
```

Right wall

```
sx *= -1;
```

Top

```
sy *= -1;
```

Bottom

```
sy *= -1;
```

The assignment specifically says

> A shape touches the wall when its **bounding box** touches the window.

Meaning use

```cpp
shape.getLocalBounds()
```

or

```cpp
shape.getGlobalBounds()
```

depending on how you implement it.

---

# Step 7

Draw the name

Every shape must display its name.

Example

```
CGreen
```

inside the green circle.

```
RGrey
```

inside rectangle.

---

# Step 8

Center the text

This is the hardest requirement.

Use

```cpp
text.getLocalBounds();
```

Then

```
shape center

↓

text width

↓

text height

↓

position text
```

The README even says

> This may be the trickiest part.

---

# Step 9

Game Loop

```
while(window.isOpen())
{
    events

    update positions

    collision

    draw shapes

    draw text

    display
}
```

Exactly like every SFML game.

---

# Configuration File

Example

```
Window 800 600

Font fonts/tech.ttf 18 255 255 255

Circle CGreen 100 100 -0.03 0.02 0 255 0 50

Circle CBlue 200 200 0.02 0.04 0 0 255 100

Circle CPurple 300 300 -0.02 -0.01 255 0 255 75

Rectangle RRed 200 200 0.1 0.15 255 0 0 50 25

Rectangle RGrey 300 250 -0.02 0.02 100 100 100 50 100

Rectangle RTeal 25 100 -0.02 -0.02 0 255 255 100 100
```

Your program should work for **any valid configuration**, not just this one.

---

# What You Must Use

From the hints and examples:

- ✅ `std::ifstream`
    
- ✅ `std::string`
    
- ✅ `std::vector`
    
- ✅ `while(file >> type)`
    
- ✅ `sf::CircleShape`
    
- ✅ `sf::RectangleShape`
    
- ✅ `sf::Text`
    
- ✅ `sf::Font`
    
- ✅ `shape.getLocalBounds()`
    
- ✅ `text.getLocalBounds()`
    
- ✅ `shape.setPosition()`
    
- ✅ `shape.move()`
    

---

# Submission

Only submit:

```
main.cpp
```

Do **not** submit:

- Visual Studio solution
    
- `.sln`
    
- `.vcxproj`
    
- ZIP containing the whole project
    

---

# What You'll Learn

Although it looks like an SFML assignment, it's really teaching several core game programming concepts:

1. **File parsing** (`ifstream`)
    
2. **Data-driven programming** (behavior comes from config files instead of hardcoded values)
    
3. **Object creation at runtime**
    
4. **Game loop architecture**
    
5. **Rendering**
    
6. **Basic physics** (movement + bouncing)
    
7. **Containers** (`std::vector`)
    
8. **Coordinate systems**
    
9. **Collision with window boundaries**
    
10. **Text rendering and alignment**
    

This is actually a classic introductory game engine exercise. It introduces the idea of separating **data (the config file)** from **logic (your C++ code)**—a pattern you'll see in real game engines like Unreal, Unity, and Godot.

From the screenshots, I'd estimate the assignment is around **200–300 lines** if written cleanly, or **350–450 lines** if you create your own classes and organize it well.

























