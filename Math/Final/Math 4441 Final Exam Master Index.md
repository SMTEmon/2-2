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
> Welcome to the comprehensive study suite for **Math 4441 Final Examination**. These notes cover all discrete and continuous probability distributions, theoretical frequency fitting, bivariate random variable analysis (covariance & correlation), normal distribution theory with full integration proofs, and Chebyshev's inequality.

---

## 📚 Navigation & Topic Modules

| Module # | Topic Note | Core Content & Solved Examples |
|:---:|---|---|
| **1** | [[1. Binomial Distribution & Frequency Fitting]] | Bernoulli trials, PMF, Mean ($np$) & Variance ($npq$) proofs, Binomial Recursion relation, theoretical frequency fitting step-by-step example ($n=5, N=200$), parameter diagnostic rule ($Var < Mean$). |
| **2** | [[2. Poisson Distribution & Bernoulli Distribution]] | Bernoulli PMF/Mean/Variance, Poisson process real-world examples, PMF verification proof ($\sum f = 1$), Mean ($\mu$) & Variance ($\mu$) proofs, operator & switchboard numerical problems. |
| **3** | [[3. Covariance & Correlation]] | Definition & operational covariance formula, independence proof ($\text{Cov}(X,Y)=0$), 4 algebraic covariance properties with proofs, Variance of Sum theorem, joint PDF integration, Pearson's $\rho$ properties & scatter plots. |
| **4** | [[4. Normal Distribution & Chebyshev Inequality]] | Empirical rule vs Chebyshev inequality, complete Chebyshev integral proof, $630x^4(1-x)^4$ numerical problem, Normal PDF integration proofs (Area = 1, Mean = $\mu$, Var = $\sigma^2$), $Z$-score, CDF symmetry, central area & variance finding problems. |
| **5** | [[5. Practice Problems & Exam Exercises]] | Comprehensive exam-level practice problems covering Binomial, Poisson, Covariance, Portfolio Variance, Pearson Correlation Coefficient, Chebyshev's Inequality, and Normal Distribution Z-table reading. |
| **6** | [[6. Classroom Lecture Problems & Handwritten Proofs]] | Exact handwritten lecture math from your scans: Joint PDF $f(x,y)=\frac{6}{5}(x^2+2xy)$ complete covariance & correlation ($\rho = -0.055$), classroom Z-table central 50% solution, and general Variance of Sum proof. |

---

## ⚡ Master Formula Cheat Sheet

### 1. Discrete Distributions

$$\begin{array}{|l|c|c|c|}
\hline
\textbf{Distribution} & \textbf{Probability Mass Function } P(X = x) & \textbf{Mean } E[X] & \textbf{Variance } V[X] \\ \hline
\text{Bernoulli } B(1, p) & f(x; p) = p^x (1-p)^{1-x}, \quad x \in \{0, 1\} & p & pq = p(1-p) \\ \hline
\text{Binomial } B(n, p) & \binom{n}{x} p^x q^{n-x}, \quad x = 0, 1, \dots, n & np & npq \\ \hline
\text{Poisson } P(\mu) & \frac{e^{-\mu} \mu^x}{x!}, \quad x = 0, 1, 2, \dots, \infty & \mu & \mu \\ \hline
\end{array}$$

- **Binomial Recursion Relation:**
  $$b(x+1; n, p) = \left( \frac{n-x}{x+1} \right) \left( \frac{p}{q} \right) b(x; n, p)$$
- **Expected Frequency Formula:**
  $$E_x = N \cdot b(x; n, p) \quad \text{where } N = \sum f_i, \quad p = \frac{\bar{x}}{n}$$

---

### 2. Covariance & Correlation

- **Standard Definition:**
  $$\text{Cov}(X, Y) = E[(X - \mu_X)(Y - \mu_Y)]$$
- **Operational Computation Formula:**
  $$\text{Cov}(X, Y) = E[XY] - \mu_X \mu_Y$$
- **Variance of Sum Theorem:**
  $$\text{Var}(X + Y) = \text{Var}(X) + \text{Var}(Y) + 2 \text{Cov}(X, Y)$$
  $$\text{Var}\left(\sum_{i=1}^{n} X_i\right) = \sum_{i=1}^{n} \text{Var}(X_i) + 2 \sum_{1 \le i < j \le n} \text{Cov}(X_i, X_j)$$
- **Pearson's Correlation Coefficient:**
  $$\rho(X, Y) = \frac{\text{Cov}(X, Y)}{\sqrt{\text{Var}(X) \text{Var}(Y)}} = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y}, \quad -1 \le \rho \le 1$$

---

### 3. Continuous Distributions & Inequalities

- **Chebyshev's Inequality:**
  $$P(|X - \mu| < k\sigma) \ge 1 - \frac{1}{k^2} \quad \text{or} \quad P(|X - \mu| \ge k\sigma) \le \frac{1}{k^2}$$
- **Normal PDF $N(\mu, \sigma^2)$:**
  $$f(x; \mu, \sigma^2) = \frac{1}{\sigma \sqrt{2\pi}} \exp\left[ -\frac{1}{2} \left( \frac{x - \mu}{\sigma} \right)^2 \right], \quad -\infty < x < \infty$$
- **Standard Normal Transformation ($Z$-score):**
  $$Z = \frac{X - \mu}{\sigma} \sim N(0, 1) \implies f(z) = \frac{1}{\sqrt{2\pi}} e^{-\frac{1}{2} z^2}$$
- **Standard Normal Symmetry:**
  $$\Phi(z) + \Phi(-z) = 1 \implies P(Z \ge -z) = \Phi(z)$$

---

## 🎯 Exam Proof Checklist

> [!tip] High-Priority Proofs for Final Exam
> Make sure to practice writing these mathematical derivations by hand before the exam:
> 
> 1. [ ] Binomial Distribution: Proof of $E[X] = np$ and $V[X] = npq$ using combination expansions.
> 2. [ ] Binomial Recursion Relation: Derivation of $\frac{b(x+1)}{b(x)} = \frac{n-x}{x+1} \cdot \frac{p}{q}$.
> 3. [ ] Poisson Distribution: Proof of $\sum_{x=0}^{\infty} f(x; \mu) = 1$ using Maclaurin series for $e^\mu$.
> 4. [ ] Poisson Distribution: Proof of $E[X] = \mu$ and $V[X] = \mu$.
> 5. [ ] Covariance: Proof of $\text{Cov}(X,Y) = E[XY] - \mu_X \mu_Y$.
> 6. [ ] Independence Theorem: Proof that independent $X, Y \implies \text{Cov}(X,Y) = 0$.
> 7. [ ] Covariance Properties: Proof of $\text{Cov}(aX, bY) = ab \text{Cov}(X,Y)$ and $\text{Cov}(X+Y, Z) = \text{Cov}(X,Z) + \text{Cov}(Y,Z)$.
> 8. [ ] Variance of Sum: Proof of $\text{Var}(X+Y) = \text{Var}(X) + \text{Var}(Y) + 2\text{Cov}(X,Y)$.
> 9. [ ] Chebyshev's Inequality: Full integral proof splitting into 3 regions and bounding tails.
> 10. [ ] Normal Distribution: Proof that $\int_{-\infty}^{\infty} f(x) dx = 1$ using Gamma function $\Gamma(1/2) = \sqrt{\pi}$.
> 11. [ ] Normal Distribution: Proof of $E[X] = \mu$ (odd function symmetry) and $V[X] = \sigma^2$ (integration by parts / Gamma function).
