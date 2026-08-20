# Practice Problems & Solutions: Test of Hypothesis (Hypothesis Testing)

> [!info] How to Use This Problem Set
> Each problem below is drawn directly from or modeled upon the **handwritten lecture notes** and **textbook slides (up to Section 16.8 / Page 41)**.
> - Attempt each problem on paper first.
> - Click on the **`> [!example]- Click to Reveal Step-by-Step Solution`** callout to view the complete 6-step solution, calculations, critical region checks, and conclusions.

---

## Section 1: Conceptual & Theoretical Drill

### Problem 1.1: Decision Errors & Legal Analogy
**Problem:**
1. A quality inspector tests whether a shipment of steel rods meets safety standards ($H_0: \text{Rods meet standard}$). What constitute the Type I and Type II errors in this scenario? Which error is more critical if the rods are for bridge construction?
2. Recall the lecture mnemonics **`RT -> I`** and **`AF -> II`**. Express Type I error probability $\alpha$, Confidence Coefficient, and Power of the test in formal conditional probability notation.

> [!example]- Click to Reveal Step-by-Step Solution
> **Part 1: Error Identification**
> - **Null Hypothesis ($H_0$):** Rods meet safety standards (Safe / Conforming).
> - **Alternative Hypothesis ($H_1$):** Rods do not meet safety standards (Unsafe / Defective).
> - **Type I Error ($\alpha$ - `RT -> I`):** Rejecting $H_0$ when $H_0$ is true $\implies$ Concluding that safe rods are defective and rejecting a good shipment (Producer's Risk / False Alarm).
> - **Type II Error ($\beta$ - `AF -> II`):** Accepting (failing to reject) $H_0$ when $H_0$ is false $\implies$ Concluding that defective rods are safe and using them in construction (Consumer's Risk / Missed Detection).
> - **Criticality:** For bridge construction, **Type II error is far more hazardous and critical**, as using defective structural steel risks catastrophic structural collapse and loss of life.
> 
> **Part 2: Mathematical Formulations**
> - **Type I Error Rate (Level of Significance $\alpha$):**
>   $$\alpha = P(\text{Reject } H_0 \mid H_0 \text{ is True})$$
> - **Confidence Coefficient ($1 - \alpha$):**
>   $$1 - \alpha = P(\text{Accept } H_0 \mid H_0 \text{ is True})$$
> - **Power of the Test ($1 - \beta$):**
>   $$\text{Power} = 1 - \beta = P(\text{Reject } H_0 \mid H_0 \text{ is False})$$

---

### Problem 1.2: Simple vs. Composite Hypotheses & Directionality
**Problem:**
Classify each of the following hypotheses as **Simple** or **Composite**, and specify whether the alternative hypothesis dictates a **Left-Tailed**, **Right-Tailed**, or **Two-Tailed** test:
1. $X \sim N(\mu, \sigma^2 = 25)$; testing $H_0: \mu = 100$ against $H_1: \mu > 100$.
2. $X \sim N(\mu, \sigma^2)$; testing $H_0: \mu = 50$ against $H_1: \mu \neq 50$ (where $\sigma^2$ is unknown).
3. A battery manufacturer claims that a new chemistry increases battery life beyond $48\text{ hours}$. State $H_0$ and $H_1$.

> [!example]- Click to Reveal Step-by-Step Solution
> **1. First Case:**
> - $H_0: \mu = 100$ is **Simple** because the distribution is fully specified as $N(100, 25)$.
> - $H_1: \mu > 100$ is **Composite** (infinitely many values for $\mu > 100$).
> - Directionality: **Right-Tailed Test** (since $H_1$ uses $>$).
> 
> **2. Second Case:**
> - $H_0: \mu = 50$ is **Composite** because the variance $\sigma^2$ is unknown and unspecified.
> - $H_1: \mu \neq 50$ is **Composite**.
> - Directionality: **Two-Tailed Test** (since $H_1$ uses $\neq$).
> 
> **3. Third Case (Battery Claim):**
> - $H_0: \mu = 48\text{ hours}$ (or $\mu \le 48$)
> - $H_1: \mu > 48\text{ hours}$ (Right-Tailed Test).

---

### Problem 1.3: $P$-Value Interpretation Scale
**Problem:**
Suppose hypothesis tests were conducted for four independent research experiments, resulting in the following observed $p$-values:
- Case A: $p = 0.003$
- Case B: $p = 0.038$
- Case C: $p = 0.074$
- Case D: $p = 0.280$

Using the textbook and lecture interpretation benchmark scale, describe the strength of evidence against $H_0$ and state whether $H_0$ should be rejected at $\alpha = 0.05$.

> [!example]- Click to Reveal Step-by-Step Solution
> - **Case A ($p = 0.003 < 0.01$):**
>   - *Evidence:* Very strong evidence against $H_0$. Highly statistically significant.
>   - *Decision at $\alpha = 0.05$:* **Reject $H_0$** (since $0.003 \le 0.05$).
> - **Case B ($0.01 \le p = 0.038 \le 0.05$):**
>   - *Evidence:* Strong evidence against $H_0$. Statistically significant.
>   - *Decision at $\alpha = 0.05$:* **Reject $H_0$** (since $0.038 \le 0.05$).
> - **Case C ($0.05 < p = 0.074 \le 0.10$):**
>   - *Evidence:* Weak / marginal evidence against $H_0$. Tending toward statistical significance.
>   - *Decision at $\alpha = 0.05$:* **Fail to Reject $H_0$** (since $0.074 > 0.05$).
> - **Case D ($p = 0.280 > 0.10$):**
>   - *Evidence:* No evidence against $H_0$. Not statistically significant.
>   - *Decision at $\alpha = 0.05$:* **Fail to Reject $H_0$** (since $0.280 > 0.05$).

---

## Section 2: Single Population Mean Tests (Module A)

### Problem 2.1: Managing Director's Production Claim (Handwritten Notes & Book Ex 16.6.1)
**Problem:**
The Managing Director of a manufacturing firm claims that his factory produces **$110$ items on average daily**. A random sample of $n = 15$ days yields the following daily production figures:
$$110,\ 118,\ 130,\ 140,\ 142,\ 146,\ 112,\ 100,\ 95,\ 98,\ 96,\ 122,\ 123,\ 124,\ 130$$
It is known that the number of items produced daily follows a normal distribution with **known variance $\sigma^2 = 300$**.

Can we conclude at the **$5\%$ Level of Significance** that the average daily production of the firm is:
1. **$110$ items** (Two-tailed test)?
2. **More than $110$ items** (Right-tailed test)?
3. **Less than $110$ items** (Left-tailed test)?
4. Compute the exact $p$-value for the two-tailed test.

> [!example]- Click to Reveal Step-by-Step Solution
> **Preliminary Calculations:**
> - Sample size: $n = 15$
> - Known population variance: $\sigma^2 = 300 \implies \sigma = \sqrt{300} \approx 17.3205$
> - Sum of sample observations:
>   $$\sum x_i = 110 + 118 + 130 + 140 + 142 + 146 + 112 + 100 + 95 + 98 + 96 + 122 + 123 + 124 + 130 = 1786$$
> - Sample mean:
>   $$\bar{x} = \frac{\sum x_i}{n} = \frac{1786}{15} \approx 119.07$$
> - Standard error of the mean:
>   $$\text{SE}(\bar{x}) = \frac{\sigma}{\sqrt{n}} = \sqrt{\frac{\sigma^2}{n}} = \sqrt{\frac{300}{15}} = \sqrt{20} \approx 4.4721$$
> 
> ---
> 
> **Part 1: Two-Tailed Test ($H_0: \mu = 110$ vs. $H_1: \mu \neq 110$)**
> - **Step 1: Hypotheses:**
>   $$H_0: \mu = 110 \quad \text{vs.} \quad H_1: \mu \neq 110$$
> - **Step 2: Level of Significance:** $\alpha = 0.05$.
> - **Step 3: Test Statistic:** Population is normal with known $\sigma^2$, so we use the $Z$-statistic:
>   $$Z = \frac{\bar{x} - \mu_0}{\sigma / \sqrt{n}} \sim N(0, 1)$$
> - **Step 4: Decision Rule:**
>   For a two-tailed test at $\alpha = 0.05$, critical values are $\pm Z_{\alpha/2} = \pm Z_{0.025} = \pm 1.96$.
>   $$\text{Reject } H_0 \text{ if } \lvert Z_{cal} \rvert > 1.96 \quad (Z_{cal} > 1.96 \text{ or } Z_{cal} < -1.96)$$
> - **Step 5: Compute Test Statistic:**
>   $$Z_{cal} = \frac{119.07 - 110}{4.4721} = \frac{9.07}{4.4721} = +2.028 \approx +2.03$$
> - **Step 6: Statistical Decision & Conclusion:**
>   Since $Z_{cal} = 2.03 > 1.96$, the test statistic falls in the upper critical region.
>   **Decision:** Reject $H_0$ at $5\%$ level of significance.
>   **Conclusion:** We cannot accept the Managing Director's claim that average daily production is $110$ items. The data indicates average production is significantly different from $110$.
> 
> ---
> 
> **Part 2: Right-Tailed Test ($H_0: \mu = 110$ vs. $H_1: \mu > 110$)**
> - **Step 1: Hypotheses:** $H_0: \mu = 110$ vs. $H_1: \mu > 110$.
> - **Step 2: LOS:** $\alpha = 0.05$.
> - **Step 3: Test Statistic:** $Z_{cal} = +2.03$.
> - **Step 4: Decision Rule:** For right-tailed test at $\alpha=0.05$, critical value $Z_{0.05} = +1.645$. Reject $H_0$ if $Z_{cal} > 1.645$.
> - **Step 5 & 6:** Since $Z_{cal} = 2.03 > 1.645$, **Reject $H_0$**.
>   **Conclusion:** There is significant evidence that average daily production exceeds $110$ items.
> 
> ---
> 
> **Part 3: Left-Tailed Test ($H_0: \mu = 110$ vs. $H_1: \mu < 110$)**
> - **Step 1: Hypotheses:** $H_0: \mu = 110$ vs. $H_1: \mu < 110$.
> - **Step 2: LOS:** $\alpha = 0.05$.
> - **Step 3: Test Statistic:** $Z_{cal} = +2.03$.
> - **Step 4: Decision Rule:** For left-tailed test at $\alpha=0.05$, critical value $-Z_{0.05} = -1.645$. Reject $H_0$ if $Z_{cal} < -1.645$.
> - **Step 5 & 6:** Since $Z_{cal} = +2.03 \not< -1.645$, **Fail to Reject $H_0$**.
>   **Conclusion:** There is no evidence to conclude that daily production is less than $110$ items.
> 
> ---
> 
> **Part 4: $P$-Value Calculation (Two-Tailed)**
> - For $Z = 2.03$, the right-tail probability from standard normal tables is:
>   $$P(Z > 2.03) = 1 - \Phi(2.03) = 1 - 0.9788 = 0.0212$$
> - For a two-tailed test:
>   $$p\text{-value} = 2 \times P(Z > 2.03) = 2 \times 0.0212 = 0.0424$$
> - Since $p = 0.0424 < 0.05$, we **Reject $H_0$**, confirming our critical value decision.

---

### Problem 2.2: Selling Price Verification (Book Example 16.6.3 / 16.6.11)
**Problem:**
A producer claims that the mean selling price of his product across retail stores is **Tk. 1500**. A random sample of **$n = 100$ stores** reveals a sample mean price of **Tk. 1450** with a sample standard deviation of **$s = \text{Tk. } 225$**.
1. Test the producer's claim against the alternative that the mean price is different from Tk. 1500 at $\alpha = 0.05$.
2. Test whether the mean price is significantly less than Tk. 1500 at $\alpha = 0.01$.
3. Calculate the $p$-value for the two-tailed test.

> [!example]- Click to Reveal Step-by-Step Solution
> **Given Data:**
> - Hypothesized mean: $\mu_0 = 1500$
> - Sample size: $n = 100$ (Large sample, $n \ge 30$)
> - Sample mean: $\bar{x} = 1450$
> - Sample SD: $s = 225$
> - Standard Error: $\text{SE}(\bar{x}) = \frac{s}{\sqrt{n}} = \frac{225}{\sqrt{100}} = \frac{225}{10} = 22.5$
> 
> ---
> 
> **Part 1: Two-Tailed Test at $\alpha = 0.05$**
> - **Step 1: Hypotheses:** $H_0: \mu = 1500 \quad \text{vs.} \quad H_1: \mu \neq 1500$.
> - **Step 2: Level of Significance:** $\alpha = 0.05$.
> - **Step 3: Test Statistic:** Large sample ($n = 100$), so by Central Limit Theorem, we use $Z$:
>   $$Z = \frac{\bar{x} - \mu_0}{s / \sqrt{n}} \sim N(0, 1)$$
> - **Step 4: Decision Rule:** Critical values $\pm Z_{0.025} = \pm 1.96$. Reject $H_0$ if $\lvert Z_{cal} \rvert > 1.96$.
> - **Step 5: Compute:**
>   $$Z_{cal} = \frac{1450 - 1500}{22.5} = \frac{-50}{22.5} = -2.22$$
> - **Step 6: Decision & Conclusion:**
>   Since $\lvert Z_{cal} \rvert = \lvert -2.22 \rvert = 2.22 > 1.96$, $Z_{cal}$ falls in the critical region.
>   **Decision:** Reject $H_0$ at $5\%$ level of significance.
>   **Conclusion:** The claim that the mean selling price is Tk. 1500 is rejected; the actual mean price is significantly different (lower).
> 
> ---
> 
> **Part 2: Left-Tailed Test at $\alpha = 0.01$**
> - **Step 1: Hypotheses:** $H_0: \mu = 1500 \quad \text{vs.} \quad H_1: \mu < 1500$.
> - **Step 2: Level of Significance:** $\alpha = 0.01$.
> - **Step 3 & 4: Decision Rule:** Critical value $-Z_{0.01} = -2.33$. Reject $H_0$ if $Z_{cal} < -2.33$.
> - **Step 5: Test Statistic:** $Z_{cal} = -2.22$.
> - **Step 6: Decision & Conclusion:**
>   Since $Z_{cal} = -2.22 \not< -2.33$, $Z_{cal}$ does **not** fall in the critical region.
>   **Decision:** Fail to Reject $H_0$ at $1\%$ level of significance.
>   **Conclusion:** At the strict $1\%$ level, there is insufficient evidence to conclude that the mean price is below Tk. 1500.
> 
> ---
> 
> **Part 3: $P$-Value Computation**
> - From normal tables, $P(Z < -2.22) = 0.0132$.
> - For the two-tailed test: $p\text{-value} = 2 \times 0.0132 = 0.0264$.
> - Since $0.01 \le p = 0.0264 \le 0.05$, the result is statistically significant at $\alpha = 0.05$, but not at $\alpha = 0.01$.

---

### Problem 2.3: Small Sample $t$-Test for Fluorescent Tube Lifetime (Book Ex 16.6.4 / 16.6.14)
**Problem:**
A manufacturer of fluorescent tubes claims that his tubes have an average lifetime of **$9000\text{ hours}$**. A testing laboratory takes a random sample of **$n = 10\text{ tubes}$** and finds a sample mean lifetime of **$\bar{x} = 8800\text{ hours}$** with a sample standard deviation of **$s = 250\text{ hours}$**.
Assuming tube lifetime is normally distributed, test the manufacturer's claim against the alternative that the average lifetime is less than $9000\text{ hours}$ at $\alpha = 0.05$. (Critical value $t_{0.05, 9} = 1.833$).

> [!example]- Click to Reveal Step-by-Step Solution
> **Given Data:**
> - $\mu_0 = 9000\text{ hrs}$
> - Sample size: $n = 10$ ($n < 30$, small sample)
> - Sample mean: $\bar{x} = 8800\text{ hrs}$
> - Sample SD: $s = 250\text{ hrs}$
> - Population distribution: Normal, but population variance $\sigma^2$ is unknown.
> 
> ---
> 
> **6-Step Solution:**
> - **Step 1: Hypotheses:**
>   $$H_0: \mu = 9000 \quad \text{vs.} \quad H_1: \mu < 9000 \quad (\text{Left-Tailed Test})$$
> - **Step 2: Level of Significance:** $\alpha = 0.05$.
> - **Step 3: Test Statistic:** Small sample ($n < 30$) with unknown $\sigma$, so we use Student's $t$-statistic:
>   $$t = \frac{\bar{x} - \mu_0}{s / \sqrt{n}} \sim t_{\nu = n - 1}$$
>   Degrees of freedom: $\nu = 10 - 1 = 9$.
> - **Step 4: Critical Value & Decision Rule:**
>   For a left-tailed test at $\alpha = 0.05$ with $\nu = 9$ df, the critical value is $-t_{0.05, 9} = -1.833$.
>   $$\text{Reject } H_0 \text{ if } t_{cal} < -1.833$$
> - **Step 5: Compute Test Statistic:**
>   $$\text{SE}(\bar{x}) = \frac{s}{\sqrt{n}} = \frac{250}{\sqrt{10}} = \frac{250}{3.1623} = 79.057$$
>   $$t_{cal} = \frac{8800 - 9000}{79.057} = \frac{-200}{79.057} = -2.53$$
> - **Step 6: Statistical Decision & Conclusion:**
>   Since $t_{cal} = -2.53 < -1.833$, the test statistic falls squarely inside the lower critical rejection region.
>   **Decision:** Reject $H_0$ at $5\%$ level of significance.
>   **Conclusion:** The manufacturer's claim is refuted; the average lifetime of the fluorescent tubes is significantly less than $9000\text{ hours}$.

---

## Section 3: Difference Between Two Independent Means (Module B)

### Problem 3.1: Wage Comparison of Typists (Book Example 16.7.1 - Large Samples)
**Problem:**
A researcher investigates whether male and female typists in a metropolitan city earn comparable weekly wages. Independent random samples yield the following data:
- **Male Typists ($1$):** $n_1 = 40$, $\bar{x}_1 = \text{Tk. } 158.50$, $s_1^2 = 18.29$
- **Female Typists ($2$):** $n_2 = 50$, $\bar{x}_2 = \text{Tk. } 141.60$, $s_2^2 = 20.56$

1. Test at $\alpha = 0.05$ whether there is any significant difference between the average weekly wages of male and female typists.
2. Test whether male typists earn significantly higher average wages than female typists at $\alpha = 0.01$.

> [!example]- Click to Reveal Step-by-Step Solution
> **Preliminary Calculations:**
> - $n_1 = 40, n_2 = 50$ (both $n_1, n_2 \ge 30$, large independent samples)
> - $\bar{x}_1 = 158.50, \bar{x}_2 = 141.60 \implies \bar{x}_1 - \bar{x}_2 = 158.50 - 141.60 = 16.90$
> - Standard error of the difference:
>   $$\text{SE}(\bar{x}_1 - \bar{x}_2) = \sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}} = \sqrt{\frac{18.29}{40} + \frac{20.56}{50}} = \sqrt{0.45725 + 0.41120} = \sqrt{0.86845} \approx 0.9319$$
> 
> ---
> 
> **Part 1: Two-Tailed Test at $\alpha = 0.05$**
> - **Step 1: Hypotheses:**
>   $$H_0: \mu_1 - \mu_2 = 0 \quad (\mu_1 = \mu_2) \quad \text{vs.} \quad H_1: \mu_1 - \mu_2 \neq 0$$
> - **Step 2: Level of Significance:** $\alpha = 0.05$.
> - **Step 3: Test Statistic:** Two-sample large sample $Z$-statistic:
>   $$Z = \frac{(\bar{x}_1 - \bar{x}_2) - 0}{\sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}} \sim N(0, 1)$$
> - **Step 4: Decision Rule:** Reject $H_0$ if $\lvert Z_{cal} \rvert > Z_{0.025} = 1.96$.
> - **Step 5: Compute:**
>   $$Z_{cal} = \frac{16.90}{0.9319} = +18.14$$
> - **Step 6: Decision & Conclusion:**
>   Since $Z_{cal} = 18.14 \gg 1.96$, $Z_{cal}$ falls deeply into the critical region ($p \approx 0.000$).
>   **Decision:** Reject $H_0$ at $5\%$ level of significance.
>   **Conclusion:** There is a highly statistically significant difference between male and female typist earnings.
> 
> ---
> 
> **Part 2: Right-Tailed Test at $\alpha = 0.01$**
> - **Step 1: Hypotheses:** $H_0: \mu_1 \le \mu_2$ vs. $H_1: \mu_1 > \mu_2$.
> - **Step 2: LOS:** $\alpha = 0.01$.
> - **Step 3 & 4: Decision Rule:** Critical value $Z_{0.01} = +2.33$. Reject $H_0$ if $Z_{cal} > 2.33$.
> - **Step 5 & 6:** $Z_{cal} = 18.14 > 2.33 \implies$ **Reject $H_0$**.
>   **Conclusion:** Male typists earn significantly higher average weekly wages than female typists at the $1\%$ significance level.

---

### Problem 3.2: Traffic Speeding Fines in Two Cities (Book Ex 16.7.6 - Pooled $t$-Test)
**Problem:**
A study compares traffic speeding fines across two neighboring cities:
- **City A ($1$):** Sample of $n_1 = 10$ citations with mean $\bar{x}_1 = \text{Tk. } 120$ and sample variance $s_1^2 = 64$.
- **City B ($2$):** Sample of $n_2 = 8$ citations with mean $\bar{x}_2 = \text{Tk. } 105$ and sample variance $s_2^2 = 49$.

Assuming speeding fine amounts are normally distributed in both cities with equal population variances ($\sigma_1^2 = \sigma_2^2$), test whether there is a significant difference in the mean fine amount between the two cities at $\alpha = 0.05$. (Critical value for $t$ with $16$ df at $\alpha=0.05$ two-tailed is $2.120$).

> [!example]- Click to Reveal Step-by-Step Solution
> **Given Data:**
> - $n_1 = 10, \bar{x}_1 = 120, s_1^2 = 64$
> - $n_2 = 8, \bar{x}_2 = 105, s_2^2 = 49$
> - Small samples ($n_1, n_2 < 30$), normal populations, equal variances $\implies$ **Pooled Two-Sample $t$-Test**.
> 
> ---
> 
> **Preliminary Calculation: Pooled Variance ($s_p^2$) & Degrees of Freedom**
> - Degrees of freedom: $\nu = n_1 + n_2 - 2 = 10 + 8 - 2 = 16$.
> - Pooled Variance:
>   $$s_p^2 = \frac{(n_1 - 1)s_1^2 + (n_2 - 1)s_2^2}{n_1 + n_2 - 2} = \frac{(10 - 1)(64) + (8 - 1)(49)}{16} = \frac{9(64) + 7(49)}{16} = \frac{576 + 343}{16} = \frac{919}{16} = 57.4375$$
> - Pooled Standard Deviation:
>   $$s_p = \sqrt{57.4375} \approx 7.5788$$
> - Standard Error of Mean Difference:
>   $$\text{SE}(\bar{x}_1 - \bar{x}_2) = s_p \sqrt{\frac{1}{n_1} + \frac{1}{n_2}} = 7.5788 \sqrt{\frac{1}{10} + \frac{1}{8}} = 7.5788 \sqrt{0.10 + 0.125} = 7.5788 \sqrt{0.225} = 7.5788 \times 0.47434 \approx 3.595$$
> 
> ---
> 
> **6-Step Hypothesis Test:**
> - **Step 1: Hypotheses:**
>   $$H_0: \mu_1 - \mu_2 = 0 \quad \text{vs.} \quad H_1: \mu_1 - \mu_2 \neq 0$$
> - **Step 2: Level of Significance:** $\alpha = 0.05$.
> - **Step 3: Test Statistic:**
>   $$t = \frac{(\bar{x}_1 - \bar{x}_2) - 0}{s_p \sqrt{\frac{1}{n_1} + \frac{1}{n_2}}} \sim t_{\nu = 16}$$
> - **Step 4: Decision Rule:** Critical value $\pm t_{0.025, 16} = \pm 2.120$. Reject $H_0$ if $\lvert t_{cal} \rvert > 2.120$.
> - **Step 5: Compute:**
>   $$t_{cal} = \frac{120 - 105}{3.595} = \frac{15}{3.595} = +4.172$$
> - **Step 6: Statistical Decision & Conclusion:**
>   Since $t_{cal} = 4.172 > 2.120$, $t_{cal}$ falls in the critical region.
>   **Decision:** Reject $H_0$ at $5\%$ level of significance.
>   **Conclusion:** There is a statistically significant difference in the average speeding fines between City A and City B.

---

## Section 4: Paired Samples / Matched Observations (Module C)

### Problem 4.1: Cholesterol Reduction Drug Trial (Book Ex 16.7.7 - Paired $t$-Test)
**Problem:**
A pharmaceutical company conducts a paired trial to compare the effectiveness of two cholesterol-lowering formulations (**Drug X** and **Drug Y**). Twelve matched pairs of patients were selected; one patient in each pair received Drug X and the other received Drug Y. The reduction in cholesterol levels (in points) after 4 weeks is recorded below:

| Pair ($i$) | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Drug X ($x_{1i}$)** | 25 | 32 | 18 | 28 | 35 | 40 | 22 | 30 | 27 | 34 | 20 | 29 |
| **Drug Y ($x_{2i}$)** | 22 | 30 | 19 | 24 | 31 | 38 | 25 | 28 | 25 | 30 | 22 | 26 |

Test at the **$1\%$ Level of Significance** whether there is a significant difference between the average reduction in cholesterol achieved by the two drugs. (Critical value for $t$ with $11$ df at $\alpha = 0.01$ two-tailed is $3.106$).

> [!example]- Click to Reveal Step-by-Step Solution
> **Step 1: Compute Pairwise Differences ($d_i = x_{1i} - x_{2i}$)**
> 
> | Pair ($i$) | Drug X ($x_{1i}$) | Drug Y ($x_{2i}$) | $d_i = x_{1i} - x_{2i}$ | $d_i^2$ |
> | :---: | :---: | :---: | :---: | :---: |
> | 1 | 25 | 22 | $+3$ | 9 |
> | 2 | 32 | 30 | $+2$ | 4 |
> | 3 | 18 | 19 | $-1$ | 1 |
> | 4 | 28 | 24 | $+4$ | 16 |
> | 5 | 35 | 31 | $+4$ | 16 |
> | 6 | 40 | 38 | $+2$ | 4 |
> | 7 | 22 | 25 | $-3$ | 9 |
> | 8 | 30 | 28 | $+2$ | 4 |
> | 9 | 27 | 25 | $+2$ | 4 |
> | 10 | 34 | 30 | $+4$ | 16 |
> | 11 | 20 | 22 | $-2$ | 4 |
> | 12 | 29 | 26 | $+3$ | 9 |
> | **Total** | | | **$\sum d_i = 22$** | **$\sum d_i^2 = 96$** |
> 
> ---
> 
> **Preliminary Calculations:**
> - Number of pairs: $n = 12$
> - Mean difference:
>   $$\bar{d} = \frac{\sum d_i}{n} = \frac{22}{12} = 1.8333$$
> - Standard deviation of differences ($s_d$):
>   $$s_d = \sqrt{\frac{\sum d_i^2 - \frac{(\sum d_i)^2}{n}}{n - 1}} = \sqrt{\frac{96 - \frac{(22)^2}{12}}{11}} = \sqrt{\frac{96 - \frac{484}{12}}{11}} = \sqrt{\frac{96 - 40.3333}{11}} = \sqrt{\frac{55.6667}{11}} = \sqrt{5.0606} \approx 2.2496$$
> - Standard error:
>   $$\text{SE}(\bar{d}) = \frac{s_d}{\sqrt{n}} = \frac{2.2496}{\sqrt{12}} = \frac{2.2496}{3.4641} \approx 0.6494$$
> 
> ---
> 
> **6-Step Hypothesis Test:**
> - **Step 1: Hypotheses:**
>   $$H_0: \mu_d = 0 \quad \text{vs.} \quad H_1: \mu_d \neq 0$$
> - **Step 2: Level of Significance:** $\alpha = 0.01$.
> - **Step 3: Test Statistic:**
>   $$t = \frac{\bar{d} - 0}{s_d / \sqrt{n}} \sim t_{\nu = n - 1 = 11}$$
> - **Step 4: Critical Value & Decision Rule:**
>   For two-tailed test with $\nu = 11$ df at $\alpha = 0.01$, critical value is $\pm t_{0.005, 11} = \pm 3.106$.
>   $$\text{Reject } H_0 \text{ if } \lvert t_{cal} \rvert > 3.106$$
> - **Step 5: Compute Test Statistic:**
>   $$t_{cal} = \frac{1.8333}{0.6494} = +2.823$$
> - **Step 6: Statistical Decision & Conclusion:**
>   Since $\lvert t_{cal} \rvert = 2.823 < 3.106$, $t_{cal}$ does **not** fall in the rejection region.
>   **Decision:** Fail to Reject $H_0$ at $1\%$ level of significance.
>   **Conclusion:** There is no statistically significant difference between the average cholesterol reduction achieved by Drug X and Drug Y at the $1\%$ level.

---

### Problem 4.2: Employee Training Effectiveness (Book Ex 16.7.8 - One-Tailed Paired $t$-Test)
**Problem:**
Ten probationary officers took an assessment test before and after completing a 6-month specialized training program. Their scores (out of 100) were recorded:

| Officer | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Before Training ($x_{1i}$)** | 60 | 70 | 55 | 65 | 80 | 72 | 68 | 50 | 75 | 65 |
| **After Training ($x_{2i}$)** | 70 | 75 | 65 | 70 | 85 | 80 | 74 | 62 | 82 | 75 |

Test at the **$5\%$ Level of Significance** whether the training program significantly improved officer performance ($H_1: \mu_{\text{After}} > \mu_{\text{Before}}$). (Critical value $t_{0.05, 9} = 1.833$).

> [!example]- Click to Reveal Step-by-Step Solution
> **Step 1: Compute Gain / Improvement ($d_i = x_{2i} - x_{1i}$)**
> 
> | Officer | Before ($x_{1i}$) | After ($x_{2i}$) | Gain $d_i = x_{2i} - x_{1i}$ | $d_i^2$ |
> | :---: | :---: | :---: | :---: | :---: |
> | 1 | 60 | 70 | $+10$ | 100 |
> | 2 | 70 | 75 | $+5$ | 25 |
> | 3 | 55 | 65 | $+10$ | 100 |
> | 4 | 65 | 70 | $+5$ | 25 |
> | 5 | 80 | 85 | $+5$ | 25 |
> | 6 | 72 | 80 | $+8$ | 64 |
> | 7 | 68 | 74 | $+6$ | 36 |
> | 8 | 50 | 62 | $+12$ | 144 |
> | 9 | 75 | 82 | $+7$ | 49 |
> | 10 | 65 | 75 | $+10$ | 100 |
> | **Total** | | | **$\sum d_i = 78$** | **$\sum d_i^2 = 668$** |
> 
> ---
> 
> **Preliminary Calculations:**
> - Number of officers: $n = 10$
> - Mean improvement: $\bar{d} = \frac{\sum d_i}{n} = \frac{78}{10} = 7.80$
> - Variance of differences ($s_d^2$):
>   $$s_d^2 = \frac{\sum d_i^2 - \frac{(\sum d_i)^2}{n}}{n - 1} = \frac{668 - \frac{(78)^2}{10}}{9} = \frac{668 - 608.4}{9} = \frac{59.6}{9} \approx 6.6222$$
> - Standard deviation: $s_d = \sqrt{6.6222} \approx 2.5734$
> - Standard Error: $\text{SE}(\bar{d}) = \frac{s_d}{\sqrt{n}} = \frac{2.5734}{\sqrt{10}} = \frac{2.5734}{3.1623} \approx 0.8138$
> 
> ---
> 
> **6-Step Hypothesis Test:**
> - **Step 1: Hypotheses:**
>   $$H_0: \mu_d = 0 \quad (\text{No improvement}) \quad \text{vs.} \quad H_1: \mu_d > 0 \quad (\text{Performance improved})$$
> - **Step 2: Level of Significance:** $\alpha = 0.05$.
> - **Step 3: Test Statistic:** Paired $t$-test with $\nu = n - 1 = 9$ df:
>   $$t = \frac{\bar{d} - 0}{s_d / \sqrt{n}} \sim t_{9}$$
> - **Step 4: Critical Value & Decision Rule:**
>   For a right-tailed test at $\alpha = 0.05$ with $9$ df, critical value $t_{0.05, 9} = +1.833$.
>   $$\text{Reject } H_0 \text{ if } t_{cal} > 1.833$$
> - **Step 5: Compute:**
>   $$t_{cal} = \frac{7.80}{0.8138} = +9.585$$
> - **Step 6: Statistical Decision & Conclusion:**
>   Since $t_{cal} = 9.585 \gg 1.833$, $t_{cal}$ falls far inside the critical rejection region.
>   **Decision:** Reject $H_0$ at $5\%$ level of significance.
>   **Conclusion:** The training program resulted in a highly significant improvement in officer performance.

---

## Section 5: Tests of Hypothesis Concerning Attributes / Proportions (Module D - Page 41)

### Problem 5.1: Manufacturer Equipment Specification Claim (Book Example 16.8.3)
**Problem:**
A manufacturer claims that **at least $95\%$** of the precision electronic equipments supplied to a government agency conform strictly to technical specifications. An inspection team examines a random sample of **$n = 200$ equipments** and finds that **$18$ equipments are faulty**.
Test the manufacturer's claim at the **$5\%$ Level of Significance**.

> [!example]- Click to Reveal Step-by-Step Solution
> **Given Data:**
> - Claimed conforming proportion: $\pi_0 = 0.95$ (so claimed non-conforming / defect rate is $\le 0.05$)
> - Sample size: $n = 200$
> - Number of conforming equipments in sample: $x = 200 - 18 = 182$
> - Observed sample proportion of conforming equipment:
>   $$P = \frac{x}{n} = \frac{182}{200} = 0.91$$
> 
> ---
> 
> **6-Step Hypothesis Test:**
> - **Step 1: Hypotheses:**
>   The manufacturer claims $\pi \ge 0.95$. The challenge to this claim is that conforming proportion is less than $95\%$:
>   $$H_0: \pi = 0.95 \quad (\text{or } \pi \ge 0.95) \quad \text{vs.} \quad H_1: \pi < 0.95 \quad (\text{Left-Tailed Test})$$
> - **Step 2: Level of Significance:** $\alpha = 0.05$.
> - **Step 3: Test Statistic:**
>   Since $n = 200$ is large ($n\pi_0 = 200 \times 0.95 = 190 \ge 5$ and $n(1-\pi_0) = 10 \ge 5$), the sampling distribution of $P$ is normal:
>   $$Z = \frac{P - \pi_0}{\sqrt{\frac{\pi_0 (1 - \pi_0)}{n}}} \sim N(0, 1)$$
> - **Step 4: Decision Rule:**
>   For a left-tailed test at $\alpha = 0.05$, the critical value is $-Z_{0.05} = -1.645$.
>   $$\text{Reject } H_0 \text{ if } Z_{cal} < -1.645$$
> - **Step 5: Compute Test Statistic:**
>   $$\text{SE}(P) = \sqrt{\frac{\pi_0 (1 - \pi_0)}{n}} = \sqrt{\frac{0.95 \times 0.05}{200}} = \sqrt{\frac{0.0475}{200}} = \sqrt{0.0002375} \approx 0.01541$$
>   $$Z_{cal} = \frac{0.91 - 0.95}{0.01541} = \frac{-0.04}{0.01541} = -2.596 \approx -2.60$$
> - **Step 6: Statistical Decision & Conclusion:**
>   Since $Z_{cal} = -2.60 < -1.645$, $Z_{cal}$ falls in the lower critical rejection region.
>   **Decision:** Reject $H_0$ at $5\%$ level of significance.
>   **Conclusion:** The manufacturer's claim that at least $95\%$ of supplied equipment conforms to specifications is rejected. The observed defect rate is significantly higher than claimed.

---

### Problem 5.2: Bank Auditor Mistake Claim (Book Example 16.8.4)
**Problem:**
An auditor claims that **$10\%$ of customer ledger accounts** in a retail bank contain posting or balancing mistakes. A random sample of **$n = 600$ accounts** is audited, revealing **$45$ accounts** with mistakes.
Are these sample results consistent with the auditor's claim at $\alpha = 0.05$?

> [!example]- Click to Reveal Step-by-Step Solution
> **Given Data:**
> - Hypothesized proportion of erroneous accounts: $\pi_0 = 0.10$
> - Sample size: $n = 600$
> - Number of erroneous accounts: $x = 45$
> - Observed sample proportion: $P = \frac{45}{600} = 0.075$ ($7.5\%$)
> 
> ---
> 
> **6-Step Hypothesis Test:**
> - **Step 1: Hypotheses:**
>   $$H_0: \pi = 0.10 \quad \text{vs.} \quad H_1: \pi \neq 0.10 \quad (\text{Two-Tailed Test})$$
> - **Step 2: Level of Significance:** $\alpha = 0.05$.
> - **Step 3: Test Statistic:**
>   $$Z = \frac{P - \pi_0}{\sqrt{\frac{\pi_0 (1 - \pi_0)}{n}}} \sim N(0, 1)$$
> - **Step 4: Decision Rule:**
>   For a two-tailed test at $\alpha = 0.05$, critical values are $\pm Z_{0.025} = \pm 1.96$.
>   $$\text{Reject } H_0 \text{ if } \lvert Z_{cal} \rvert > 1.96$$
> - **Step 5: Compute Test Statistic:**
>   $$\text{SE}(P) = \sqrt{\frac{0.10 \times (1 - 0.10)}{600}} = \sqrt{\frac{0.10 \times 0.90}{600}} = \sqrt{\frac{0.09}{600}} = \sqrt{0.00015} \approx 0.012247$$
>   $$Z_{cal} = \frac{0.075 - 0.10}{0.012247} = \frac{-0.025}{0.012247} = -2.041 \approx -2.04$$
> - **Step 6: Statistical Decision & Conclusion:**
>   Since $\lvert Z_{cal} \rvert = 2.04 > 1.96$, $Z_{cal}$ falls inside the critical region.
>   **Decision:** Reject $H_0$ at $5\%$ level of significance.
>   **Conclusion:** The sample results are not consistent with the auditor's claim of $10\%$ error rate; the actual error rate in the ledger accounts is significantly lower than claimed.\n