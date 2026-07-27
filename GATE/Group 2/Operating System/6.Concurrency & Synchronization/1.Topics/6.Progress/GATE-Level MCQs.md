# 🎯 GATE-Level MCQs

## Q1 ⭐

The **Progress** requirement applies when:

A) The Critical Section is occupied.

B) The Critical Section is empty.

C) All processes have terminated.

D) A deadlock has occurred.

<details> <summary>✅ Answer</summary>

**B**

Progress is concerned with what happens **when the Critical Section is free**.

</details>

---

## Q2 ⭐⭐

Which statement best describes Progress?

A) Two processes may enter the Critical Section together.

B) If the Critical Section is free and some processes wish to enter, one of them should eventually be selected.

C) Every process must enter the Critical Section exactly once.

D) Every process executes simultaneously.

<details> <summary>✅ Answer</summary>

**B**

This is the textbook definition in simpler words.

</details>

---

## Q3 ⭐⭐⭐ (GATE Style)

A synchronization algorithm keeps the Critical Section empty even though Process P₁ wants to enter. Processes P₂ and P₃ are executing only their remainder sections.

Which property is violated?

A) Mutual Exclusion

B) Progress

C) Bounded Waiting

D) Deadlock Freedom

<details> <summary>✅ Answer</summary>

**B**

Processes not interested in the Critical Section must not delay those that are.

</details>

---

## Q4 ⭐⭐⭐

Which of the following processes should participate in deciding who enters the Critical Section next?

A) All processes in the system.

B) Only the operating system.

C) Only the processes that want to enter the Critical Section.

D) Only the highest-priority process.

<details> <summary>✅ Answer</summary>

**C**

This is an important part of the Progress requirement.

</details>

---

## Q5 ⭐⭐⭐⭐ (Statement-Based)

Consider the following statements:

1. Progress is relevant only when the Critical Section is empty.
2. Processes executing their remainder sections should not influence the decision of who enters next.
3. Progress guarantees that every waiting process will eventually enter the Critical Section.

Choose the correct option:

A) 1 only

B) 1 and 2 only

C) 2 and 3 only

D) 1, 2 and 3

<details> <summary>✅ Answer</summary>

**B**

Statement 3 describes **Bounded Waiting**, not Progress.

</details>

---

## Q6 ⭐⭐⭐⭐ (Classic GATE Trap)

Suppose the Critical Section is empty.

Process P₁ wants to enter.

Processes P₂ and P₃ are not interested in entering.

However, the algorithm waits for permission from P₂ before allowing P₁ to enter.

Which property is violated?

A) Mutual Exclusion

B) Progress

C) Bounded Waiting

D) Race Freedom

<details> <summary>✅ Answer</summary>

**B**

An uninterested process should never delay an interested one.

</details>

---

## Q7 ⭐⭐⭐⭐⭐ (Previous-Year Style)

Which of the following is **NOT** a requirement of the Critical Section Problem?

A) Mutual Exclusion

B) Progress

C) Bounded Waiting

D) Preemptive Scheduling

<details> <summary>✅ Answer</summary>

**D**

The three requirements are exactly:

- Mutual Exclusion
- Progress
- Bounded Waiting

</details>

---

## Q8 ⭐⭐⭐⭐⭐ (Comparison)

Which property ensures that a process **currently not interested** in entering the Critical Section cannot delay another process that is interested?

A) Mutual Exclusion

B) Progress

C) Bounded Waiting

D) Context Switching

<details> <summary>✅ Answer</summary>

**B**

This sentence is one of the defining ideas behind Progress.

</details>

---

# 🧠 The Three Properties So Far

Think of them as answering three different questions:

| Property             | What does it guarantee?                                                                                     |
| -------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Mutual Exclusion** | **Can two processes be inside the Critical Section together?** → **No.**                                    |
| **Progress**         | **If the Critical Section is empty, can an interested process enter without unnecessary delay?** → **Yes.** |
| **Bounded Waiting**  | **Will a waiting process eventually get its turn?** → **Yes, within a finite number of entries by others.** |