# **1) ALL FUNCTIONS IN `std::list` (Doubly Linked List)**

This supports **insert anywhere**, **erase anywhere**, **push_back**, **push_front**, etc.

---

# ⭐ INSERTION FUNCTIONS

|Function|Meaning|
|---|---|
|`push_front(x)`|Insert at beginning|
|`push_back(x)`|Insert at end|
|`insert(it, x)`|Insert **before** iterator|
|`emplace_front(x)`|Construct & insert at front|
|`emplace_back(x)`|Construct & insert at end|
|`emplace(it, x)`|Construct & insert before iterator|

---

# ⭐ DELETION FUNCTIONS

|Function|Meaning|
|---|---|
|`pop_front()`|Remove first element|
|`pop_back()`|Remove last element|
|`erase(it)`|Remove element at iterator|
|`erase(start, end)`|Remove range|
|`remove(x)`|Remove all elements equal to x|
|`remove_if(condition)`|Remove if condition true|
|`clear()`|Remove all items|

---

# ⭐ ACCESS FUNCTIONS

|Function|Meaning|
|---|---|
|`front()`|First element|
|`back()`|Last element|
|`begin()`|Iterator to first|
|`end()`|Iterator to end|
|`rbegin()`|Reverse begin|
|`rend()`|Reverse end|
|`size()`|Number of elements|
|`empty()`|True if list empty|

---

# ⭐ MODIFYING FUNCTIONS

|Function|Meaning|
|---|---|
|`assign(n, x)`|Fill list with n copies of x|
|`assign({})`|Assign new values|
|`resize(n)`|Increase/decrease size|
|`swap(other)`|Swap with another list|

---

# ⭐ SPECIAL DOUBLY-LINKED LIST FUNCTIONS

|Function|Meaning|
|---|---|
|`splice(pos, other)`|Move all nodes from another list|
|`splice(pos, other, it)`|Move one node|
|`splice(pos, other, start, end)`|Move a range|
|`merge(other)`|Merge sorted lists|
|`sort()`|Sort the list|
|`reverse()`|Reverse the list|
|`unique()`|Remove consecutive duplicates|
# 1️⃣ **CREATE (Insert Elements)**

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> l;

    l.push_front(20);  // front
    l.push_back(40);   // back
    l.push_back(50);   // back

    auto it = l.begin();
    advance(it, 1);    // move to pos 1
    l.insert(it, 30);  // insert at pos 1

    for(int x : l) cout << x << " ";
}

```

👉 Output:

```
20 30 40 50

```

---

# 2️⃣ **READ (Traverse)**

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> l = {10, 20, 30};

    for(int x : l)
        cout << x << " ";
}

```

👉 Output:

```
10 20 30

```

---

# 3️⃣ **UPDATE (Modify an Element)**

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> l = {10, 20, 30};

    for(int &x : l)
        if(x == 20) x = 200;  // update 20 → 200

    for(int x : l) cout << x << " ";
}

```

👉 Output:

```
10 200 30

```

---

# 4️⃣ **DELETE (pop, erase, remove)**

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> l = {10, 20, 30, 40};

    l.pop_front();        // removes 10
    l.pop_back();         // removes 40
    l.remove(20);         // remove all 20s

    for(int x : l) cout << x << " ";
}

```




# **HOW you SHOULD read a linked list (simple loop)**

## ✔ Using a normal `for` loop with iterator

```cpp
#include <iostream>
#include <list>
using namespace std;

int main() {
    list<int> l = {10, 20, 30, 40};

    for (auto it = l.begin(); it != l.end(); it++) {
        cout << *it << " ";
    }
}

```

👉 This **is the linked-list version of a normal loop**.

👉 `*it` is the value.

👉 `it++` moves to next node.

---

# ✔ Using range-based for loop (even simpler)

```cpp
for (int x : l)
    cout << x << " ";

```