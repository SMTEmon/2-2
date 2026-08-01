---
tags:
  - math
  - probability
  - statistics
  - binomial
  - poisson
  - final
course: Math 2-2
lecture: Lecture 8 - Poisson & Binomial Distribution
date: 2026-07-31
---

# Lecture 8: Poisson Distribution & Binomial Distribution

> [!abstract] Executive Summary & Study Guide
> This comprehensive guide covers **Discrete Probability Distributions**, specifically focusing on the **Binomial Distribution** and the **Poisson Distribution**.
> 
> You will learn:
> - The core assumptions of **Bernoulli trials** and **Poisson processes**.
> - Complete step-by-step mathematical derivations for **Mean**, **Variance**, and **Moment Generating Functions (MGF)**.
> - The **Binomial Recursion Formula** for rapid probability computation.
> - **Poisson as a limiting case** of the Binomial distribution ($n \to \infty, p \to 0$).
> - Important diagnostic pitfalls (e.g., why $Var > Mean$ is impossible for Binomial).
> - Fully solved textbook problems and self-test practice exercises.

---

## 1. Binomial Distribution

### 1.1 Bernoulli Trials & Experiment Assumptions
A **Binomial Experiment** consists of a series of repeated random trials called **Bernoulli Trials**. For an experiment to be Binomial, it must satisfy **4 strict conditions**:

1. **Fixed Number of Trials ($n$):** The experiment consists of $n$ identical and repeated trials.
2. **Two Mutually Exclusive Outcomes:** Each trial results in one of two possible outcomes:
   - **Success ($S$)** with probability $p$
   - **Failure ($F$)** with probability $q = 1 - p$
3. **Constant Probability ($p$):** The probability of success $p$ remains constant from trial to trial.
4. **Independence:** All $n$ trials are independent of each other (the outcome of one trial does not affect any other).

---

### 1.2 Probability Mass Function (PMF)
If $X$ is a discrete random variable representing the total number of **successes** in $n$ independent Bernoulli trials, then $X \sim B(n, p)$.

The probability of obtaining exactly $x$ successes is given by:

$$b(x; n, p) = P(X = x) = \binom{n}{x} p^x q^{n-x} = \frac{n!}{x!(n-x)!} p^x (1-p)^{n-x}, \quad x = 0, 1, 2, \dots, n$$

> [!info] Parameter Breakdown
> - $n$ = total number of trials
> - $x$ = number of successes ($0 \le x \le n$)
> - $p$ = probability of success in a single trial
> - $q = 1 - p$ = probability of failure in a single trial

---

### 1.3 Mathematical Derivation of Mean and Variance

#### A. Derivation of Mean $E[X]$
By definition of expected value for a discrete random variable:

$$E[X] = \sum_{x=0}^{n} x \cdot P(X = x) = \sum_{x=0}^{n} x \binom{n}{x} p^x q^{n-x}$$

Since the term for $x = 0$ equals 0:

$$E[X] = \sum_{x=1}^{n} x \cdot \frac{n!}{x!(n-x)!} p^x q^{n-x}$$

Notice that $x / x! = 1 / (x-1)!$ and $n! = n \cdot (n-1)!$:

$$E[X] = n \cdot p \sum_{x=1}^{n} \frac{(n-1)!}{(x-1)!((n-1)-(x-1))!} p^{x-1} q^{(n-1)-(x-1)}$$

Let $y = x - 1$ and $m = n - 1$. As $x$ ranges from $1$ to $n$, $y$ ranges from $0$ to $m$:

$$E[X] = np \sum_{y=0}^{m} \binom{m}{y} p^y q^{m-y}$$

By the Binomial Theorem, $\sum_{y=0}^{m} \binom{m}{y} p^y q^{m-y} = (p + q)^m = 1^m = 1$.

$$\therefore E[X] = np$$

---

#### B. Derivation of Variance $V[X]$
Variance is defined as $V[X] = E[X^2] - (E[X])^2$.
To find $E[X^2]$, we use the identity $X^2 = X(X-1) + X$:

$$E[X^2] = E[X(X-1)] + E[X]$$

Let's compute $E[X(X-1)]$:

$$E[X(X-1)] = \sum_{x=0}^{n} x(x-1) \binom{n}{x} p^x q^{n-x} = \sum_{x=2}^{n} x(x-1) \frac{n!}{x!(n-x)!} p^x q^{n-x}$$

Simplifying $x(x-1) / x! = 1 / (x-2)!$ and factoring out $n(n-1)p^2$:

$$E[X(X-1)] = n(n-1)p^2 \sum_{x=2}^{n} \frac{(n-2)!}{(x-2)!((n-2)-(x-2))!} p^{x-2} q^{(n-2)-(x-2)}$$

Letting $k = x - 2$ and $N = n - 2$:

$$E[X(X-1)] = n(n-1)p^2 \sum_{k=0}^{N} \binom{N}{k} p^k q^{N-k} = n(n-1)p^2 (p+q)^N = n(n-1)p^2$$

Now substitute back into $E[X^2]$:

$$E[X^2] = n(n-1)p^2 + np = n^2 p^2 - np^2 + np$$

Now compute Variance $V[X]$:

$$V[X] = E[X^2] - (E[X])^2 = (n^2 p^2 - np^2 + np) - (np)^2 = np - np^2 = np(1-p) = npq$$

$$\therefore V[X] = npq \quad \text{and Standard Deviation } \sigma = \sqrt{npq}$$

---

### 1.4 Moment Generating Function (MGF)
The Moment Generating Function $M_X(t)$ is given by:

$$M_X(t) = E[e^{tX}] = \sum_{x=0}^{n} e^{tx} \binom{n}{x} p^x q^{n-x} = \sum_{x=0}^{n} \binom{n}{x} (p e^t)^x q^{n-x}$$

Applying the Binomial Theorem:

$$M_X(t) = (q + p e^t)^n$$

---

### 1.5 Binomial Recursion Formula
When evaluating probabilities for successive values of $x$, computing factorials repeatedly is inefficient. We use the **Recursion Relation**:

$$\frac{b(x+1; n, p)}{b(x; n, p)} = \frac{\binom{n}{x+1} p^{x+1} q^{n-x-1}}{\binom{n}{x} p^x q^{n-x}} = \frac{\frac{n!}{(x+1)!(n-x-1)!}}{\frac{n!}{x!(n-x)!}} \cdot \frac{p}{q} = \frac{n-x}{x+1} \cdot \frac{p}{q}$$

$$\implies b(x+1; n, p) = \left( \frac{n-x}{x+1} \right) \left( \frac{p}{q} \right) b(x; n, p)$$

> [!tip] Workflow
> 1. Calculate the initial term $b(0; n, p) = q^n$.
> 2. Iteratively calculate $b(1), b(2), \dots$ using the ratio $\left( \frac{n-x}{x+1} \right) \left( \frac{p}{q} \right)$.

---

### 1.6 Critical Parameter Constraint

> [!error] Critical Exam Pitfall: Binomial Variance vs Mean
> For any valid Binomial distribution, since $0 < q < 1$:
> 
> $$V[X] = npq = (np)q < np = E[X]$$
> 
> **Rule:** In a Binomial distribution, **Variance MUST be strictly less than Mean** ($V[X] < E[X]$).
> - If an exam question gives a distribution with $Mean = 5$ and $SD = 3 \implies Var = 9$, it is **IMPOSSIBLE** for it to be a Binomial distribution because $Var (9) > Mean (5)$.

---

## 2. Poisson Distribution

### 2.1 Concept & Real-World Context
The **Poisson Distribution** (named after French mathematician Siméon Denis Poisson) models the number of times a rare event occurs within a **fixed interval of time or region of space**.

#### Common Examples:
- Number of telephone calls received at a switchboard per minute.
- Number of defective parts per batch produced by a factory.
- Number of typos/errors per page in a textbook.
- Number of traffic accidents at an intersection per week.
- Number of gas pipeline leaks per 100 km.

---

### 2.2 Characteristics of a Poisson Process
For a process to be classified as a **Poisson Process**, it must satisfy three conditions:

1. **Independence:** The number of occurrences in one time/space interval is independent of occurrences in any other disjoint interval.
2. **Proportionality:** The probability of a single occurrence in a very short interval $\Delta t$ is proportional to the length of the interval ($\approx \mu \Delta t$) and does not depend on past occurrences outside the interval.
3. **Non-simultaneity:** The probability of two or more occurrences in an extremely small interval is negligible ($\approx 0$).

---

### 2.3 Poisson as a Limiting Case of Binomial
The Poisson distribution is a special limiting case of the Binomial distribution $B(n, p)$ when:
1. $n \to \infty$ (number of trials is extremely large)
2. $p \to 0$ (probability of success on a single trial is extremely small)
3. $\mu = np$ remains constant (average rate of occurrence is a fixed small/moderate positive number).

---

### 2.4 PMF & Total Probability Proof

#### Probability Mass Function
If $X$ is a Poisson random variable with mean occurrence rate $\mu > 0$, then:

$$f(x, \mu) = P(X = x) = \frac{e^{-\mu} \mu^x}{x!}, \quad x = 0, 1, 2, 3, \dots$$

where $e \approx 2.71828$.

#### Proof that Total Probability = 1
To be a valid probability distribution, $\sum_{x=0}^{\infty} f(x, \mu) = 1$:

$$\sum_{x=0}^{\infty} \frac{e^{-\mu} \mu^x}{x!} = e^{-\mu} \sum_{x=0}^{\infty} \frac{\mu^x}{x!}$$

Recall from Taylor series expansion that $e^\mu = \sum_{x=0}^{\infty} \frac{\mu^x}{x!} = 1 + \mu + \frac{\mu^2}{2!} + \frac{\mu^3}{3!} + \dots$:

$$\sum_{x=0}^{\infty} f(x, \mu) = e^{-\mu} \cdot e^\mu = e^0 = 1 \quad \text{(Proved)}$$

---

### 2.5 Derivation of Mean and Variance

#### A. Derivation of Mean $E[X]$
$$E[X] = \sum_{x=0}^{\infty} x \frac{e^{-\mu} \mu^x}{x!} = \sum_{x=1}^{\infty} x \frac{e^{-\mu} \mu^x}{x!}$$

Since $x / x! = 1 / (x-1)!$:

$$E[X] = \sum_{x=1}^{\infty} \frac{e^{-\mu} \mu^x}{(x-1)!} = \mu e^{-\mu} \sum_{x=1}^{\infty} \frac{\mu^{x-1}}{(x-1)!}$$

Let $y = x - 1$. As $x$ goes from 1 to $\infty$, $y$ goes from 0 to $\infty$:

$$E[X] = \mu e^{-\mu} \sum_{y=0}^{\infty} \frac{\mu^y}{y!} = \mu e^{-\mu} (e^\mu) = \mu$$

$$\therefore E[X] = \mu$$

---

#### B. Derivation of Variance $V[X]$
First find $E[X(X-1)]$:

$$E[X(X-1)] = \sum_{x=0}^{\infty} x(x-1) \frac{e^{-\mu} \mu^x}{x!} = \sum_{x=2}^{\infty} \frac{e^{-\mu} \mu^x}{(x-2)!} = \mu^2 e^{-\mu} \sum_{x=2}^{\infty} \frac{\mu^{x-2}}{(x-2)!}$$

Let $k = x - 2$:

$$E[X(X-1)] = \mu^2 e^{-\mu} \sum_{k=0}^{\infty} \frac{\mu^k}{k!} = \mu^2 e^{-\mu} (e^\mu) = \mu^2$$

Now find $E[X^2]$:

$$E[X^2] = E[X(X-1)] + E[X] = \mu^2 + \mu$$

Finally compute Variance $V[X]$:

$$V[X] = E[X^2] - (E[X])^2 = (\mu^2 + \mu) - \mu^2 = \mu$$

$$\therefore V[X] = \mu \quad \text{and Standard Deviation } \sigma = \sqrt{\mu}$$

> [!note] Hallmark Feature of Poisson Distribution
> For a Poisson distribution, **Mean = Variance = $\mu$**.

---

## 3. Step-by-Step Worked Examples (From Lecture)

> [!example]- Worked Example 1: Switchboard Call Rate (Poisson)
> **Problem Statement:**
> Telephone calls arrive at a switchboard at a mean rate of 0.5 calls per minute. Calculate the probability that exactly 2 calls arrive during a 5-minute period.
> 
> **Solution:**
> 1. Identify the time interval and mean rate $\mu$:
>    - Given rate = 0.5 calls/min.
>    - Specified interval = 5 minutes.
>    - Average number of calls in 5 minutes $\mu = 0.5 \times 5 = 2.5$.
> 2. Identify target $x$:
>    - We want $P(X = 2)$.
> 3. Apply Poisson PMF:
>    
>    $$P(X = 2) = f(2; 2.5) = \frac{e^{-2.5} (2.5)^2}{2!}$$
> 
>    - $e^{-2.5} \approx 0.082085$
>    - $(2.5)^2 = 6.25$
>    - $2! = 2$
>    
>    $$P(X = 2) = \frac{0.082085 \times 6.25}{2} = \frac{0.51303}{2} \approx 0.2565 \text{ (or } 25.65\% \text{)}$$

> [!example]- Worked Example 2: Cumulative Poisson Probabilities
> **Problem Statement:**
> The average number of calls received by a telephone operator during a 10-minute interval is 3. Find the probability that tomorrow during the same interval the operator receives:
> 1. No call ($P(X = 0)$)
> 2. Exactly one call ($P(X = 1)$)
> 3. At least two calls ($P(X \ge 2)$)
> 
> **Solution:**
> Here, $\mu = 3$.
> 
> 1. **No call ($x = 0$):**
>    
>    $$P(X = 0) = \frac{e^{-3} 3^0}{0!} = e^{-3} \approx 0.0498$$
> 
> 2. **Exactly 1 call ($x = 1$):**
>    
>    $$P(X = 1) = \frac{e^{-3} 3^1}{1!} = 3 e^{-3} = 3 \times 0.0498 = 0.1494$$
> 
> 3. **At least 2 calls ($X \ge 2$):**
>    Using the complement rule:
>    
>    $$P(X \ge 2) = 1 - P(X < 2) = 1 - [P(X = 0) + P(X = 1)]$$
>    $$P(X \ge 2) = 1 - [0.0498 + 0.1494] = 1 - 0.1992 = 0.8008 \text{ (or } 80.08\% \text{)}$$

> [!example]- Worked Example 3: Coin Tossing & Binomial Recursion
> **Problem Statement:**
> Five coins are tossed together and the experiment is repeated 200 times. The observed distribution of obtaining heads is:
> 
> | No. of Heads ($x$) | 0 | 1 | 2 | 3 | 4 | 5 | Total |
> | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | Frequency ($f$) | 12 | 56 | 74 | 39 | 18 | 1 | 200 |
> 
> Assuming a Binomial distribution, calculate the expected frequencies using the Binomial Recursion Formula.
> 
> **Solution:**
> 1. **Find Sample Mean $\bar{x}$:**
>    
>    $$\bar{x} = \frac{\sum f x}{N} = \frac{(0 \times 12) + (1 \times 56) + (2 \times 74) + (3 \times 39) + (4 \times 18) + (5 \times 1)}{200}$$
>    $$\bar{x} = \frac{0 + 56 + 148 + 117 + 72 + 5}{200} = \frac{398}{200} = 1.99$$
> 
> 2. **Estimate $p$:**
>    Since $\bar{x} = np$ with $n = 5$:
>    
>    $$5p = 1.99 \implies p = \frac{1.99}{5} = 0.398 \implies q = 1 - 0.398 = 0.602$$
> 
> 3. **Compute Base Probability $b(0; 5, 0.398)$:**
>    
>    $$b(0; 5, 0.398) = q^5 = (0.602)^5 \approx 0.07907$$
> 
> 4. **Apply Recursion Formula $b(x+1) = \left(\frac{5-x}{x+1}\right)\left(\frac{0.398}{0.602}\right) b(x)$:**
>    - $b(1) = \left(\frac{5}{1}\right) (0.66113) (0.07907) = 0.26136$
>    - $b(2) = \left(\frac{4}{2}\right) (0.66113) (0.26136) = 0.34559$
>    - $b(3) = \left(\frac{3}{3}\right) (0.66113) (0.34559) = 0.22847$
>    - $b(4) = \left(\frac{2}{4}\right) (0.66113) (0.22847) = 0.07553$
>    - $b(5) = \left(\frac{1}{5}\right) (0.66113) (0.07553) = 0.00998$
> 
> 5. **Expected Frequencies $E(x) = N \times b(x) = 200 \times b(x)$:**
> 
> | Heads ($x$) | 0 | 1 | 2 | 3 | 4 | 5 | Total |
> | :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Observed $f$** | 12 | 56 | 74 | 39 | 18 | 1 | **200** |
> | **Expected $E(x)$** | **16** | **52** | **69** | **46** | **15** | **2** | **200** |

> [!example]- Worked Example 4: Parameter Validity Test
> **Problem Statement:**
> Is it possible to have a Binomial distribution with Mean = 5 and Standard Deviation = 3?
> 
> **Solution:**
> 1. Given parameters:
>    - $Mean = np = 5$
>    - $SD = \sqrt{npq} = 3 \implies Variance = npq = 3^2 = 9$
> 2. Ratio of Variance to Mean:
>    
>    $$\frac{npq}{np} = q \implies q = \frac{9}{5} = 1.8$$
> 
> 3. **Conclusion:**
>    Since probability $q$ cannot exceed 1 ($0 \le q \le 1$), $q = 1.8$ is mathematically **impossible**.
>    Therefore, **no Binomial distribution can have Mean = 5 and Standard Deviation = 3**. (Variance cannot exceed Mean in a Binomial distribution).

> [!example]- Worked Example 5: Defective TV Sets Marketing Batch
> **Problem Statement:**
> 20% of manufactured TV sets are defective. If 4 TVs are placed in a marketing box, find the number of boxes containing:
> 1. Exactly 1 defective TV
> 2. Exactly 2 defective TVs
> 3. At most 2 defective TVs
> 
> in a total shipment of $N = 2000$ boxes.
> 
> **Solution:**
> Parameters: $n = 4$, $p = 0.20$, $q = 0.80$, $N = 2000$.
> 
> 1. **Exactly 1 defective TV ($x = 1$):**
>    
>    $$P(X = 1) = \binom{4}{1} (0.2)^1 (0.8)^3 = 4 \times 0.2 \times 0.512 = 0.4096$$
>    
>    Number of boxes = $N \times P(X=1) = 2000 \times 0.4096 = \mathbf{819 \text{ boxes}}$.
> 
> 2. **Exactly 2 defective TVs ($x = 2$):**
>    
>    $$P(X = 2) = \binom{4}{2} (0.2)^2 (0.8)^2 = 6 \times 0.04 \times 0.64 = 0.1536$$
>    
>    Number of boxes = $N \times P(X=2) = 2000 \times 0.1536 = \mathbf{307 \text{ boxes}}$.
> 
> 3. **At most 2 defective TVs ($x \le 2$):**
>    - $P(X = 0) = \binom{4}{0} (0.2)^0 (0.8)^4 = 0.4096$
>    - $P(X \le 2) = P(X=0) + P(X=1) + P(X=2) = 0.4096 + 0.4096 + 0.1536 = 0.9728$
>    
>    Number of boxes = $2000 \times 0.9728 = \mathbf{1946 \text{ boxes}}$.

---

## 4. Comprehensive Self-Test & Practice Problems

Try solving these problems on your own first! Click the collapsed block to check your step-by-step solution.

> [!note] Practice Problem 1: Fiber Optic Flaws (Poisson)
> **Problem:**
> An optical fiber cable has an average of 1.2 flaws per kilometer.
> 1. Find the probability of finding exactly 2 flaws in a 1 km section.
> 2. Find the probability of finding at least 1 flaw in a 2 km section.
> 
> > [!example]- Solution
> > **Part 1:**
> > - $\mu = 1.2$ for $1\text{ km}$. Target $x = 2$.
> > - $P(X = 2) = \frac{e^{-1.2} (1.2)^2}{2!} = \frac{0.30119 \times 1.44}{2} \approx \mathbf{0.2169 \text{ (or } 21.69\% \text{)}}$
> > 
> > **Part 2:**
> > - For a $2\text{ km}$ section, new mean $\mu = 1.2 \times 2 = 2.4$.
> > - $P(X \ge 1) = 1 - P(X = 0) = 1 - \frac{e^{-2.4} (2.4)^0}{0!} = 1 - e^{-2.4} \approx 1 - 0.0907 = \mathbf{0.9093 \text{ (or } 90.93\% \text{)}}$

> [!note] Practice Problem 2: Parameter Verification (Binomial)
> **Problem:**
> A quality manager claims that for a batch process, the mean number of defective items is 12 with a variance of 4. Is this claim mathematically valid for a Binomial distribution? If so, find $n, p, q$.
> 
> > [!example]- Solution
> > **Step 1: Test validity:**
> > - $Mean = np = 12$
> > - $Variance = npq = 4$
> > - $q = \frac{npq}{np} = \frac{4}{12} = \frac{1}{3} \approx 0.333$
> > - Since $q = 1/3 < 1$, $V[X] < E[X]$, which **IS VALID** for a Binomial distribution!
> > 
> > **Step 2: Find $p$ and $n$:**
> > - $p = 1 - q = 1 - \frac{1}{3} = \frac{2}{3}$
> > - $np = 12 \implies n \left(\frac{2}{3}\right) = 12 \implies n = 12 \times \frac{3}{2} = \mathbf{18}$
> > 
> > **Result:** Valid! $n = 18$, $p = 2/3$, $q = 1/3$.

> [!note] Practice Problem 3: Train Passengers Reading Newspaper
> **Problem:**
> 70% of passengers on a train trip buy a daily newspaper. In a compartment of 8 passengers:
> 1. Find the probability that all 8 bought a newspaper.
> 2. Find the probability that none bought a newspaper.
> 3. What is the most likely number of passengers in the compartment who bought a newspaper?
> 
> > [!example]- Solution
> > Parameters: $n = 8$, $p = 0.7$, $q = 0.3$.
> > 
> > **1. All 8 bought ($x = 8$):**
$$P(X = 0) = \\binom{8}{0} (0.7)^0 (0.3)^8 = (0.3)^8 \\approx \\mathbf{0.000066}$$\n
> > 
> > **2. None bought ($x = 0$):**
$$P(X = 0) = \\binom{8}{0} (0.7)^0 (0.3)^8 = (0.3)^8 \\approx \\mathbf{0.000066}$$\n
> > 
> > **3. Most likely number:**
> > Expected value $E[X] = np = 8 \times 0.7 = 5.6$.
> > Compare probabilities around 5.6 ($x = 5$ and $x = 6$):
> > - $P(X = 5) = \binom{8}{5} (0.7)^5 (0.3)^3 = 56 \times 0.16807 \times 0.027 = 0.2541$
> > - $P(X = 6) = \binom{8}{6} (0.7)^6 (0.3)^2 = 28 \times 0.11765 \times 0.09 = 0.2965$
> > 
> > Since $P(X = 6) > P(X = 5)$, the most likely number of passengers is **6**.

---

## 5. Summary Cheat Sheet

| Feature | Binomial Distribution $B(n, p)$ | Poisson Distribution $P(\mu)$ |
| :--- | :--- | :--- |
| **Nature of Variable** | Discrete ($0 \le x \le n$) | Discrete ($0 \le x < \infty$) |
| **Parameters** | Two: $n$ (trials), $p$ (prob of success) | One: $\mu$ (mean rate) |
| **PMF $P(X = x)$** | $\binom{n}{x} p^x (1-p)^{n-x}$ | $\frac{e^{-\mu} \mu^x}{x!}$ |
| **Mean $E[X]$** | $np$ | $\mu$ |
| **Variance $V[X]$** | $npq = np(1-p)$ | $\mu$ |
| **Mean vs Variance** | **Variance < Mean** ($npq < np$) | **Variance = Mean** ($V[X] = \mu$) |
| **MGF $M_X(t)$** | $(q + p e^t)^n$ | $e^{\mu(e^t - 1)}$ |
| **Primary Use Case** | Fixed number of independent pass/fail trials | Rate of rare events in continuous time or space |
