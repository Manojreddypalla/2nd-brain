# Test-and-Set (TSL) Quiz

---

## Q1 ⭐

Test-and-Set is a:

- [ ] A. Software Algorithm
- [ ] B. Hardware Instruction
- [ ] C. Scheduling Algorithm
- [ ] D. Deadlock Prevention Algorithm

> [!question]- Show Answer
> ✅ **Answer:** **B. Hardware Instruction**
>
> Test-and-Set is a **hardware synchronization instruction** provided by the CPU.

---

## Q2 ⭐

The primary purpose of Test-and-Set is to:

- [ ] A. Schedule Processes
- [ ] B. Achieve Mutual Exclusion
- [ ] C. Detect Deadlock
- [ ] D. Allocate Memory

> [!question]- Show Answer
> ✅ **Answer:** **B**
>
> Test-and-Set is used to implement **Mutual Exclusion** so that only one process enters the Critical Section.

---

## Q3 ⭐⭐

Test-and-Set performs:

- [ ] A. Read operation only
- [ ] B. Write operation only
- [ ] C. Read and Write atomically
- [ ] D. Compare only

> [!question]- Show Answer
> ✅ **Answer:** **C**
>
> It **reads the old value**, **sets the lock to 1**, and **returns the old value** as one atomic operation.

---

## Q4 ⭐⭐

Initially,

```text
lock = 0
```

After executing

```c
old = TestAndSet(lock);
```

what are the values?

- [ ] A. old = 0, lock = 1
- [ ] B. old = 1, lock = 1
- [ ] C. old = 0, lock = 0
- [ ] D. old = 1, lock = 0

> [!question]- Show Answer
> ✅ **Answer:** **A**
>
> The previous value is copied into `old`, then the lock is set to `1`.

---

## Q5 ⭐⭐

If Test-and-Set returns **1**, it means:

- [ ] A. Lock was free
- [ ] B. Lock was already occupied
- [ ] C. Lock becomes free
- [ ] D. Process enters immediately

> [!question]- Show Answer
> ✅ **Answer:** **B**
>
> A return value of **1** means another process already owns the lock.

---

## Q6 ⭐⭐⭐

If Test-and-Set returns **0**, then:

- [ ] A. The process acquires the lock
- [ ] B. The process waits
- [ ] C. The lock remains unlocked
- [ ] D. Deadlock occurs

> [!question]- Show Answer
> ✅ **Answer:** **A**
>
> Returning **0** means the lock was free before the operation.

---

## Q7 ⭐⭐⭐

Which waiting mechanism is used with Test-and-Set?

- [ ] A. Blocking
- [ ] B. Sleeping
- [ ] C. Busy Waiting (Spin Waiting)
- [ ] D. Interrupt Waiting

> [!question]- Show Answer
> ✅ **Answer:** **C**
>
> Processes repeatedly execute Test-and-Set until the lock becomes available.

---

## Q8 ⭐⭐⭐

Who releases the lock after leaving the Critical Section?

- [ ] A. CPU
- [ ] B. Operating System
- [ ] C. The Process itself
- [ ] D. Scheduler

> [!question]- Show Answer
> ✅ **Answer:** **C**
>
> The process executes:
>
> ```c
> lock = false;
> ```
>
> after finishing the Critical Section.

---

## Q9 ⭐⭐⭐⭐ (GATE)

Which of the following is **NOT** guaranteed by Test-and-Set?

- [ ] A. Mutual Exclusion
- [ ] B. Atomicity
- [ ] C. Fairness
- [ ] D. Hardware Support

> [!question]- Show Answer
> ✅ **Answer:** **C**
>
> Test-and-Set does **not** guarantee fairness.
> A process may wait indefinitely (starvation).

---

## Q10 ⭐⭐⭐⭐ (Classic GATE)

Which statement is FALSE?

- [ ] A. Test-and-Set is an atomic instruction.
- [ ] B. Test-and-Set is a hardware synchronization primitive.
- [ ] C. Test-and-Set eliminates Busy Waiting.
- [ ] D. Test-and-Set can be used to implement Spin Locks.

> [!question]- Show Answer
> ✅ **Answer:** **C**
>
> Test-and-Set **uses Busy Waiting**. Waiting processes continuously check the lock.

---

## Q11 ⭐⭐⭐⭐ (GATE MSQ)

Which of the following statements are correct regarding Test-and-Set?

- [ ] A. It is atomic.
- [ ] B. It returns the previous value of the lock.
- [ ] C. It guarantees starvation freedom.
- [ ] D. It can be used to implement Spin Locks.

> [!question]- Show Answer
> ✅ **Answer:** **A, B and D**
>
> - ✔ Atomic operation
> - ✔ Returns old value
> - ✔ Used to build Spin Locks
> - ✘ Does **not** guarantee starvation freedom

---

## Q12 ⭐⭐⭐⭐⭐ (GATE Numerical)

Initially,

```text
lock = 0
```

Processes **P1** and **P2** simultaneously execute:

```c
old = TestAndSet(lock);
```

Which of the following is possible?

- [ ] A. P1 gets 0, P2 gets 1
- [ ] B. P1 gets 1, P2 gets 0
- [ ] C. Both get 0
- [ ] D. Both get 1

> [!question]- Show Answer
> ✅ **Answer:** **A and B**
>
> Since Test-and-Set is **atomic**, exactly **one** process acquires the lock.
>
> Therefore, exactly one process receives **0**, while the other receives **1**.

---

# Score

| Correct | Level |
|---------:|-------|
| 11–12 | 🟢 GATE Ready |
| 9–10 | 🟡 One More Revision |
| 6–8 | 🟠 Revise Test-and-Set |
| 0–5 | 🔴 Study Again |
