***
## Mathematical Expectation

The mathematical expectation (or expected value) of a random variable is essentially the theoretical mean of its probability distribution. It represents the long-run average value of the variable if the experiment were repeated infinitely many times.

> [!definition] Expected Value (Mean), $\mu$ or $E(X)$
> If $X$ is a random variable having a probability function $f(x)$, then the expected value of $X$ is denoted with $E(X)$ or $\mu$, and is defined as:
> 
> **For a [[Discrete Random Variable]]:**
> $$E(X) = \sum_{x} x f(x)$$
> *(In other words, the expected value is a weighted average of the possible values that $X$ can take on, each value being weighted by its probability).*
> 
> **For a [[Continuous Random Variable]]:**
> $$E(X) = \int_{-\infty}^{\infty} x f(x) \, dx$$

### Expected Value of a Function of $X$
Let $X$ be a random variable with probability distribution $f(x)$. The expected value of a function $W(X)$ is defined as:
$$ E[W(X)] = \begin{cases} 
\sum_{x} w(x) f(x), & \text{if } X \text{ is discrete} \\
\int_{-\infty}^{\infty} w(x) f(x) \, dx, & \text{if } X \text{ is continuous}
\end{cases} $$

---

## Properties and Theorems of Expectation

> [!theorem] Theorem 1: Expectation of a Constant
> Let $X$ be a random variable with probability function $f(x)$ and $c$ be a constant. Then:
> $$E(c) = c$$
> 
> > [!abstract] Proof (Discrete Case)
> > By definition, $E[W(X)] = \sum W(x) f(x)$. Letting $W(X) = c$:
> > $$E(c) = \sum_{x} c f(x)$$
> > $$E(c) = c \sum_{x} f(x)$$
> > Since $f(x)$ is a PMF, we know that $\sum_x f(x) = 1$. Therefore:
> > $$E(c) = c \cdot 1 = c \quad \blacksquare$$

> [!theorem] Theorem 2: Linear Transformation
> Let $X$ be a random variable with a finite mean. For any numerical constants $a$ and $b$:
> $$E(aX + b) = aE(X) + b$$

> [!theorem] Theorem 3: Sum of Random Variables
> The expected value of the sum of two random variables $X$ and $Y$ is the sum of their expected values:
> $$E[X + Y] = E(X) + E(Y)$$

> [!theorem] Theorem 4: Product of Independent Random Variables
> The expected value of the product of two random variables $X$ and $Y$ is equal to the product of their expected values **only when the variables are independent**:
> $$E(XY) = E(X) \cdot E(Y)$$

---

## Variance and Standard Deviation

While expectation tells us the central tendency or average of a random variable, variance measures the spread or dispersion of the variable's values around the mean.

> [!definition] Variance, $V(X)$ or $\sigma^2$
> Let $X$ be a random variable with finite mean $\mu = E(X)$. The variance of $X$ is denoted by $V(X)$ or $\sigma^2$, and is defined as the expected value of the squared deviation from the mean:
> $$V(X) = E[(X - \mu)^2]$$

> [!definition] Standard Deviation, $\sigma$
> The positive square root of the variance is known as the standard deviation.
> $$\sigma = \sqrt{V(X)} = \sqrt{E[(X-\mu)^2]}$$

### Computational Formula for Variance
Calculating $E[(X - \mu)^2]$ directly can be tedious. A much more common and simpler theorem is used for calculation.

> [!theorem] Theorem: Computational Formula for Variance
> Let $X$ be a random variable with mean $\mu$. Then the variance can be calculated as:
> $$V(X) = E(X^2) - [E(X)]^2 = E(X^2) - \mu^2$$
> 
> > [!abstract] Proof
> > By definition: $V(X) = E[(X - \mu)^2]$
> > Expanding the square:
> > $V(X) = E(X^2 - 2X\mu + \mu^2)$
> > Using the linearity of expectation (Theorem 2 and 3):
> > $V(X) = E(X^2) - E(2X\mu) + E(\mu^2)$
> > Since $\mu$ is a constant, $2\mu$ and $\mu^2$ are constants. Thus:
> > $V(X) = E(X^2) - 2\mu E(X) + \mu^2$
> > Substitute $E(X) = \mu$:
> > $V(X) = E(X^2) - 2\mu(\mu) + \mu^2 = E(X^2) - 2\mu^2 + \mu^2$
> > $$V(X) = E(X^2) - \mu^2 \quad \blacksquare$$

---

## Applied Examples

> [!example] Example 1: Calculating Mean, Variance, and SD from a Table
> **Given:** A discrete random variable $X$ has the following PMF:
> 
> | $x$ | $-3$ | $-2$ | $0$ | $1$ | $2$ |
> | :--- | :--- | :--- | :--- | :--- | :--- |
> | $f(x)$ | $0.10$ | $0.30$ | $0.15$ | $0.40$ | $0.05$ |
> 
> **Find $E(X)$ and $V(X)$.**
> 
> **1. Expected Value, $E(X)$:**
> $$E(X) = \sum x f(x) = (-3)(0.10) + (-2)(0.30) + (0)(0.15) + (1)(0.40) + (2)(0.05)$$
> $$E(X) = -0.30 - 0.60 + 0 + 0.40 + 0.10 = -0.40$$
> $\mu = -0.40$
> 
> **2. Expected Value of $X^2$, $E(X^2)$:**
> $$E(X^2) = \sum x^2 f(x) = (-3)^2(0.10) + (-2)^2(0.30) + 0 + (1)^2(0.40) + (2)^2(0.05)$$
> $$E(X^2) = 9(0.10) + 4(0.30) + 0 + 1(0.40) + 4(0.05)$$
> $$E(X^2) = 0.90 + 1.20 + 0.40 + 0.20 = 2.70$$
> 
> **3. Variance and Standard Deviation:**
> $$V(X) = E(X^2) - \mu^2 = 2.70 - (-0.40)^2 = 2.70 - 0.16 = 2.54$$
> $$\text{Standard Deviation } \sigma = \sqrt{2.54} \approx 1.59$$

> [!example] Example 2: Expected Gain in a Game of Chance
> **Problem:** In a coin-tossing game, a man is promised Tk. 5 if he gets all heads or all tails when 3 coins are tossed, and he pays (loses) Tk. 3 if either 1 or 2 heads appear. How much is he expected to gain in the long run?
> 
> **Solution:** 
> Let $X$ be the amount of money the man wins. 
> The sample space for 3 coins has 8 equally likely outcomes: {HHH, HHT, HTH, HTT, THH, THT, TTH, TTT}.
> - "All heads or all tails" (HHH, TTT) happens 2 out of 8 times. Prob = $2/8$. Here $X = +5$.
> - "1 or 2 heads" happens 6 out of 8 times. Prob = $6/8$. Here $X = -3$.
> 
> **PMF of $X$:**
> | $x$ | $5$ | $-3$ |
> | :--- | :--- | :--- |
> | $P(X=x)$ | $2/8$ | $6/8$ |
> 
> **Expected Gain:**
> $$E(X) = \sum x f(x) = 5\left(\frac{2}{8}\right) + (-3)\left(\frac{6}{8}\right) = \frac{10}{8} - \frac{18}{8} = -\frac{8}{8} = -1$$
> **Conclusion:** Thus, the man is expected to lose Tk. 1 in the long run per game.

---

## Moment Generating Function (MGF)

The moment generating function is an alternative way to completely specify a probability distribution. It is particularly useful for finding the moments of a distribution (mean, variance, skewness, etc.).

> [!definition] Moment Generating Function (MGF)
> Let $X$ be a random variable with probability function $f(x)$. The function $M_X(t)$ is called the moment generating function of the random variable $X$ and is defined as the expected value of $e^{tX}$:
> $$M_X(t) = E(e^{tx})$$
> 
> **Formulas:**
> - For a **discrete** random variable: $M_X(t) = \sum_{x} e^{tx} f(x)$
> - For a **continuous** random variable: $M_X(t) = \int_{-\infty}^{\infty} e^{tx} f(x) \, dx$

***
