---
Lecturer: MUA
---
***

# Chapter 1: Probability Laws

**Topics Covered:**
* Sets & Sets Theorem
* Probabilistic Models
* Conditional Probability
* Independence
* Total Probability Theorem
* Bayes' Theorem
* Counting (Permutation & Combination)

---

## Introduction to Probability

> [!info] Definition: Probability
> The term **"Probability"** is an estimate of the proportion of one or more uncertain experimental outcomes when the experiment is performed at random.

**Some examples:**
1. What is the chance that the country will experience severe food this year if dredging of the major rivers is not undertaken?
2. What is the likelihood that the new vaccine will be more effective than the old one in controlling COVID-19 / any emerging epidemic?
3. How likely it is that tomorrow will be a sunny day?
4. What is the probability that the stock price / stock market will show an abrupt rise soon after the forth-coming budget announcement?

---

## A Brief Review of Set Theory

> [!def] Set
> A list / collection of **well-defined** and **unique** elements / objects.
> * The individual object of a set are elements / members.
> * The set is the collection of its elements.
> * The elements of the set must be unique / distinct, i.e. $\rightarrow$ each element must appear once & only once.

> [!def] Universal Set ($U$)
> A universal set is the set of all elements that may possibly be considered in a particular discussion.
> 
> **Example:**
> $U = \{ x : x \text{ is the sum of points appeared at the uppermost side on the two dices} \}$
> $= \{ 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 \}$
> 
> *Note: Probability of dices rolling 7 or higher = $\frac{21}{36} = \frac{7}{12}$*

> [!def] Null Set / Empty Set ($\emptyset$ or $\{ \}$)
> The null set, which is also important, may seem like it is not a set at all. By definition it has no elements.
> We define null set by $\emptyset$ or $\{ \}$.
> For any set $A$, $\emptyset \subset A$

### Venn Diagram
It is customary to refer to Venn Diagram to display relationships among sets.
* Rectangle $\rightarrow$ Complete Set
* Circle $\rightarrow$ Set (Normal Set)

If a circle $A$ is inside a circle $B$:
$A \subset B$ (Here, $B$ = Mother Set of $A$)

### Set Operations
Using the basic concepts of set algebra, we can form new sets from existing sets. There are mainly three operations:

**i) Union ($A \cup B$)**
$$x \in A \text{ or } x \in B$$
$$\Rightarrow x \in (A \cup B)$$

**ii) Intersection ($A \cap B$)**
$$x \in A \text{ and } x \in B$$
$$\Rightarrow x \in (A \cap B)$$

**iii) Complement ($A^c$)**
The complement of a set $A$, is the set of all elements in $S$ that are not in $A$. ($S \rightarrow$ Sample Space, same as $U$).
Complement of the universal set is the null set $\emptyset$.
$$x \in A^c \text{ iff } x \notin A$$

---

## Properties of Collection of Sets
Two important properties of collection of sets while working with probability in real life:

1. **Mutually Exclusive / Disjoint Sets:**
   A collection of sets $A_1, A_2, A_3, \dots, A_n$ is mutually exclusive iff:
   $$A_i \cap A_j = \emptyset, \quad i \neq j$$

2. **Collectively Exhaustive:**
   A collection of sets $A_1, A_2, A_3, \dots, A_n$ is collectively exhaustive iff:
   $$A_1 \cup A_2 \cup A_3 \cup \dots \cup A_n = S$$
   $$\Rightarrow \bigcup_{i=1}^{n} A_i = S$$

> [!important] Partition
> A collection of sets $A_1, A_2, A_3, \dots, A_n$ is a **partition** if it is both mutually exclusive and collectively exhaustive.

*(Note on Intersection of Multiple Sets)*
$$\bigcap_{i=1}^{n} A_i = A_1 \cap A_2 \cap A_3 \cap \dots \cap A_n$$

### De Morgan's Laws
1. $(A \cup B)^c = A^c \cap B^c$
2. $(A \cap B)^c = A^c \cup B^c$

---

## Applying Set Theory to Probability

Probability is based on repeatable experiments that consist of a procedure and observations.
* An **outcome** is an observation.
* An **event** is a set of outcomes.

**Some examples:**
1. Flip a coin. Did it land with heads or tails facing up?
2. Walk to a bus stop. How long do you wait for the arrival of a bus?
3. Give a lecture. How many students are seated in the third row?

> [!def] Outcome
> An outcome of an experiment is any possible observation of that experiment.

> [!def] Random Experiment
> An experiment that can result in different outcomes, even though it is repeated in the same manner every time.
> 
> **Example:** An experiment consists of the following procedure observation and model;
> * **Procedure:** Monitor activity at a Phoneset store.
> * **Observe:** Which type of phone (Android/IOS) the next customer is going to purchase.
> * **Model:** Android or IOS is equally likely. The result of each purchase is unrelated to the previous/next purchase.

### Sample Space ($S$)
*(Similar to Outcomes)*
> [!def] Definition 1
> The set of all possible outcomes of a random experiment is called the sample space of that experiment. It is denoted with $S$.
> The sample space of an experiment is the finest grain, mutually exclusive, collectively exhaustive set of all possible outcomes.

> [!def] Definition 2 (Alternative)
> A sample space of an experiment is a set or collection of all possible outcomes of the same experiment such that any outcome of the experiment corresponds to exactly one element in the set.

**Types of Sample Space:**
1. **Discrete Sample Space:** If the sample space consists of finite or countably infinite set of outcomes. (Looks like infinite, but actually countable).
   *Alternatively:* If a sample space contains a finite number of possibilities or an unending sequence with as many elements as there are whole numbers, it is then called a discrete sample space.
2. **Continuous Sample Space:** If a sample space contains an infinite number of possibilities equal to the number of points on a line segment is called a continuous sample space.
   *Alternatively:* A sample space is continuous if it contains an interval (either finite or infinite) of real numbers. Example: $(1, 2)$.

### Equally Likely Events
Two or more events are set to be equally likely if they have the same chance of occurrence.
That means, whenever a sample space consists of $N$ possible outcomes that are equally likely then the probability of each outcome is $1/N$.

For a discrete sample space, the probability of an event $E$, denoted by $P(E)$, equals to the sum of the probabilities of the outcomes in $E$.

### Axioms of Probability
Probability is a number that is assigned to each member of a collection of events from a random experiment that satisfies the following properties:

If $S$ is the sample space and $E$ is any event random experiment, then,
1. $P(S) = 1$
2. $0 \le P(S_i) \le 1$
3. For any two events $E_1$ & $E_2$ with $E_1 \cap E_2 = \emptyset$:
   $$P(E_1 \cup E_2) = P(E_1) + P(E_2)$$

---

## Laws of Sets

1. **Commutative Laws:** For any given sets $A$ & $B$, union & intersection of sets are commutative.
   $$A \cup B = B \cup A \quad \& \quad A \cap B = B \cap A$$

2. **Associative Laws:** For any given sets $A, B$ & $C$ associative law holds for the union & intersection of the sets, i.e.,
   $$(A \cup B) \cup C = A \cup (B \cup C)$$
   $$(A \cap B) \cap C = A \cap (B \cap C)$$

3. **Distributive Laws:** For any given sets $A, B$ & $C$
   $$A \cap (B \cup C) = (A \cap B) \cup (A \cap C)$$
   $$A \cup (B \cap C) = (A \cup B) \cap (A \cup C)$$

4. **Identity Laws:** For any given set $A$ and any null set $\emptyset$, and universal set $U$
   $$A \cup \emptyset = A$$
   $$A \cap U = A$$
   $$A \cap \emptyset = \emptyset$$
   $$A \cup U = U$$

5. **Idempotent Law:** For any set $A$
   $$A \cup A = A \quad \& \quad A \cap A = A$$

6. **Complementary Laws:** For every subset $A$ of a universal set $U$ ($A \subseteq U$), there is one and only one complement of $A$ namely $A^c$ that follows.
   $$A \cup A^c = U$$
   $$A \cap A^c = \emptyset$$
   $$(A^c)^c = A$$

> [!example] Example Application
> Let $E$ be an event and $E^c$ be its complementary event. By def$^n$ $E$ and $E^c$ are always mutually exclusive and also $E \cup E^c = S$.
> $$P(S) = 1$$
> $$\Rightarrow P(E \cup E^c) = 1$$
> $$\Rightarrow P(E) + P(E^c) = 1$$

---

## Joint Probability

> [!def] Definition
> Two or more events for a joint event, if all of them occur simultaneously and the probability of these joint events are called joint probability.

**For Example:**
* $A \cap B$
* $A \cap B \cap C$
* $A \cap B \cap C \cap D$
are joint events.

*Concept Example:* For $A \cap B$ (Where $A$ is Smoking, $B$ is Heart Disease Patient): Represents a joint event describing that a randomly chosen person is a smoker who suffers from heart disease.

---

## Foundational Problems

> [!example] Problem 1
> Suppose that a sample space consists of 500 persons and are distributed according to their gender and employment status as given below.
> 
> | Gender $\downarrow$ | Employed (E) | Unemployed (U) | Total |
> | :--- | :---: | :---: | :---: |
> | **Male** | 255 | 20 | 275 |
> | **Female** | 80 | 145 | 225 |
> | **Total** | 335 | 165 | 500 |
> 
> One of these 500 persons was selected at random. Let us define the following simple events:
> * $M$: The selected person is Male.
> * $F$: The selected person is Female.
> * $E$: The selected person is Employed.
> * $U$: The selected person is Unemployed.
> 
> The joint events that can be formed as follows:
> * $M \cap E$: The selected person is Male & Employed.
> * $M \cap U$: The selected person is Male & Unemployed.
> * $F \cap E$: The selected person is Female & Employed.
> * $F \cap U$: The selected person is Female & Unemployed.
> 
> **Solution & Concepts:**
> Since the totals $n(M), n(F), n(E), n(U)$ all appear in the margins of the table. They are known as *marginal totals* and their corresponding probabilities $P(M), P(F), P(E), P(U)$ are called *marginal probabilities*.
> 
> Here, 
> $n(M) = 275$; $n(F) = 225$; $n(E) = 335$; $n(U) = 165$; $n(S) = 500$.
> 
> Thus, the marginal probability that a randomly chosen person will be a male is:
> $$P(M) = \frac{n(M)}{n(S)} = \frac{275}{500} = 0.55$$
> $$P(E) = \frac{n(E)}{n(S)} = \frac{335}{500} = 0.67$$
> 
> Now, the probability that a randomly selected person is a male and at the same time employed is given by:
> $$P(M \cap E) = \frac{n(M \cap E)}{n(S)} = \frac{255}{500} = 0.51$$
> Similarly,
> $$P(M \cap U) = \frac{n(M \cap U)}{n(S)} = \frac{20}{500} = 0.04$$
> Also,
> $$P(M) = P(M \cap E) + P(M \cap U) = 0.51 + 0.04 = 0.55$$
> Again,
> $$P(F \cap E) = \frac{n(F \cap E)}{n(S)} = \frac{80}{500} = 0.16$$
> $$P(F \cap U) = \frac{n(F \cap U)}{n(S)} = \frac{145}{500} = 0.29$$
> Notice: $P(F) = P(F \cap E) + P(F \cap U) = 0.45$. And $P(F) = \frac{n(F)}{n(S)} = \frac{225}{500} = 0.45$ (Equal).


> [!example] Problem 2
> In an office of 100 employees, 75 read English, 50 read Bangla dailies and 40 read both and employee is selected at random. Then, what is the probability that the selected employee
> i) Read English Newspaper?
> ii) Reads at least one of the newspapers?
> iii) Reads none?
> iv) Reads English but not Bangla?
> 
> **Solution:**
> Let us define the events as follows:
> $E$: Reads English Newspaper
> $B$: Reads Bangla Newspaper
> $\bar{E} \cap \bar{B}$: Reads none
> $E \cap \bar{B}$: Reads English but not Bangla at the same time
> 
> | | $E$ | $\bar{E}$ | Total |
> | :---: | :---: | :---: | :---: |
> | **$B$** | $n(B \cap E) = 40$ | $n(B \cap \bar{E}) = 10$ | $n(B) = 50$ |
> | **$\bar{B}$** | $n(\bar{B} \cap E) = 35$ | $n(\bar{B} \cap \bar{E}) = 15$ | $n(\bar{B}) = 50$ |
> | **Total** | $n(E) = 75$ | $n(\bar{E}) = 25$ | $n(S) = 100$ |
> 
> **i)** The probability that the randomly selected employee reads english newspaper is given by:
> $$P(E) = \frac{n(E)}{n(S)} = \frac{75}{100} = 0.75$$
> 
> **ii)** The probability that the randomly selected employee reads at least one newspaper is given by:
> $$P(E \cup B) = P(E) + P(B) - P(E \cap B)$$
> $$= \frac{n(E)}{n(S)} + \frac{n(B)}{n(S)} - \frac{n(E \cap B)}{n(S)}$$
> $$= \frac{75}{100} + \frac{50}{100} - \frac{40}{100} = 0.85$$
> 
> **iii)** The probability that the randomly selected employee reads no newspaper is given by:
> $$P(\bar{E} \cap \bar{B}) = P(\overline{E \cup B}) = 1 - P(E \cup B)$$
> $$= 1 - 0.85 = 0.15$$
> 
> **iv)** The probability that the randomly selected employee reads english but not Bangla newspaper is given by:
> $$P(E \cap \bar{B}) = \frac{n(E) - n(E \cap B)}{n(S)} = \frac{75 - 40}{100} = \frac{35}{100} = 0.35$$

---

## Conditional Probability

Conditional Probability corresponds to a modified probability model that reflects partial information about the outcome of an experiment. This modified model has a smaller sample space than the original model.

> [!def] Definition
> The probability of an event $A$ when it is known that some other event $B$ has already occurred is called conditional probability of $A$ and is denoted by:
> $$P(A|B)$$
> *(A is unknown/calculate, B is known)*

Thus, the conditional probability of the event $A$ given the occurrence of the event $B$ is expressed as:
$$P(A|B) = \frac{P(A \cap B)}{P(B)} = \frac{P(AB)}{P(B)}$$
$$\Rightarrow P(A \cap B) = P(A|B) \cdot P(B) \dots \dots \text{(i)}$$

Similarly,
$$P(B|A) = \frac{P(B \cap A)}{P(A)} = \frac{P(A \cap B)}{P(A)}$$
$$\Rightarrow P(A \cap B) = P(B|A) \cdot P(A) \dots \dots \text{(ii)}$$

Therefore:
$$P(A|B) \cdot P(B) = P(B|A) \cdot P(A) = P(A \cap B)$$
*(This is known as the **Multiplication Theorem / Law** or **Law of Compound Probability**)*

**For any three events $A_1, A_2, A_3$:**
$$P(A_1, A_2, A_3) = P(A_1) \cdot P(A_2 | A_1) \cdot P(A_3 | A_1 \cap A_2)$$
$$P(A_1, A_2, A_3, A_4) = P(A_1) \cdot P(A_2 | A_1) \cdot P(A_3 | A_1 \cap A_2) \cdot P(A_4 | A_1 \cap A_2 \cap A_3)$$

**Generalizing for $n$ events:**
$$P(A_1 \cap A_2 \cap A_3 \dots \cap A_n) = P(A_1) \cdot P(A_2 | A_1) \cdot P(A_3 | A_1 \cap A_2) \dots P(A_n | A_1 \cap A_2 \cap \dots \cap A_{n-1})$$

---

## Conditional Probability Problems

> [!example] Problem 1
> A pair of dice is thrown. Find the probability that some of the points on the faces of the 2 dices is 10 or greater, **if** a 5 appears on the first dice.
> 
> **Solution:**
> Let us define the events as follows:
> $E$: The event that the sum of the points on the dices is 10 or greater.
> $F$: The event that a 5 appears on the face of the first dice.
> We have to calculate $P(E|F) = ?$
> $$P(E|F) = \frac{P(E \cap F)}{P(F)}$$
> 
> Here, $E = \{ (4,6), (5,5), (5,6), (6,4), (6,5), (6,6) \}$
> $F = \{ (5,1), (5,2), (5,3), (5,4), (5,5), (5,6) \}$
> $E \cap F = \{ (5,5), (5,6) \}$
> 
> $n(E) = 6$
> $n(F) = 6$
> $n(E \cap F) = 2$
> $n(S) = 36$
> 
> $$P(E|F) = \frac{P(E \cap F)}{P(F)} = \frac{\frac{n(E \cap F)}{n(S)}}{\frac{n(F)}{n(S)}} = \frac{2/36}{6/36} = \frac{1}{3}$$

> [!example] Problem 2
> A family has two children. What is the conditional probability that both are boys given that at least one of them is a boy?
> 
> **Solution:**
> Assume that the sample space $S$ is given by
> $S = \{ (b,b), (b,g), (g,b), (g,g) \}$, where all the outcomes are equally likely. For instance, $(b,g)$ means that the older child is a boy and the younger one is a girl.
> 
> Let us first define the events as follow:
> $E$: The event that both children are boys
> $F$: The event that at least one of them is a boy.
> 
> Here, $E = \{ (b,b) \}$
> $F = \{ (b,b), (b,g), (g,b) \}$
> $E \cap F = \{ (b,b) \}$
> 
> $n(S) = 4, n(E) = 1, n(F) = 3, n(E \cap F) = 1$
> 
> Hence, 
> $$P(E|F) = \frac{P(E \cap F)}{P(F)} = \frac{\frac{n(E \cap F)}{n(S)}}{\frac{n(F)}{n(S)}} = \frac{1/4}{3/4} = \frac{1}{3}$$

> [!example] Problem 3
> The probability that a married man watches a certain TV show is 0.4 and that of his wife is 0.5. The probability that a man watches the show given that his wife does is 0.7. Find the following probabilities:
> i) The probability that a married couple watches the show.
> ii) The probability that a wife watches the show given that her husband does.
> iii) The probability that at least one of them will watch the show.
> 
> **Solution:**
> Let us first define the events as follows:
> $H$: Husband watches the show.
> $W$: Wife watches the show.
> 
> ATQ, $P(H) = 0.4$; $P(W) = 0.5$; $P(H|W) = 0.7$;
> 
> **i)** The probability that the couple watches the show is given by,
> $$P(H \cap W) = P(W \cap H) = P(W) \cdot P(H|W)$$
> $$= (0.5) \cdot (0.7) = 0.35$$
> 
> **ii)** The conditional probability that a wife watches the show given that her husband does is represented by
> $$P(W|H) = \frac{P(W \cap H)}{P(H)} = \frac{0.35}{0.4} = 0.875$$
> 
> **iii)** The probability that at least one of them watches the show is given by
> $$P(W \cup H) = P(W) + P(H) - P(W \cap H)$$
> $$= 0.5 + 0.4 - 0.35 = 0.90 - 0.35 = 0.55$$

> [!example] Problem 4
> A box contains 7 red balls, 3 black balls. Three balls are drawn at random from the box, one after another. Find the probability that the first two balls are red and the third one is black if
> i) The balls are replaced before the next draw (with replacement)
> ii) The balls are not replaced (without replacement)
> 
> **Solution:**
> Let us define the events as follows:
> $R$: The event of drawing red ball.
> $B$: The event of drawing black ball.
> 
> **i)** If a ball is replaced before the next draw, then the subsequent drawings are not affected as the number of balls in the bag remains the same. Hence, it is a case of unconditional probability.
> $R_1$: The first ball is red
> $R_2$: The second ball is red
> $B_3$: The third ball is black
> 
> Therefore, the required probability is:
> $$P(R_1 \cap R_2 \cap B_3) = P(R_1) \cdot P(R_2) \cdot P(B_3)$$
> $$= \left(\frac{7}{10}\right) \left(\frac{7}{10}\right) \left(\frac{3}{10}\right)$$
> $$= \frac{147}{1000} = 0.147$$
> 
> **ii)** When the balls are not replaced to the bag before the next draw, it is a case of conditional probability. In this case, the number of balls decreases at each subsequent drawings.
> Therefore, the required probability is:
> $$P(R_1 \cap R_2 \cap B_3) = P(R_1) \cdot P(R_2|R_1) \cdot P(B_3|R_1 \cap R_2)$$
> $$= \left(\frac{7}{10}\right) \left(\frac{6}{9}\right) \left(\frac{3}{8}\right) = \frac{7}{40}$$

> [!example] Problem 5
> A coin is tossed until a head appears or it has been tossed three times. Given that the head does not appear on the first toss, what is the probability that the coin is tossed three times?
> 
> **Solution:**
> Let us consider the sample space of the experiment be $S = \{ H, TH, TTH, TTT \}$
> 
> Therefore, 
> $P(H) = P(T) = \frac{1}{2}$
> $P(TH) = P(T) \cdot P(H) = \frac{1}{2} \cdot \frac{1}{2} = \frac{1}{4}$
> $P(TTH) = P(T) \cdot P(T) \cdot P(H) = \frac{1}{2} \cdot \frac{1}{2} \cdot \frac{1}{2} = \frac{1}{8}$
> $P(TTT) = P(T) \cdot P(T) \cdot P(T) = \frac{1}{2} \cdot \frac{1}{2} \cdot \frac{1}{2} = \frac{1}{8}$
> 
> Let us define the two events as follows:
> $A$: The event that the coin is tossed 3-times.
> $B$: The event that no head appears on the first toss.
> 
> Hence, $A = \{ TTH, TTT \}$
> $B = \{ TH, TTH, TTT \}$
> $A \cap B = \{ TTH, TTT \}$
> 
> $P(A) = \frac{1}{8} + \frac{1}{8} = \frac{1}{4}$
> $P(B) = \frac{1}{4} + \frac{1}{8} + \frac{1}{8} = \frac{1}{2}$
> $\therefore P(A \cap B) = \frac{1}{8} + \frac{1}{8} = \frac{1}{4}$
> 
> Hence, the required conditional probability is
> $$P(A|B) = \frac{P(A \cap B)}{P(B)} = \frac{1/4}{1/2} = \frac{1}{2}$$

> [!example] Problem 6
> The cards are drawn in succession without replacement from an ordinary package of playing cards (52). Find the probability that the first card is a red Ace, the second card is a 10 or a Jack and the third card is greater than 3 but less than 7.
> 
> *(Cards context: Red Aces = 2. 10s and Jacks = 8. Greater than 3 but less than 7 (4,5,6) = $3 \times 4$ suits = 12).*
> 
> **Solution:**
> Let us define the event as follows.
> $A$: The first card is a red ace.
> $B$: The second card is 10 or a Jack.
> $C$: The third card is greater than 3 but less than 7.
> 
> $P(A) = \frac{2}{52} = \frac{1}{26}$
> $P(B|A) = \frac{8}{51}$
> $P(C|(A \cap B)) = \frac{12}{50} = \frac{6}{25}$
> 
> Hence, the required conditional probability can be calculated using the multiplication law for the three events.
> $$P(A \cap B \cap C) = P(A) P(B|A) P(C|A \cap B)$$
> $$= \left(\frac{1}{26}\right) \left(\frac{8}{51}\right) \left(\frac{6}{25}\right) = \frac{8}{5525}$$

---

## Independent Events

> [!def] Independent Events
> If $E$ and $F$ are two events and if the occurrence of $E$ does not affect and is not affected by the occurrence of $F$, then $E$ and $F$ are said to be independent.
> 
> In other words, two events $E$ and $F$ are said to be independent if
> $$P(E \cap F) = P(EF) = P(E) \cdot P(F) \dots \dots (I)$$

Using the def$^n$ of conditional probability:
$$P(E|F) = \frac{P(E \cap F)}{P(F)} \dots \dots (II)$$

We can say that two events $E$ and $F$ are independent if:
$$P(E|F) = \frac{P(E \cap F)}{P(F)} = \frac{P(E) \cdot P(F)}{P(F)}$$
$$P(E|F) = P(E)$$
*(Conditional Probability = Unconditional Probability)*

### Independence Problem Set

> [!example] Problem 1
> Two ideal coins are tossed. Let $A$ be the event "head on the first coin" and $B$ be the event that "head on the second coin". Check whether the events are independent or not.
> 
> **Solution:**
> Let us take,
> $A$: Head on the first coin
> $B$: Head on the 2nd coin
> 
> A sample space $S$ for the given experiment is
> $S = \{ HH, HT, TH, TT \}$
> $A = \{ HH, HT \} \quad ; \quad B = \{ HH, TH \}$
> $A \cap B = \{ HH \}$
> 
> $P(A) = \frac{2}{4} = \frac{1}{2}$
> $P(B) = \frac{2}{4} = \frac{1}{2}$
> $P(A \cap B) = \frac{1}{4}$
> 
> $P(A) \cdot P(B) = \frac{1}{2} \cdot \frac{1}{2} = \frac{1}{4} = P(A \cap B)$
> Thus, $A$ & $B$ are independent events.

> [!example] Problem 2
> Three coins are tossed. Show that the events "a head on the first coin" and "tails on the last two coins" are independent.
> 
> **Solution:**
> Let us take,
> $A$: Head on the first coin
> $B$: Tails on the last two coins
> 
> $S = \{ HHH, HTH, THH, TTH, HHT, HTT, THT, TTT \}$
> 
> $A = \{ HHH, HHT, HTH, HTT \}$
> $B = \{ HTT, TTT \}$
> $A \cap B = \{ HTT \}$
> 
> $P(A) = \frac{4}{8} = \frac{1}{2}$
> $P(B) = \frac{2}{8} = \frac{1}{4}$
> $P(A \cap B) = \frac{1}{8}$
> 
> $P(A) \cdot P(B) = \frac{1}{2} \cdot \frac{1}{4} = \frac{1}{8} = P(A \cap B)$
> So, $A$ & $B$ are independent events.

> [!example] Problem 3
> Suppose we toss two fair dice. Let $E$ denotes the event that the sum of dices 6 and $F$ denotes the event that the first dice is 4.
> 
> **Solution:**
> $S = \{ (1,1), (1,2), \dots, (1,6), \dots , (6,6) \}$ (36 outcomes)
> 
> $E = \{ (2,4), (4,2), (3,3), (1,5), (5,1) \}$
> $F = \{ (4,1), (4,2), \dots, (4,6) \}$
> $E \cap F = \{ (4,2) \}$
> 
> $P(E) = \frac{5}{36}$
> $P(F) = \frac{6}{36} = \frac{1}{6}$
> $P(E \cap F) = \frac{1}{36}$
> 
> $P(E) \cdot P(F) = \frac{5}{36} \cdot \frac{1}{6} = \frac{5}{216} \neq \frac{1}{36}$
> $\therefore P(E) \cdot P(F) \neq P(E \cap F)$
> So, $E$ & $F$ are not independent events.

> [!example] Problem 4
> A fire brigade has two fire engines operating independently. The probability that a specific fire engine is available when required is 0.99, then
> i) What is the probability that an engine is available when needed?
> ii) What is the probability that neither is available when needed?
> 
> **Solution:**
> Let $A$ be the event that the 1st fire engine is available when needed.
> Let $B$ be the event that the 2nd fire engine is available when needed.
> ATQ, 
> $P(A) = P(B) = 0.99$
> 
> Therefore, the probability that both the engines will be available when required is $P(A \cap B) = P(A) \cdot P(B)$ [Since they are working independently]
> $= (0.99)^2 = 0.9801$
> 
> **i)** $P(A \cup B) = P(A) + P(B) - P(A \cap B)$
> $= 0.99 + 0.99 - 0.9801$
> $= 0.9999$
> 
> **ii)** $P(A^c \cap B^c) = P((A \cup B)^c) = 1 - P(A \cup B)$
> $= 1 - 0.9999$
> $= 0.0001$

### Independence of more than two events
The multiplication Rule for independent events extends very simply to three or more independent events.

For three events, $A, B, C$ when all are independent of each other,
$$P(A \cap B \cap C) = P(A) \cdot P(B) \cdot P(C)$$

For $n$-independent events,
$$P(A_1 \cap A_2 \cap A_3 \cap \dots \cap A_n) = \prod_{i=1}^{n} P(A_i) \dots \dots (*)$$

> [!def] Complete vs Pairwise Independence
> The $n$-events are said to be **completely independent** iff and only if every combination of these events, taken any number at a time, is independent.
> 
> If every combination other than equ$^n$ $(*)$ is independent, then we say that the events are **pairwise independent but not completely independent**.
> 
> Ex: $A, B, C, D$
> $P(A \cap B \cap C) \neq P(A)P(B)P(C)$
> But, $P(A \cap B) = P(A) \cdot P(B)$
> $P(A \cap C) = P(A) \cdot P(C)$

> [!example] Problem (Pairwise vs Complete)
> Two coins are tossed. If $A$ is the event "head on the first coin", $B$ is the event "Head on the second coin", and $C$ is the event "Coins fall alike". Now, show that the events $A, B, C$ are pairwise independent but not completely independent.
> 
> **Solution:**
> The sample space is $S = \{ HH, HT, TH, TT \}$
> 
> Let us define the event as follows:
> $A = \{ HH, HT \} \implies P(A) = 2/4 = 1/2$
> $B = \{ HH, TH \} \implies P(B) = 2/4 = 1/2$
> $C = \{ HH, TT \} \implies P(C) = 2/4 = 1/2$
> 
> $A \cap B = \{ HH \} \implies P(A \cap B) = 1/4$
> $A \cap C = \{ HH \} \implies P(A \cap C) = 1/4$
> $B \cap C = \{ HH \} \implies P(B \cap C) = 1/4$
> $A \cap B \cap C = \{ HH \} \implies P(A \cap B \cap C) = 1/4$
> 
> Thus, the associated probabilities are:
> $P(A) = P(B) = P(C) = \frac{1}{2}$
> 
> $\because P(A) \cdot P(B) = \frac{1}{2} \cdot \frac{1}{2} = \frac{1}{4} = P(A \cap B) \checkmark$
> $P(A) \cdot P(C) = \frac{1}{2} \cdot \frac{1}{2} = \frac{1}{4} = P(A \cap C) \checkmark$
> $P(B) \cdot P(C) = \frac{1}{2} \cdot \frac{1}{2} = \frac{1}{4} = P(B \cap C) \checkmark$
> 
> Also, $P(A) \cdot P(B) \cdot P(C) = \frac{1}{2} \cdot \frac{1}{2} \cdot \frac{1}{2} = \frac{1}{8}$
> But, $P(A \cap B \cap C) = \frac{1}{4}$
> 
> Hence, $P(A \cap B \cap C) \neq P(A) \cdot P(B) \cdot P(C)$
> Therefore, the events are not independent when taken altogether, i.e., they are not completely independent but they are pairwise independent.

---

## Conditional Probability & Partitions

Let $S$ denotes the sample space of some random events / experiments and consider $n$ events $A_1, A_2, A_3, \dots, A_n$ such that they are mutually exclusive and collectively exhaustive, that is, $A_i \cap A_j = \emptyset$ for all $i \neq j$ and
$S = A_1 \cup A_2 \cup A_3 \cup \dots \cup A_n$

Then, it is said that these events form a partition of $S$.

### Total Probability Rule / Partition Rule (Theorem)

Suppose that the events $A_1, A_2, A_3, \dots, A_n$ make partition of the sample space $S$ and $P(A_j) > 0$ for $j = 1, 2, 3, \dots, n$. Then for any event $B \in S$:

$$P(B) = \sum_{j=1}^{n} P(A_j) \cdot P(B|A_j)$$

**Derivation:**
The events $A_1 \cap B, A_2 \cap B, A_3 \cap B, \dots, A_n \cap B$ form a partition of the event $B$.
Hence, $B = (A_1 \cap B) \cup (A_2 \cap B) \cup (A_3 \cap B) \cup \dots \cup (A_n \cap B)$
Since the $n$-events on the RHS of the above eq$^n$ are mutually exclusive:
$$P(B) = P(A_1 \cap B) + P(A_2 \cap B) + \dots + P(A_n \cap B)$$
$$\Rightarrow P(B) = \sum_{j=1}^{n} P(A_j \cap B) \dots \dots (1)$$

Now, using the multiplication rule we have:
$$P(A_j \cap B) = P(A_j) \cdot P(B|A_j) \dots \dots (2)$$

Hence, 
$$P(B) = \sum_{j=1}^{n} P(A_j) \cdot P(B|A_j) \dots \dots (3)$$

It is noted that the probability of $B$ is the weighted average of the conditional probabilities $P(B|A_j)$ with weight $P(A_j)$.

---

## Background of Bayes' Theorem

Very often we start our probability analysis with initial or prior probability estimates for a specific event of interest. Then from sources such as a sample, a specific report or document, we obtain some additional information about the events.
Given this new information, we want to revise and update prior probabilities. The new and revised probabilities for the events are referred to as **posterior probabilities**.

Bayes' Theorem, which will be dealt here, provides a means of computing these revised probabilities.

**Application:**
Let us consider $n$-mutually exclusive and collectively exhaustive events $A_1, A_2, \dots, A_3$ and let $B$ be any event.
If $P(B|A_1), P(B|A_2), \dots, P(B|A_n)$ are known, that the Bayes' theorem is useful to compute the conditional probabilities of $A_j$ events given $B$.

### Bayes' Theorem (Statement)
Let the events $A_1, A_2, A_3, \dots, A_n$ form a partition of the sample space $S$ such that $P(A_j) > 0$ for $j=1,2,3,\dots,n$ and let $B$ be an event such that $P(B) > 0$.

Then:
$$P(A_i|B) = \frac{P(A_i) \cdot P(B|A_i)}{\sum_{j=1}^{n} P(A_j) \cdot P(B|A_j)} \quad \text{for } i = 1, 2, 3, \dots, n$$
*(The denominator is Total Probability)*

**Proof:**
Using the def$^n$ of conditional probability we have:
$$P(A_i|B) = \frac{P(A_i \cap B)}{P(B)} \dots \dots (1)$$

Since the events $A_1, A_2, A_3, \dots, A_n$ form a partition in $S$, and $B$ be any event in $S$
$$B = S \cap B = (A_1 \cup A_2 \cup A_3 \cup \dots \cup A_n) \cap B$$
$$\Rightarrow B = (A_1 \cap B) \cup (A_2 \cap B) \cup (A_3 \cap B) \cup \dots \cup (A_n \cap B) \quad \text{[using distribution law]}$$

Thus, 
$$P(B) = P[ (A_1 \cap B) \cup (A_2 \cap B) \cup (A_3 \cap B) \cup \dots \cup (A_n \cap B) ]$$
$$= P(A_1 \cap B) + P(A_2 \cap B) + \dots + P(A_n \cap B) \quad \text{[since the events are mutually exclusive]}$$
$$\Rightarrow P(B) = \sum_{j=1}^{n} P(A_j \cap B) \dots \dots (2) \quad \text{[using total probability law]}$$

Using eq$^n$ (2), eq$^n$ (1) can be written as:
$$P(A_i|B) = \frac{P(A_i \cap B)}{\sum_{j=1}^{n} P(A_j \cap B)} \dots \dots (3)$$

According to multiplication law,
$$P(A_j \cap B) = P(A_j) \cdot P(B|A_j) \dots \dots (4)$$

Using eq$^n$ (4), eq$^n$ (3) can be re-written as,
$$P(A_i|B) = \frac{P(A_i) \cdot P(B|A_i)}{\sum_{j=1}^{n} P(A_j) \cdot P(B|A_j)} \quad \text{for, } i = 1, 2, 3, \dots, n$$
*[Proved]*

---

## Bayes' Theorem Problems

> [!example] Problem 1
> In a factory, three different machines $M_1, M_2, M_3$ were used to produce a large batch of similar manufactured items. Suppose that, $M_1, M_2$ and $M_3$ produced $25\%, 35\%$ and $40\%$ of the total items, respectively. Suppose further that out of the items produced by the respective machines, $5\%, 4\%$ and $2\%$ are defective. Finally, suppose that one item is selected at random from the entire batch is found to be defective. Determine the probability that this item is produced by machine $M_1$; by $M_2$ or $M_3$.
> 
> **Solution:**
> Let us define the events as follows:
> $A_1$: the selected item was produced by $M_1$
> $A_2$: the selected item was produced by $M_2$
> $A_3$: the selected item was produced by $M_3$
> $B$: item is defective.
> 
> Then we have,
> $P(A_1) = 25\% = 25/100 = 0.25$
> $P(A_2) = 35\% = 0.35$
> $P(A_3) = 40\% = 0.40$
> 
> Also,
> $P(B|A_1) = 5\% = 0.05$
> $P(B|A_2) = 4\% = 0.04$
> $P(B|A_3) = 2\% = 0.02$
> 
> Using Bayes' Theorem, the probability that the selected defective item produced by machine $M_1$ is given by,
> $$P(A_1|B) = \frac{P(A_1) \cdot P(B|A_1)}{\sum_{j=1}^{n} P(A_j) \cdot P(B|A_j)}$$
> $$= \frac{P(A_1) \cdot P(B|A_1)}{P(A_1)P(B|A_1) + P(A_2)P(B|A_2) + P(A_3)P(B|A_3)}$$
> $$= \frac{(0.25)(0.05)}{(0.25 \times 0.05) + (0.35 \times 0.04) + (0.40 \times 0.02)}$$
> $$= \frac{0.0125}{0.0125 + 0.014 + 0.008} = \frac{0.0125}{0.0345} \approx 0.36$$
> The probability is 0.36 or 36% that a randomly selected defective item is produced by machine $M_1$.
> 
> (Or do, $P(A_2 \cup A_3 | B) = 1 - P(A_1 | B) = P(A_1 | B)^c$)
> $$P(A_2 \cup A_3 | B) = P(A_2 | B) + P(A_3 | B)$$
> $$= \frac{P(A_2) \cdot P(B|A_2)}{\sum_{j=1}^{n} P(A_j) \cdot P(B|A_j)} + \frac{P(A_3) \cdot P(B|A_3)}{\sum_{j=1}^{n} P(A_j) \cdot P(B|A_j)}$$
> $$= \frac{P(A_2) \cdot P(B|A_2) + P(A_3) \cdot P(B|A_3)}{P(A_1)P(B|A_1) + P(A_2)P(B|A_2) + P(A_3)P(B|A_3)}$$
> $$= 0.6377$$

> [!example] Problem 2
> A company produces spare parts and supplies in packets. The company produces 2000 packets by plant #1 and 3000 packets by plant #2. Previous experience indicates that 10% produced by plant #1 are defective, while 15% are defective by plant #2. One day, a defective packet was identified compute the probability that the packet was produced by plant #1.
> 
> **Solution:**
> $A_1$: Produced by plant #1
> $A_2$: Produced by plant #2
> $B$: Packet is defective
> 
> Given,
> $P(A_1) = \frac{2000}{2000 + 3000} = 0.40$
> $P(A_2) = \frac{3000}{5000} = 0.60$
> 
> $P(B|A_1) = 10\% = 0.10$
> $P(B|A_2) = 15\% = 0.15$
> 
> $$P(A_1|B) = \frac{P(A_1) P(B|A_1)}{\sum P(A_j) \cdot P(B|A_j)}$$
> $$= \frac{P(A_1) \cdot P(B|A_1)}{P(A_1) \cdot P(B|A_1) + P(A_2) \cdot P(B|A_2)}$$
> $$= \frac{(0.40)(0.10)}{(0.40 \times 0.10) + (0.60 \times 0.15)}$$
> $$= \frac{0.04}{0.04 + 0.09} = \frac{0.04}{0.13} \approx 0.31$$

> [!example] Problem 3
> This year, suppose that there will be three candidates for the post of principal in NDC. They are Fr. Costa, Fr. Hemanta, Fr. Adam. The chances that they get the post are 4:2:3. The probability that Fr. Costa if selected will introduce co-education in the college is 0.3. The probabilities of Fr. Hemanta and Fr. Adam doing the same are 0.5 and 0.8, respectively. What is the probability that there will be co-education this year in the college. If co-education will be introduced, what will be the chance that it will be introduced by Principal Fr. Costa?
> 
> **Solution:**
> $A_1$: Fr. Costa will be selected as Principal
> $A_2$: Fr. Hemanta will be selected as Principal
> $A_3$: Fr. Adam will be selected as Principal
> $B$: Co-education will be introduced.
> 
> Given,
> $P(A_1) = 4/9$  |  $P(B|A_1) = 0.3$
> $P(A_2) = 2/9$  |  $P(B|A_2) = 0.5$
> $P(A_3) = 3/9$  |  $P(B|A_3) = 0.8$
> 
> **Part 1: Probability of Co-education**
> $$P(B) = \sum_{j=1}^{3} P(A_j) \cdot P(B|A_j) = \frac{23}{45}$$
> 
> **Part 2: Chance introduced by Fr. Costa given it's introduced**
> $$P(A_1|B) = \frac{P(A_1) \cdot P(B|A_1)}{\sum_{j=1}^{3} P(A_j)P(B|A_j)}$$
> $$= \frac{(4/9) \times 0.3}{(23/45)}$$
> $$= \frac{6}{23}$$