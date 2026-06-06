# **ALL FUNCTIONS of `std::forward_list` (SINGLY LINKED LIST)**

---

# ⭐ 1) **INSERTION FUNCTIONS**

|Function|Meaning|
|---|---|
|`push_front(x)`|Insert at the beginning|
|`emplace_front(args...)`|Construct & insert at front|
|`insert_after(it, x)`|Insert **after** a node|
|`emplace_after(it, args...)`|Construct & insert after|
|`assign(n, x)`|Fill with n copies|
|`assign({list})`|Assign new values|
|`assign(first, last)`|Assign from range|

⚠ `forward_list` does **NOT** have:

- `push_back` ❌
- `insert(it, x)` ❌ (only `insert_after`)
- `emplace_back` ❌

---

# ⭐ 2) **DELETION FUNCTIONS**

|Function|Meaning|
|---|---|
|`pop_front()`|Remove first element|
|`erase_after(it)`|Remove element after iterator|
|`erase_after(it1, it2)`|Remove range after `it1` until `it2`|
|`remove(x)`|Remove all values equal to x|
|`remove_if(pred)`|Remove if condition matches|
|`clear()`|Remove all nodes|

⚠ No:

- `pop_back()` ❌
- `erase(it)` ❌
- `erase(it1, it2)` only after positions

---

# ⭐ 3) **ACCESS FUNCTIONS**

|Function|Meaning|
|---|---|
|`front()`|Returns first element|
|`begin()`|Iterator to first|
|`end()`|Iterator to end|
|`before_begin()`|Points BEFORE first element|
|`cbegin()`|Const begin|
|`cend()`|Const end|

⚠ No:

- `back()` ❌
- `operator[]` ❌
- `at()` ❌

---

# ⭐ 4) **CAPACITY / SIZE FUNCTIONS**

|Function|Meaning|
|---|---|
|`empty()`|True if list is empty|
|`max_size()`|Max possible size|

⚠ **NO `size()`** (because counting each node is slow).

---

# ⭐ 5) **MODIFIERS / ALGORITHMIC FUNCTIONS**

|Function|Meaning|
|---|---|
|`reverse()`|Reverse the list|
|`sort()`|Sort the list|
|`merge(other)`|Merge two sorted forward_lists|
|`unique()`|Remove consecutive duplicates|
|`swap(other)`|Swap with another list|

---

# ⭐ 6) **UTILITY FUNCTIONS**

|Function|Meaning|
|---|---|
|`splice_after(pos, other)`|Move all nodes from `other` after pos|
|`splice_after(pos, other, it)`|Move one node|
|`splice_after(pos, other, first, last)`|Move a range|

---

# 🎯 SUPER SIMPLE SUMMARY

### ✔ `forward_list` = FAST & LIGHT

### ✔ Best for inserting/removing at front

### ✔ Single-direction only (forward)

### ❌ NO:

- indexing (`list[2]`)
- push_back
- pop_back
- insert at position directly
- size()