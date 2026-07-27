# Bakery Algorithm Quiz

---

## Q1 ⭐

Bakery Algorithm is designed for:

- [ ] A. One Process
- [ ] B. Two Processes
- [ ] C. N Processes
- [ ] D. Dual-core CPUs

> [!question]- Show Answer
> ✅ **Answer: C**
>
> Bakery Algorithm supports **multiple (N) processes**.

---

## Q2 ⭐

Which array stores ticket numbers?

- [ ] A. `flag[]`
- [ ] B. `turn`
- [ ] C. `choosing[]`
- [ ] D. `number[]`

> [!question]- Show Answer
> ✅ **Answer: D**
>
> `number[]` stores each process's ticket.

---

## Q3 ⭐⭐

What is the purpose of `choosing[]`?

- [ ] A. Store ticket number
- [ ] B. Indicate a process is selecting its ticket
- [ ] C. Count processes
- [ ] D. Store PID

> [!question]- Show Answer
> ✅ **Answer: B**

---

## Q4 ⭐⭐

If two processes receive the same ticket number, who enters first?

- [ ] A. Larger PID
- [ ] B. Smaller PID
- [ ] C. Randomly
- [ ] D. Both together

> [!question]- Show Answer
> ✅ **Answer: B**
>
> The tie is broken using the process ID.

---

## Q5 ⭐⭐

Bakery Algorithm guarantees:

- [ ] A. Mutual Exclusion only
- [ ] B. Progress only
- [ ] C. Mutual Exclusion, Progress and Bounded Waiting
- [ ] D. Deadlock Freedom only

> [!question]- Show Answer
> ✅ **Answer: C**

---

## Q6 ⭐⭐⭐ (GATE)

Which statement is FALSE?

- [ ] A. Bakery Algorithm supports multiple processes.
- [ ] B. Smaller ticket numbers have higher priority.
- [ ] C. It uses Busy Waiting.
- [ ] D. Higher ticket numbers enter first.

> [!question]- Show Answer
> ✅ **Answer: D**
>
> The **smallest** ticket number gets priority.