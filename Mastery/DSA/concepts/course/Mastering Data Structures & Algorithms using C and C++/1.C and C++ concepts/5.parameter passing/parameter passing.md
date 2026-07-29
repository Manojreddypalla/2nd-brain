---
category: CSE_Core
topic: Memory Management & Functions
language: C / C++
status: active
tags: [pointers, memory-layout, function-calls]
---

# Functions & Data Passing Mechanics

When dealing with complex data types (Arrays and Structures), understanding how they are passed into the function's stack frame determines both performance and mutability.

## 1. Parameter Passing Methods Overview

| Method | How it Works | Memory Impact | Mutability |
| :--- | :--- | :--- | :--- |
| **Pass by Value** | A complete copy of the data is created in the function's stack frame. | High (Duplicates data) | Original data is safe from modification. |
| **Pass by Reference** | An alias (reference `&`) to the original memory address is passed. | Low (No duplication) | Modifies the original data directly. |
| **Pass by Address** | A pointer `*` holding the memory address is passed. | Low (Only copies pointer) | Modifies the original data directly. |

---

## 2. Arrays as Function Parameters

**Rule of Thumb:** Arrays are *always* passed by address/reference by default in C/C++. 

When you pass an array to a function, you are actually passing a pointer to its first element (`array[0]`). The function does not know the size of the array, which is why you must usually pass the size as a separate parameter.

```cpp
// The function receives a pointer, NOT a copy of the array
void modifyArray(int arr[], int size) {
    arr[0] = 99; // This permanently changes the original array
}

int main() {
    int myArray[5] = {1, 2, 3, 4, 5};
    modifyArray(myArray, 5); 
    // myArray[0] is now 99.
}
# Excalidraw Data

## Text Elements
array as function  ^BnqyOcoW

%%
## Drawing
```compressed-json
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
```
%%