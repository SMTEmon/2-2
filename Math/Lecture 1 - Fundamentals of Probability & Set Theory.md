***

> [!definition] Probability
> The term **Probability** is an estimate of the proportion of one or more uncertain experimental outcomes when the experiment is performed at random.

## Review of Sets & Set Notations
Before diving deeply into probability, it is essential to understand basic set theory, as probability builds directly upon these concepts.

> [!definition] Set and Elements
> - **Set:** A set is simply a well-defined list or collection of distinct objects.
> - **Elements:** The individual objects of a set are called elements or members. The elements of a set must be distinct (each element must appear once and only once).

> [!definition] Universal Set ($U$ or $S$)
> A universal set is the set of all elements that may possibly be considered in a particular discussion. 
> *Example:* Let $U = \{x \mid x \text{ is the sum of points uppermost side on two dices}\} = \{2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12\}$.

> [!definition] Null Set ($\emptyset$)
> The null set (or empty set) is a set that contains exactly no elements. It is denoted by $\phi$ or $\emptyset$. For any set $A$, $\emptyset \subset A$.

### Venn Diagrams
Named after the English logician John Venn, Venn Diagrams are customarily used to display relationships among sets.
*   **Universal Set:** Conventionally, the region enclosed by a large rectangle is the universal set.
*   **Sets:** Closed surfaces (usually circles) within this rectangle denote sets.

## Set Operations
Using the basic concepts of set algebra, we can form new sets from existing ones. There are three main operations:

> [!note] 1. Union ($A \cup B$)
> The union of sets $A$ and $B$ is the set of all elements that are either in $A$, or in $B$, or in both. It corresponds to the logical "**OR**" operation.
> $$x \in A \cup B \iff x \in A \text{ or } x \in B$$

> [!note] 2. Intersection ($A \cap B$)
> The intersection of two sets $A$ and $B$ is the set of all elements that are contained in both $A$ and $B$. It corresponds to the logical "**AND**" operation.
> $$x \in A \cap B \iff x \in A \text{ and } x \in B$$

> [!note] 3. Complement ($A^c$ or $\bar{A}$)
> The complement of a set $A$, denoted with $A^c$, is the set of all elements in the universal set $S$ that are **not** in $A$.
> $$x \in A^c \iff x \notin A$$
> *Note:* The complement of the universal set is the null set ($S^c = \emptyset$).

## Important Set Properties

> [!definition] Mutually Exclusive / Disjoint Sets
> A collection of sets $A_1, A_2, \dots, A_n$ is mutually exclusive (or disjoint) if and only if (iff) no two sets share any common elements:
> $$A_i \cap A_j = \emptyset \quad \text{for } i \neq j$$

> [!definition] Collectively Exhaustive Sets
> A collection of sets $A_1, A_2, \dots, A_n$ is collectively exhaustive iff their union makes up the entire universal set $S$:
> $$A_1 \cup A_2 \cup \dots \cup A_n = S$$
> $$\bigcup_{i=1}^{n} A_i = S$$

> [!definition] Partition
> A collection of sets $A_1, A_2, \dots, A_n$ is a **partition** if it is *both* [[Mutually Exclusive]] and Collectively Exhaustive.

### De Morgan's Laws
These laws relate the union and intersection of sets through their complements:
1. $(A \cup B)^c = A^c \cap B^c$
2. $(A \cap B)^c = A^c \cup B^c$

---

## Applying Set Theory to Probability

There is a direct correlation between set algebra and probability terminology.

| Set Algebra | Probability Terminology |
| :--- | :--- |
| Set | [[Event]] |
| Universal Set | [[Sample Space]] ($S$) |
| Element | Outcome |

### Random Experiments & Sample Spaces

> [!definition] Random Experiment
> An experiment that can result in different outcomes, even though it is repeated in the exact same manner every time, is called a random experiment.
> It usually consists of a **procedure**, an **observation**, and a **model**.

> [!example] Key Example: Random Experiment
> **Procedure:** Monitor activity at a phone store.
> **Observation:** Observe which type of phone (Apricot or Banana) the next customer purchases.
> **Model:** Apricots and Bananas are equally likely. The result of each purchase is unrelated to previous purchases.

> [!definition] Sample Space ($S$)
> The set of all possible outcomes of a random experiment is called the sample space. It is the finest-grain, mutually exclusive, collectively exhaustive set of all possible outcomes.
> 
> **Types of Sample Spaces:**
> 1. **Discrete Sample Space:** If it contains a finite or countable infinite set of outcomes (e.g., set of integers).
> 2. **Continuous Sample Space:** If it contains an infinite number of possibilities equal to the number of points on a line segment (e.g., an interval of real numbers).

### Events

> [!definition] Event
> An event is a subset of the sample space of a random experiment. 
> - **Union of Events ($E_1 \cup E_2$):** The event that consists of all outcomes contained in either $E_1$ or $E_2$.
> - **Intersection of Events ($E_1 \cap E_2$):** The event that consists of all outcomes contained in both $E_1$ and $E_2$.
> - **Complement of an Event ($E^c$):** The set of outcomes in the sample space that are *not* in $E$.

> [!note] Equally Likely Events
> Two or more events are said to be equally likely if they have the same chance of occurrence. For a discrete sample space with $N$ possible outcomes that are equally likely, the probability of each outcome is $1/N$.

## Axioms of Probability

Probability is a number assigned to each member of a collection of events from a random experiment that satisfies specific rigid properties.

> [!theorem] Axioms of Probability
> If $S$ is the [[Sample Space]] and $E$ is any [[Event]] in a random experiment, the following axioms must hold:
> 
> 1. **Certainty:** $P(S) = 1$
> 2. **Non-negativity:** $0 \le P(E) \le 1$
> 3. **Additivity (for Mutually Exclusive Events):** For any two events $E_1$ and $E_2$ where $E_1 \cap E_2 = \emptyset$:
>    $$P(E_1 \cup E_2) = P(E_1) + P(E_2)$$

***
