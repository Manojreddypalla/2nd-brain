# 🎯 GATE-Level MCQs

## Q1 ⭐

Mutual Exclusion ensures that:

A) Multiple threads execute the Critical Section simultaneously.

B) Only one thread executes the Critical Section at a time.

C) Every thread executes first.

D) Context switching never occurs.

<details> <summary>✅ Answer</summary>

**B**

This is the definition of Mutual Exclusion.

</details>

---

## Q2 ⭐⭐

The primary purpose of Mutual Exclusion is to:

A) Increase CPU speed

B) Prevent race conditions

C) Reduce memory usage

D) Eliminate scheduling

<details> <summary>✅ Answer</summary>

**B**

Mutual Exclusion prevents simultaneous access to shared resources.

</details>

---

## Q3 ⭐⭐⭐ (GATE Style)

Which of the following is **NOT** guaranteed by Mutual Exclusion alone?

A) Only one process enters the Critical Section.

B) Race conditions are prevented.

C) Fair access to the Critical Section.

D) Protection of shared resources.

<details> <summary>✅ Answer</summary>

**C**

Fairness is handled by **Bounded Waiting**, not Mutual Exclusion.

</details>

---

## Q4 ⭐⭐⭐

Consider the following statements:

1. Mutual Exclusion prevents two processes from executing the Critical Section simultaneously.
2. Mutual Exclusion alone guarantees that every waiting process will eventually enter the Critical Section.
3. Mutual Exclusion helps maintain data consistency.

Which are correct?

A) 1 only

B) 1 and 3 only

C) 2 and 3 only

D) 1, 2 and 3

<details> <summary>✅ Answer</summary>

**B**

Statement 2 is false. Mutual Exclusion does **not** prevent starvation.

</details>

---

## Q5 ⭐⭐⭐⭐ (Code-Based)

```
lock();

shared++;

unlock();
```

The purpose of `lock()` is to:

A) Speed up execution

B) Enforce Mutual Exclusion

C) Allocate memory

D) Perform context switching

<details> <summary>✅ Answer</summary>

**B**

The lock ensures only one thread enters the Critical Section.

</details>

---

## Q6 ⭐⭐⭐⭐ (Classic GATE Trap)

A synchronization algorithm satisfies Mutual Exclusion but allows one process to wait forever while another repeatedly enters the Critical Section.

Which property is **missing**?

A) Race Condition

B) Progress

C) Bounded Waiting

D) Parallelism

<details> <summary>✅ Answer</summary>

**C**

This is starvation, which violates **Bounded Waiting**.

</details>

---

## Q7 ⭐⭐⭐⭐⭐ (Statement-Based)

Which of the following mechanisms can be used to implement Mutual Exclusion?

1. Peterson's Algorithm
2. Mutex
3. Semaphore
4. Test-and-Set

A) 1 and 2 only

B) 2 and 3 only

C) 1, 2 and 4 only

D) 1, 2, 3 and 4

<details> <summary>✅ Answer</summary>

**D**

All of these mechanisms can enforce Mutual Exclusion (with appropriate use).

</details>

---

## Q8 ⭐⭐⭐⭐⭐ (Conceptual)

Suppose only one process ever accesses a variable.

Which statement is correct?

A) Mutual Exclusion is still required.

B) Mutual Exclusion is unnecessary because there is no concurrent access.

C) A race condition must occur.

D) A semaphore is mandatory.

<details> <summary>✅ Answer</summary>

**B**

Mutual Exclusion is only needed when **multiple concurrent execution units share a resource**.

</details>

---

# 🎯 Previous-Year Style Questions

### PYQ Style 1

A correct solution to the Critical Section Problem must satisfy:

A) Mutual Exclusion only

B) Mutual Exclusion and Progress only

C) Mutual Exclusion, Progress, and Bounded Waiting

D) Mutual Exclusion and Deadlock Freedom only

✅ **Answer:** **C**

---

### PYQ Style 2 (Very Common)

Which property ensures that **at most one process** executes inside its Critical Section?

A) Progress

B) Mutual Exclusion

C) Bounded Waiting

D) Scheduling

✅ **Answer:** **B**

---

# 🧠 Memory Trick

Remember the three properties as answering three different questions:

|Property|Question it Answers|
|---|---|
|**Mutual Exclusion**|**Can two processes be inside together?** → **No**|
|**Progress**|**If nobody is inside, who gets to enter next?**|
|**Bounded Waiting**|**Will a waiting process eventually get a turn?**|