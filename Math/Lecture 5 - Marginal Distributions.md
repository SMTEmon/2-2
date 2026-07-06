***
When dealing with two random variables $X$ and $Y$ with a known joint probability distribution $f(x,y)$, we might only be interested in the probability distribution of one of the variables alone (e.g., just $X$ or just $Y$). 

> [!definition] Marginal Distribution
> The probability distribution of a single variable derived from a joint probability distribution is called a **Marginal Distribution**. 
> - We conventionally denote the marginal distribution of $X$ as $g(x)$.
> - We conventionally denote the marginal distribution of $Y$ as $h(y)$.

Marginal distributions are indeed valid probability distributions on their own, meaning they satisfy all the properties of a standard probability distribution (e.g., they must sum or integrate to $1$).

---

## 1. Marginal Distribution for Discrete Variables

For discrete random variables, the marginal probability function of one variable is found by summing the joint probabilities over all possible values of the other variable. This is equivalent to summing the rows or columns in a joint probability table.

> [!note] Marginal PMF Formulas
> Given a joint discrete probability function $f(x,y)$:
> - The marginal probability distribution of $X$ alone is:
>   $$g(x) = \sum_{y} f(x,y)$$
> - The marginal probability distribution of $Y$ alone is:
>   $$h(y) = \sum_{x} f(x,y)$$

> [!example] Example: Discrete Marginal Distribution
> **Given:** The joint probability distribution of $X$ and $Y$ in tabular form:
> 
> | $Y \downarrow \setminus X \rightarrow$ | $0$ | $1$ | $2$ | $3$ | **Row Sum ($h(y)$)** |
> | :--- | :--- | :--- | :--- | :--- | :--- |
> | **$0$** | $0$ | $1/8$ | $2/8$ | $1/8$ | $4/8$ |
> | **$1$** | $1/8$ | $2/8$ | $1/8$ | $0$ | $4/8$ |
> | **Col Sum ($g(x)$)**| $1/8$ | $3/8$ | $3/8$ | $1/8$ | **$1$** |
> 
> **Find:** The marginal distributions of $X$ and $Y$.
> 
> **Solution:** 
> For the random variable $X$, we sum over all values of $y$ ($y=0, 1$):
> - $g(0) = \sum_{y=0}^{1} f(0,y) = f(0,0) + f(0,1) = 0 + 1/8 = 1/8$
> - $g(1) = \sum_{y=0}^{1} f(1,y) = f(1,0) + f(1,1) = 1/8 + 2/8 = 3/8$
> - $g(2) = \sum_{y=0}^{1} f(2,y) = f(2,0) + f(2,1) = 2/8 + 1/8 = 3/8$
> - $g(3) = \sum_{y=0}^{1} f(3,y) = f(3,0) + f(3,1) = 1/8 + 0 = 1/8$
> 
> The marginal distributions can be represented as their own separate tables:
> 
> **Marginal Distribution of $X$:**
> | $x$ | $0$ | $1$ | $2$ | $3$ | Sum |
> | :--- | :--- | :--- | :--- | :--- | :--- |
> | **$g(x)$** | $1/8$ | $3/8$ | $3/8$ | $1/8$ | $1$ |
> 
> **Marginal Distribution of $Y$:**
> | $y$ | $0$ | $1$ | Sum |
> | :--- | :--- | :--- | :--- |
> | **$h(y)$** | $4/8$ | $4/8$ | $1$ |

---

## 2. Marginal Distribution for Continuous Variables

For continuous random variables, the marginal density function of one variable is found by integrating the joint probability density function over the entire continuous range of the other variable.

> [!note] Marginal PDF Formulas
> Given a joint continuous density function $f(x,y)$:
> - The marginal density function of $X$ alone is:
>   $$g(x) = \int_{-\infty}^{\infty} f(x,y) \, dy$$
> - The marginal density function of $Y$ alone is:
>   $$h(y) = \int_{-\infty}^{\infty} f(x,y) \, dx$$
> 
> *Proof of Validity:* Integrating $g(x)$ over all $x$ yields $1$:
> $$\int_{-\infty}^{\infty} g(x) \, dx = \int_{-\infty}^{\infty} \left( \int_{-\infty}^{\infty} f(x,y) \, dy \right) dx = 1$$

> [!example] Example: Continuous Marginal Distribution
> **Given:** The joint density function of $X$ and $Y$ is:
> $$f(x,y) = \begin{cases} \frac{1}{8}(6-x-y), & \text{for } 0 < x < 2, \ 2 < y < 4 \\ 0, & \text{otherwise} \end{cases}$$
> 
> **Find:** The marginal densities of $X$ and $Y$.
> 
> **Solution:**
> **1. Marginal density of $X$, $g(x)$:**
> We integrate the joint PDF with respect to $y$ over its domain $(2, 4)$:
> $$g(x) = \int_{2}^{4} \frac{1}{8}(6-x-y) \, dy$$
> $$g(x) = \frac{1}{8} \left[ 6y - xy - \frac{y^2}{2} \right]_{2}^{4}$$
> $$g(x) = \frac{1}{8} \left[ \left(24 - 4x - 8\right) - \left(12 - 2x - 2\right) \right]$$
> $$g(x) = \frac{1}{8} [ 16 - 4x - 10 + 2x ]$$
> $$g(x) = \frac{1}{8} (6 - 2x) = \frac{1}{4}(3-x) \quad \text{for } 0 < x < 2$$
> 
> **2. Marginal density of $Y$, $h(y)$:**
> We integrate the joint PDF with respect to $x$ over its domain $(0, 2)$:
> $$h(y) = \int_{0}^{2} \frac{1}{8}(6-x-y) \, dx$$
> $$h(y) = \frac{1}{8} \left[ 6x - \frac{x^2}{2} - xy \right]_{0}^{2}$$
> $$h(y) = \frac{1}{8} \left[ \left(12 - 2 - 2y\right) - 0 \right]$$
> $$h(y) = \frac{1}{8} (10 - 2y) = \frac{1}{4}(5-y) \quad \text{for } 2 < y < 4$$

***