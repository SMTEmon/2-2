---
title: "04 - Book Extras & Professor Traps"
course: "CSE 4411"
chapter: "Physical Layer (Forouzan Ch 4)"
tags:
  - cse4411
  - networking
  - physical-layer
  - exam-traps
  - textbook-extras
  - companding
  - final-exam
aliases:
  - Physical Layer Exam Traps
  - Forouzan Chapter 4 Extras
---

# 04 - Book Extras & Professor Traps (Physical Layer)

> [!abstract] Key Takeaway
> This document details advanced theoretical nuances from **Behrouz Forouzan (4th Edition)** on **Companding models**, **MLT-3 state machine rules**, and top physical layer exam pitfalls.

---

## 1. Mathematical Models of Companding

In telephone PCM, companding standardizes voice dynamic range across loud and soft speakers:

```
                  Compressor (Tx) ──► Channel (PCM) ──► Expander (Rx)
Linear Input x ──► [ y = f(x) ]   ──► Quantization  ──► [ x = f^-1(y) ] ──► Output
```

1. **$\mu$-Law (North America & Japan - $\mu = 255$):**
   $$y = \frac{\ln(1 + \mu |x|)}{\ln(1 + \mu)} \cdot \text{sgn}(x)$$
2. **A-Law (Europe & International - $A = 87.6$):**
   $$y = \begin{cases} \frac{A |x|}{1 + \ln A} \cdot \text{sgn}(x) & \text{for } |x| < \frac{1}{A} \\ \frac{1 + \ln(A |x|)}{1 + \ln A} \cdot \text{sgn}(x) & \text{for } \frac{1}{A} \le |x| \le 1 \end{cases}$$

---

## 2. Top 5 Professor Traps in Physical Layer

| # | Common Student Error | Ground Truth Reality | Exam Context |
| :---: | :--- | :--- | :--- |
| **1** | Misidentifying the **Manchester transition direction**. | In standard 802.3 Manchester: **Low-to-High ($\uparrow$) is `0`**, **High-to-Low ($\downarrow$) is `1`**. | Waveform decoding problem. |
| **2** | Forgetting the $+1.76\text{ dB}$ constant in the SQNR formula. | $\text{SNR}_{dB} = 6.02 n_b + 1.76\text{ dB}$ (The $1.76\text{ dB}$ comes from sinusoidal signal power: $10 \log_{10}(1.5)$). | Quantization noise proof. |
| **3** | Applying **B8ZS substitution** to 4 zeros instead of 8. | **B8ZS** substitutes **8 zeros** (`000VB0VB`); **HDB3** substitutes **4 zeros** (`000V` or `B00V`). | Scrambling trace problem. |
| **4** | Resetting HDB3 pulse counter on violation ($V$). | In HDB3, when a substitution occurs (`000V` or `B00V`), the pulse count since last substitution is **reset to 0**. | HDB3 state tracing. |
| **5** | Confusing NRZ-L with NRZ-I. | **NRZ-L:** Voltage level represents bit value ($+V$ is `0`, $-V$ is `1`). **NRZ-I:** Inversion/transition represents `1`; no transition represents `0`. | Waveform drawing. |

---
#### Navigation
← Previous: [[03 - Analog-to-Digital Conversion (PCM, Quantization, SQNR, DM)]] | Next: [[05 - Comprehensive Worked Numericals & Waveform Traces]] →
