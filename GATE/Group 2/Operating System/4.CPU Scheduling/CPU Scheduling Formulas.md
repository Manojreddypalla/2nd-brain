# 📘 GATE Corner – CPU Scheduling Formulas

## Formula Sheet

### Symbols

| Symbol | Meaning |
|--------|---------|
| AT | Arrival Time |
| BT | Burst Time (CPU Burst Time) |
| CT | Completion Time |
| TAT | Turnaround Time |
| WT | Waiting Time |
| RT | Response Time |

---

## Turnaround Time (TAT)

Time from process arrival until completion.

**Formula**

```
TAT = CT - AT
```

Rearrangements:

```
CT = AT + TAT
AT = CT - TAT
```

---

## Waiting Time (WT)

Time spent waiting in the Ready Queue.

**Formula**

```
WT = TAT - BT
```

Rearrangements:

```
TAT = WT + BT
BT = TAT - WT
```

---

## Response Time (RT)

Time from process arrival until it gets the CPU for the first time.

**Formula**

```
RT = First CPU Start Time - AT
```

Rearrangement:

```
First CPU Start Time = AT + RT
```

---

## Average Waiting Time

```
Average WT = ΣWT / n
```

---

## Average Turnaround Time

```
Average TAT = ΣTAT / n
```

---

## Average Response Time

```
Average RT = ΣRT / n
```

---

## CPU Utilization

Percentage of time the CPU is busy.

```
CPU Utilization = (CPU Busy Time / Total Time) × 100%
```

If CPU idle time is given:

```
CPU Utilization = ((Total Time - Idle Time) / Total Time) × 100%
```

---

## Throughput

Number of completed processes per unit time.

```
Throughput = Number of Completed Processes / Total Time
```

---

## Normalized Turnaround Time (NTAT)

*(Rarely asked in GATE)*

```
NTAT = TAT / BT
```

---

# Memory Triangle

```
        TAT
       /   \
     WT     BT
```

```
TAT = WT + BT
WT  = TAT - BT
BT  = TAT - WT
```

---

# Formula Chain

```
AT → First Start → CT

RT  = First Start - AT
TAT = CT - AT
WT  = TAT - BT
```

---

## 🎯 GATE Corner

Memorize these seven formulas:

1. TAT = CT − AT
2. WT = TAT − BT
3. RT = First Start Time − AT
4. Average WT = ΣWT / n
5. Average TAT = ΣTAT / n
6. CPU Utilization = (Busy Time / Total Time) × 100%
7. Throughput = Completed Processes / Total Time