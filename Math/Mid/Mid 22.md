***

# Solution: Mid-Semester Examination (Math 4441)
**Summer Semester, 2023-2024 | Islamic University of Technology (IUT)**

---

## Question 1

### (a) Venn Diagram Problem
**Given:** 
Total people asked, $N = 100$.
- $n(C) = 43$ (Coffee)
- $n(T) = 33$ (Tea)
- $n(J) = 56$ (Juice)
- $n(C \cap T) = 5$ 
- $n(T \cap J) = 20$
- $n(C \cap J) = 23$
- $n(C \cap T \cap J) = 2$

**Step 1: Fill in the Venn Diagram**
- Center (All three): $n(C \cap T \cap J) = 2$
- Only Coffee and Tea: $n(C \cap T) - 2 = 5 - 2 = 3$
- Only Tea and Juice: $n(T \cap J) - 2 = 20 - 2 = 18$
- Only Coffee and Juice: $n(C \cap J) - 2 = 23 - 2 = 21$
- Only Coffee: $43 - (3 + 21 + 2) = 43 - 26 = 17$
- Only Tea: $33 - (3 + 18 + 2) = 33 - 23 = 10$
- Only Juice: $56 - (21 + 18 + 2) = 56 - 41 = 15$

**Total people who drink at least one beverage:**
$$n(C \cup T \cup J) = 17 + 10 + 15 + 3 + 18 + 21 + 2 = 86$$

**Answers:**
**i. How many people don't drink any of these beverages in the morning?**
$$n(C \cup T \cup J)' = N - n(C \cup T \cup J) = 100 - 86 = 14$$

**ii. How many people drink exactly two of these beverages in the morning?**
This is the sum of the "only two" regions:
$$\text{Exactly two} = 3 \text{ (C and T)} + 18 \text{ (T and J)} + 21 \text{ (C and J)} = 42$$

---

### (b) Definitions
> [!definition] Random Experiment
> An experiment that can result in different outcomes, even though it is repeated in the exact same manner every time, is called a random experiment.

> [!definition] Conditional Probability & Multiplication Law
> **Conditional Probability:** The probability of an event $A$ occurring given that event $B$ has already occurred.
> $$P(A|B) = \frac{P(A \cap B)}{P(B)}, \quad \text{provided } P(B) > 0$$
> **Multiplication Law:** The probability of the simultaneous occurrence of two events $A$ and $B$.
> $$P(A \cap B) = P(A) \cdot P(B|A) = P(B) \cdot P(A|B)$$

---

### (c) Bayes' Theorem & Application
**i. Statement of Bayes Theorem:**
Let $A_1, A_2, \dots, A_n$ form a partition of the sample space $S$. Let $B$ be an event such that $P(B) > 0$. Bayes' theorem computes the conditional probability of $A_i$ given $B$:
$$P(A_i|B) = \frac{P(A_i) \cdot P(B|A_i)}{\sum_{j=1}^{n} P(A_j) \cdot P(B|A_j)}$$

**ii. Chess Tournament Problem:**
Let the events for opponent types be $T_1$, $T_2$, and $T_3$. Let $W$ be the event of winning.
**Given prior probabilities:**
- $P(T_1) = 1/2 = 0.5$
- $P(T_2) = 1/4 = 0.25$
- $P(T_3) = 1/4 = 0.25$

**Given conditional probabilities of winning:**
- $P(W|T_1) = 0.3$
- $P(W|T_2) = 0.4$
- $P(W|T_3) = 0.5$

**Probability of Winning (Total Probability):**
$$P(W) = P(W|T_1)P(T_1) + P(W|T_2)P(T_2) + P(W|T_3)P(T_3)$$
$$P(W) = (0.3)(0.5) + (0.4)(0.25) + (0.5)(0.25) = 0.15 + 0.10 + 0.125 = 0.375$$

**Probability opponent was Type-I given you won (Bayes' Theorem):**
$$P(T_1|W) = \frac{P(W|T_1)P(T_1)}{P(W)} = \frac{0.15}{0.375} = 0.4 \quad (\text{or } 40\%)$$

***

## Question 2

### (a) Definitions
> [!definition] PMF and CDF
> - **Probability Mass Function (PMF):** For a discrete random variable $X$, the PMF is a function $f(x) = P(X=x)$. 
>   *Conditions:* (1) $f(x) \ge 0$, (2) $\sum_x f(x) = 1$.
> - **Cumulative Distribution Function (CDF):** The CDF $F(x)$ represents the probability that $X$ takes a value less than or equal to $x$. 
>   *Expression:* $F(x) = P(X \le x) = \sum_{t \le x} f(t)$.

---

### (b) Drawing Balls (PMF & CDF)
**Scenario:** Box has 2 black and 3 red balls. Total = 5 balls. Drawn *without replacement* until the *last (2nd)* black ball is drawn. $X = \text{number of draws}$.
The 2nd black ball could be drawn on the 2nd, 3rd, 4th, or 5th draw.
Total possible arrangements of 2 Black and 3 Red balls is $\binom{5}{2} = 10$.
- **$X=2$:** Sequence `BB`. (1 arrangement). $P(X=2) = 1/10 = 0.1$
- **$X=3$:** Sequence `BRB`, `RBB`. (2 arrangements). $P(X=3) = 2/10 = 0.2$
- **$X=4$:** Sequences ending in B on the 4th draw (3 arrangements). $P(X=4) = 3/10 = 0.3$
- **$X=5$:** Sequences ending in B on the 5th draw (4 arrangements). $P(X=5) = 4/10 = 0.4$

**Tabular PMF:**

| $x$ | 2 | 3 | 4 | 5 |
| :--- | :--- | :--- | :--- | :--- |
| $P(X=x)$ | $0.1$ | $0.2$ | $0.3$ | $0.4$ |

**i. Check if $P(X)$ is a PMF:**
Yes, because $P(X=x) \ge 0$ for all $x$, and $\sum P(X=x) = 0.1+0.2+0.3+0.4 = 1.0$.

**ii. Find $P(X=6)$:** 
$P(X=6) = 0$ (The box only has 5 balls).

**iii. Evaluate $P(X \ge 4)$:**
$P(X \ge 4) = P(X=4) + P(X=5) = 0.3 + 0.4 = 0.7$

**iv. Evaluate $P(2 \le X \le 3)$:**
$P(2 \le X \le 3) = P(X=2) + P(X=3) = 0.1 + 0.2 = 0.3$

**v. Find the CDF of X:**
$$ F(x) = \begin{cases} 
0, & \text{for } x < 2 \\
0.1, & \text{for } 2 \le x < 3 \\
0.3, & \text{for } 3 \le x < 4 \\
0.6, & \text{for } 4 \le x < 5 \\
1, & \text{for } x \ge 5 
\end{cases} $$

---

### (c) Conditional Distributions
**Definitions:** (As defined in previous notes, $f(x|y) = f(x,y)/h(y)$ and $f(y|x) = f(x,y)/g(x)$).

**Given Joint PMF Table:**

| $X \downarrow \setminus Y \rightarrow$ | $0$ | $1$ | $2$ | **Marginal $g(x)$** |
| :--- | :--- | :--- | :--- | :--- |
| **0** | 1/36 | 6/36 | 3/36 | **10/36** |
| **1** | 8/36 | 12/36| 0 | **20/36** |
| **2** | 6/36 | 0 | 0 | **6/36** |
| **Marginal $h(y)$** | **15/36** | **18/36** | **3/36** | **Sum = 1** |

*Correction Note on table reading based on matrix layout standard:* In the provided image, rows are marked as X, columns as Y. Therefore: 
- $h(y)$ is the column sum (for Y): $h(0)=15/36$, $h(1)=18/36$, $h(2)=3/36$
- $g(x)$ is the row sum (for X): $g(0)=10/36$, $g(1)=20/36$, $g(2)=6/36$

**Evaluate $f(x|2)$:** (This means conditional distribution of X given Y=2)
$$h(2) = 3/36$$
- $f(0|2) = f(0,2) / h(2) = (3/36) / (3/36) = 1$
- $f(1|2) = f(1,2) / h(2) = 0$
- $f(2|2) = f(2,2) / h(2) = 0$

**Evaluate $f(y|1)$:** (Conditional distribution of Y given X=1)
$$g(1) = 20/36$$
- $f(0|1) = f(1,0) / g(1) = (8/36) / (20/36) = 8/20 = 2/5$
- $f(1|1) = f(1,1) / g(1) = (12/36) / (20/36) = 12/20 = 3/5$
- $f(2|1) = f(1,2) / g(1) = 0 / (20/36) = 0$

**Evaluate $P(X=1 | Y=2)$:**
$$P(X=1 | Y=2) = f(1|2) = \frac{f(1,2)}{h(2)} = \frac{0}{3/36} = 0$$

***

## Question 3

### (a) PDF Definition
> [!definition] Probability Density Function (PDF)
> A PDF $f(x)$ describes the relative likelihood for a continuous random variable to take on a given value. 
> *Properties:* 
> 1. $f(x) \ge 0$ for all $x$
> 2. $\int_{-\infty}^{\infty} f(x) dx = 1$
> 3. $P(a \le X \le b) = \int_{a}^{b} f(x)dx$

---

### (b) Continuous Joint PDF
**Given:** $f(x,y) = k(x^2 + 2xy)$ for $0 \le x \le 1, 0 \le y \le 1$

**i. Determine the value of constant $k$:**
$$\int_{0}^{1} \int_{0}^{1} k(x^2 + 2xy) \, dy \, dx = 1$$
Inner integral (w.r.t $y$):
$$\int_{0}^{1} (x^2 + 2xy) \, dy = \left[ x^2y + xy^2 \right]_{0}^{1} = (x^2 + x)$$
Outer integral (w.r.t $x$):
$$\int_{0}^{1} k(x^2 + x) \, dx = k \left[ \frac{x^3}{3} + \frac{x^2}{2} \right]_{0}^{1} = k \left( \frac{1}{3} + \frac{1}{2} \right) = \frac{5k}{6}$$
Setting equal to 1: $\frac{5k}{6} = 1 \implies k = \frac{6}{5}$

**ii. Find $P(X \le 1/2, Y \ge 1/2)$:**
$$P\left(X \le \frac{1}{2}, Y \ge \frac{1}{2}\right) = \int_{1/2}^{1} \int_{0}^{1/2} \frac{6}{5}(x^2 + 2xy) \, dx \, dy$$
Inner integral (w.r.t $x$):
$$\int_{0}^{1/2} (x^2 + 2xy) \, dx = \left[ \frac{x^3}{3} + x^2y \right]_{0}^{1/2} = \frac{1}{24} + \frac{1}{4}y$$
Outer integral (w.r.t $y$):
$$\frac{6}{5} \int_{1/2}^{1} \left( \frac{1}{24} + \frac{y}{4} \right) \, dy = \frac{6}{5} \left[ \frac{y}{24} + \frac{y^2}{8} \right]_{1/2}^{1} = \frac{6}{5} \left[ \left(\frac{1}{24} + \frac{1}{8}\right) - \left(\frac{1}{48} + \frac{1}{32}\right) \right]$$
$$= \frac{6}{5} \left[ \frac{4}{24} - \frac{5}{96} \right] = \frac{6}{5} \left[ \frac{16}{96} - \frac{5}{96} \right] = \frac{6}{5} \times \frac{11}{96} = \frac{11}{80}$$

**iii. What is the probability of the event $P(X \le Y)$:**
$$P(X \le Y) = \int_{0}^{1} \int_{0}^{y} \frac{6}{5}(x^2 + 2xy) \, dx \, dy$$
Inner integral (w.r.t $x$):
$$\int_{0}^{y} (x^2 + 2xy) \, dx = \left[ \frac{x^3}{3} + x^2y \right]_{0}^{y} = \frac{y^3}{3} + y^3 = \frac{4y^3}{3}$$
Outer integral (w.r.t $y$):
$$\int_{0}^{1} \frac{6}{5} \left( \frac{4y^3}{3} \right) \, dy = \frac{8}{5} \int_{0}^{1} y^3 \, dy = \frac{8}{5} \left[ \frac{y^4}{4} \right]_{0}^{1} = \frac{8}{5} \left( \frac{1}{4} \right) = \frac{2}{5}$$

---

### (c) Discrete Joint Distribution Independence
**Given:** $f(x,y) = \frac{1}{9}(x+y)$; for $x=0,1$ and $y=0,1,2$.

**i. Determine the marginal probability functions of X and Y:**
**Marginal of X, $g(x)$:**
$$g(x) = \sum_{y=0}^{2} \frac{1}{9}(x+y) = \frac{1}{9} [ (x+0) + (x+1) + (x+2) ] = \frac{1}{9}(3x+3) = \frac{x+1}{3}$$
(for $x = 0, 1$)

**Marginal of Y, $h(y)$:**
$$h(y) = \sum_{x=0}^{1} \frac{1}{9}(x+y) = \frac{1}{9} [ (0+y) + (1+y) ] = \frac{2y+1}{9}$$
(for $y = 0, 1, 2$)

**ii. Are X and Y independent?**
Two variables are independent if $f(x,y) = g(x) \cdot h(y)$ for all $x,y$.
$$g(x) \cdot h(y) = \left(\frac{x+1}{3}\right) \cdot \left(\frac{2y+1}{9}\right) = \frac{(x+1)(2y+1)}{27}$$
Since $\frac{(x+1)(2y+1)}{27} \neq \frac{x+y}{9}$, the condition fails. 
*(Example: at $x=0, y=0$, $f(0,0)=0$, but $g(0)h(0) = (1/3)(1/9) = 1/27$.)*
**Conclusion:** $X$ and $Y$ are **NOT** independent.