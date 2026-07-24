This is actually a great idea. Instead of memorizing functions randomly, build yourself a **"C++ STL & Utility Cheat Sheet"**. Over time, you'll stop Googling basic things like `max_element()` or `isdigit()`.

Here's a collection of the functions you'll use constantly in DSA and interviews.

---

# 📌 C++ Utility Functions Cheat Sheet

## 🔹 Numbers

### Maximum

```cpp
max(a, b);
```

```cpp
int x = max(10, 20); // 20
```

---

### Minimum

```cpp
min(a, b);
```

---

### Absolute Value

```cpp
abs(x);
```

```cpp
abs(-15); // 15
```

---

### Swap

```cpp
swap(a, b);
```

---

### Power

```cpp
pow(a, b);
```

```cpp
pow(2, 5); // 32
```

---

### Square Root

```cpp
sqrt(x);
```

---

### Ceiling

```cpp
ceil(2.1);   // 3
```

---

### Floor

```cpp
floor(2.9);  // 2
```

---

### Round

```cpp
round(2.6);  // 3
```

---

### Maximum of three

```cpp
max({a, b, c});
```

---

### Minimum of three

```cpp
min({a, b, c});
```

---

# 🔹 Strings

### Integer → String

```cpp
string s = to_string(123);
```

---

### String → Integer

```cpp
int x = stoi(s);
```

---

### String → Long Long

```cpp
long long x = stoll(s);
```

---

### Character → Integer

```cpp
char c = '7';

int x = c - '0';
```

---

### Integer → Character

```cpp
char c = num + '0';
```

---

### Length

```cpp
s.length();
```

or

```cpp
s.size();
```

---

### Reverse String

```cpp
reverse(s.begin(), s.end());
```

---

### Sort String

```cpp
sort(s.begin(), s.end());
```

---

### Substring

```cpp
s.substr(start, length);
```

---

### Find

```cpp
s.find("abc");
```

Returns index or `string::npos`.

---

### Append

```cpp
s += "abc";
```

---

### Erase

```cpp
s.erase(pos, len);
```

---

# 🔹 Character Checking (`<cctype>`)

### Is Digit

```cpp
isdigit(c);
```

---

### Is Alphabet

```cpp
isalpha(c);
```

---

### Is Lowercase

```cpp
islower(c);
```

---

### Is Uppercase

```cpp
isupper(c);
```

---

### Is Space

```cpp
isspace(c);
```

---

### Is Alphanumeric

```cpp
isalnum(c);
```

---

### To Upper

```cpp
toupper(c);
```

---

### To Lower

```cpp
tolower(c);
```

---

# 🔹 Arrays / Vector

### Length

```cpp
v.size();
```

---

### First Element

```cpp
v.front();
```

---

### Last Element

```cpp
v.back();
```

---

### Add

```cpp
v.push_back(x);
```

---

### Remove Last

```cpp
v.pop_back();
```

---

### Clear

```cpp
v.clear();
```

---

### Is Empty

```cpp
v.empty();
```

---

### Reverse

```cpp
reverse(v.begin(), v.end());
```

---

### Sort Ascending

```cpp
sort(v.begin(), v.end());
```

---

### Sort Descending

```cpp
sort(v.begin(), v.end(), greater<int>());
```

---

### Maximum Element

```cpp
*max_element(v.begin(), v.end());
```

---

### Minimum Element

```cpp
*min_element(v.begin(), v.end());
```

---

### Count Occurrences

```cpp
count(v.begin(), v.end(), x);
```

---

### Find Element

```cpp
find(v.begin(), v.end(), x);
```

Check:

```cpp
if (find(v.begin(), v.end(), x) != v.end())
```

---

### Sum

```cpp
accumulate(v.begin(), v.end(), 0);
```

Requires:

```cpp
#include <numeric>
```

---

### Binary Search

```cpp
binary_search(v.begin(), v.end(), x);
```

---

### Lower Bound

```cpp
lower_bound(v.begin(), v.end(), x);
```

Returns iterator.

Index:

```cpp
lower_bound(v.begin(), v.end(), x) - v.begin();
```

---

### Upper Bound

```cpp
upper_bound(v.begin(), v.end(), x);
```

---

# 🔹 Math Tricks

### Count Digits

```cpp
to_string(n).size();
```

or

```cpp
(int)log10(n) + 1;
```

---

### Last Digit

```cpp
n % 10;
```

---

### Remove Last Digit

```cpp
n /= 10;
```

---

### First Digit

```cpp
while (n >= 10)
    n /= 10;
```

---

# 🔹 GCD / LCM

### GCD

```cpp
gcd(a, b);
```

or (standard)

```cpp
std::gcd(a, b);
```

Requires:

```cpp
#include <numeric>
```

---

### LCM

```cpp
std::lcm(a, b);
```

---

# 🔹 Bit Functions

### Count Set Bits

```cpp
__builtin_popcount(x);
```

---

### Count Set Bits (long long)

```cpp
__builtin_popcountll(x);
```

---

### Leading Zeros

```cpp
__builtin_clz(x);
```

---

### Trailing Zeros

```cpp
__builtin_ctz(x);
```

---

# 🔹 Queue

Front

```cpp
q.front();
```

Back

```cpp
q.back();
```

Insert

```cpp
q.push(x);
```

Remove

```cpp
q.pop();
```

---

# 🔹 Stack

Top

```cpp
st.top();
```

Push

```cpp
st.push(x);
```

Pop

```cpp
st.pop();
```

---

# 🔹 Priority Queue

Maximum Heap

```cpp
priority_queue<int> pq;
```

Minimum Heap

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

---

# 🔹 Set

Insert

```cpp
s.insert(x);
```

Erase

```cpp
s.erase(x);
```

Find

```cpp
s.find(x);
```

Count

```cpp
s.count(x);
```

---

# 🔹 Map

Insert

```cpp
mp[key] = value;
```

Check

```cpp
mp.count(key);
```

Erase

```cpp
mp.erase(key);
```

Find

```cpp
mp.find(key);
```

---

# 🔹 Common Headers

```cpp
#include <algorithm>   // sort, reverse, max, min, find
#include <numeric>     // accumulate, gcd, lcm
#include <cmath>       // sqrt, pow, abs, ceil, floor
#include <cctype>      // isdigit, isalpha, isspace
#include <string>      // string utilities
#include <vector>
#include <queue>
#include <stack>
#include <set>
#include <map>
#include <unordered_map>
#include <unordered_set>
```

---

## ⭐ My recommendation for your DSA notes

Keep this as **"Module 0 — C++ STL & Utility Toolkit"** in Obsidian. Before diving into arrays, strings, trees, or graphs, spend a few days becoming comfortable with these utilities. You'll use **80–90% of them repeatedly** across LeetCode, GATE coding questions, and interviews, so having them in one reference note will save you a lot of time.