***

> [!definition] Random Variable (RV)
> A random variable is a real-valued function defined over a [[Sample Space]]. Its values are numbers or quantities that arise as a result of chance factors, meaning they cannot be predicted exactly in advance. 

Random variables are generally classified into two types depending on the specific numerical values they can take on:
1.  **Discrete Random Variable:** Defined over a discrete sample space. It may only take on a finite or countable number of distinct, isolated values (e.g., integers). 
    *   *Examples:* Number of telephone calls received, number of defective bulbs produced, number of correct answers on a multiple-choice test.
2.  **Continuous Random Variable:** Defined over a continuous sample space. It can take on *any* value within a certain interval or collection of intervals.
    *   *Examples:* Time taken to serve a customer, weight of a baby, temperature recorded, height.

---

## Probability Distributions

The idea of a probability distribution exactly parallels that of a frequency distribution. In a frequency distribution, each class is paired with a frequency. In a probability distribution, each value (or class interval) is paired with its **probability**. 

While the sum of frequencies equals the total number of cases ($N$), the sum of the probabilities in a probability distribution must strictly be **$1.0$**. A probability distribution represents the long-run theoretical behavior of a frequency distribution.

### Probability Mass Function (PMF)

For a discrete random variable $X$, we represent its probability distribution using a mathematical formula called the Probability Mass Function (PMF).

> [!definition] Probability Mass Function (PMF)
> If a random variable $X$ has a discrete distribution, the probability distribution of $X$ is defined as the function $f(x)$ such that for any real number $x$:
> $$f(x) = P(X=x)$$
> 
> To be considered a valid PMF, the function $f(x)$ **must satisfy the following three conditions**:
> 1. **Non-negativity:** $f(x) \ge 0$ for all $x$
> 2. **Sum equals one:** $\sum_{x} f(x) = 1$
> 3. **Probability mapping:** $P(X=x) = f(x)$

> [!example] Example: Verifying a PMF
> **Question:** Verify whether the following function is a PMF: 
> $$f(x) = \frac{3x+6}{21} \quad \text{for } x = 1, 2$$
> 
> **Solution:** 
> Step 1: Check values for each $x$.
> - For $x=1$: $f(1) = \frac{3(1)+6}{21} = \frac{9}{21}$
> - For $x=2$: $f(2) = \frac{3(2)+6}{21} = \frac{12}{21}$
> 
> Step 2: Check condition 1 ($f(x) \ge 0$). Both $9/21$ and $12/21$ are $\ge 0$.
> 
> Step 3: Check condition 2 ($\sum f(x) = 1$).
> $$\sum_{x=1}^{2} f(x) = f(1) + f(2) = \frac{9}{21} + \frac{12}{21} = \frac{21}{21} = 1$$
> Since all conditions are satisfied, $f(x)$ is a valid PMF.

### Graphical Representation of a Discrete PMF
In addition to a formula or a table, a PMF can be represented graphically. 
*   The X-axis contains the different values the random variable can take on.
*   The Y-axis is a measure of probability.
*   Above each value $x$, a vertical line (or a rectangle/histogram bar centered at $x$) is drawn to the height corresponding to $P(X=x)$.

---

## Discrete Cumulative Distribution Function (CDF)

In many occasions, we are interested in knowing the probability that a random variable takes on a value *less than or equal to* a prescribed number $x$ (e.g., "What is the probability of getting *at most* 2 heads?").

> [!definition] Cumulative Distribution Function (CDF)
> The cumulative distribution function, denoted by $F(x)$, of a discrete random variable $X$ with PMF $f(t)$, is the cumulative probability up to and including the point $x$. Symbolically:
> $$F(x) = P(X \le x) = \sum_{t \le x} f(t)$$

### Properties of the CDF
The function $F(x)$ must satisfy the following conditions:
1.  **Monotonically Increasing:** $F(a) \le F(b)$ for $a \le b$.
2.  **Limits:** 
    - The limit to the far left is 0: $\lim_{x \to -\infty} F(x) = 0$
    - The limit to the far right is 1: $\lim_{x \to \infty} F(x) = 1$
3.  **Bounded:** $0 \le F(x) \le 1$ for all real numbers $x$.

> [!note] Step Function
> For a discrete random variable, the graph of the CDF $F(x)$ has a distinctive appearance of a set of steps. Therefore, the distribution function for a discrete random variable is always a **step function** because it increases only at a countable number of points and remains flat in between.

> [!example] Example: Constructing a CDF
> Let $X$ be the number of heads obtained when tossing a coin 3 times. 
> The PMF is given as:
> - $P(X=0) = 1/8$
> - $P(X=1) = 3/8$
> - $P(X=2) = 3/8$
> - $P(X=3) = 1/8$
> 
> The CDF $F(x)$ is calculated by accumulating these probabilities:
> - $F(0) = P(X \le 0) = f(0) = 1/8$
> - $F(1) = P(X \le 1) = f(0) + f(1) = 1/8 + 3/8 = 4/8$
> - $F(2) = P(X \le 2) = f(0) + f(1) + f(2) = 4/8 + 3/8 = 7/8$
> - $F(3) = P(X \le 3) = f(0) + f(1) + f(2) + f(3) = 7/8 + 1/8 = 1$
> 
> **Formal piecewise representation:**
> $$
> F(x) = \begin{cases} 
> 0, & \text{for } x < 0 \\
> 1/8, & \text{for } 0 \le x < 1 \\
> 4/8, & \text{for } 1 \le x < 2 \\
> 7/8, & \text{for } 2 \le x < 3 \\
> 1, & \text{for } x \ge 3 
> \end{cases}
> $$

***
