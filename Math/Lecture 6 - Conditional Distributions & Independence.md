***
## Conditional Probability Distributions

The concept of conditional distributions is exactly analogous to conditional probability for events: $P(A|B) = \frac{P(A \cap B)}{P(B)}$ (where $P(B) > 0$). By replacing the events $A$ and $B$ with random variables $X$ and $Y$, we can define the conditional distribution of one variable given the other.

> [!definition] Conditional Distribution 
> Let $X$ and $Y$ be two random variables with a joint probability distribution $f(x,y)$, and marginal distributions $g(x)$ and $h(y)$ respectively.
> 
> **1. Conditional distribution of $Y$ given $X=x$:**
> $$f(y|x) = \frac{f(x,y)}{g(x)} \quad \text{provided } g(x) > 0$$
> 
> **2. Conditional distribution of $X$ given $Y=y$:**
> $$f(x|y) = \frac{f(x,y)}{h(y)} \quad \text{provided } h(y) > 0$$

These formulas apply identically whether the random variables are [[Discrete Random Variable|discrete]] or [[Continuous Random Variable|continuous]]. The resulting function $f(y|x)$ is a valid probability distribution of $y$ with $x$ held fixed.

### Evaluating Conditional Probabilities
If someone wishes to find the probability that a random variable $X$ falls between $a$ and $b$ when it is known that $Y=y$, we evaluate it as follows:

> [!note] Formula: Evaluating Conditional Intervals
> $$ P(a < X < b \mid Y = y) = \begin{cases} 
> \sum_{x=a}^{b} f(x|y), & \text{if } X \text{ is discrete} \\
> \int_{a}^{b} f(x|y) \, dx, & \text{if } X \text{ is continuous}
> \end{cases} $$

> [!example] Example: Continuous Conditional Distribution
> **Given:** The joint density function:
> $$f(x,y) = \frac{6-x-y}{8}, \quad \text{for } 0 < x < 2, \ 2 < y < 4$$
> **Find:** The conditional density $f(y|x)$ and compute $P(2 < Y < 3 \mid X = 2)$.
> 
> **Step 1: Find the marginal distribution $g(x)$.**
> $$g(x) = \int_{2}^{4} \frac{6-x-y}{8} \, dy = \frac{1}{8} \left[ 6y - xy - \frac{y^2}{2} \right]_{2}^{4}$$
> $$g(x) = \frac{1}{8} \left[ (24 - 4x - 8) - (12 - 2x - 2) \right] = \frac{1}{8} [16 - 4x - 10 + 2x] = \frac{1}{4}(3-x)$$
> 
> **Step 2: Find the conditional distribution $f(y|x)$.**
> $$f(y|x) = \frac{f(x,y)}{g(x)} = \frac{(6-x-y)/8}{(3-x)/4} = \frac{6-x-y}{2(3-x)}, \quad \text{for } 2 < y < 4$$
> 
> **Step 3: Evaluate the specific probability $P(2 < Y < 3 \mid X = 2)$.**
> Substitute $x=2$ into the conditional distribution:
> $$f(y|2) = \frac{6-2-y}{2(3-2)} = \frac{4-y}{2}$$
> Now integrate this over the requested interval $(2, 3)$:
> $$P(2 < Y < 3 \mid X = 2) = \int_{2}^{3} \frac{4-y}{2} \, dy = \frac{1}{2} \left[ 4y - \frac{y^2}{2} \right]_{2}^{3}$$
> $$= \frac{1}{2} \left[ \left(12 - \frac{9}{2}\right) - \left(8 - 2\right) \right] = \frac{1}{2} \left( \frac{15}{2} - 6 \right) = \frac{1}{2} \left( \frac{3}{2} \right) = \frac{3}{4}$$

---

## Independence of Random Variables

In probability, two events are independent if knowing the outcome of one does not change the probability of the other. The same principle applies to random variables.

> [!definition] Independence of Random Variables
> Two random variables $X$ and $Y$ with marginal densities $g(x)$ and $h(y)$, respectively, are said to be **independent** if and only if their joint distribution is equal to the product of their marginal distributions for all values of $x$ and $y$:
> $$f(x,y) = g(x) \cdot h(y)$$

There are two primary methods to check for independence:

**Method 1: Using Marginal Distributions**
Calculate both marginal distributions $g(x)$ and $h(y)$. Multiply them together. If $g(x) \cdot h(y) = f(x,y)$ for *every* possible pair of $(x,y)$, they are independent. If even one pair fails, they are *not* independent.

**Method 2: Using Conditional Distributions**
Calculate the conditional distribution $f(y|x)$ or $f(x|y)$. If $X$ and $Y$ are independent, the conditional distribution will simply equal the marginal distribution.
- $f(y|x) = h(y)$ 
- $f(x|y) = g(x)$

> [!example] Example: Verifying Independence (Independent Case)
> **Given:** The joint density function:
> $$f(x,y) = \begin{cases} 6xy^2, & 0 \le x \le 1, \ 0 \le y \le 1 \\ 0, & \text{elsewhere} \end{cases}$$
> **Verify if $X$ and $Y$ are independent.**
> 
> **Step 1: Find marginal density $g(x)$.**
> $$g(x) = \int_{0}^{1} 6xy^2 \, dy = 6x \left[ \frac{y^3}{3} \right]_{0}^{1} = 2x, \quad \text{for } 0 \le x \le 1$$
> 
> **Step 2: Find marginal density $h(y)$.**
> $$h(y) = \int_{0}^{1} 6xy^2 \, dx = 6y^2 \left[ \frac{x^2}{2} \right]_{0}^{1} = 3y^2, \quad \text{for } 0 \le y \le 1$$
> 
> **Step 3: Check condition $f(x,y) = g(x) \cdot h(y)$.**
> $$g(x) \cdot h(y) = (2x) \cdot (3y^2) = 6xy^2$$
> Since $g(x) \cdot h(y) = f(x,y)$ for all real numbers, $X$ and $Y$ are **independent**.

> [!example] Example: Verifying Independence (Dependent Case)
> **Given:** The joint density function:
> $$f(x,y) = \frac{x+y}{8}, \quad \text{for } 0 < x < 2, \ 0 < y < 2$$
> **Show that $X$ and $Y$ are not independent.**
> 
> **Step 1: Find marginals.**
> $$g(x) = \int_{0}^{2} \frac{x+y}{8} \, dy = \frac{1}{8} \left[ xy + \frac{y^2}{2} \right]_{0}^{2} = \frac{1}{8}(2x + 2) = \frac{1}{4}(x+1)$$
> $$h(y) = \int_{0}^{2} \frac{x+y}{8} \, dx = \frac{1}{8} \left[ \frac{x^2}{2} + xy \right]_{0}^{2} = \frac{1}{8}(2 + 2y) = \frac{1}{4}(y+1)$$
> 
> **Step 2: Check condition.**
> $$g(x) \cdot h(y) = \frac{1}{4}(x+1) \cdot \frac{1}{4}(y+1) = \frac{1}{16}(x+1)(y+1)$$
> Clearly, $\frac{1}{16}(x+1)(y+1) \neq \frac{x+y}{8}$. 
> Therefore, $f(x,y) \neq g(x) \cdot h(y)$, so $X$ and $Y$ are **not independent**.

***
