# Example: Checksum Calculation (1's Complement Addition)

> [!example]
> Compute the **Checksum** for the following **4-bit data words**.
>
> ```
> 1010
> 0111
> 1100
> ```

---

# Step 1: Add the First Two Words

```
   1010
+  0111
---------
 10001
```

The result has **5 bits**, but our word size is **4 bits**.

Split the result into:

```
Carry  = 1
Result = 0001
```

```
1 | 0001
```

---

# Step 2: Perform End-Around Carry

In **1's Complement Addition**, the carry is **not discarded**.

Instead, add it back to the result.

```
   0001
+  0001
---------
   0010
```

Current Sum

```
0010
```

---

# Step 3: Add the Third Word

```
   0010
+  1100
---------
   1110
```

No carry is generated.

Current Sum

```
1110
```

---

# Step 4: Find the Checksum

Take the **1's Complement** of the final sum.

```
1110
```

↓

```
0001
```

Therefore,

```
Checksum = 0001
```

---

# Step 5: Data Transmitted

The sender transmits

```
1010
0111
1100
0001   ← Checksum
```

---

# Receiver Verification

Receiver adds **all the received words**, including the checksum.

```
   1010
+  0111
---------
 10001
```

Wrap the carry.

```
0001
+0001
------
0010
```

Continue adding.

```
0010
1100
------
1110
```

Now add the checksum.

```
1110
0001
------
1111
```

The final result is

```
1111
```

Since the result is **all 1s**, the receiver concludes:

✅ **No Error Detected**

---

> [!important]
> In **1's Complement Addition**, whenever a carry is generated beyond the Most Significant Bit (MSB), **add the carry back to the Least Significant Bit (LSB)**. This process is called **End-Around Carry**.

---

# Memory Trick

```
Add Data Words

↓

If Carry Exists

↓

Wrap Carry Around

↓

Continue Addition

↓

Take 1's Complement

↓

Checksum
```