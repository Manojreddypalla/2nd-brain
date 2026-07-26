
# Sliding Window

> **Definition:** Sliding Window is an optimization technique used on **contiguous** data (subarrays/substrings) to reduce repeated work, often improving **O(n²) → O(n)**.

---

# Core Idea

Instead of recalculating every window:

1. Expand the window.
2. Remove unnecessary elements.
3. Reuse previous computation.
4. Update the answer.

---

# When to Use

✅ Subarray

✅ Substring

✅ Contiguous elements

✅ Maximum / Minimum

✅ Longest / Shortest

✅ Exactly K

✅ At Most K

✅ At Least K

**Keywords:** Window, Consecutive, Continuous

---

# When NOT to Use

❌ Subsequences

❌ Non-contiguous selections

❌ Graphs

❌ Trees

❌ Arbitrary combinations

---

# Types

## 1. Fixed Window

**Window size remains constant.**

### Recognition
- Exactly K
- Size K
- Length K

### Examples
- Maximum Sum Subarray of Size K
- Maximum Average Subarray
- Count negatives in every window
- Maximum vowels in K

### Template

```cpp
int window = 0;

// First window
for(int i=0;i<k;i++)
    window += nums[i];

int ans = window;

// Slide
for(int i=k;i<n;i++){
    window += nums[i];
    window -= nums[i-k];
    ans = max(ans, window);
}
```

Time: **O(n)**

Space: **O(1)**

---

## 2. Variable Window

**Window size changes dynamically.**

### Recognition
- Longest
- Shortest
- At Most K
- At Least K
- Distinct
- Unique
- No Repeating Characters

### Template

```cpp
int left = 0;

for(int right=0; right<n; right++){

    // Expand

    while(window_is_invalid){

        // Shrink
        left++;
    }

    // Update answer
}
```

Time: **O(n)**

Space: Depends on data structure (O(1) / O(k) / O(26) / O(256))

---

# Four-Step Algorithm

1. Expand (`right++`)
2. Check validity
3. Shrink (`left++`) while invalid
4. Update answer

> Every variable window problem follows these four steps.

---

# Why O(n)?

Both pointers move **only forward**.

```
Left  : 0 → 1 → 2 → ... → n
Right : 0 → 1 → 2 → ... → n
```

Total pointer movements ≤ 2n

**Overall Complexity:** O(n)

---

# Fixed vs Variable

| Fixed | Variable |
|--------|----------|
| Size fixed | Size changes |
| Given K | Discover K |
| No while loop | Usually while loop |
| Easier | Harder |

---

# Common Mistakes

❌ Using Sliding Window on non-contiguous data

❌ Forgetting to remove outgoing element

❌ Using `if` instead of `while` when shrinking

❌ Updating answer before making the window valid

❌ Choosing the wrong template (Fixed vs Variable)

---

# Recognition Flow

```
Contiguous?

      ↓

YES

      ↓

Exactly K?

      ↓

YES → Fixed Window

NO

↓

Longest / Shortest / At Most K / Distinct?

↓

Variable Window
```

---

# Pattern Combinations

- Sliding Window + HashMap
- Sliding Window + HashSet
- Sliding Window + Frequency Array
- Sliding Window + Two Pointers
- Sliding Window + Prefix Sum

---

# High-Yield Problems

## Fixed Window

1. Maximum Average Subarray I (643)
2. Maximum Sum Subarray of Size K

## Variable Window

3. Minimum Size Subarray Sum (209)
4. Longest Substring Without Repeating Characters (3)
5. Longest Repeating Character Replacement (424)
6. Permutation in String (567)
7. Find All Anagrams in a String (438)

---

# Interview Checklist

Before solving, ask:

- Is it contiguous?
- Fixed or Variable window?
- What makes the window valid?
- When do I expand?
- When do I shrink?
- When do I update the answer?

> **Golden Rule:** Never recompute the whole window. Update only what changes.