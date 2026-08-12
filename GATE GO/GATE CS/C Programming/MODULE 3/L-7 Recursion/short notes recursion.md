# Recursion — Short Notes

## 1. Recursion

- A function calling itself is **recursion**.
    
- Must have:
    
    - **Base case** → stops recursion.
        
    - **Recursive case** → calls itself on a smaller/subproblem.
        

```c
f(n) {
    if (base_case)
        return;
    f(smaller_problem);
}
```

## 2. Call Stack

- Every recursive call creates a **new activation record / stack frame**.
    
- Each frame stores its own:
    
    - parameters
        
    - local automatic variables
        
    - return information
        
- Calls go **down the stack**; returns happen **bottom → top**.
    

### Example

```text
f(3)
 ↓
f(2)
 ↓
f(1)
 ↓
base case
 ↑
return
```

## 3. Before vs After Recursive Call

```c
printf("%d", n);
f(n-1);
```

→ output during **calling phase** → usually descending.

```c
f(n-1);
printf("%d", n);
```

→ output during **returning phase** → usually ascending.

**GATE trick:** Always identify whether `printf()` is before or after recursion.

## 4. Recursion Tree

- One recursive call → usually a **chain**.
    
- Multiple recursive calls → **tree**.
    

```text
        f(5)
       /    \
     f(4)   f(3)
```

- Repeated subproblems → important connection to **DP**.
    

## 5. Static Variable in Recursion

```c
static int x;
```

- Only **one copy** exists.
    
- Shared by **all recursive calls**.
    
- Retains its value between calls.
    

```text
Normal variable:
f() → x₁
f() → x₂
f() → x₃

static variable:
       x
      /|\
    f() f() f()
```

### Remember

**Local/parameter → separate per activation**

**Static → shared among activations**

The lecture's output questions heavily test this distinction.

## 6. Pre/Post Decrement

```c
--i
```

→ decrement **first**, then use value.

```c
i--
```

→ use value **first**, then decrement.

Very common recursion output trap.

## 7. Recursion + DP

```text
Recursion
   ↓
Subproblems
   ↓
Repeated subproblems?
   ↓ YES
Memoization / Tabulation
```

**DP = recursive subproblem structure + storing previously computed results.**

## 8. Complexity

Don't assume recursion = `O(n)`.

Derive the recurrence:

```text
T(n) = T(n-1) + O(1)       → O(n)

T(n) = T(n-1) + T(n-2)     → exponential-type growth
```

## 9. GATE Output Strategy

For recursive output questions:

1. Find **base case**.
    
2. Draw the **call chain/tree**.
    
3. Identify **static vs local** variables.
    
4. Mark statements **before/after recursion**.
    
5. Trace **calling phase**.
    
6. Trace **returning phase**.
    

### 🔥 Must Remember

> **CALL → creates stack frame**

> **RETURN → removes stack frame**

> **Before recursion → execute while going down**

> **After recursion → execute while coming back**

> **Static → one shared copy**

> **Local/parameter → separate copy per call**







