This is exactly the kind of sheet you want before solving problems. It's not theory—it's a **"What should my brain think first?"** guide.

---

# 📘 Phase 1 Cheat Sheet (Arrays → Sliding Window)

---

# 🔹 Arrays

## Basic Traversal

```cpp
for(int i = 0; i < n; i++)
```

Think:

- Visit every element once
    
- Keep answer in a variable
    

---

## Maximum

```cpp
int mx = arr[0];

for(int x : arr)
    mx = max(mx, x);
```

Pattern

```
answer = max(answer, current)
```

---

## Minimum

```cpp
int mn = arr[0];

for(int x : arr)
    mn = min(mn, x);
```

---

## Reverse

Two pointers

```text
L       R

1 2 3 4 5

↓

5 4 3 2 1
```

```cpp
swap(arr[l], arr[r]);
l++;
r--;
```

---

## Rotation

Right Rotate by k

```cpp
k %= n;

reverse(all);

reverse(first k);

reverse(rest);
```

Remember

```
Reverse
↓

Reverse

↓

Reverse
```

---

## Frequency

```cpp
unordered_map<int,int> freq;

freq[x]++;
```

Useful for

- Counting
    
- Duplicates
    
- Majority
    

---

## Kadane

```cpp
current += arr[i];

best = max(best,current);

if(current<0)
    current=0;
```

Mental model

```
Negative prefix?

↓

Throw it away.
```

---

## Buy & Sell Stock

Track

```cpp
minimum price

maximum profit
```

Formula

```cpp
profit = price - minimum
```

---

# 🔹 Strings

---

## Reverse

```cpp
reverse(s.begin(), s.end());
```

or

Two pointers.

---

## Palindrome

```text
L ---> <--- R
```

Compare

```cpp
s[l]==s[r]
```

---

## Anagram

Count frequency.

```cpp
int freq[26]={0};
```

Increase

Decrease

Compare

---

## Frequency

```cpp
unordered_map<char,int>
```

or

```cpp
int freq[26]
```

---

## Longest Common Prefix

Compare characters

Column by column.

```
flower

flow

flight
```

Stop on first mismatch.

---

## String Builder

```cpp
string ans;

ans += c;

ans += word;
```

---

## Character Array

```cpp
char arr[100];
```

Remember

```
'\0'

Null character
```

---

# 🔹 Hashing

---

## HashMap

```
Key → Value
```

```cpp
unordered_map<int,int>
```

Operations

```cpp
insert

find

erase

count
```

---

## HashSet

Stores

Only unique values.

```cpp
unordered_set<int>
```

---

## Frequency Map

```cpp
freq[x]++;
```

Most common interview trick.

---

## Duplicate Detection

```cpp
if(set.count(x))

Duplicate
```

---

## Two Sum

Store

```
Target-current
```

Pattern

```cpp
if(map.count(target-x))
```

---

# 🔹 Prefix Sum

---

Formula

```cpp
prefix[i]

=

prefix[i-1]+arr[i]
```

---

## Range Sum

```
L R

=

prefix[R]

-

prefix[L-1]
```

---

## Difference Array

Range Update

```
+X at L

-X at R+1
```

Then

Prefix Sum

---

## Subarray Sum

Running Sum

```cpp
sum += arr[i]
```

HashMap stores

```
Previous Prefix
```

---

# 🔹 Two Pointers

---

## Opposite Direction

```text
L ---->

<---- R
```

Examples

- Reverse
    
- Pair Sum
    

---

## Same Direction

```text
Slow

Fast
```

Examples

- Remove Duplicates
    
- Move Zeroes
    

---

## Remove Duplicates

Keep

```
Unique Index
```

Overwrite duplicates.

---

## Sorted Arrays

Always ask

```
Can I use two pointers?
```

---

## Container With Most Water

Formula

```
Area

=

min(height)

×

Width
```

Move

Smaller pointer.

---

# 🔹 Sliding Window

---

Whenever question says

```
Continuous

Subarray

Substring
```

Think

```
Sliding Window
```

---

## Fixed Window

Window size fixed.

```
Add Right

Remove Left
```

---

## Variable Window

Grow

Shrink

Repeat

```
Expand

↓

Condition breaks

↓

Shrink

↓

Continue
```

---

## Longest Substring

Usually

```
HashMap

+

Window
```

---

## Maximum Sum Window

```
sum+=arr[r]

sum-=arr[l]
```

---

# STL You Should Remember

```cpp
max(a,b)

min(a,b)

swap(a,b)

reverse(v.begin(),v.end())

sort(v.begin(),v.end())

accumulate(v.begin(),v.end(),0)

max_element()

min_element()
```

---

# Time Complexities

|Operation|Complexity|
|---|---|
|Traversal|O(n)|
|Reverse|O(n)|
|Frequency Map|O(n)|
|Prefix Sum Build|O(n)|
|Range Query|O(1)|
|HashMap Lookup|O(1) average|
|Two Pointers|O(n)|
|Sliding Window|O(n)|
|Kadane|O(n)|
|Buy Sell Stock|O(n)|

---

# Pattern Recognition

|Question says...|Think...|
|---|---|
|Largest / Smallest|Linear Traversal|
|Count occurrences|HashMap / Frequency|
|Pair in sorted array|Two Pointers|
|Pair in unsorted array|HashMap|
|Continuous subarray|Sliding Window|
|Range sum|Prefix Sum|
|Rotate array|Reverse Algorithm|
|Maximum subarray|Kadane|
|Maximum profit|Min-so-far|
|Duplicate|HashSet|
|Anagram|Frequency Array|
|Palindrome|Two Pointers|
|Remove duplicates|Slow/Fast Pointers|
|Longest substring|Sliding Window + HashMap|

---

# 🚨 Common Interview Triggers

- **"Subarray"** → Prefix Sum, Sliding Window, Kadane
    
- **"Substring"** → Sliding Window
    
- **"Sorted array"** → Two Pointers or Binary Search
    
- **"Frequency" / "Count"** → HashMap
    
- **"Unique" / "Duplicate"** → HashSet
    
- **"Range queries"** → Prefix Sum
    
- **"Maximum/Minimum in one pass"** → Running answer
    
- **"Rotate" / "Reverse"** → Two Pointers + Reverse
    
- **"Buy once, sell once"** → Track minimum so far
    

---

# 🧠 The 30-Second Decision Tree

When you see an array/string problem, ask yourself these questions in order:

1. **Can I solve it with one linear scan?**
    
2. **Does it ask about frequency or duplicates?** → HashMap/HashSet
    
3. **Is it a continuous subarray/substring?** → Sliding Window or Prefix Sum
    
4. **Is the array sorted?** → Two Pointers or Binary Search
    
5. **Does it ask for a range sum?** → Prefix Sum
    
6. **Does it ask for the maximum/minimum while scanning?** → Running answer (Kadane, Buy & Sell, etc.)
    

If you build the habit of asking these six questions first, you'll start recognizing patterns instead of treating every problem as completely new. This is one of the biggest shifts from beginner to intermediate problem solving.