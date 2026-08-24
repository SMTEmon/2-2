---
title: "05 - Comprehensive Worked Numericals & Waveform Traces"
course: "CSE 4411"
chapter: "Physical Layer (Forouzan Ch 4)"
tags:
  - cse4411
  - networking
  - physical-layer
  - practice-problems
  - worked-numericals
  - exam-prep
  - line-coding
  - scrambling
aliases:
  - Physical Layer Practice Problems
  - Scrambling and Waveform Numericals
---

# 05 - Comprehensive Worked Numericals & Waveform Traces (Physical Layer)

> [!abstract] Exam Objective
> This document provides fully derived solutions for **B8ZS & HDB3 Scrambling state traces**, **Line Coding waveform drawing**, and **PCM Audio Digitization** calculations.

---

## 🧮 Problem Set 1: Scrambling State Tracing (B8ZS vs HDB3)

### Problem Statement
Given the bit sequence:
$$\mathbf{1 \quad 0 \quad 0 \quad 0 \quad 0 \quad 0 \quad 0 \quad 0 \quad 0 \quad 1 \quad 0 \quad 0 \quad 0 \quad 0 \quad 1}$$
Assume that the pulse preceding this sequence was **Positive ($+V$)**.

**Questions:**
1. Generate the transmitted pulse sequence using **B8ZS**.
2. Generate the transmitted pulse sequence using **HDB3**.

---

### Step-by-Step Solution

#### Part 1: B8ZS Substitution
- Preceding pulse $= +V$.
- Initial bit `1` alternates in AMI $\implies \mathbf{-V}$.
- Followed by **8 consecutive zeros** (`00000000`):
  - Preceding pulse is negative ($-V$).
  - B8ZS rule for negative preceding pulse: `0 0 0 - + 0 + -`
- Followed by bit `1` $\implies$ AMI alternates opposite of last non-zero pulse ($-V$) $\implies \mathbf{+V}$.
- Followed by **4 consecutive zeros** $\implies$ B8ZS does NOT substitute (only triggers on 8 zeros) $\implies \mathbf{0 \ 0 \ 0 \ 0}$.
- Followed by bit `1` $\implies$ AMI alternates opposite of $+V \implies \mathbf{-V}$.

**Final B8ZS Sequence:**
$$\mathbf{- \quad 0 \quad 0 \quad 0 \quad - \quad + \quad 0 \quad + \quad - \quad + \quad 0 \quad 0 \quad 0 \quad 0 \quad -}$$

---

#### Part 2: HDB3 Substitution
- Preceding pulse $= +V$.
- Initial bit `1` alternates in AMI $\implies \mathbf{-V}$ (Pulse count since start $= 1$, which is **Odd**).
- First block of 4 zeros (`0000`):
  - Pulses since last substitution $= 1$ (**Odd**).
  - Preceding pulse is negative ($-V$).
  - Rule for Odd count and Negative pulse: `0 0 0 V` $\implies \mathbf{0 \ 0 \ 0 \ -}$.
  - (Reset pulse count to $0$).
- Second block of 4 zeros (`0000`):
  - Pulses since last substitution $= 0$ (**Even**).
  - Preceding pulse is negative ($-V$).
  - Rule for Even count and Negative pulse: `B 0 0 V` $\implies \mathbf{+ \ 0 \ 0 \ +}$.
  - (Reset pulse count to $0$).
- Followed by bit `1` $\implies$ AMI alternates opposite of last pulse ($+V$) $\implies \mathbf{-V}$ (Pulse count $= 1$, **Odd**).
- Third block of 4 zeros (`0000`):
  - Pulses since last substitution $= 1$ (**Odd**).
  - Preceding pulse is negative ($-V$).
  - Rule for Odd count: `0 0 0 V` $\implies \mathbf{0 \ 0 \ 0 \ -}$.
- Followed by bit `1` $\implies$ AMI alternates $\implies \mathbf{+V}$.

**Final HDB3 Sequence:**
$$\mathbf{- \quad 0 \quad 0 \quad 0 \quad - \quad + \quad 0 \quad 0 \quad + \quad - \quad 0 \quad 0 \quad 0 \quad - \quad +}$$

---

## 🧮 Problem Set 2: Hi-Fi Audio Digitization (PCM Calculations)

### Problem Statement
A high-fidelity stereo audio signal has bandwidth spanning up to $f_{max} = 22\text{ kHz}$. The signal is sampled at a rate **$20\%$ higher than the theoretical Nyquist rate** and quantized into $L = 65,536$ levels.

**Calculate:**
1. The sampling rate ($f_s$).
2. The number of bits per sample ($n_b$).
3. The bit rate for a single audio channel ($N$).
4. The resulting Signal-to-Quantization-Noise Ratio ($\text{SNR}_{dB}$).

---

### Step-by-Step Solution

#### 1. Sampling Rate ($f_s$)
- Theoretical Nyquist rate $= 2 \times f_{max} = 2 \times 22,000\text{ Hz} = 44,000\text{ samples/sec}$.
- Actual sampling rate ($20\%$ higher):
  $$f_s = 44,000 \times 1.20 = \mathbf{52,800\text{ samples/sec}}$$

#### 2. Bits per Sample ($n_b$)
$$L = 2^{n_b} \implies 65,536 = 2^{n_b} \implies n_b = \log_2(65536) = \mathbf{16\text{ bits/sample}}$$

#### 3. Bit Rate ($N$)
$$N = f_s \times n_b = 52,800\text{ samples/s} \times 16\text{ bits/sample} = \mathbf{844,800\text{ bps}} = \mathbf{844.8\text{ kbps}}$$

#### 4. Signal-to-Quantization-Noise Ratio ($\text{SNR}_{dB}$)
$$\text{SNR}_{dB} = 6.02 n_b + 1.76\text{ dB} = 6.02(16) + 1.76 = 96.32 + 1.76 = \mathbf{98.08\text{ dB}}$$

---
#### Navigation
← Previous: [[04 - Book Extras & Professor Traps]] | Next: [[00 - CSE 4411 Final Exam Master Blueprint & Formula Sheet]] →
