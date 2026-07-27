# 🎯 GATE Quiz — Mutual Exclusion, Progress & Bounded Waiting

---

## Question 1 ⭐

Which property ensures that **only one process can execute the Critical Section at a time**?

- [ ] A. Progress
- [ ] B. Bounded Waiting
- [ ] C. Mutual Exclusion
- [ ] D. Starvation

> [!question]- Show Answer
> **✅ Correct Answer: C. Mutual Exclusion**
>
> **Explanation:**
> Mutual Exclusion guarantees that only one process/thread can execute the Critical Section at any given time, preventing race conditions.

---

## Question 2 ⭐⭐

A process has been waiting to enter the Critical Section for a long time while other processes continue entering repeatedly.

Which property is violated?

- [ ] A. Mutual Exclusion
- [ ] B. Progress
- [ ] C. Bounded Waiting
- [ ] D. Context Switching

> [!question]- Show Answer
> **✅ Correct Answer: C. Bounded Waiting**
>
> **Explanation:**
> Bounded Waiting prevents starvation by ensuring every waiting process eventually gets its turn.

---

## Question 3 ⭐⭐

The Critical Section is empty, but an interested process is not allowed to enter.

Which property is violated?

- [ ] A. Mutual Exclusion
- [ ] B. Progress
- [ ] C. Bounded Waiting
- [ ] D. Deadlock

> [!question]- Show Answer
> **✅ Correct Answer: B. Progress**
>
> **Explanation:**
> Progress ensures that when the Critical Section is empty, one of the interested processes is selected without unnecessary delay.

---

## Score

- [ ] Q1
- [ ] Q2
- [ ] Q3

**Total Score:** ___ / 3