Excellent. This is where you'll start seeing **patterns** instead of individual problems. Every one of these questions uses the same core idea:

> **Extract → Process → Remove → Repeat**

```cpp
while (n != 0)
{
    int digit = n % 10;   // Extract
    // Process
    n /= 10;              // Remove
}
```

Once this pattern becomes second nature, you'll solve most beginner number problems in minutes.

---

# 📘 C++ Handbook

# Segment 1 - Part 2: Number Problems (Digit Manipulation)

---

# 1. Reverse Integer

---

## 🧠 Intuition

Imagine you have the number:

```
1234
```

Take one digit at a time from the back.

```
4
3
2
1
```

Now build another number.

```
0

↓

4

↓

43

↓

432

↓

4321
```

The trick is:

Before adding a new digit,

move previous digits one place left.

```
rev = rev * 10 + digit
```

---

## 📌 Pattern

```cpp
digit = n % 10;
rev = rev * 10 + digit;
n /= 10;
```

---

## 💻 Code

```cpp
#include<iostream>
using namespace std;

int main()
{
    int n;
    cin >> n;

    int rev = 0;

    while(n != 0)
    {
        int digit = n % 10;
        rev = rev * 10 + digit;
        n /= 10;
    }

    cout << rev;
}
```

---

## 🔄 Dry Run

Input

```
123
```

|n|digit|rev|
|---|---|---|
|123|3|3|
|12|2|32|
|1|1|321|

Output

```
321
```

---

## ⏱️ Complexity

Time

```
O(log₁₀N)
```

Space

```
O(1)
```

---

## 🐞 Common Mistakes

❌

```cpp
rev += digit;
```

Should be

```cpp
rev = rev * 10 + digit;
```

---

## 🎯 Pattern Recognition

Keywords

- Reverse
    
- Mirror
    
- Build number
    
- Reverse digits
    

---

# 2. Count Digits

---

## 🧠 Intuition

Every time you remove one digit,

increase count.

```
9876

↓

987

↓

98

↓

9

↓

0
```

Four removals

↓

Four digits

---

## 📌 Pattern

```cpp
count++;
n /= 10;
```

---

## 💻 Code

```cpp
#include<iostream>
using namespace std;

int main()
{
    int n;
    cin >> n;

    int count = 0;

    while(n != 0)
    {
        count++;
        n /= 10;
    }

    cout << count;
}
```

---

## 🔄 Dry Run

```
12345
```

```
count=1

count=2

count=3

count=4

count=5
```

---

## ⏱️ Complexity

```
O(log₁₀N)
```

---

## 🐞 Common Mistakes

Input

```
0
```

This code prints

```
0
```

Mathematically,

0 has **1 digit**.

Handle separately if required.

---

## 🎯 Pattern Recognition

Questions asking

- Number of digits
    
- Length of integer
    

---

# 3. Sum of Digits

---

## 🧠 Intuition

Instead of building another number,

keep adding digits.

```
123

↓

1+2+3

↓

6
```

---

## 📌 Pattern

```cpp
sum += digit;
```

---

## 💻 Code

```cpp
#include<iostream>
using namespace std;

int main()
{
    int n;
    cin >> n;

    int sum = 0;

    while(n != 0)
    {
        int digit = n % 10;
        sum += digit;
        n /= 10;
    }

    cout << sum;
}
```

---

## 🔄 Dry Run

```
472
```

```
2

↓

2+7

↓

9+4

↓

13
```

---

## ⏱️ Complexity

```
O(log₁₀N)
```

---

## 🐞 Common Mistakes

Using

```cpp
sum = digit;
```

instead of

```cpp
sum += digit;
```

---

## 🎯 Pattern Recognition

Keywords

- Sum digits
    
- Digit sum
    
- Digital root (related)
    

---

# 4. Palindrome Number

---

## 🧠 Intuition

Reverse the number.

Compare with original.

If equal

↓

Palindrome.

---

## 📌 Pattern

Always save

```cpp
original = n;
```

before changing

```
n
```

---

## 💻 Code

```cpp
#include<iostream>
using namespace std;

int main()
{
    int n;
    cin >> n;

    int original = n;
    int rev = 0;

    while(n != 0)
    {
        int digit = n % 10;
        rev = rev * 10 + digit;
        n /= 10;
    }

    if(rev == original)
        cout << "Palindrome";
    else
        cout << "Not Palindrome";
}
```

---

## 🔄 Dry Run

```
121
```

Reverse

```
121
```

Compare

```
121==121

True
```

---

## 🐞 Common Mistake

This mistake is very common:

```cpp
while(n!=0)
{
    ...
}

if(rev==n)
```

After loop

```
n=0
```

Always compare with

```
original
```

---

## ⏱️ Complexity

```
O(log₁₀N)
```

---

## 🎯 Pattern Recognition

Questions mentioning

- Palindrome
    
- Mirror
    
- Reverse comparison
    

---

# 5. Armstrong Number

---

## 🧠 Intuition

Every digit contributes

```
digit^(number of digits)
```

Example

```
153

↓

1³

+

5³

+

3³

↓

1

+

125

+

27

↓

153
```

---

## 📌 Pattern

Need

- Count digits
    
- Extract digits
    
- Power
    
- Sum
    

---

## 💻 Code

```cpp
#include<iostream>
#include<cmath>
using namespace std;

int main()
{
    int n;
    cin >> n;

    int original = n;

    int digits = 0;
    int temp = n;

    while(temp != 0)
    {
        digits++;
        temp /= 10;
    }

    temp = n;
    int sum = 0;

    while(temp != 0)
    {
        int digit = temp % 10;
        sum += pow(digit, digits);
        temp /= 10;
    }

    if(sum == original)
        cout << "Armstrong";
    else
        cout << "Not Armstrong";
}
```

---

## 🔄 Dry Run

```
153
```

Digits

```
3
```

Calculation

```
1³=1

5³=125

3³=27
```

Sum

```
153
```

Match

```
Yes
```

---

## ⏱️ Complexity

```
O(log₁₀N)
```

---

## 🐞 Common Mistakes

❌ Forgetting to count digits first.

❌ Comparing with modified `n`.

❌ Forgetting `#include <cmath>` for `pow()`.

> **Note:** `pow()` returns a `double`. For these small integer problems it usually works fine, but in more advanced code you may prefer writing an integer power function to avoid floating-point issues.

---

## 🎯 Pattern Recognition

Keywords

- Armstrong
    
- Narcissistic Number
    
- Sum of powers
    

---

# 🔥 Master Pattern

Almost every beginner number problem follows this template:

```cpp
while (n != 0)
{
    int digit = n % 10;   // Step 1: Extract

    // Step 2: Process the digit

    n /= 10;              // Step 3: Remove the digit
}
```

The only thing that changes is **what you do in Step 2**:

|Problem|Processing Step|
|---|---|
|Count Digits|`count++`|
|Sum of Digits|`sum += digit`|
|Reverse Number|`rev = rev * 10 + digit`|
|Palindrome|Reverse then compare|
|Armstrong|`sum += pow(digit, digits)`|

---

# 📝 5-Minute Revision

|Problem|Key Formula|
|---|---|
|Reverse|`rev = rev * 10 + digit`|
|Count Digits|`count++`|
|Sum Digits|`sum += digit`|
|Palindrome|`reverse == original`|
|Armstrong|`sum += pow(digit, digits)`|

---

## 🎯 What You Learned

You didn't just learn five problems—you learned the **Digit Manipulation Pattern**.

Later topics such as:

- Happy Number
    
- Strong Number
    
- Harshad Number
    
- Number Theory
    
- Bit Manipulation (conceptually similar iterative processing)
    

all build on the same habit: **extract data, process it, update state, and repeat**. Once this pattern is automatic, you'll recognize an entire class of problems immediately.



