# Question — Why Returning a Pointer is Needed

Let’s build this like a real interview/DSA situation instead of random syntax.

---

# Problem

Create a function that:

- creates a new linked list node
- stores a value inside it
- returns the node to main()

---

# First Attempt (WRONG)

```cpp
#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* next;
};

Node* createNode(int value)
{
    Node temp;

    temp.data = value;
    temp.next = NULL;

    return &temp;
}

int main()
{
    Node* first = createNode(10);

    cout << first->data;
}
```

---

# QUESTION

Why does this code fail even though:

```cpp
return &temp;
```

returns an address?

---

# THINK STEP-BY-STEP

---

# STEP 1 — Function Starts

```cpp
Node temp;
```

creates:

```
STACK FRAME OF createNode()

temp:
+------+-------+
| data | next  |
+------+-------+
```

Suppose:

```
temp is at address 100
```

---

# STEP 2 — Data Assigned

```cpp
temp.data = 10;
```

Memory:

```
Address 100:
+------+-------+
|  10  | NULL  |
+------+-------+
```

---

# STEP 3 — Return

```cpp
return &temp;
```

returns:

```
100
```

(address of temp)

So main gets:

```
first = 100
```

Seems correct…

BUT…

---

# STEP 4 — Function Ends

When function ends:

```
createNode stack frame destroyed
```

So:

```
memory at address 100 is no longer valid
```

BUT:

```
first STILL stores 100
```

Now:

```
first points to dead memory
```

This is:

```
dangling pointer
```

---

# VISUALIZATION

After function:

```
STACK:
(empty)

first -> 100   ❌ invalid
```

---

# WHY THIS HAPPENS

Because:

```
stack memory belongs to function lifetime
```

When function dies:

```
its local variables die too
```

---

# CORRECT SOLUTION — USE HEAP

```cpp
#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* next;
};

Node* createNode(int value)
{
    Node* temp = new Node();

    temp->data = value;
    temp->next = NULL;

    return temp;
}

int main()
{
    Node* first = createNode(10);

    cout << first->data;

    delete first;
}
```

---

# NOW WHAT CHANGES?

---

# STEP 1

```cpp
Node* temp = new Node();
```

creates node in:

```
HEAP MEMORY
```

---

# MEMORY

```
STACK:
temp -> 500

HEAP:
Address 500:
+------+-------+
| data | next  |
+------+-------+
```

---

# STEP 2 — Return

```cpp
return temp;
```

returns:

```
500
```

---

# STEP 3 — Function Ends

Local pointer temp dies.

BUT:

```
HEAP MEMORY STILL EXISTS
```

because heap is NOT tied to function lifetime.

---

# AFTER FUNCTION

```
STACK:
first -> 500

HEAP:
500:
+------+-------+
|  10  | NULL  |
+------+-------+
```

Valid memory.

Program works.

---

# FINAL CORE LESSON

We return pointers when:

```
memory must survive after function call
```

Stack cannot do that.

Heap can.

That’s why:

- linked lists
- trees
- graphs
- dynamic arrays

all heavily use:

```cpp
new
pointer returns
```

because their memory must continue existing outside the function that created them.