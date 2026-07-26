| STL Algorithm        | LeetCode Problem                                                            | Difficulty | Why it fits                                           |
| -------------------- | --------------------------------------------------------------------------- | ---------- | ----------------------------------------------------- |
| `sort()`             | **75. Sort Colors**                                                         | Medium     | Learn custom sorting / Dutch National Flag comparison |
|                      | **49. Group Anagrams**                                                      | Medium     | Sort each string as a key                             |
|                      | **56. Merge Intervals**                                                     | Medium     | Sort intervals first                                  |
| `reverse()`          | **344. Reverse String**                                                     | Easy       | Direct use of `reverse()`                             |
|                      | **31. Next Permutation**                                                    | Medium     | Reverse suffix after pivot                            |
| `lower_bound()`      | **35. Search Insert Position**                                              | Easy       | Exactly what `lower_bound()` does                     |
|                      | **34. Find First and Last Position of Element**                             | Medium     | Find first occurrence                                 |
|                      | **744. Find Smallest Letter Greater Than Target**                           | Easy       | Binary search with STL                                |
| `upper_bound()`      | **34. Find First and Last Position of Element**                             | Medium     | Find last occurrence (`upper_bound()-1`)              |
|                      | **852. Peak Index in a Mountain Array** _(manual BS preferred)_             | Easy       | Good binary search practice                           |
| `binary_search()`    | **704. Binary Search**                                                      | Easy       | Compare manual vs STL                                 |
|                      | **349. Intersection of Two Arrays** _(sorted approach)_                     | Easy       | Search each element in sorted array                   |
| `next_permutation()` | **31. Next Permutation**                                                    | Medium     | STL version is literally one line                     |
|                      | **46. Permutations**                                                        | Medium     | Generate permutations repeatedly                      |
| `max_element()`      | **414. Third Maximum Number**                                               | Easy       | Finding maximums                                      |
|                      | **53. Maximum Subarray**                                                    | Medium     | Compare with Kadane's result                          |
| `min_element()`      | **153. Find Minimum in Rotated Sorted Array** _(manual solution preferred)_ | Medium     | Understand why STL isn't enough                       |
|                      | **228. Summary Ranges**                                                     | Easy       | Simple practice                                       |
| `accumulate()`       | **1480. Running Sum of 1d Array**                                           | Easy       | Sum concepts                                          |
|                      | **724. Find Pivot Index**                                                   | Easy       | Total sum with `accumulate()`                         |
|                      | **643. Maximum Average Subarray I**                                         | Easy       | Initial window sum                                    |
| `find()`             | **27. Remove Element**                                                      | Easy       | Search element                                        |
|                      | **28. Find the Index of the First Occurrence in a String**                  | Easy       | Compare STL `find()` vs manual                        |
| `count()`            | **1207. Unique Number of Occurrences**                                      | Easy       | Count frequencies                                     |
|                      | **136. Single Number**                                                      | Easy       | Can solve with `count()` (not optimal)                |

---

# ⭐ Must-do STL Practice (10 Problems)

If you only do 10, I'd recommend:

1. ✅ 35. Search Insert Position (`lower_bound`)
2. ✅ 34. First and Last Position (`lower_bound` + `upper_bound`)
3. ✅ 704. Binary Search (`binary_search`)
4. ✅ 344. Reverse String (`reverse`)
5. ✅ 31. Next Permutation (`next_permutation`)
6. ✅ 49. Group Anagrams (`sort`)
7. ✅ 56. Merge Intervals (`sort`)
8. ✅ 724. Pivot Index (`accumulate`)
9. ✅ 349. Intersection of Two Arrays (`find` / hashing / sorted approaches)
10. ✅ 75. Sort Colors (understand why `sort()` works but isn't the optimal solution)

---

# 📚 STL Functions Every C++ DSA Student Should Master

## `<algorithm>`

```
sort()
stable_sort()
reverse()
find()
count()
binary_search()
lower_bound()
upper_bound()
equal_range()
next_permutation()
prev_permutation()
max_element()
min_element()
max()
min()
swap()
rotate()
unique()
remove()
remove_if()
fill()
copy()
merge()
```

## `<numeric>`

```
accumulate()
partial_sum()
adjacent_difference()
iota()
```

## `<set>` / Set Algorithms (Rare)

```
set_union()
set_intersection()
set_difference()
set_symmetric_difference()
```

These are useful to know exist, but they're far less common in interview solutions than the hashing approach with `unordered_set`.

---

For your **21-day DSA plan**, I'd strongly recommend spending **one dedicated day on STL mastery**. After that, you'll recognize opportunities to use these algorithms naturally instead of reinventing them every time.