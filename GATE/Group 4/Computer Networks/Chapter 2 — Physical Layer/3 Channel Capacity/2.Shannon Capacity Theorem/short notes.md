# 🎓 GATE Corner ⭐

## 1. When to use Nyquist vs Shannon

| If the Question Mentions... | Use |
|------------------------------|-----|
| Noiseless Channel | Nyquist |
| Ideal Channel | Nyquist |
| Signal Levels (L) | Nyquist |
| Baud Rate | Nyquist |
| Noise | Shannon |
| SNR | Shannon |
| Channel Capacity | Shannon |

> [!tip]
> **Noise or SNR ⇒ Think Shannon**
>
> **No Noise ⇒ Think Nyquist**

---

## 2. Most Common GATE Mistakes

### ❌ Mistake 1

Using **SNR in dB** directly in Shannon's formula.

Wrong:

$$
C=B\log_2(1+30)
$$

Correct:

Convert first

$$
30\text{ dB}=1000
$$

Then

$$
C=B\log_2(1001)
$$

---

### ❌ Mistake 2

Using Nyquist when the question mentions noise.

Nyquist **ignores noise**.

Whenever the problem mentions:

- Noise
- SNR
- Signal Power
- Noise Power
- Capacity

Use **Shannon**.

---

### ❌ Mistake 3

Confusing Bandwidth with Capacity.

Bandwidth

- Unit → Hz

Capacity

- Unit → bps

They are **not the same**.

---

### ❌ Mistake 4

Thinking capacity can be increased infinitely by increasing signal levels.

Wrong.

In real channels, increasing signal levels eventually causes errors because of noise.

Shannon gives the upper limit.

---

## 3. PYQ Patterns

GATE usually asks:

- Calculate channel capacity.
- Convert SNR (dB) to linear.
- Difference between Nyquist and Shannon.
- Effect of increasing Bandwidth.
- Effect of increasing Noise.
- Effect of increasing Signal Power.
- Conceptual questions on SNR.

---

## 4. Shortcut Values

### SNR (dB) → Linear

| dB | Linear SNR |
|----|-----------:|
| 0 | 1 |
| 3 | 2 (Approx.) |
| 10 | 10 |
| 20 | 100 |
| 30 | 1000 |
| 40 | 10000 |

> [!tip]
> Memorize these values. They save a lot of calculation time.

---

## 5. Concept-Based Questions

### Q1

Increasing **Bandwidth** while keeping SNR constant will:

✅ Increase Channel Capacity.

---

### Q2

Increasing **Noise Power** while keeping Signal Power constant will:

- Decrease SNR
- Decrease Capacity

---

### Q3

Increasing **Signal Power** while keeping Noise constant will:

- Increase SNR
- Increase Capacity

---

### Q4

If SNR = 0 dB,

Then

$$
SNR=1
$$

Capacity becomes

$$
C=B\log_2(2)=B
$$

---

## 6. Formula Checklist

### Shannon Capacity

$$
\boxed{C=B\log_2(1+\text{SNR})}
$$

---

### Signal-to-Noise Ratio

$$
\boxed{\text{SNR}=\frac{S}{N}}
$$

---

### SNR in dB

$$
\boxed{\text{SNR}_{dB}=10\log_{10}\left(\frac{S}{N}\right)}
$$

---

## 7. Memory Trick 🧠

```text
Bandwidth + No Noise
        │
        ▼
     Nyquist
        │
2B log₂L

Bandwidth + Noise
        │
        ▼
     Shannon
        │
B log₂(1 + SNR)
```

---

## 8. One-Line Revision

- **Nyquist → Noiseless Channel**
- **Shannon → Noisy Channel**
- **Noise is represented by SNR**
- **Convert dB → Linear before using the formula**
- **Higher SNR ⇒ Higher Capacity**
- **Higher Bandwidth ⇒ Higher Capacity**
- **Channel Capacity is measured in bps**