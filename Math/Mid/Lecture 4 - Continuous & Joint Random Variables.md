***
## Continuous Probability Distributions

The probability distribution of a [[Continuous Random Variable]] cannot be presented in a tabular form because a continuous variable can take on an infinite (non-denumerably infinite) set of values. 

> [!important] Probability at an Exact Value
> Because a continuous variable can take on any value in a given interval, the probability of it assuming *exactly* any specific value is zero: $P(X = a) = 0$. 
> 
> This implies that the probability of $X$ falling within an interval does not depend on whether the endpoints are included:
> $$P(a \le X \le b) = P(a < X \le b) = P(a \le X < b) = P(a < X < b)$$

### Probability Density Function (PDF)

Instead of a Probability Mass Function (PMF), we use a functional notation $f(x)$ for continuous variables, called the **Probability Density Function (PDF)**. 

> [!definition] Probability Density Function (PDF)
> A probability density function $f(x)$ is a non-negative function constructed so that the area under its curve, bounded by the x-axis, is equal to $1$ (unity) when computed over the entire range of $x$.

A function $f(x)$ is a valid PDF if it possesses the following properties:
1.  **Non-negativity:** $f(x) \ge 0$ for all $x \in (-\infty, \infty)$
2.  **Total Area is 1:** $\int_{-\infty}^{\infty} f(x) dx = 1$
3.  **Probability as Area:** The probability that $X$ falls in the interval $(a, b)$ is the area under the curve between $x=a$ and $x=b$:
    $$P(a < X < b) = \int_{a}^{b} f(x) dx$$

> [!example] Example: Finding a constant $k$ for a PDF
> **Given:** A continuous random variable $X$ has the density function:
> $$f(x) = \begin{cases} kx, & 0 < x < 4 \\ 0, & \text{elsewhere} \end{cases}$$
> **1. Determine $k$ so that $f(x)$ is a PDF:**
> For $f(x)$ to be a PDF, the total integral must be $1$:
> $$\int_{-\infty}^{\infty} f(x) dx = \int_{0}^{4} kx \, dx = 1$$
> $$k \left[ \frac{x^2}{2} \right]_{0}^{4} = k \left( \frac{16}{2} - 0 \right) = 8k = 1 \implies k = \frac{1}{8}$$
> 
> **2. Find $P(1 < X < 2)$:**
> Using $k = 1/8$, integrate over the interval $(1, 2)$:
> $$P(1 < X < 2) = \int_{1}^{2} \frac{1}{8} x \, dx = \frac{1}{8} \left[ \frac{x^2}{2} \right]_{1}^{2} = \frac{1}{8} \left( \frac{4}{2} - \frac{1}{2} \right) = \frac{1}{8} \left( \frac{3}{2} \right) = \frac{3}{16}$$

---

## Continuous Cumulative Distribution Function (CDF)

Exactly analogous to the discrete case, a continuous random variable has a cumulative distribution function.

> [!definition] Continuous CDF
> The cumulative distribution function $F(x)$ of a continuous random variable $X$ with density function $f(x)$ is defined as the area under the curve to the left of $x$:
> $$F(x) = P(X \le x) = \int_{-\infty}^{x} f(t) dt$$

> [!theorem] Relationship Between CDF and PDF
> If the derivative of the CDF $F(x)$ exists, then the PDF $f(x)$ is the derivative of the CDF:
> $$f(x) = \frac{d}{dx} F(x) = F'(x)$$

### Properties of Continuous CDF
1.  **Derivative:** $F'(x) = f(x) \ge 0$ (The CDF is monotonically non-decreasing).
2.  **Lower Bound:** $F(-\infty) = 0$
3.  **Upper Bound:** $F(\infty) = 1$
4.  **Interval Probability:** $P(a < X < b) = F(b) - F(a)$

---

## Joint Probability Distributions

In many instances, it is necessary to consider the properties of two or more random variables simultaneously (e.g., measuring both height $X$ and weight $Y$ of a person). These are referred to as **bi-variate** (or joint) probability distributions.

### 1. Joint Probability Distribution for Discrete Variables

> [!definition] Joint PMF
> If $X$ and $Y$ are discrete random variables, their joint probability distribution $f(x,y)$ gives the probability that the outcomes $x$ and $y$ occur at the same time:
> $$f(x,y) = P(X=x, Y=y)$$
> 
> **Conditions for Joint PMF:**
> 1. $f(x,y) \ge 0$ for all $(x,y)$
> 2. $\sum_{x} \sum_{y} f(x,y) = 1$
> 3. $P((X,Y) \in R) = \sum \sum_{(x,y) \in R} f(x,y)$ for any region $R$ in the xy-plane.

### 2. Joint Distribution for Continuous Variables

> [!definition] Joint PDF
> Let $X$ and $Y$ be two continuous random variables. A non-negative function $f(x,y)$ defined on the entire xy-plane is called the joint density function (Joint PDF) if, for any region $R$, the probability is given by the double integral over $R$:
> $$P[(X,Y) \in R] = \iint_{R} f(x,y) \, dx \, dy$$
> 
> **Conditions for Joint PDF:**
> 1. $f(x,y) \ge 0$ for all $(x,y)$
> 2. $\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} f(x,y) \, dx \, dy = 1$
> 3. For an interval, $P(a \le X \le b, c \le Y \le d) = \int_{a}^{b} \int_{c}^{d} f(x,y) \, dy \, dx$

> [!example] Example: Continuous Joint PDF
> **Given:** The joint PDF of $X$ and $Y$ is:
> $$f(x,y) = \begin{cases} ky^2, & 0 \le x \le 2, \ 0 \le y \le 1 \\ 0, & \text{elsewhere} \end{cases}$$
> **Find the constant $k$:**
> To find $k$, we set the double integral of the entire region to $1$:
> $$\int_{0}^{2} \int_{0}^{1} k y^2 \, dy \, dx = 1$$
> Integrating with respect to $y$ first:
> $$\int_{0}^{2} k \left[ \frac{y^3}{3} \right]_{0}^{1} dx = \int_{0}^{2} \frac{k}{3} dx$$
> Now integrating with respect to $x$:
> $$\frac{k}{3} \left[ x \right]_{0}^{2} = \frac{2k}{3} = 1 \implies k = \frac{3}{2}$$
> Thus, the complete joint PDF is $f(x,y) = \frac{3}{2}y^2$.

***
