---
tags:
  - math
  - probability
  - statistics
  - final
  - moc
  - index
course: Math 4441
topic: Final Exam Master Index & Formula Sheet
---

# Math 4441 Final Exam Master Index & Study Guide

> [!abstract] Course Overview
> Welcome to the comprehensive study suite for **Math 4441 Final Examination**. These notes cover all discrete and continuous probability distributions, theoretical frequency fitting, bivariate random variable analysis (covariance & correlation), normal distribution theory with full integration proofs, Chebyshev's inequality, stochastic process classifications, and hypothesis testing (strictly bounded to class notes and textbook pages 1–42).
> *(Note: Moment Generating Functions and Book pages 43–75 are strictly excluded).*

---

## 📚 Navigation & Topic Modules

| Module # | Topic Note | Core Content & Solved Examples |
|:---:|---|---|
| **1** | [[1. Binomial Distribution & Frequency Fitting]] | Bernoulli trials, PMF, Mean ($np$) & Variance ($npq$) proofs, Binomial Recursion relation, theoretical frequency fitting ($n=5, N=200$), mice inoculation & TV sets problems, diagnostic rule ($Var < Mean$). |
| **2** | [[2. Poisson Distribution & Bernoulli Distribution]] | Bernoulli PMF/Mean/Variance, Poisson process real-world examples, PMF verification proof ($\sum f = 1$), Mean ($\lambda$) & Variance ($\lambda$) proofs, Poisson Recurrence relation, frequency fitting, operator & switchboard numerical problems. |
| **3** | [[3. Covariance & Correlation]] | Operational covariance formula, independence proof ($\text{Cov}(X,Y)=0$), 4 algebraic covariance properties with proofs, Variance of Sum theorem, continuous joint PDF integration, Pearson's $\rho$ bounds proof ($-1 \le \rho \le 1$), scale invariance proof, discrete bivariate table problem. |
| **4** | [[4. Normal Distribution & Chebyshev Inequality]] | Empirical rule vs Chebyshev inequality, complete Chebyshev integral proof, $630x^4(1-x)^4$ problem, 7 Normal curve properties, inflection points proof ($x = \mu \pm \sigma$), Normal PDF integration proofs (Area = 1, Mean = $\mu$, Var = $\sigma^2$), $Z$-score, CDF symmetry, central area & variance finding problems. |
| **5** | [[7. Uniform and Exponential Distributions]] | Discrete Uniform, Continuous Rectangular Uniform (PDF, CDF, Mean $\frac{a+b}{2}$, Variance $\frac{(b-a)^2}{12}$), Continuous Exponential (PDF, CDF, Mean $\frac{1}{\lambda}$, Variance $\frac{1}{\lambda^2}$), **Memoryless Property Proof**, waiting time & component lifespan problems. |
| **6** | [[8. Stochastic Processes & Classifications]] | Formal definition $\{X(t), t \in T\}$, Parameter space $T$ (discrete/continuous), State space $S$ (discrete/continuous), 4 Fundamental Classifications with physical & engineering examples, connection to Bernoulli/Poisson processes. |
| **7** | [[9. Hypothesis Testing - Class Notes & Book Guide]] | Courtroom analogy, $H_0$ vs $H_1$, Type I ($\alpha$) & Type II ($\beta$) errors, Power ($1-\beta$), Critical/Acceptance regions, $p$-value, 6-Step Testing Procedure, Single Mean ($Z$-test & $t$-test), Difference of Means ($Z$-test & pooled $t$-test), Paired $t$-test ($d_i = x_i - y_i$), exact classroom $Z=2.03$ problem, textbook Table 16.13 problem. |
| **8** | [[5. Practice Problems & Exam Exercises]] | Comprehensive exam-level practice problems covering Binomial, Poisson, Covariance, Portfolio Variance, Pearson Correlation Coefficient, Chebyshev's Inequality, and Normal Distribution Z-table reading. |
| **9** | [[6. Classroom Lecture Problems & Handwritten Proofs]] | Exact handwritten lecture math from scans: Joint PDF $f(x,y)=\frac{6}{5}(x^2+2xy)$ complete covariance & correlation ($\rho = -0.055$), classroom Z-table central 50% solution, and general Variance of Sum proof. |
| **10** | [[10. Master Exam Problem Bank & Solutions]] | **Master Drill Bank:** All 23 syllabus problem archetypes with hidden foldable step-by-step solutions (Binomial, Poisson, Uniform, Exponential, Chebyshev, Normal, Covariance/Correlation, Stochastic, and Hypothesis Testing). |
| **Assignment** | [[Assignment - Hypothesis Testing (Sections 16.6 & 16.7)]] | **Course Assignment Complete Guide:** Complete theory of Cases 1–5 (single mean) and Cases A–C (two means) + fully worked step-by-step solutions for all 15 examples in 16.6 (16.6.1–16.6.15) and all 8 examples in 16.7 (16.7.1–16.7.8). |

---

## ⚡ Master Formula Cheat Sheet

### 1. Discrete Distributions

$$\begin{array}{|l|c|c|c|}
\hline
\textbf{Distribution} & \textbf{Probability Mass Function } P(X = x) & \textbf{Mean } E[X] & \textbf{Variance } V[X] \\ \hline
\text{Bernoulli } B(1, p) & f(x; p) = p^x (1-p)^{1-x}, \quad x \in \{0, 1\} & p & pq = p(1-p) \\ \hline
\text{Binomial } B(n, p) & \binom{n}{x} p^x q^{n-x}, \quad x = 0, 1, \dots, n & np & npq \\ \hline
\text{Poisson } P(\lambda) & \frac{e^{-\lambda} \lambda^x}{x!}, \quad x = 0, 1, 2, \dots, \infty & \lambda & \lambda \\ \hline
\text{Discrete Uniform } U(k) & \frac{1}{k}, \quad x \in \{x_1, \dots, x_k\} & \frac{\sum x_i}{k} & \frac{\sum x_i^2}{k} - \mu^2 \\ \hline
\end{array}$$

- **Binomial Recursion Relation:**
  $$b(x+1; n, p) = \left( \frac{n-x}{x+1} \right) \left( \frac{p}{q} \right) b(x; n, p)$$
- **Poisson Recursion Relation:**
  $$f(x+1; \lambda) = \left( \frac{\lambda}{x+1} \right) f(x; \lambda)$$
- **Theoretical Expected Frequency Formula:**
  $$E_x = N \cdot P(X = x)$$

---

### 2. Continuous Distributions & Inequalities

$$\begin{array}{|l|c|c|c|}
\hline
\textbf{Distribution} & \textbf{Probability Density Function } f(x) & \textbf{Mean } E[X] & \textbf{Variance } V[X] \\ \hline
\text{Continuous Uniform } U(a, b) & \frac{1}{b - a}, \quad a \le x \le b & \frac{a + b}{2} & \frac{(b - a)^2}{12} \\ \hline
\text{Exponential } \text{Exp}(\lambda) & \lambda e^{-\lambda x}, \quad x \ge 0 & \frac{1}{\lambda} & \frac{1}{\lambda^2} \\ \hline
\text{Normal } N(\mu, \sigma^2) & \frac{1}{\sigma \sqrt{2\pi}} \exp\left[ -\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^2 \right] & \mu & \sigma^2 \\ \hline
\text{Standard Normal } N(0, 1) & \frac{1}{\sqrt{2\pi}} e^{-\frac{1}{2} z^2}, \quad z = \frac{x-\mu}{\sigma} & 0 & 1 \\ \hline
\end{array}$$

- **Chebyshev's Inequality (Any Distribution):**
  $$P(|X - \mu| < k\sigma) \ge 1 - \frac{1}{k^2} \quad \text{or} \quad P(|X - \mu| \ge k\sigma) \le \frac{1}{k^2} \quad (k > 1)$$
- **Exponential Memoryless Property:**
  $$P(X > s + t \mid X > s) = P(X > t) = e^{-\lambda t}$$
- **Normal Curve Points of Inflection:**
  $$x = \mu - \sigma \quad \text{and} \quad x = \mu + \sigma$$

---

### 3. Covariance & Correlation

- **Standard Definition:**
  $$\text{Cov}(X, Y) = E[(X - \mu_X)(Y - \mu_Y)]$$
- **Operational Computation Formula:**
  $$\text{Cov}(X, Y) = E[XY] - \mu_X \mu_Y$$
- **Variance of Sum Theorem:**
  $$\text{Var}(X + Y) = \text{Var}(X) + \text{Var}(Y) + 2 \text{Cov}(X, Y)$$
  $$\text{Var}\left(\sum_{i=1}^{n} X_i\right) = \sum_{i=1}^{n} \text{Var}(X_i) + 2 \sum_{1 \le i < j \le n} \text{Cov}(X_i, X_j)$$
- **Pearson's Correlation Coefficient:**
  $$\rho(X, Y) = \frac{\text{Cov}(X, Y)}{\sqrt{\text{Var}(X) \text{Var}(Y)}} = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y}, \quad -1 \le \rho(X, Y) \le 1$$

---

### 4. Hypothesis Testing Master Decision Formulas (Pages 1–42)

$$\begin{array}{|l|l|l|c|}
\hline
\textbf{Scenario} & \textbf{Sample Size / Conditions} & \textbf{Test Statistic Formula} & \textbf{Distribution / df} \\ \hline
\textbf{Single Mean } \mu & n \ge 30 \ (\sigma \text{ known or unknown}) & Z = \frac{\bar{X} - \mu_0}{\sigma / \sqrt{n}} \text{ (or } s / \sqrt{n}) & N(0, 1) \\ \hline
\textbf{Single Mean } \mu & n < 30 \ (\sigma \text{ unknown, normal pop}) & t = \frac{\bar{X} - \mu_0}{s / \sqrt{n}} & t \text{ with } \nu = n - 1 \\ \hline
\textbf{Two Means } \mu_1 - \mu_2 & n_1, n_2 \ge 30 \ (\text{Independent}) & Z = \frac{\bar{X}_1 - \bar{X}_2}{\sqrt{\frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{n_2}}} & N(0, 1) \\ \hline
\textbf{Two Means } \mu_1 - \mu_2 & n_1, n_2 < 30 \ (\sigma_1^2 = \sigma_2^2 \text{ unknown}) & t = \frac{\bar{X}_1 - \bar{X}_2}{s_p \sqrt{\frac{1}{n_1} + \frac{1}{n_2}}} & t \text{ with } \nu = n_1 + n_2 - 2 \\ 
& \text{Pooled Variance: } s_p^2 = \frac{(n_1-1)s_1^2 + (n_2-1)s_2^2}{n_1+n_2-2} & & \\ \hline
\textbf{Paired Means } \mu_d & n \text{ pairs, dependent before-after} & t = \frac{\bar{d} - 0}{s_d / \sqrt{n}}, \quad d_i = X_{1i} - X_{2i} & t \text{ with } \nu = n - 1 \\ \hline
\end{array}$$

#### Standard Critical Values for $Z$-Statistic
- **Two-Tailed:** $\alpha = 0.10 \implies \pm 1.645$, $\alpha = 0.05 \implies \pm 1.96$, $\alpha = 0.01 \implies \pm 2.58$.
- **Right-Tailed:** $\alpha = 0.05 \implies +1.645$, $\alpha = 0.01 \implies +2.33$.
- **Left-Tailed:** $\alpha = 0.05 \implies -1.645$, $\alpha = 0.01 \implies -2.33$.

---

## 🎯 Exam Proof Checklist

> [!tip] High-Priority Proofs for Final Exam
> Make sure to practice writing these mathematical derivations by hand before the exam:
> 
> 1. [ ] **Binomial Distribution:** Proof of $E[X] = np$ and $V[X] = npq$ using combination expansions.
> 2. [ ] **Binomial Recursion Relation:** Derivation of $\frac{b(x+1)}{b(x)} = \frac{n-x}{x+1} \cdot \frac{p}{q}$.
> 3. [ ] **Poisson Distribution:** Proof of $\sum_{x=0}^{\infty} f(x; \lambda) = 1$ using Maclaurin series for $e^\lambda$.
> 4. [ ] **Poisson Distribution:** Proof of $E[X] = \lambda$ and $V[X] = \lambda$.
> 5. [ ] **Poisson Recursion Relation:** Derivation of $f(x+1) = \left(\frac{\lambda}{x+1}\right) f(x)$.
> 6. [ ] **Continuous Uniform:** Proof of Mean $E[X] = \frac{a+b}{2}$ and Variance $\text{Var}(X) = \frac{(b-a)^2}{12}$.
> 7. [ ] **Exponential Distribution:** Proof of Mean $E[X] = \frac{1}{\lambda}$ and Variance $\text{Var}(X) = \frac{1}{\lambda^2}$.
> 8. [ ] **Exponential Memoryless Property:** Proof that $P(X > s+t \mid X > s) = P(X > t) = e^{-\lambda t}$.
> 9. [ ] **Covariance:** Proof of $\text{Cov}(X,Y) = E[XY] - \mu_X \mu_Y$.
> 10. [ ] **Independence Theorem:** Proof that independent $X, Y \implies \text{Cov}(X,Y) = 0$.
> 11. [ ] **Covariance Properties:** Proof of $\text{Cov}(aX, bY) = ab \text{Cov}(X,Y)$ and $\text{Cov}(X+Y, Z) = \text{Cov}(X,Z) + \text{Cov}(Y,Z)$.
> 12. [ ] **Variance of Sum:** Proof of $\text{Var}(X+Y) = \text{Var}(X) + \text{Var}(Y) + 2\text{Cov}(X,Y)$.
> 13. [ ] **Correlation Bounds:** Proof that $-1 \le \rho(X, Y) \le 1$ using quadratic non-negativity $E[(X^* \pm Y^*)^2] \ge 0$.
> 14. [ ] **Chebyshev's Inequality:** Full continuous integral proof splitting into 3 regions and bounding tails.
> 15. [ ] **Normal Distribution:** Proof that $\int_{-\infty}^{\infty} f(x) dx = 1$ using Gamma function $\Gamma(1/2) = \sqrt{\pi}$.
> 16. [ ] **Normal Distribution:** Proof of $E[X] = \mu$ (odd function symmetry) and $V[X] = \sigma^2$ (Gamma function / integration by parts).
> 17. [ ] **Normal Points of Inflection:** Proof that $f''(x) = 0 \implies x = \mu \pm \sigma$.
