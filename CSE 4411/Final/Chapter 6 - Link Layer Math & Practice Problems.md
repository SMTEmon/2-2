---
title: "Chapter 6: Link Layer Math & Practice Problems"
course: "CSE 4411 - Computer Networks"
type: "Math Cheat Sheet & Solved Problems"
tags:
  - networking
  - link-layer
  - crc
  - aloha
  - csma-cd
  - practice-problems
  - cse4411
aliases:
  - Link Layer Math Notes
  - Chapter 6 Solved Problems
---

# Chapter 6: Link Layer Math & Practice Problems

> [!abstract] Overview
> This note contains all mathematical formulas, efficiency proofs, step-by-step algorithms, and solved end-of-chapter textbook problems for **Chapter 6 (Up to Ethernet Frame Structure)**.

---

## 1. Error Detection & Correction Mathematics

### 1.1 Parity Checking Mathematics

#### Two-Dimensional Parity Matrix Formulation
Given $d$ data bits arranged in $i$ rows and $j$ columns ($d = i \times j$):
* Row parity $r_k$ for row $k$ ($1 \le k \le i$):
  $$ r_k = d_{k,1} \oplus d_{k,2} \oplus \dots \oplus d_{k,j} $$
* Column parity $c_m$ for column $m$ ($1 \le m \le j$):
  $$ c_m = d_{1,m} \oplus d_{2,m} \oplus \dots \oplus d_{i,m} $$

#### Single-Bit Error Correction Rule
If a single bit error occurs at position $(r^*, c^*)$:
* Row $r^*$ parity check fails.
* Column $c^*$ parity check fails.
* The corrupted bit is inverted: $d_{r^*, c^*} \leftarrow d_{r^*, c^*} \oplus 1$.

---

### 1.2 Cyclic Redundancy Check (CRC) Formulas & Step-by-Step Algorithm

#### Mathematical Definitions:
* $D$: Data bit pattern of $d$ bits.
* $G$: Generator bit pattern of $r+1$ bits (agreed standard, MSB = 1).
* $R$: CRC checksum of $r$ bits.

#### Key Relation:
The sender constructs $\langle D, R \rangle = D \cdot 2^r \text{ XOR } R$ such that:
$$ (D \cdot 2^r \text{ XOR } R) = n \cdot G \quad (\text{mod } 2) $$

Taking XOR of $R$ on both sides:
$$ D \cdot 2^r = n \cdot G \text{ XOR } R $$

Therefore, $R$ is the **remainder** when $D \cdot 2^r$ is divided by $G$ using modulo-2 binary division:
$$ R = \text{remainder}\left[ \frac{D \cdot 2^r}{G} \right] $$

---

#### Solved CRC Calculation Walkthrough (Slide 16 Example)

**Given**:
* Data $D = 101110_2$ ($d = 6$)
* Generator $G = 1001_2$ ($r+1 = 4 \implies r = 3$)

**Step 1**: Append $r = 3$ zeros to $D$:
$$ D \cdot 2^3 = 101110000_2 $$

**Step 2**: Perform Modulo-2 Division of $101110000_2$ by $1001_2$:

```
            1 0 1 0 1 1  <-- Quotient (n)
   1 0 0 1 | 1 0 1 1 1 0 0 0 0
             1 0 0 1
             -------
               0 1 0 1
                 0 0 0 0
                 -------
                 1 0 1 0
                 1 0 0 1
                 -------
                   0 1 1 0
                     0 0 0 0
                     -------
                     1 1 0 0
                     1 0 0 1
                     -------
                       1 0 1 0
                       1 0 0 1
                       -------
                         0 1 1  <-- Remainder (R = 011)
```

**Step 3**: The CRC bits $R = 011_2$.
The transmitted bit pattern is $\langle D, R \rangle = 101110011_2$.

**Verification at Receiver**:
$$\frac{101110011_2}{1001_2} = 101011_2 \quad \text{with Remainder } 000 \implies \text{No error!}$$

---

## 2. MAC Protocol Efficiency Proofs & Formulas

### 2.1 Slotted ALOHA Efficiency Derivation

#### Setup:
* $N$ active nodes, each with probability $p$ of transmitting a frame in a given slot.
* A slot is **successful** if **exactly 1 node** transmits, and $N-1$ nodes do not transmit.

#### Derivation:
1. Probability that a specific node succeeds in a slot:
   $$ P(\text{specific node succeeds}) = p(1-p)^{N-1} $$

2. Probability that **any** of the $N$ nodes succeeds in a slot ($S$):
   $$ S = N \cdot p (1-p)^{N-1} $$

3. To find optimal $p^*$ maximizing $S$, take derivative with respect to $p$ and set to 0:
   $$ \frac{dS}{dp} = N(1-p)^{N-1} - N(N-1)p(1-p)^{N-2} = 0 $$
   $$ N(1-p)^{N-2} \left[ (1-p) - (N-1)p \right] = 0 $$
   $$ 1 - p - Np + p = 0 \implies p^* = \frac{1}{N} $$

4. Substitute $p^* = \frac{1}{N}$ into $S$:
   $$ S_{\text{max}} = N \left( \frac{1}{N} \right) \left( 1 - \frac{1}{N} \right)^{N-1} = \left( 1 - \frac{1}{N} \right)^{N-1} $$

5. Taking the limit as $N \to \infty$:
   $$ \lim_{N \to \infty} \left( 1 - \frac{1}{N} \right)^{N-1} = \frac{1}{e} \approx 0.3678 $$

> [!success] Result
> Maximum Slotted ALOHA Efficiency = **36.8% (or $1/e$)**.

---

### 2.2 Pure ALOHA Efficiency Derivation

#### Setup:
* Unslotted time. Frame transmission time = $1$ unit of time.
* Suppose Node $i$ begins transmitting at time $t_0$.

#### Vulnerable Period:
For Node $i$'s frame to suffer no collision:
1. No other node can begin transmitting in $[t_0 - 1, t_0]$ (which would overlap the beginning of frame $i$).
2. No other node can begin transmitting in $[t_0, t_0 + 1]$ (which would overlap the end of frame $i$).
* Total vulnerable interval length = $2$ frame times.

```
                    Vulnerable Period (Length = 2 frame times)
            |-------------------|-------------------|
          t0 - 1               t0                 t0 + 1
                     Node i transmits
```

#### Derivation:
1. Probability no other node transmits in $[t_0 - 1, t_0]$: $(1-p)^{N-1}$
2. Probability no other node transmits in $[t_0, t_0 + 1]$: $(1-p)^{N-1}$
3. Probability a given node succeeds:
   $$ P(\text{success}) = p (1-p)^{2(N-1)} $$
4. Total Efficiency $S = N p (1-p)^{2(N-1)}$.
5. Maximizing gives $p^* = \frac{1}{2N}$.
6. As $N \to \infty$:
   $$ S_{\text{max}} = \lim_{N \to \infty} \left( 1 - \frac{1}{2N} \right)^{2(N-1)} = \frac{1}{2e} \approx 0.1839 $$

> [!success] Result
> Maximum Pure ALOHA Efficiency = **18.4% (or $1/(2e)$)**.

---

### 2.3 CSMA/CD Efficiency & Backoff Math

#### CSMA/CD Efficiency Approximation:
$$ \text{Efficiency} = \frac{1}{1 + 5 \cdot \left( \frac{t_{prop}}{t_{trans}} \right)} $$

Where:
* $t_{prop}$: Maximum propagation delay between any 2 nodes in the LAN.
* $t_{trans}$: Time required to transmit a maximum-sized frame ($L / R$).

#### Binary Exponential Backoff Parameters:
After $m$-th collision:
* Random integer $K$ chosen from $\{0, 1, 2, \dots, 2^m - 1\}$ for $m \le 10$.
* For $m > 10$, set $m = 10$ (max set size $\{0, \dots, 1023\}$).
* After 16 collisions, transmission is aborted and error reported.
* **Wait Time**:
  $$ \text{Wait Time} = K \times 512 \text{ bit times} = K \times \left( \frac{512}{R} \right) \text{ seconds} $$

---

## 3. Textbook Problems & Solutions (Chapter 6)

### Problem P1 (Two-Dimensional Parity)
> **Question**: Suppose the information content of a packet is the bit pattern `1110 0110 1001 0101` and an even parity scheme is being used. What would the value of the field containing the parity bits be for the case of a two-dimensional parity scheme? Format as a minimum-length field.

**Solution**:
Arranging the 16 bits into a $4 \times 4$ matrix:

$$
\begin{matrix}
\text{Data Matrix} & \text{Row Parity} \\
1 \quad 1 \quad 1 \quad 0 & \mathbf{1} \\
0 \quad 1 \quad 1 \quad 0 & \mathbf{0} \\
1 \quad 0 \quad 0 \quad 1 & \mathbf{0} \\
0 \quad 1 \quad 0 \quad 1 & \mathbf{0} \\
\hline
\mathbf{0} \quad \mathbf{1} \quad \mathbf{0} \quad \mathbf{0} & \mathbf{1} \quad (\text{Corner})
\end{matrix}
$$

* Row Parity bits: `1 0 0 0`
* Column Parity bits: `0 1 0 0`
* Corner Parity bit: `1`

**Parity Field Output**: `1000 0100 1` (or combined 9 bits).

---

### Problem P5 (CRC Computation)
> **Question**: Consider the 5-bit generator $G = 10011$, and suppose that $D$ has the value $1010101010$. What is the value of $R$?

**Solution**:
1. $G = 10011 \implies r+1 = 5 \implies r = 4$ bits.
2. $D \cdot 2^4 = 10101010100000_2$.
3. Divide $10101010100000$ by $10011$ using XOR arithmetic:

```
            1 0 1 1 0 1 0 1 1 0
   10011 | 10101010100000
           10011
           -----
            01100
            00000
            -----
             11001
             10011
             -----
              10100
              10011
              -----
               01111
               00000
               -----
                11110
                10011
                -----
                 11010
                 10011
                 -----
                  10010
                  10011
                  -----
                   000100  --> Remainder R = 0100 (4 bits)
```

**Answer**: $R = 0100_2$.

---

### Problem P8 (Slotted ALOHA Probability)
> **Question**:
> (a) Find the value of $p$ that maximizes $S = N p (1-p)^{N-1}$.  
> (b) Find the efficiency as $N \to \infty$.

**Solution**:
* (a) Taking the derivative $\frac{dS}{dp} = 0$ yields $p^* = \frac{1}{N}$.
* (b) $\lim_{N \to \infty} N \left(\frac{1}{N}\right) \left(1 - \frac{1}{N}\right)^{N-1} = \frac{1}{e} \approx 0.368$.

---

### Problem P17 (CSMA/CD Backoff Time)
> **Question**: In CSMA/CD, the adapter waits $K \cdot 512$ bit times after a collision. For $K = 100$, how long does the adapter wait until returning to Step 2 for:
> (a) a 100 Mbps channel?  
> (b) a 1 Gbps channel?

**Solution**:
Total bit times to wait = $100 \times 512 = 51,200 \text{ bits}$.

* **(a) 100 Mbps Channel ($R = 100 \times 10^6 \text{ bps}$)**:
  $$ \text{Wait Time} = \frac{51,200 \text{ bits}}{100 \times 10^6 \text{ bps}} = 512 \times 10^{-6} \text{ seconds} = 512 \ \mu\text{s} = 0.512 \text{ ms} $$

* **(b) 1 Gbps Channel ($R = 1 \times 10^9 \text{ bps}$)**:
  $$ \text{Wait Time} = \frac{51,200 \text{ bits}}{1 \times 10^9 \text{ bps}} = 51.2 \times 10^{-6} \text{ seconds} = 51.2 \ \mu\text{s} = 0.0512 \text{ ms} $$

---

### Problem P18 (CSMA/CD Collision Detection Scenario)
> **Question**: Suppose nodes A and B are on the same 10 Mbps broadcast channel, and propagation delay between them is 325 bit times. CSMA/CD is used. Suppose A begins transmitting a frame at $t=0$. Before A finishes, B begins transmitting. Can A finish transmitting before it detects that B has transmitted? (Assume min Ethernet frame size = 512 bits + 64 preamble bits = 576 bits).

**Solution**:
* A finishes transmission at $t = 576 \text{ bit times}$.
* In the worst case, B begins transmitting just before A's signal reaches B (at $t = 325 - \epsilon \text{ bit times}$).
* B's signal takes another 325 bit times to propagate back to A.
* A detects B's signal at $t = 325 + 325 = 650 \text{ bit times}$.
* Since $650 > 576$, A would have already finished transmitting its frame at $t = 576$ before B's colliding signal reaches A!

> [!warning] Conclusion
> **Yes, A can finish transmitting before detecting the collision!** This illustrates why Ethernet imposes a **minimum frame length (64 bytes / 512 bits)** relative to the max LAN distance, ensuring $t_{trans} \ge 2 \cdot t_{prop}$.

---

> [!success] Completion
> Both study guide and math cheat sheet notes are complete and saved in your vault folder!
