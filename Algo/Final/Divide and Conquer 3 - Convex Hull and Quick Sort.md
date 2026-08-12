---
title: Divide and Conquer 3 - Convex Hull & Quick Sort
date: 2026-08-12
tags:
  - algorithms
  - divide-and-conquer
  - convex-hull
  - quick-sort
  - computational-geometry
  - complexity-analysis
  - proofs
  - exam-prep
aliases:
  - D&C Part 3
---

# Divide and Conquer 3: Convex Hull & Quick Sort

> [!abstract] Overview
> This note covers two advanced applications of Divide and Conquer:
> 1. **Convex Hull (D&C)**: A fundamental computational geometry algorithm that combines two sub-hulls using upper/lower tangents in linear time $O(n)$.
> 2. **Quick Sort**: A partition-based sorting algorithm where the **SPLIT step does all the smart work** ($O(n)$), while the **COMBINE step is free** ($O(1)$).

---

## 1. Convex Hull Problem (Geometric Divide & Conquer)

> [!info] Geometric Definition
> In geometry, the **Convex Hull** $CH(S)$ of a set of points $S$ in a 2D plane is the smallest convex polygon that contains all points in $S$. 
> 
> *Analogy*: Think of placing pins at all point locations on a wooden board and stretching a tight rubber band around them. The outer shape formed by the rubber band is the convex hull.

```
          p2
        .   .
     p1       . p3
    .  p6   p7 .
     .        .
       p5---p4
```

---

### 1.1 Problem Statement & Assumptions

* **Input**: A set of $n$ points $S = \{(x_i, y_i) \mid i = 1, 2, \dots, n\}$.
* **Assumptions** (for simplicity):
  1. No two points have the same $x$-coordinate.
  2. No two points have the same $y$-coordinate.
  3. No three points are collinear.
* **Output**: The sequence of boundary points forming the convex polygon $CH(S)$ in **clockwise order**.

---

### 1.2 Brute Force Approach

> [!note] Brute Force Algorithm
> 1. For every pair of points $(p_i, p_j)$:
>    * Construct a line segment connecting $p_i$ and $p_j$.
>    * Check if all remaining $n - 2$ points lie entirely on **one side** of the directed line.
>    * If yes $\implies$ $(p_i, p_j)$ is an edge of the Convex Hull.
> 2. Total pairs = $\binom{n}{2} = O(n^2)$. Checking $n-2$ points per pair takes $O(n)$ time.
> 3. **Brute Force Time Complexity**: $\mathbf{O(n^3)}$ (can be reduced to $O(n^2)$ with Graham Scan precursor).

---

### 1.3 Divide & Conquer Convex Hull Algorithm

> [!tip] The D&C Strategy
> 1. **Pre-processing (Sort)**: Sort all points by $x$-coordinate in $O(n \log n)$ time.
> 2. **Divide**: Split sorted set $S$ into left half $A$ and right half $B$ by the median vertical line.
> 3. **Conquer**: Recursively compute $CH(A)$ and $CH(B)$. Base case ($n \le 3$) is solved via brute force in $O(1)$ time.
> 4. **Combine (Merge)**: Find the **Upper Tangent** and **Lower Tangent** connecting $CH(A)$ and $CH(B)$, then discard the inner vertices ("Cut and Paste" method).

---

### 1.4 The Two-Finger Algorithm for Finding Tangents

To merge $CH(A)$ and $CH(B)$, we must find the line segment connecting a point in $A$ to a point in $B$ such that all points of both hulls lie below it (**Upper Tangent**), and another such that all points lie above it (**Lower Tangent**).

```
   CH(A)                       CH(B)
    a4-------(Upper Tangent)-------b2
   /  \                           /  \
  a3   a5                       b1   b3
   \  /                           \  /
    a2-------(Lower Tangent)-------b4
```

> [!example]- Two-Finger Algorithm Pseudocode (Upper Tangent)
> ```python
> function findUpperTangent(CH_A, CH_B):
>     # Start with rightmost point of A and leftmost point of B
>     a = rightmost_point(CH_A)
>     b = leftmost_point(CH_B)
>     
>     done = False
>     while not done:
>         done = True
>         
>         # 1. Move b clockwise on CH_B while line (a, b) intersects CH_B
>         while line_intersects_hull_above(a, b, CH_B):
>             b = clockwise_next(b, CH_B)
>             done = False
>             
>         # 2. Move a counter-clockwise on CH_A while line (a, b) intersects CH_A
>         while line_intersects_hull_above(a, b, CH_A):
>             a = counter_clockwise_next(a, CH_A)
>             done = False
>             
>     return (a, b) # Upper tangent line segment
> ```

---

### 1.5 Correctness Proof of Convex Hull Merge

#### 1. Intuitive Proof (Plain English)
Imagine two separate, non-overlapping solid islands $A$ and $B$ (representing $CH(A)$ and $CH(B)$). You want to wrap a single gigantic rubber band around both islands together:
1. You anchor a rope between the rightmost point of $A$ and leftmost point of $B$.
2. If the rope cuts through island $B$, you slide your end on island $B$ **upwards (clockwise)** until it clears island $B$.
3. If the rope now cuts through island $A$, you slide your end on island $A$ **upwards (counter-clockwise)** until it clears island $A$.
4. Repeating this "walking" process up both hulls naturally brings the rope to the **highest possible bridge** (Upper Tangent) where both islands lie completely underneath it.
5. Doing the opposite for the bottom bridge yields the Lower Tangent. Removing all the inner points between these two bridges leaves the perfect combined outer shell!

#### 2. Formal Proof (Convergence & Tangency)
* **Definition**: A directed line $L(a,b)$ from $a \in CH(A)$ to $b \in CH(B)$ is an upper tangent if all vertices of $CH(A)$ and $CH(B)$ lie on or to the right (below) of $L(a,b)$.
* **Monotonicity**: $CH(A)$ and $CH(B)$ are strictly convex polygons separated by a vertical line $x = x_{mid}$.
* The height $y$-intercept of the line $L(a,b)$ at $x = x_{mid}$ is a convex function over the cyclic vertex orderings of $CH(A)$ and $CH(B)$.
* The two-finger algorithm performs coordinate descent on this height function. Since the function is strictly convex, coordinate descent cannot get stuck in local minima and is guaranteed to converge to the global maximum $y$-intercept, which corresponds uniquely to the upper tangent. $\blacksquare$

> [!tip] 3. Exam-Ready Proof (Fast to write on paper)
> **Goal**: Prove Two-Finger algorithm finds Upper Tangent between disjoint $CH(A)$ and $CH(B)$.
> * **Definition**: Upper tangent $(a, b)$ has all points of $CH(A) \cup CH(B)$ lying below line $L(a, b)$.
> * **Separation**: Vertical line $x = x_{mid}$ separates $A$ and $B$.
> * **Convexity & Convergence**:
>   * Slope/intercept of segment $(a, b)$ is a unimodal (convex) function over clockwise vertex iteration.
>   * Walking $b$ clockwise on $CH(B)$ and $a$ counter-clockwise on $CH(A)$ monotonically increases line height at $x_{mid}$.
>   * Terminates when no point lies above $L(a, b)$, guaranteeing global upper tangent.

---

### 1.6 Cut and Paste Merge Step

Once the Upper Tangent $(a_u, b_u)$ and Lower Tangent $(a_l, b_l)$ are found:
1. Traverse $CH(A)$ clockwise from $a_u$ to $a_l$.
2. Jump across the Lower Tangent to $b_l$.
3. Traverse $CH(B)$ clockwise from $b_l$ to $b_u$.
4. Jump across the Upper Tangent back to $a_u$.
5. All internal points between the two tangents are discarded!

---

### 1.7 Complexity Analysis of D&C Convex Hull

#### 1. Time Complexity Recurrence
* **Sorting Step**: $O(n \log n)$ performed once at the start.
* **Divide & Conquer Phase**:
  $$T(n) = 2 T\left(\frac{n}{2}\right) + O(n)$$
  * $2 T(n/2)$ for left and right hull subproblems.
  * $O(n)$ for Two-Finger tangent search (each point on the hull boundary is walked over at most a constant number of times).
* **Master Theorem**: Case 2 applies $\implies T(n) = \Theta(n \log n)$.
* **Total Time Complexity**: $\mathbf{O(n \log n)}$.

#### 2. Space Complexity
* **Auxiliary Space**: $O(n)$ space to store polygon boundary vertices.
* **Recursion Stack**: $O(\log n)$ stack frames.
* **Total Space Complexity**: $\mathbf{O(n)}$.

---

## 2. Quick Sort Algorithm

> [!info] The Dual of Merge Sort
> While Merge Sort has a **trivial split** ($O(1)$) and a **smart combine** ($O(n)$), Quick Sort has a **smart split (partitioning)** ($O(n)$), while the **COMBINE step is free** ($O(1)$).

---

### 2.1 Pseudocode & Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Lomuto's Partition Scheme
int partition(vector<int>& arr, int low, int high) {
    // Select the rightmost element as the pivot
    int pivot = arr[high];
    
    // Index of smaller element (indicates right boundary of elements < pivot)
    int i = low - 1;
    
    // Traverse elements from low to high - 1
    for (int j = low; j <= high - 1; j++) {
        // If current element is smaller than the pivot
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    
    // Place pivot in its correct sorted position
    swap(arr[i + 1], arr[high]);
    return i + 1; // Return pivot index
}

// QuickSort Recursive Function
void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        // pi is the partitioning index; arr[pi] is now at right place
        int pi = partition(arr, low, high);
        
        // Recursively sort elements before and after partition
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

---

### 2.2 Trace of Lomuto's Partitioning

Consider `arr = [10, 80, 30, 90, 40, 50, 70]`, `low = 0, high = 6`:
* Pivot = `arr[6] = 70`.
* Initial `i = -1`.

| $j$ | `arr[j]` | `arr[j] < 70`? | Action | Array State | $i$ |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | 10 | Yes | `i++ (0)`, `swap(arr[0], arr[0])` | `[10, 80, 30, 90, 40, 50, 70]` | 0 |
| 1 | 80 | No | None | `[10, 80, 30, 90, 40, 50, 70]` | 0 |
| 2 | 30 | Yes | `i++ (1)`, `swap(arr[1], arr[2])` | `[10, 30, 80, 90, 40, 50, 70]` | 1 |
| 3 | 90 | No | None | `[10, 30, 80, 90, 40, 50, 70]` | 1 |
| 4 | 40 | Yes | `i++ (2)`, `swap(arr[2], arr[4])` | `[10, 30, 40, 90, 80, 50, 70]` | 2 |
| 5 | 50 | Yes | `i++ (3)`, `swap(arr[3], arr[5])` | `[10, 30, 40, 50, 80, 90, 70]` | 3 |
| - | - | Loop Ends | `swap(arr[i+1], arr[high])` | `[10, 30, 40, 50, 70, 90, 80]` | 3 |

* Returned pivot index `pi = i + 1 = 4`. Element `70` is now at index 4 in its final sorted position.

---

### 2.3 Correctness Proof of Lomuto Partition

#### 1. Intuitive Proof (Plain English)
Imagine sorting a line of people by height relative to a chosen target height (the pivot):
1. Index $i$ acts as a **fence position** dividing the line into "shorter people" (to the left of $i$) and "taller/equal people" (between $i+1$ and $j-1$).
2. Pointer $j$ walks down the line person by person.
3. Whenever $j$ finds a person shorter than the pivot, we expand the fence ($i++$) and swap that short person into the short section.
4. When $j$ reaches the end, everyone before index $i+1$ is strictly shorter than the pivot, and everyone from $i+1$ to the end is taller/equal to the pivot.
5. Placing the pivot at $i+1$ puts it in its **exact, permanent sorted position**!

#### 2. Formal Proof (by Loop Invariants)
* **Loop Invariant**: At the start of each iteration of the `for` loop (line 14) for index $j$:
  1. For all indices $k \in [\text{low} \dots i]$, $\text{arr}[k] < \text{pivot}$.
  2. For all indices $k \in [i+1 \dots j-1]$, $\text{arr}[k] \ge \text{pivot}$.
  3. $\text{arr}[\text{high}] = \text{pivot}$.
* **Initialization**: Before the first iteration, $i = \text{low} - 1$ and $j = \text{low}$. The ranges $[\text{low} \dots i]$ and $[i+1 \dots j-1]$ are empty. The invariant holds trivially.
* **Maintenance**:
  * Case A ($\text{arr}[j] \ge \text{pivot}$): $j$ increments. $\text{arr}[j-1]$ is $\ge \text{pivot}$, so range $[i+1 \dots j-1]$ maintains condition 2. Invariant holds.
  * Case B ($\text{arr}[j] < \text{pivot}$): $i$ increments, `swap(arr[i], arr[j])` places the small element into index $i$, and the large element previously at $i$ into index $j$. Incrementing $j$ restores condition 1 and condition 2 for all elements. Invariant holds.
* **Termination**: Upon termination, $j = \text{high}$. Thus:
  * All elements in $[\text{low} \dots i]$ are $< \text{pivot}$.
  * All elements in $[i+1 \dots \text{high}-1]$ are $\ge \text{pivot}$.
  * Swapping $\text{arr}[i+1]$ with $\text{arr}[\text{high}]$ places $\text{pivot}$ at $i+1$, satisfying the partition property. $\blacksquare$

> [!tip] 3. Exam-Ready Proof (Fast to write on paper)
> **Goal**: Prove `Partition(arr, low, high)` correctly places pivot `arr[high]`.
> * **Loop Invariant**: At loop index $j$:
>   * `arr[low..i] < pivot`
>   * `arr[i+1..j-1] >= pivot`
>   * `arr[high] == pivot`
> * **Init**: $i = low-1, j = low$ (ranges empty, invariant holds).
> * **Step**:
>   * If `arr[j] < pivot`: $i++$, swap `arr[i]` with `arr[j]`. Keeps small elements $\le i$.
>   * If `arr[j] >= pivot`: do nothing. Keeps large elements $> i$.
> * **Termination**: At $j=high$, swap `arr[i+1]` with `arr[high]`. Pivot is now at $i+1$, with all smaller elements to left and larger to right.

---

### 2.4 Comprehensive Complexity Analysis of Quick Sort

#### 1. Best-Case Time Complexity
* **Condition**: Pivot always splits array into two equal halves ($n/2$ and $n/2$).
* **Recurrence**: $T(n) = 2T(n/2) + \Theta(n)$.
* **Result**: $\mathbf{\Omega(n \log n)}$.

#### 2. Average-Case Time Complexity
* **Condition**: Random pivot choices yielding balanced split ratios on average (e.g., 9:1 or 3:1 splits).
* **Recurrence**: $T(n) = T(9n/10) + T(n/10) + \Theta(n)$.
* **Recursion Tree Depth**: $\log_{10/9} n = O(\log n)$. Total work per level $= O(n)$.
* **Result**: $\mathbf{\Theta(n \log n)}$.

#### 3. Worst-Case Time Complexity
* **Condition**: Array is already sorted (ascending or descending) and pivot is picked as the extreme element (`arr[high]`).
* **Split**: One subproblem has $0$ elements, the other has $n - 1$ elements.
* **Recurrence**:
  $$T(n) = T(n - 1) + \Theta(n) = T(n - 2) + (n - 1) + n = \sum_{k=1}^{n} k = \frac{n(n + 1)}{2} = \mathbf{O(n^2)}$$

> [!important] Mitigating Quick Sort Worst-Case
> To prevent $O(n^2)$ worst-case behavior, use **Randomized QuickSort** (pick pivot at random index) or **Median-of-Three** pivot selection.

#### 4. Space Complexity Analysis
* Quick Sort works **in-place** without extra arrays for data elements.
* **Call Stack Depth**:
  * **Best/Average Case**: $O(\log n)$ call stack frames.
  * **Worst Case**: $O(n)$ call stack frames (unbalanced tree).
* **Total Space Complexity**: $\mathbf{O(\log n)}$ average, $\mathbf{O(n)}$ worst case.
