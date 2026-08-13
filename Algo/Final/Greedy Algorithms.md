---
title: Greedy Algorithms
date: 2026-08-12
tags:
  - algorithms
  - greedy
  - cs4403
  - lecture19-21
aliases:
  - Greedy Method
  - Fractional Knapsack
  - Activity Selection
  - Job Sequencing
  - Interval Partitioning
  - Huffman Encoding
---

# Greedy Algorithms

> [!abstract] Overview & Paradigm
> A **Greedy Algorithm** builds up a solution piece by piece, always choosing the next piece that offers the most immediate (local) benefit without reconsidering earlier choices. It operates under the hypothesis that locally optimal choices lead to a globally optimal solution.

---

## 0. Fundamental Concepts

### Core Properties required for Greedy Optimality

1. **Greedy Choice Property**: A globally optimal solution can be arrived at by making locally optimal (greedy) choices at each step without ever backtracking or revisiting past choices.
2. **Optimal Substructure**: An optimal solution to the overall problem contains within it optimal solutions to its subproblems.

> [!info] General Greedy Strategy
> 1. **Define Criterion**: Select a measure to rank candidates (e.g., ratio $\frac{v_i}{w_i}$, finish time $f_i$, profit $p_i$, start time $s_i$, frequency $f$).
> 2. **Sort Candidates**: Order inputs by the greedy criterion, typically in $O(n \log n)$ time.
> 3. **Iterate & Choose**: Walk through sorted candidates; take each candidate if it preserves solution feasibility.
> 4. **Never Reconsider**: Decisions are permanent (no backtracking).

---

## 1. Fractional Knapsack Problem

> [!note] Problem Definition
> Given $n$ items, each with a weight $w_i > 0$ and value $v_i > 0$, and a knapsack of capacity $W$. We wish to choose fractions $x_i \in [0, 1]$ of items to maximize the total value carried without exceeding capacity $W$.

$$\text{Maximize } \sum_{i=1}^{n} x_i v_i \quad \text{subject to} \quad \sum_{i=1}^{n} x_i w_i \le W \quad \text{where } 0 \le x_i \le 1$$

> [!warning] Contrast: Fractional vs 0/1 Knapsack
> - **Fractional Knapsack**: Items can be broken into arbitrary fractions ($x_i \in [0, 1]$). Solvable greedily in $O(n \log n)$.
> - **0/1 Knapsack**: Items must either be taken whole ($x_i = 1$) or left behind ($x_i = 0$). Greedy strategy **fails**; requires Dynamic Programming ($O(nW)$).

### Greedy Criterion
Calculate value per unit weight $r_i = \frac{v_i}{w_i}$ for each item. Select items in descending order of $r_i$.

### Code Implementation (C++)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Item {
    int id;
    double weight;
    double value;
    double ratio;
};

pair<double, vector<double>> fractionalKnapsack(vector<Item>& items, double capacity) {
    int n = items.size();
    
    // Step 1: Compute value-to-weight ratios - O(n)
    for (int i = 0; i < n; i++) {
        items[i].ratio = items[i].value / items[i].weight;
    }

    // Step 2: Sort items by ratio descending - O(n log n)
    sort(items.begin(), items.end(), [](const Item& a, const Item& b) {
        return a.ratio > b.ratio;
    });

    vector<double> x(n, 0.0);
    double current_weight = 0.0;
    double max_value = 0.0;

    // Step 3 & 4: Fill knapsack greedily - O(n)
    for (int i = 0; i < n; i++) {
        if (current_weight + items[i].weight <= capacity) {
            x[items[i].id] = 1.0;
            current_weight += items[i].weight;
            max_value += items[i].value;
        } else {
            double remaining = capacity - current_weight;
            x[items[i].id] = remaining / items[i].weight;
            max_value += x[items[i].id] * items[i].value;
            current_weight = capacity;
            break; // Knapsack is full
        }
    }

    return {max_value, x};
}
```

### Complexity Calculation & Step-by-Step Derivation

#### Time Complexity: $O(n \log n)$
1. **Ratio Computation**: Loop over $n$ items to calculate $r_i = \frac{v_i}{w_i}$. Requires $n$ divisions $\implies O(n)$ time.
2. **Sorting**: Sorting an array of $n$ items by ratio in descending order using `std::sort` $\implies O(n \log n)$ time.
3. **Greedy Selection Loop**: Iterate through sorted array. At each item, perform constant $O(1)$ arithmetic comparisons. At most $n$ iterations $\implies O(n)$ time.
4. **Total Time**: 
   $$T(n) = O(n) + O(n \log n) + O(n) = O(n \log n)$$

#### Space Complexity: $O(n)$
1. **Auxiliary Array**: Storing item structs with computed ratios requires $O(n)$ space.
2. **Fraction Vector $x$**: Storing the result vector of length $n$ requires $O(n)$ space.
3. **Total Space**: $S(n) = O(n)$.

---

## 2. Activity Selection Problem (Interval Scheduling)

> [!note] Problem Definition
> Given a set of $n$ activities $S = \{1, 2, \dots, n\}$, each with a start time $s_i$ and finish time $f_i$ (representing time interval $[s_i, f_i)$). Select the largest possible subset of mutually compatible activities. Two activities $i$ and $j$ are compatible if $s_i \ge f_j$ or $s_j \ge f_i$.

### Exploring Greedy Criteria

> [!question]- Why do other candidate greedy criteria fail?
> 1. **Earliest Start Time ($s_i$)**: Fails if an early starting activity spans a huge duration (e.g., $[1, 10)$ blocks $[2, 3)$ and $[4, 5)$).
> 2. **Shortest Duration ($f_i - s_i$)**: Fails if a short activity overlaps two larger non-overlapping activities (e.g., $[4, 6)$ blocks $[1, 5)$ and $[5, 9)$).
> 3. **Minimum Overlaps / Fewest Conflicts**: Fails in complex overlap structures where choosing a minimum overlap node breaks larger independent chains.
> 
> **Correct Criterion: Earliest Finish Time ($f_i$)**
> Selecting the activity that finishes first leaves the maximum possible remaining time for subsequent activities.

### Code Implementation (C++)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Activity {
    int id;
    int start;
    int finish;
};

vector<int> activitySelection(vector<Activity>& activities) {
    int n = activities.size();
    if (n == 0) return {};

    // Step 1: Sort activities by finish time ascending - O(n log n)
    sort(activities.begin(), activities.end(), [](const Activity& a, const Activity& b) {
        return a.finish < b.finish;
    });

    vector<int> selected;
    
    // Always select the first activity (earliest finish time)
    selected.push_back(activities[0].id);
    int last_finish = activities[0].finish;

    // Step 2: Linear pass over remaining activities - O(n)
    for (int i = 1; i < n; i++) {
        if (activities[i].start >= last_finish) {
            selected.push_back(activities[i].id);
            last_finish = activities[i].finish;
        }
    }

    return selected;
}
```

### Complexity Calculation & Step-by-Step Derivation

#### Time Complexity: $O(n \log n)$
1. **Sorting**: Ordering $n$ activities by finish time $f_i$ using `std::sort` takes $O(n \log n)$ time.
2. **Linear Pass**: Iterating through the $n$ sorted activities, comparing `start >= last_finish` in $O(1)$ time per step. Total pass time is $O(n)$.
3. **Total Time**:
   $$T(n) = O(n \log n) + O(n) = O(n \log n)$$
   *(Note: If pre-sorted by finish time, time complexity is $O(n)$).*

#### Space Complexity: $O(n)$
1. **Sorting Memory / Storage**: Storing activities takes $O(n)$ space.
2. **Output Vector `selected`**: Storing selected activity IDs requires $O(n)$ space in worst case.
3. **Total Space**: $S(n) = O(n)$.

---

### Proof of Correctness

> [!info] 🧠 Intuitive Explanation (Plain English)
> **The Free Time Analogy**: Imagine you want to attend as many events as possible in a day. If you pick an event that finishes at 10:00 AM instead of one that finishes at 1:00 PM, you free up your schedule 3 hours earlier! By always picking the event that finishes earliest, you leave the **maximum possible remaining time** to fit in more events later. Swapping an optimal event for our earliest-finishing event can never hurt your schedule.

> [!tip]- 📝 Exam-Ready Proof (Short & Concise)
> - **Setup**: Let $G = \{g_1, g_2, \dots, g_m\}$ be the Greedy set and $O = \{r_1, r_2, \dots, r_n\}$ be an Optimal set sorted by finish times. Assume $G \ne O$.
> - **First Mismatch**: Let $k$ be the first index where $g_k \ne r_k$.
> - **The Exchange**: Since Greedy picks the earliest finish time, $f(g_k) \le f(r_k)$. Replacing $r_k$ with $g_k$ in $O$ gives a valid schedule $O' = (O \setminus \{r_k\}) \cup \{g_k\}$ of size $n$, because $g_k$ finishes no later than $r_k$, leaving enough time for $r_{k+1}$.
> - **Contradiction**: Repeat swaps until $O$ matches $G$. If $n > m$, there must be an activity $r_{m+1} \in O$ starting after $g_m$. But Greedy only stops when no compatible activity remains, so $r_{m+1}$ cannot exist.
> - **Conclusion**: $m = n \implies |G| = |O|$. Greedy is optimal. $\blacksquare$

> [!success]- 🔬 Rigorous Mathematical Proof (Full Step-by-Step)
> ##### Step 1: Setup & Definition
> Let $G = \{g_1, g_2, \dots, g_m\}$ be the set of activities selected by Greedy, ordered by finish time: $f_{g_1} \le f_{g_2} \le \dots \le f_{g_m}$.
> Let $O = \{r_1, r_2, \dots, r_n\}$ be an optimal solution set, ordered by finish time: $f_{r_1} \le f_{r_2} \le \dots \le f_{r_n}$.
> Since $O$ is optimal, $n \ge m$. Assume $G \ne O$.
> 
> ##### Step 2: The First Mismatch
> Let $k$ be the first index where $g_k \ne r_k$:
> $G = \{g_1, \dots, g_{k-1}, g_k, \dots, g_m\}$ and $O = \{g_1, \dots, g_{k-1}, r_k, \dots, r_n\}$.
> 
> ##### Step 3: The Exchange Argument
> By definition of Greedy, $g_k$ has the earliest finish time among activities compatible with $g_{k-1}$.
> Since $r_k$ is compatible with $g_{k-1}$, Greedy had the choice of $r_k$, so $f_{g_k} \le f_{r_k}$.
> Construct $O' = (O \setminus \{r_k\}) \cup \{g_k\}$. Since $s_{r_{k+1}} \ge f_{r_k} \ge f_{g_k}$, $g_k$ does not conflict with $r_{k+1}$.
> Thus $O'$ is mutually compatible and $|O'| = |O| = n$, so $O'$ is optimal.
> 
> ##### Step 4: Iterative Replacement
> Repeat the exchange for all differing elements until $O'' = \{g_1, \dots, g_m, r_{m+1}, \dots, r_n\}$.
> 
> ##### Step 5: Termination & Contradiction
> If $n > m$, there exists $r_{m+1}$ starting after $g_m$. But Greedy only terminates when no remaining activity is compatible with $g_m$. Contradiction!
> 
> $\therefore m = n \implies |G| = |O|$. $\blacksquare$

---

## 3. Job Sequencing with Deadlines

> [!note] Problem Definition
> Given $n$ jobs, each taking **exactly 1 unit of time** to execute. Each job $i$ has a deadline $d_i \ge 1$ and a profit $p_i > 0$. At most one job can be executed in any time slot. A job earns its profit $p_i$ if and only if it completes by its deadline $d_i$ (i.e., scheduled in time slot $t \le d_i$).
> 
> **Goal**: Maximize the total profit earned.

### Greedy Criterion
Sort all jobs in descending order of profit $p_i$. For each job in profit order, assign it to the **latest possible free time slot** $t \le d_i$. If no such slot is free, skip the job.

### Code Implementation (C++)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Job {
    int id;
    int deadline;
    int profit;
};

pair<vector<int>, int> jobSequencing(vector<Job>& jobs) {
    int n = jobs.size();
    
    // Step 1: Sort jobs by profit descending - O(n log n)
    sort(jobs.begin(), jobs.end(), [](const Job& a, const Job& b) {
        return a.profit > b.profit;
    });

    int maxDeadline = 0;
    for (const auto& j : jobs) {
        maxDeadline = max(maxDeadline, j.deadline);
    }

    // Slots array (1-indexed time slots 1..maxDeadline, -1 indicates free)
    vector<int> slot(maxDeadline + 1, -1); 
    int totalProfit = 0;

    // Step 2 & 3: Scan jobs in profit order and find latest free slot
    for (int i = 0; i < n; i++) {
        for (int j = min(maxDeadline, jobs[i].deadline); j >= 1; j--) {
            if (slot[j] == -1) {
                slot[j] = jobs[i].id; // Place job
                totalProfit += jobs[i].profit;
                break;
            }
        }
    }

    return {slot, totalProfit};
}
```

---

### Complexity Calculation & Step-by-Step Derivation

#### Time Complexity Analysis

1. **Sorting Jobs by Profit**:
   - Sorting $n$ jobs by profit $p_i$ in descending order $\implies O(n \log n)$.

2. **Naive Slot Search Approach**:
   - For each of the $n$ jobs, we scan backward from $\min(d_i, d_{\max})$ down to slot $1$.
   - In the worst case, searching for a free slot takes $O(d)$ time, where $d = \min(n, \max d_i)$.
   - Performing this backward search for all $n$ jobs gives:
     $$T_{\text{naive}}(n) = O(n \log n) + O(n \cdot d) = O(n \cdot d)$$
   - When deadlines are large ($d \approx n$), the worst-case time is **$O(n^2)$**.

3. **Disjoint-Set Union (Union-Find) Speedup**:
   - Instead of scanning backward linearly, we maintain a Disjoint-Set Union (DSU) data structure over time slots.
   - Each set's parent represents the **latest available free slot** at or before that position.
   - When a job is placed in slot $t$, we union slot $t$ with slot $t-1$.
   - Slot lookup takes nearly constant time $O(\alpha(n))$ using path compression.
   - Total time with DSU speedup:
     $$T_{\text{DSU}}(n) = O(n \log n) + O(n \cdot \alpha(n)) = O(n \log n)$$

#### Space Complexity: $O(n + d)$
- Storing sorted jobs takes $O(n)$ space.
- Slot array of size $d = \min(n, \max d_i)$ takes $O(d)$ space.
- Overall Auxiliary Space: $O(n + d)$.

---

### Proof of Correctness

> [!info] 🧠 Intuitive Explanation (Plain English)
> **The Procrastination Analogy**: To make the most money, focus on the highest-paying tasks first. But **don't do a task early if you don't need to!** Postpone each high-paying task to the absolute latest available slot right before its deadline. That way, you lock in the big payout while keeping earlier slots wide open for tighter-deadline tasks.

> [!tip]- 📝 Exam-Ready Proof (Short & Concise)
> - **Setup**: Let $I$ be Greedy's job set and $J$ be an Optimal set. Assume $I \ne J$.
> - **Alignment**: Align shared jobs to identical slots in both schedules. Since neither set contains the other, there exists a job $a \in I \setminus J$ and a job $b \in J \setminus I$.
> - **Profit Comparison**: Let $a$ be the highest-profit job in $I \setminus J$ placed in slot $s$, and $b$ be the job in slot $s$ of $J$. Since Greedy processes jobs by descending profit, if $profit(b) > profit(a)$, Greedy would have considered $b$ before $a$ and placed $b$ in slot $s$, meaning $b \in I$. Contradiction! Thus, $profit(a) \ge profit(b)$.
> - **Swap**: Replacing $b$ with $a$ in slot $s$ gives a new optimal schedule $J' = (J \setminus \{b\}) \cup \{a\}$ with $profit(J') \ge profit(J)$.
> - **Conclusion**: Repeat swaps until $J$ becomes $I$. $profit(I) = profit(J) \implies$ Greedy is optimal. $\blacksquare$

> [!success]- 🔬 Rigorous Mathematical Proof (Full Step-by-Step)
> ##### Step 1: Setup
> Let $I$ be the job set selected by Greedy, and $J$ be the job set in an optimal solution. Assume $I \ne J$.
> 
> ##### Step 2: Mismatch Existence
> Neither set can be a proper subset of the other:
> - If $I \subsetneq J$, then $J$ contains extra jobs, meaning Greedy skipped a job it could have scheduled, which Greedy never does.
> - If $J \subsetneq I$, then $\text{profit}(I) > \text{profit}(J)$, contradicting $J$'s optimality.
> Thus, there exists $a \in I \setminus J$ and $b \in J \setminus I$.
> 
> ##### Step 3: Aligning Schedules
> Align shared jobs between Greedy schedule $S_i$ and Optimal schedule $S_j$. Moving jobs to earlier slots never breaks deadlines, giving aligned schedule $S_j'$.
> 
> ##### Step 4: Comparing Profits
> Let $a$ be the highest-profit job in $I \setminus J$ sitting in slot $s$ of $S_i$, and $b$ be the job in slot $s$ of $S_j'$.
> Suppose $\text{profit}(b) > \text{profit}(a)$. Greedy processes jobs in descending profit order, so Greedy considered $b$ before $a$.
> Since slot $s$ was free when Greedy placed $a$, slot $s$ was also free when Greedy processed $b$. Thus Greedy would have placed $b$ in slot $s$, so $b \in I$. Contradiction!
> Therefore, $\text{profit}(a) \ge \text{profit}(b)$.
> 
> ##### Step 5: The Swap
> Replace $b$ with $a$ in slot $s$ of $S_j'$ to form $J' = (J \setminus \{b\}) \cup \{a\}$.
> $\text{profit}(J') = \text{profit}(J) - \text{profit}(b) + \text{profit}(a) \ge \text{profit}(J)$.
> Repeat until $J = I$. $\therefore \text{profit}(I) = \text{profit}(J)$. $\blacksquare$

---

## 4. Interval Partitioning (Classroom Scheduling)

> [!note] Problem Definition
> Given $n$ lectures with start times $s_i$ and finish times $f_i$, schedule **all** lectures using the **minimum number of classrooms** such that no two lectures assigned to the same classroom overlap in time.
> 
> **Depth Definition**: The **depth** $d$ of a set of intervals is the maximum number of mutually overlapping lectures at any single point in time.

> [!danger] Counter-Example: Why Naive Room-by-Room Approach Fails
> - **Naive Approach**: Use Activity Selection to find max non-overlapping lectures for Room 1, repeat for Room 2, etc.
> - **Counter-Example**: Lectures: $(3,7), (5,9), (8,11), (12,13), (10,14), (16,18), (13,20)$.
>   - Naive assigns: Room 1 = $\{(3,7), (8,11), (12,13), (16,18)\}$, Room 2 = $\{(5,9), (10,14)\}$, Room 3 = $\{(13,20)\}$ $\implies$ **3 rooms used**.
>   - However, maximum depth is $d = 2$. Optimal solution only needs **2 rooms**. The naive approach wastes a room!

### Correct Greedy Strategy
1. **Sort all lectures by start time $s_i$ ascending**.
2. **Scan lectures in start-time order**.
3. For each lecture, if an already open classroom is free (its last assigned lecture finished on or before current start time $s_i$), assign the lecture to that room.
4. If no open classroom is free, **allocate a brand-new room**.

### Code Implementation (C++)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>
#include <unordered_map>

using namespace std;

struct Lecture {
    int id;
    int start;
    int finish;
};

pair<unordered_map<int, int>, int> intervalPartitioning(vector<Lecture>& lectures) {
    int n = lectures.size();
    if (n == 0) return {{}, 0};

    // Step 1: Sort lectures by start time ascending - O(n log n)
    sort(lectures.begin(), lectures.end(), [](const Lecture& a, const Lecture& b) {
        return a.start < b.start;
    });

    // Min-Heap stores pair<last_finish_time, room_id>
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> minHeap;
    unordered_map<int, int> roomAssignment;
    int roomCount = 0;

    // Step 2: Process each lecture in start-time order
    for (int i = 0; i < n; i++) {
        if (!minHeap.empty() && minHeap.top().first <= lectures[i].start) {
            // Reuse existing room (earliest available finish time)
            auto topRoom = minHeap.top();
            minHeap.pop();
            roomAssignment[lectures[i].id] = topRoom.second;
            minHeap.push({lectures[i].finish, topRoom.second});
        } else {
            // Open a brand-new room
            roomCount++;
            roomAssignment[lectures[i].id] = roomCount;
            minHeap.push({lectures[i].finish, roomCount});
        }
    }

    return {roomAssignment, roomCount};
}
```

---

### Complexity Calculation & Step-by-Step Derivation

#### Time Complexity: $O(n \log n)$
1. **Sorting by Start Time**: Ordering $n$ lectures by start time $s_i$ takes $O(n \log n)$.
2. **Min-Heap Operations**:
   - For each of the $n$ lectures:
     - Check minimum finish time at top of heap $\implies O(1)$.
     - `pop` and/or `push` on a min-heap (`std::priority_queue`) containing at most $d \le n$ rooms $\implies O(\log d) \le O(\log n)$.
   - Total heap operations cost $O(n \log n)$.
3. **Total Time**:
   $$T(n) = O(n \log n) + O(n \log n) = O(n \log n)$$

#### Space Complexity: $O(n)$
- Min-heap stores at most $d \le n$ elements $\implies O(d)$.
- `roomAssignment` map stores $n$ entries $\implies O(n)$.
- Total Auxiliary Space: $S(n) = O(n)$.

---

### Proof of Correctness

> [!info] 🧠 Intuitive Explanation (Plain English)
> **The Overlap Limit Analogy**: If 5 classes are all running at the exact same time at 2:00 PM, you physically cannot manage with fewer than 5 rooms! Greedy sorts by start time and reuses rooms whenever possible. It **only opens a new room** when every single existing room is currently occupied by an active class. Thus, Greedy never wastes rooms and uses exactly the minimum number of rooms forced by the maximum overlap.

> [!tip]- 📝 Exam-Ready Proof (Short & Concise)
> - **Lower Bound**: Let $d$ be the depth (max number of mutually overlapping lectures at any single instant). Any valid schedule requires $\ge d$ classrooms because $d$ overlapping lectures must be in $d$ distinct rooms.
> - **Upper Bound**: Suppose Greedy uses $d+1$ rooms, opening room $d+1$ for lecture $j$.
> - **Contradiction**: Greedy only opens room $d+1$ if all $d$ existing rooms are occupied by lectures starting before $j$ and ending after $s_j$. This means lecture $j$ PLUS $d$ active lectures overlap at time $s_j \implies d+1$ lectures overlap simultaneously. But max depth is $d$, a contradiction!
> - **Conclusion**: Greedy uses exactly $d$ rooms $\implies$ Greedy is optimal. $\blacksquare$

> [!success]- 🔬 Rigorous Mathematical Proof (Full Step-by-Step)
> ##### 1. Lower Bound Claim: $\text{Rooms Needed} \ge d$
> Let $d$ be the depth of lecture set $R$. There exists a time $t^*$ where $d$ lectures overlap.
> Since all $d$ lectures overlap at $t^*$, no two can share a room $\implies \text{Optimal Rooms} \ge d$.
> 
> ##### 2. Upper Bound Claim: Greedy Uses $\le d$ Rooms
> Suppose Greedy allocates room $d+1$ for lecture $j$.
> Let $s_j$ be the start time of lecture $j$.
> Room $d+1$ was opened because all $d$ existing rooms were occupied by lectures with finish times $f_k > s_j$.
> Since lectures were processed in start-time order, all $d$ active lectures started at or before $s_j$.
> Thus, at time $s_j$, lecture $j$ AND $d$ active lectures are running simultaneously, giving depth $\ge d+1$.
> This contradicts that depth is $d$.
> 
> $\therefore \text{Greedy Rooms} = d = \text{Optimal Rooms}$. $\blacksquare$

---

## 5. Huffman Encoding

> [!note] Problem Definition
> Given a text string containing symbols with known character frequencies $C = \{c_1, c_2, \dots, c_n\}$, construct a **prefix-free binary code** for each symbol that minimizes the total encoded bit length.

$$\text{Minimize Total Cost } B(T) = \sum_{c \in C} \text{freq}(c) \cdot d_T(c)$$
where $d_T(c)$ is the depth of character leaf node $c$ in binary tree $T$ (representing code length of $c$).

> [!info] Why Prefix-Free Matters
> In a **prefix-free code**, no code is a prefix of another. This guarantees unique, unambiguous decoding without delimiters.
> - *Example of non prefix-free failure*: $a = 0, b = 1, c = 01$. Encoded `0101` can decode as `abab` or `cc` (ambiguous!).
> - In a binary tree representation, prefix-free codes mean **all characters sit exclusively at leaf nodes**. Internal nodes carry no characters.

---

### Code Implementation (C++)

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <queue>
#include <unordered_map>

using namespace std;

// Tree Node structure
struct Node {
    char ch;
    int freq;
    Node* left;
    Node* right;

    Node(char character, int frequency) {
        ch = character;
        freq = frequency;
        left = right = nullptr;
    }
};

// Custom Comparator for Priority Queue (Min-Heap)
struct Compare {
    bool operator()(Node* a, Node* b) {
        return a->freq > b->freq;
    }
};

// Step 2: Build Huffman Tree using Min-Heap - O(n log n)
Node* buildHuffmanTree(const unordered_map<char, int>& freqMap) {
    priority_queue<Node*, vector<Node*>, Compare> minHeap;

    for (auto pair : freqMap) {
        minHeap.push(new Node(pair.first, pair.second));
    }

    // Perform n - 1 merges
    while (minHeap.size() > 1) {
        Node* x = minHeap.top(); minHeap.pop();
        Node* y = minHeap.top(); minHeap.pop();

        Node* z = new Node('\0', x->freq + y->freq);
        z->left = x;
        z->right = y;

        minHeap.push(z);
    }

    return minHeap.top(); // Root of tree
}

// Step 3: Generate Prefix Codes via Tree Traversal - O(n)
void assignCodes(Node* root, string code, unordered_map<char, string>& codeTable) {
    if (!root) return;

    if (!root->left && !root->right) {
        codeTable[root->ch] = (code == "") ? "0" : code;
        return;
    }

    assignCodes(root->left, code + "0", codeTable);
    assignCodes(root->right, code + "1", codeTable);
}

// Step 5: Decode Bitstring Bit by Bit - O(Length of Bitstring)
string decodeBitstring(Node* root, const string& bitstring) {
    string result = "";
    Node* curr = root;

    for (char bit : bitstring) {
        if (bit == '0') curr = curr->left;
        else curr = curr->right;

        if (!curr->left && !curr->right) { // Reached leaf node
            result += curr->ch;
            curr = root; // Restart at root for next character
        }
    }

    return result;
}
```

---

### Complexity Calculation & Step-by-Step Derivation

#### Time Complexity: $O(n \log n)$
1. **Frequency Computation**: Scanning input string of length $N \implies O(N)$.
2. **Initial Min-Heap Building**: Inserting $n$ leaf nodes into `std::priority_queue` $\implies O(n)$ time.
3. **Tree Merging Loop**:
   - The loop runs $n - 1$ times (creating $n - 1$ internal nodes).
   - In each iteration:
     - 2 `pop` operations $\implies 2 \times O(\log n)$.
     - 1 `push` operation $\implies O(\log n)$.
   - Loop cost: $(n - 1) \times O(\log n) = O(n \log n)$.
4. **Code Assignment & Tree Traversal**: DFS traversal over binary tree with $2n - 1$ total nodes $\implies O(n)$ time.
5. **Total Huffman Algorithm Time**:
   $$T(n) = O(N) + O(n \log n) = O(n \log n)$$

#### Space Complexity: $O(n)$
- Min-heap stores at most $n$ nodes $\implies O(n)$.
- Binary tree contains $n$ leaves $+ (n - 1)$ internal nodes $= 2n - 1$ nodes $\implies O(n)$ space.
- `codeTable` stores $n$ entries $\implies O(n)$.
- Total Auxiliary Space: $S(n) = O(n)$.

---

### Proof of Correctness

> [!info] 🧠 Intuitive Explanation (Plain English)
> **The Deepest Leaves Analogy**: Frequent characters (like 'E') should have short codes near the top of the tree, while rare characters (like 'Z') can have long codes deep in the tree. By repeatedly picking the two absolute rarest characters and merging them at the bottom of the tree, Greedy guarantees that rare symbols get long codes, pushing frequent symbols closer to the root to minimize total message length.

> [!tip]- 📝 Exam-Ready Proof (Short & Concise)
> - **Sibling Lemma**: Let $x, y$ be the two lowest frequency characters. In an optimal tree, $x$ and $y$ can be placed as sibling leaves at maximum depth without increasing tree cost.
> - **Induction Reduction**: Merge $x, y$ into parent node $z$ with $freq(z) = freq(x) + freq(y)$, reducing the problem size from $n$ to $n-1$ symbols.
> - **Cost Equation**: $Cost(T) = Cost(T') + freq(x) + freq(y)$, where $T'$ is the tree for $n-1$ symbols.
> - **Inductive Step**: By induction hypothesis, Huffman generates an optimal tree $T'$ for $n-1$ symbols. Since $Cost(T) = Cost(T') + freq(x) + freq(y)$, $T$ must also be optimal for $n$ symbols. $\blacksquare$

> [!success]- 🔬 Rigorous Mathematical Proof (Full Step-by-Step)
> ##### Lemma: Sibling Property
> Let $x, y$ be the two lowest frequency characters in $C$. There exists an optimal prefix tree for $C$ where $x$ and $y$ are siblings at maximum depth.
> *Proof*: Swap $x, y$ with maximum depth leaves $a, b$ in optimal tree $T$. Since $f(x) \le f(a)$ and $f(y) \le f(b)$, swapping can only decrease or preserve tree cost $B(T)$. Thus swapped tree $T''$ is optimal.
> 
> ##### Main Inductive Proof
> 1. **Base Case ($n \le 2$)**: Only one valid tree structure exists, which is trivially optimal.
> 2. **Inductive Hypothesis**: Assume `HUFFMAN` generates an optimal tree for $n-1$ frequencies.
> 3. **Reduction**: Merge lowest frequency nodes $x, y$ into node $z$ with $f(z) = f(x) + f(y)$, giving reduced set $C'$ of size $n-1$.
> 4. **Cost Equation**: $B(T) = \sum_{c \in C} f(c) d_T(c) = B(T') + f(x) + f(y)$.
> 5. **Conclusion**: Let $H$ be an optimal tree for $C$ with $x, y$ as siblings. Collapsing $x, y$ gives tree $H'$ for $C'$ with $B(H) = B(H') + f(x) + f(y)$.
>    Since $T'$ is optimal for $C'$, $B(T') \le B(H')$.
>    $B(T) = B(T') + f(x) + f(y) \le B(H') + f(x) + f(y) = B(H) \implies B(T) = B(H)$.
> 
> $\therefore \text{Huffman Tree } T \text{ is Optimal}$. $\blacksquare$

---

## 6. Summary Comparison Table

| Algorithm | Greedy Criterion | Time Complexity | Space Complexity | Optimality Proof Method |
| :--- | :--- | :--- | :--- | :--- |
| **Fractional Knapsack** | Ratio $v_i / w_i$ descending | $O(n \log n)$ | $O(n)$ | Greedy choice property |
| **Activity Selection** | Finish time $f_i$ ascending | $O(n \log n)$ | $O(n)$ | Exchange argument (contradiction) |
| **Job Sequencing** | Profit $p_i$ descending (latest slot) | $O(n \log n)$ with DSU | $O(n + d)$ | 5-step swap argument |
| **Interval Partitioning** | Start time $s_i$ ascending | $O(n \log n)$ | $O(n)$ | Lower bound vs tight bound (depth $d$) |
| **Huffman Encoding** | Frequency $f_i$ ascending (merge lowest 2) | $O(n \log n)$ | $O(n)$ | Structural induction & sibling lemma |
