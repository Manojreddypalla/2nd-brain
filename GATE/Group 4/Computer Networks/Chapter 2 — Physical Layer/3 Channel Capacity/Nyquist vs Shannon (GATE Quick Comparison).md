# Nyquist vs Shannon (GATE Quick Comparison)

| Feature | Nyquist Theorem | Shannon Capacity Theorem |
|---------|-----------------|--------------------------|
| Purpose | Maximum data rate for a **noiseless** channel | Maximum data rate for a **noisy** channel |
| Considers Noise? | ❌ No | ✅ Yes |
| Depends On | Bandwidth (B) and Signal Levels (L) | Bandwidth (B) and Signal-to-Noise Ratio (SNR) |
| Formula | $$C=2B\log_2L$$ | $$C=B\log_2(1+\text{SNR})$$ |
| Unit | bps | bps |
| Limitation | Ignores channel noise | Considers channel noise |
| Used For | Ideal communication channels | Real-world communication channels |

---

# Formulas ⭐

### Nyquist

$$
C=2B\log_2L
$$

where:
- \(C\) = Maximum Data Rate (bps)
- \(B\) = Bandwidth (Hz)
- \(L\) = Number of Signal Levels

---

### Shannon

$$
C=B\log_2(1+\text{SNR})
$$

where:
- \(C\) = Channel Capacity (bps)
- \(B\) = Bandwidth (Hz)
- SNR = Signal-to-Noise Ratio (linear)

If SNR is given in decibels (dB):

$$
SNR_{dB}=10\log_{10}(SNR)
$$

Convert back to linear:

$$
SNR=10^{\frac{SNR_{dB}}{10}}
$$

---

# GATE Memory Trick 🧠

- **Nyquist → No Noise**
- **Shannon → Some Noise**