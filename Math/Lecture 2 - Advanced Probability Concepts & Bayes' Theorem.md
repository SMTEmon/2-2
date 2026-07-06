***
## Laws of Sets in Probability
When working with probabilities, we frequently use set laws. Let $A$, $B$, and $C$ be events in a [[Sample Space]] $S$ (Universal Set $U$).

> [!note] Fundamental Set Laws
> 1. **Commutative Laws:** $A \cup B = B \cup A$ and $A \cap B = B \cap A$
> 2. **Associative Laws:** 
>    - $(A \cup B) \cup C = A \cup (B \cup C)$
>    - $(A \cap B) \cap C = A \cap (B \cap C)$
> 3. **Distributive Laws:**
>    - $A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$
>    - $A \cup (B \cap C) = (A \cup B) \cap (A \cup C)$
> 4. **Identity Laws:** $A \cup \emptyset = A$, $A \cap \emptyset = \emptyset$, $A \cup S = S$
> 5. **Idempotent Laws:** $A \cup A = A$, $A \cap A = A$
> 6. **Complementary Laws:** $A \cup A^c = S$ and $A \cap A^c = \emptyset$

From the Complementary Law and Probability Axioms ($P(S)=1$), we get the **Rule of Complements**:
$$P(A \cup A^c) = P(S) \implies P(A) + P(A^c) = 1 \implies P(A^c) = 1 - P(A)$$

---

## Joint, Marginal, and Union Probabilities

> [!definition] Joint Probability
> Two or more events forming a joint event (occurring simultaneously) is called a joint probability. It is represented by the intersection of events: $P(A \cap B)$.

> [!definition] Marginal Probability
> When examining a joint probability table, the sums of the rows and columns are called marginal probabilities. They represent the unconditional probability of a single event occurring (e.g., $P(A)$ or $P(B)$), ignoring other variables.

> [!theorem] General Addition Rule (Union of Events)
> For any two events $A$ and $B$, the probability that at least one of them occurs is:
> $$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$
> *Note: If $A$ and $B$ are [[Mutually Exclusive]] ($A \cap B = \emptyset$), then $P(A \cap B) = 0$, and the formula simplifies to $P(A \cup B) = P(A) + P(B)$.*

> [!example] Example: Marginal, Joint, and Union Probabilities
> In an office of 100 employees, 75 read English dailies ($E$), 50 read Bangla dailies ($B$), and 40 read both ($E \cap B$).
> 1. **Probability an employee reads English:** (Marginal)
>    $P(E) = \frac{n(E)}{n(S)} = \frac{75}{100} = 0.75$
> 2. **Probability an employee reads at least one paper:** (Union)
>    $P(E \cup B) = P(E) + P(B) - P(E \cap B) = 0.75 + 0.50 - 0.40 = 0.85$
> 3. **Probability they read neither:** (Complement / De Morgan's)
>    $P(E^c \cap B^c) = P((E \cup B)^c) = 1 - P(E \cup B) = 1 - 0.85 = 0.15$

---

## Conditional Probability & The Multiplication Rule

> [!definition] Conditional Probability
> Conditional probability corresponds to a modified probability model that reflects partial information about the outcome of an experiment. It reduces the sample space to the event that has already occurred.
> 
> The probability of an event $A$, given that some other event $B$ has already occurred, is denoted by $P(A|B)$ and defined as:
> $$P(A|B) = \frac{P(A \cap B)}{P(B)} \quad \text{for } P(B) > 0$$
> Similarly: $P(B|A) = \frac{P(A \cap B)}{P(A)}$ for $P(A) > 0$

> [!theorem] Multiplication Theorem (Law of Compound Probability)
> By rearranging the conditional probability formula, we can find the joint probability of two events occurring simultaneously:
> $$P(A \cap B) = P(A) \cdot P(B|A) = P(B) \cdot P(A|B)$$
> 
> **Generalization for $n$ events:**
> $$P(A_1 \cap A_2 \cap \dots \cap A_n) = P(A_1) P(A_2|A_1) P(A_3|A_1 \cap A_2) \dots P(A_n|A_1 \cap \dots \cap A_{n-1})$$

---

## Total Probability Rule & Bayes' Theorem

### 1. Total Probability Rule
Let $S$ denote the sample space and consider $n$ events $A_1, A_2, \dots, A_n$ that form a [[Partition]] of $S$ (they are mutually exclusive and collectively exhaustive).

> [!theorem] Total Probability Rule
> For any event $B \in S$, the probability of $B$ can be expressed as the weighted average of the conditional probabilities $P(B|A_j)$ with weights $P(A_j)$:
> $$P(B) = \sum_{j=1}^{n} P(A_j \cap B) = \sum_{j=1}^{n} P(A_j) \cdot P(B|A_j)$$

### 2. Bayes' Theorem
Very often, we begin our analysis with initial or **prior probabilities**. Then, we obtain additional information (an event $B$ occurs). We want to revise and update the prior probabilities. These updated values are called **posterior probabilities**.

> [!theorem] Bayes' Theorem
> Let the events $A_1, A_2, \dots, A_n$ form a partition of the sample space $S$. Let $B$ be an event such that $P(B) > 0$. Then the conditional probability of $A_i$ given $B$ is:
> $$P(A_i|B) = \frac{P(A_i) \cdot P(B|A_i)}{\sum_{j=1}^{n} P(A_j) \cdot P(B|A_j)} \quad \text{for } i = 1, 2, \dots, n$$

> [!abstract] Proof of Bayes' Theorem
> Using the definition of conditional probability:
> $$P(A_i|B) = \frac{P(A_i \cap B)}{P(B)} \quad \text{--- (1)}$$
> 
> Since events $A_1, A_2, \dots, A_n$ form a partition of $S$, we can express $B$ as:
> $$B = S \cap B = (A_1 \cup A_2 \cup \dots \cup A_n) \cap B$$
> Using the distributive law:
> $$B = (A_1 \cap B) \cup (A_2 \cap B) \cup \dots \cup (A_n \cap B)$$
> 
> Because the $A_j$ events are mutually exclusive, the intersections $(A_j \cap B)$ are also mutually exclusive. Applying the addition rule for mutually exclusive events:
> $$P(B) = P(A_1 \cap B) + P(A_2 \cap B) + \dots + P(A_n \cap B) = \sum_{j=1}^{n} P(A_j \cap B)$$
> Using the Multiplication Theorem, $P(A_j \cap B) = P(A_j) \cdot P(B|A_j)$. Substituting this yields the **Total Probability Rule**:
> $$P(B) = \sum_{j=1}^{n} P(A_j) \cdot P(B|A_j) \quad \text{--- (2)}$$
> 
> Also, applying the Multiplication Theorem to the numerator of equation (1):
> $$P(A_i \cap B) = P(A_i) \cdot P(B|A_i) \quad \text{--- (3)}$$
> 
> Finally, substituting equations (2) and (3) into equation (1) gives Bayes' Theorem:
> $$P(A_i|B) = \frac{P(A_i) \cdot P(B|A_i)}{\sum_{j=1}^{n} P(A_j) \cdot P(B|A_j)}$$
> $\blacksquare$

> [!example] Example: Medical Diagnosis (Bayes' Theorem)
> Assume an Oncology Dept. is giving a free medical test for a rare lung disease. 
> - The chance of having the disease is $1$ in $10,000$.
> - The test is $90\%$ reliable: If you have the disease, $P(\text{Positive} | \text{Disease}) = 0.90$. If you do not have the disease, $P(\text{Positive} | \text{No Disease}) = 0.10$ (Wait, standard example usually says 5% false positive, let's look at the notes: The notes say if the person does not have the disease, there is a $5\%$ chance of a positive response).
> 
> **Events:**
> $A_1$: You have the disease. $\implies P(A_1) = 1/10000 = 0.0001$
> $A_2$: You do not have the disease. $\implies P(A_2) = 1 - 0.0001 = 0.9999$
> $B$: Test response is positive.
> 
> **Given:**
> $P(B|A_1) = 0.90$ (True Positive)
> $P(B|A_2) = 0.05$ (False Positive)
> 
> **Question:** If you test positive, what is the probability you actually have the disease ($P(A_1|B)$)?
> 
> **Solution:** Using Bayes' Theorem:
> $$P(A_1|B) = \frac{P(A_1)P(B|A_1)}{P(A_1)P(B|A_1) + P(A_2)P(B|A_2)}$$
> $$P(A_1|B) = \frac{0.0001 \times 0.90}{(0.0001 \times 0.90) + (0.9999 \times 0.05)}$$
> $$P(A_1|B) = \frac{0.00009}{0.00009 + 0.049995} \approx 0.002$$
> **Interpretation:** Even with a positive test result, because the disease is so rare, there is only a $0.2\%$ chance (or 2 in 1000) that you actually have the cancer!

***
