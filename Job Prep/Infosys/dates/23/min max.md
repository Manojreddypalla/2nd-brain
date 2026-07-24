For interview coding (Infosys/TCS/Wipro) and DSA, there are **three common ways** to find the maximum and minimum element in an array.

---

# Method 1: Simple Traversal (Most Common) ⭐

### Time Complexity

- **O(n)**
    

### Space Complexity

- **O(1)**
    

```cpp
#include <iostream>
#include <climits>
using namespace std;

int main() {
    int arr[] = {5, 2, 9, 1, 7};
    int n = sizeof(arr) / sizeof(arr[0]);

    int maximum = arr[0];
    int minimum = arr[0];

    for (int i = 1; i < n; i++) {
        if (arr[i] > maximum)
            maximum = arr[i];

        if (arr[i] < minimum)
            minimum = arr[i];
    }

    cout << "Maximum = " << maximum << endl;
    cout << "Minimum = " << minimum << endl;

    return 0;
}
```

---

# Method 2: Using `max()` and `min()`

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int main() {
    int arr[] = {5, 2, 9, 1, 7};
    int n = sizeof(arr) / sizeof(arr[0]);

    int maximum = arr[0];
    int minimum = arr[0];

    for (int i = 1; i < n; i++) {
        maximum = max(maximum, arr[i]);
        minimum = min(minimum, arr[i]);
    }

    cout << "Maximum = " << maximum << endl;
    cout << "Minimum = " << minimum << endl;

    return 0;
}
```

---

# Method 3: STL (`max_element` & `min_element`) ⭐

This is the shortest and most common STL solution.

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int main() {
    int arr[] = {5, 2, 9, 1, 7};
    int n = sizeof(arr) / sizeof(arr[0]);

    int maximum = *max_element(arr, arr + n);
    int minimum = *min_element(arr, arr + n);

    cout << "Maximum = " << maximum << endl;
    cout << "Minimum = " << minimum << endl;

    return 0;
}
```

---

# Using `vector`

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    vector<int> arr = {5, 2, 9, 1, 7};

    int maximum = *max_element(arr.begin(), arr.end());
    int minimum = *min_element(arr.begin(), arr.end());

    cout << "Maximum = " << maximum << endl;
    cout << "Minimum = " << minimum << endl;
}
```

---

# Dry Run

Array:

```
5 2 9 1 7
```

Initial:

```
max = 5
min = 5
```

|Element|Max|Min|
|---|---|---|
|2|5|2|
|9|9|2|
|1|9|1|
|7|9|1|

Output:

```
Maximum = 9
Minimum = 1
```

---

# For DSA/Infosys Revision

✅ Remember these STL functions:

```cpp
*max_element(begin, end)
*min_element(begin, end)
```

Example:

```cpp
int mx = *max_element(arr.begin(), arr.end());
int mn = *min_element(arr.begin(), arr.end());
```

Or for arrays:

```cpp
int mx = *max_element(arr, arr + n);
int mn = *min_element(arr, arr + n);
```

These are the standard library functions most interviewers expect you to know when using STL.