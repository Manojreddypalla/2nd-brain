## Conditions in JavaScript — Short Notes

### 1. `if`

Runs code **only when condition is true**.

```js
if (age >= 18) {
  console.log("Adult");
}
```

### 2. `if...else`

Choose between **two paths**.

```js
if (age >= 18) {
  console.log("Adult");
} else {
  console.log("Minor");
}
```

### 3. `if...else if...else`

Check **multiple conditions** from top to bottom.

```js
if (marks >= 90) {
  console.log("A");
} else if (marks >= 60) {
  console.log("B");
} else {
  console.log("C");
}
```

👉 **First true condition executes**, remaining conditions are skipped.

---

### 4. Comparison Operators

|Operator|Meaning|
|---|---|
|`==`|Equal (allows type conversion)|
|`===`|Strictly equal ⭐|
|`!=`|Not equal|
|`!==`|Strictly not equal ⭐|
|`>`|Greater than|
|`<`|Less than|
|`>=`|Greater than or equal|
|`<=`|Less than or equal|

**Prefer `===` and `!==`** in normal JavaScript code.

```js
5 === "5"   // false
5 == "5"    // true
```

---

### 5. Logical Operators

|Operator|Meaning|
|---|---|
|`&&`|AND → both must be true|
|`||
|`!`|NOT → reverses boolean|

```js
age >= 18 && hasID
```

---

### 6. Ternary Operator

Short form of `if...else`.

```js
let result = age >= 18 ? "Adult" : "Minor";
```

**Pattern:**

```js
condition ? valueIfTrue : valueIfFalse
```

---

### 🧠 Quick Mental Model

```text
Condition
   ↓
 true ──→ execute
   │
 false
   ↓
else / next condition
```

**Pattern to recognize:**

- One decision → `if`
    
- Two choices → `if...else`
    
- Many choices → `if...else if...else`
    
- Simple one-line choice → `ternary`