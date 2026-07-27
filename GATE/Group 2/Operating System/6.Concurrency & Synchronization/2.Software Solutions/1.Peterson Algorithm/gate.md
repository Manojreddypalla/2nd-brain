# Peterson's Algorithm Quiz

---

## Q1 ⭐

Peterson's Algorithm is designed for:

- [ ] A. One Process
- [x] B. Two Processes
- [ ] C. Any Number of Processes
- [ ] D. Multiple CPUs

> [!question]- Show Answer
> ✅ **Answer:** **B. Two Processes**
>
> Peterson's Algorithm is specifically designed for **exactly two processes**.

---

## Q2 ⭐

What does `flag[i] = true` indicate?

- [ ] A. Process has finished
- [x] B. Process wants to enter the Critical Section
- [ ] C. Process is waiting
- [ ] D. Process is blocked

> [!question]- Show Answer
> ✅ **Answer:** **B**
>
> `flag[i]` indicates the **interest** of process `Pi` in entering the Critical Section.

---

## Q3 ⭐⭐

What is the purpose of the `turn` variable?

- [ ] A. Stores Process ID
- [ ] B. Counts processes
- [ ] C. Breaks the tie when both processes want to enter
- [ ] D. Allocates CPU

> [!question]- Show Answer
> ✅ **Answer:** **C**
>
> When both processes want to enter, `turn` decides which process gets priority.

---

## Q4 ⭐⭐

Peterson's Algorithm guarantees:

- [ ] A. Mutual Exclusion only
- [ ] B. Mutual Exclusion and Progress
- [ ] C. Mutual Exclusion, Progress and Bounded Waiting
- [ ] D. Progress only

> [!question]- Show Answer
> ✅ **Answer:** **C**
>
> Peterson's Algorithm satisfies all three requirements of the Critical Section Problem.

---

## Q5 ⭐⭐

The waiting mechanism used in Peterson's Algorithm is:

- [ ] A. Blocking
- [ ] B. Sleeping
- [ ] C. Busy Waiting (Spin Waiting)
- [ ] D. Interrupt Waiting

> [!question]- Show Answer
> ✅ **Answer:** **C**
>
> The process repeatedly checks the condition inside the `while` loop, consuming CPU cycles.

---

## Q6 ⭐⭐⭐ (GATE)

Given:

```text
flag[0] = true
flag[1] = true
turn = 0
```

Which process enters the Critical Section first?

- [ ] A. P0
- [ ] B. P1
- [ ] C. Both
- [ ] D. Neither

> [!question]- Show Answer
> ✅ **Answer:** **A. P0**
>
> P0 checks:
>
> ```c
> while(flag[1] && turn == 1)
> ```
>
> ```
> true && false
> ```
>
> → False
>
> So **P0 enters**.
>
> P1 checks:
>
> ```c
> while(flag[0] && turn == 0)
> ```
>
> ```
> true && true
> ```
>
> → True
>
> So **P1 waits**.

---

## Q7 ⭐⭐⭐

If only **P1** wants to enter the Critical Section, what happens?

- [ ] A. P1 waits
- [ ] B. P1 immediately enters
- [ ] C. Deadlock occurs
- [ ] D. Both enter

> [!question]- Show Answer
> ✅ **Answer:** **B**
>
> Since `flag[0] = false`, the `while` condition becomes false and P1 enters immediately.

---

## Q8 ⭐⭐⭐

Which of the following is **NOT** a limitation of Peterson's Algorithm?

- [ ] A. Works only for two processes
- [ ] B. Uses Busy Waiting
- [ ] C. Requires hardware support
- [ ] D. Not practical for modern systems

> [!question]- Show Answer
> ✅ **Answer:** **C**
>
> Peterson's Algorithm is a **pure software solution** and does not require hardware support.

---

## Q9 ⭐⭐⭐

Which variable gives priority to the other process?

- [ ] A. `flag[]`
- [ ] B. `turn`
- [ ] C. `lock`
- [ ] D. `mutex`

> [!question]- Show Answer
> ✅ **Answer:** **B**
>
> `turn` resolves conflicts when both processes are interested.

---

## Q10 ⭐⭐⭐⭐ (Classic GATE Trap)

Which statement is FALSE?

- [ ] A. Peterson's Algorithm is a software solution.
- [ ] B. It guarantees Mutual Exclusion.
- [ ] C. It works for any number of processes.
- [ ] D. It uses Busy Waiting.

> [!question]- Show Answer
> ✅ **Answer:** **C**
>
> Peterson's Algorithm works for **exactly two processes**, not for an arbitrary number of processes.

---

# Score

| Correct | Level |
|---------:|-------|
| 9–10 | 🟢 GATE Ready |
| 7–8 | 🟡 One More Revision |
| 5–6 | 🟠 Revise Peterson's Algorithm |
| 0–4 | 🔴 Study Again |