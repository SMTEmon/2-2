---
title: Divide and Conquer 2 - Maximum Subarray Sum & Longest Nice Substring
date: 2026-08-12
tags:
  - algorithms
  - divide-and-conquer
  - maximum-subarray
  - longest-nice-substring
  - complexity-analysis
  - proofs
  - exam-prep
aliases:
  - D&C Part 2
---

# Divide and Conquer 2: Maximum Subarray Sum & Longest Nice Substring

> [!abstract] Overview
> In this module, we tackle two classic problem types—**Maximum Subarray Sum** and **Longest Nice Substring**—using Divide and Conquer. Both problems illustrate how splitting a linear data structure (array/string) into subproblems requires special handling during the **Combine phase** to account for elements spanning across the divide.
> 
> *For a full step-by-step execution trace and recursion tree diagram, see [[Divide and Conquer - Maximum Subarray Sum Simulation]].*

---

## 1. Maximum Subarray Sum Problem

> [!question] Problem Statement
> Given an array of integers `arr` (which may contain both positive and negative values), find the **contiguous sub-array** that produces the **maximum sum**, and return that sum.
>
> **Example**:
> Array: `[-2, -3, 4, -1, -2, 1, 5, -3]`
> Maximum Subarray: `[4, -1, -2, 1, 5]` at indices `2..6`.
> Maximum Sum: $4 + (-1) + (-2) + 1 + 5 = \mathbf{7}$.

---

### 1.1 The Divide & Conquer Strategy

When we divide the array `arr[l..r]` into two halves at `mid = (l + r) / 2`, any optimal contiguous subarray must lie in **exactly one of three places**:

1. **Entirely in the Left Half** (`arr[l..mid]`)
2. **Entirely in the Right Half** (`arr[mid+1..r]`)
3. **Crossing the Midpoint** (starts at or before `mid`, ends at or after `mid+1`)

> [!tip] The Key Insight for Crossing Subarray
> A crossing subarray **must include** both `arr[mid]` and `arr[mid+1]`.
> * The left part of the crossing sum expands from `mid` **leftwards** to `l`.
> * The right part of the crossing sum expands from `mid + 1` **rightwards** to `r`.

---

### 1.2 Pseudocode & Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>

using namespace std;

// Helper function to find the maximum sum of a sub-array crossing the midpoint
int findCrossing(const vector<int>& arr, int l, int mid, int r) {
    // 1. Find max sum expanding leftward from mid
    int left_sum = INT_MIN;
    int sum = 0;
    for (int i = mid; i >= l; i--) {
        sum += arr[i];
        left_sum = max(left_sum, sum);
    }

    // 2. Find max sum expanding rightward from mid+1
    int right_sum = INT_MIN;
    sum = 0;
    for (int i = mid + 1; i <= r; i++) {
        sum += arr[i];
        right_sum = max(right_sum, sum);
    }

    // 3. Combine left best suffix and right best prefix
    return left_sum + right_sum;
}

// Main D&C function
int maxSubArraySum(const vector<int>& arr, int l, int r) {
    // Base Case: Only one element
    if (l == r) {
        return arr[l];
    }

    int mid = l + (r - l) / 2;

    // Recurse on left and right halves
    int left_max  = maxSubArraySum(arr, l, mid);
    int right_max = maxSubArraySum(arr, mid + 1, r);

    // Find max crossing sum directly in O(n)
    int cross_max = findCrossing(arr, l, mid, r);

    // Combine: Return the maximum of the three possibilities
    return max({left_max, right_max, cross_max});
}
```

---

### 1.3 Correctness Proof of Maximum Subarray Sum

#### 1. Intuitive Proof (Plain English)
Suppose you cut an array into two pieces (Left Half and Right Half) using a vertical dividing line at `mid`. Now, pick *any* contiguous subarray `arr[i..j]`:
* Does `arr[i..j]` end **at or before** the cut line? If yes, it is **entirely in the left half**.
* Does `arr[i..j]` start **at or after** the cut line? If yes, it is **entirely in the right half**.
* Does `arr[i..j]` start to the left of the cut line and end to the right of the cut line? If yes, it **straddles across the cut line**.

Since these three options cover **100% of all possible contiguous subarrays** (there are no other places a contiguous subarray could exist), if we find the maximum subarray in the left half, the maximum subarray in the right half, and the maximum subarray crossing the middle, **the largest of these three must be the overall maximum subarray**.

#### 2. Formal Logical Proof (Exhaustive Case Analysis)
Let $S_{optimal} = \text{arr}[i^* \dots j^*]$ be an optimal contiguous subarray that maximizes the sum in $\text{arr}[l \dots r]$.
Let $mid = \lfloor (l + r) / 2 \rfloor$.
For any index pair $(i^*, j^*)$ with $l \le i^* \le j^* \le r$, exactly one of three mutually exclusive and exhaustive cases holds:
1. **Case 1**: $j^* \le mid$. Then $S_{optimal} \subseteq \text{arr}[l \dots mid]$. The recursive call `maxSubArraySum(arr, l, mid)` will correctly evaluate to $\text{sum}(S_{optimal})$.
2. **Case 2**: $i^* > mid$. Then $S_{optimal} \subseteq \text{arr}[mid+1 \dots r]$. The recursive call `maxSubArraySum(arr, mid+1, r)` will correctly evaluate to $\text{sum}(S_{optimal})$.
3. **Case 3**: $i^* \le mid < j^*$. $S_{optimal}$ crosses $mid$.
   * Since $S_{optimal}$ is contiguous, it contains $\text{arr}[i^* \dots mid]$ (a suffix of $\text{arr}[l \dots mid]$) and $\text{arr}[mid+1 \dots j^*]$ (a prefix of $\text{arr}[mid+1 \dots r]$).
   * $\text{sum}(S_{optimal}) = \sum_{k=i^*}^{mid} \text{arr}[k] + \sum_{k=mid+1}^{j^*} \text{arr}[k]$.
   * `findCrossing()` independently maximizes the left suffix sum $\sum_{k=i}^{mid} \text{arr}[k]$ and the right prefix sum $\sum_{k=mid+1}^{j} \text{arr}[k]$. Since addition is independent, maximizing both terms yields the exact optimal crossing sum.

Taking the maximum of the three cases $\max(\text{left\_max}, \text{right\_max}, \text{cross\_max})$ guarantees finding $\text{sum}(S_{optimal})$. $\blacksquare$

> [!tip] 3. Exam-Ready Proof (Fast to write on paper)
> **Goal**: Prove D&C correctly finds max subarray sum for `arr[l..r]`.
> * **Partition**: Midpoint `mid` splits any contiguous subarray `arr[i..j]` into 3 mutually exclusive cases:
>   1. $j \le mid$ (Entirely in left half).
>   2. $i > mid$ (Entirely in right half).
>   3. $i \le mid < j$ (Crosses `mid`).
> * **Correctness**:
>   * Cases 1 & 2 are solved recursively by IH.
>   * Case 3 MUST include `arr[mid]` and `arr[mid+1]`. Max crossing sum $=$ (Max suffix sum of `arr[l..mid]`) $+$ (Max prefix sum of `arr[mid+1..r]`), computed in $O(n)$.
> * **Conclusion**: Taking $\max(\text{Left}, \text{Right}, \text{Crossing})$ covers all 100% possible subarray positions.

---

### 1.4 Detailed Trace of `findCrossing`

> [!warning] Starting Index Matters!
> When calculating `left_sum`, the loop **must** start at `mid` and step backwards (`i--`).
> When calculating `right_sum`, the loop **must** start at `mid + 1` and step forwards (`i++`).
> This guarantees that the combined sub-array remains **contiguous**.

Consider `arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]`, with `l = 0, r = 8, mid = 4`:
* `mid` element is `arr[4] = -1`.
* Left loop computes cumulative sums from `arr[4]` to `arr[0]`: `[-1, 3, 0, 1, -1]`. Max left suffix sum = $3$ (from subarray `[4, -1]`).
* Right loop computes cumulative sums from `arr[5]` to `arr[8]`: `[2, 3, -2, 2]`. Max right prefix sum = $3$ (from subarray `[2, 1]`).
* `cross_max` = $3 + 3 = 6$ (from subarray `[4, -1, 2, 1]`).

---

### 1.5 Step-by-Step Complexity Calculation

#### 1. Time Complexity Recurrence
Let $T(n)$ be the time taken for array of size $n$:
$$T(n) = 2 T\left(\frac{n}{2}\right) + \Theta(n)$$

Where:
* $2T(n/2)$ represents the recursive calls for `left_max` and `right_max`.
* $\Theta(n)$ is the linear time taken by `findCrossing()` (left loop takes $n/2$ iterations, right loop takes $n/2$ iterations $\implies n$ operations total).

#### 2. Master Theorem Evaluation
For $T(n) = aT(n/b) + f(n)$:
* $a = 2, b = 2, f(n) = \Theta(n)$
* $n^{\log_b a} = n^{\log_2 2} = n^1 = n$
* Since $f(n) = \Theta(n^{\log_b a})$, this falls under **Case 2 of Master Theorem**.
* Solution: $T(n) = \mathbf{\Theta(n \log n)}$.

#### 3. Space Complexity
* **Auxiliary Space**: $O(1)$ extra space (no additional dynamically allocated arrays).
* **Recursion Stack**: $O(\log n)$ call frames on the stack due to tree height $\log_2 n$.
* **Total Space Complexity**: $\mathbf{O(\log n)}$.

---

## 2. Longest Nice Substring Problem

> [!question] Problem Statement
> A string $s$ is called **nice** if, for every letter of the alphabet present in $s$, both its **uppercase and lowercase** forms appear in $s$.
> 
> Return the **longest nice substring** of $s$. If there are multiple, return the one that occurs earliest. If no nice substring exists, return `""`.
> 
> **Examples**:
> * `s = "aAbBAAA"` $\to$ `"aAbBAAA"` (Nice string because 'a'/'A' and 'b'/'B' both appear).
> * `s = "Abc"` $\to$ `""` (Not nice).
> * `s = "abBxCDScsedEad"` $\to$ `"cCDScs"` (Substrings exist that are nice).

---

### 2.1 Trivial / Brute Force Approach

> [!info] Brute Force Complexity
> 1. Total possible non-empty substrings: $\frac{n(n+1)}{2} = \Theta(n^2)$.
> 2. Checking if a substring is nice takes $O(n)$ time using a character set.
> 3. Total Brute Force Time Complexity: $\mathbf{O(n^3)}$ (or $O(n^2)$ with sliding window set checks).

---

### 2.2 Divide & Conquer Strategy

> [!tip] The Partition Pivot Principle
> If a character $c \in s$ exists such that **its counter-character** (lowercase if $c$ is uppercase, or vice versa) is **missing from the string**, then **no nice substring can ever include $c$**.
> 
> Therefore, $c$ acts as an **impassable barrier/pivot**! We can split the string at index $i$ (where $s[i] = c$) into left and right substrings, and solve recursively.

---

### 2.3 Pseudocode & Implementation

```cpp
#include <iostream>
#include <string>
#include <unordered_set>
#include <algorithm>

using namespace std;

string solveNice(const string& s, int l, int r) {
    // Base Case: Less than 2 characters can never be nice
    if (r - l + 1 < 2) return "";

    // Step 1: Collect set of all unique characters in current range s[l..r]
    unordered_set<char> charSet(s.begin() + l, s.begin() + r + 1);

    // Step 2: Search for an invalid character lacking its pair
    for (int i = l; i <= r; i++) {
        char c = s[i];
        char opposite = islower(c) ? toupper(c) : tolower(c);

        // If opposite character is missing, split around index i
        if (charSet.find(opposite) == charSet.end()) {
            string left  = solveNice(s, l, i - 1);
            string right = solveNice(s, i + 1, r);

            // Return the longer nice substring (earliest in case of tie)
            return (left.size() >= right.size()) ? left : right;
        }
    }

    // Step 3: If all characters have their pair, the entire range s[l..r] is nice!
    return s.substr(l, r - l + 1);
}

string longestNiceSubstring(string s) {
    if (s.length() < 2) return "";
    return solveNice(s, 0, s.length() - 1);
}
```

---

### 2.4 Correctness Proof of Longest Nice Substring

#### 1. Intuitive Proof (Plain English)
Imagine you have a string like `"aA b B x C c"`. Notice the character `'x'`. There is no `'X'` anywhere in the string.
* Can any valid "nice" substring contain `'x'`? **No!** By definition, if a nice substring includes `'x'`, it must also include `'X'`. But `'X'` doesn't exist in the entire section!
* This means `'x'` is like a **brick wall**. Any nice substring must exist **entirely to the left of `'x'`** or **entirely to the right of `'x'`**. It can *never* cross over `'x'`.
* Therefore, splitting the string at `'x'` into left and right pieces and solving each recursively is guaranteed not to destroy any valid nice substring!

#### 2. Formal Proof (by Contradiction)
Let $S = s[l \dots r]$ be the current string segment. Suppose character $c = s[k]$ (for some $k \in [l, r]$) has no counter-character in $S$ (i.e., $\text{toupper}(c) \notin S$ or $\text{tolower}(c) \notin S$).
* Assume, for the sake of contradiction, that there exists a valid nice substring $N = s[i \dots j]$ ($l \le i \le j \le r$) such that $i \le k \le j$ (i.e., $N$ contains index $k$).
* Since $N \subseteq S$, every character in $N$ is also present in $S$.
* Since $N$ is nice, $c \in N \implies \text{counter}(c) \in N$.
* Since $N \subseteq S$, it follows that $\text{counter}(c) \in S$.
* However, this contradicts our premise that $\text{counter}(c) \notin S$!
* Thus, no nice substring can contain index $k$. Every nice substring in $s[l \dots r]$ must either be contained in $s[l \dots k-1]$ or in $s[k+1 \dots r]$. Splitting at $k$ is sound and complete. $\blacksquare$

> [!tip] 3. Exam-Ready Proof (Fast to write on paper)
> **Goal**: Prove splitting at character $s[k]$ without a pair is valid.
> * **Definition**: $s[k]$ lacks its uppercase/lowercase pair in string $S$.
> * **Proof by Contradiction**:
>   * Suppose a nice substring $N$ contains index $k$.
>   * By definition of a nice string, $N$ must contain $\text{pair}(s[k])$.
>   * Since $N \subseteq S$, string $S$ must contain $\text{pair}(s[k])$.
>   * Contradiction! (We established $S$ lacks $\text{pair}(s[k])$).
> * **Conclusion**: No nice substring contains index $k$. Splitting into $S[l..k-1]$ and $S[k+1..r]$ is correct.

---

### 2.5 Step-by-Step Complexity Analysis

#### 1. Time Complexity
* **Best Case (No splits needed)**: The whole string is already nice. We scan the string once to build the set and check pairs $\implies \mathbf{O(n)}$.
* **Average Case (Even splits)**: If an invalid character occurs near the middle, the recurrence is $T(n) = 2T(n/2) + O(n) \implies \mathbf{O(n \log n)}$.
* **Worst Case Analysis**:
  * **Single-Pivot Implementation (Slide 15 Code)**: If the string consists of repeated identical invalid characters (e.g., `s = "aaaaa..."` with 35 `'a'`s and no `'A'`), splitting on the first `'a'` at index 0 leaves 34 `'a'`s, then 33 `'a'`s, etc. The recursion depth reaches $N$, taking $\mathbf{O(n^2)}$ time in the naive single-element split case.
  * **Optimized / Multi-Pivot Implementation**: If we split across **all occurrences of invalid character types simultaneously** (or remove the entire alphabet letter type in one step):
    1. Each recursive step permanently eliminates at least **one unique letter of the alphabet** ($a..z$) from child branches.
    2. Since there are only $K = 26$ letters in the English alphabet, the recursion depth is strictly bounded by **$\le 26$ levels**.
    3. Total time bound: $\text{Work per level } O(n) \times 26 \text{ levels} = \mathbf{O(26n) = O(n)}$.

#### 2. Space Complexity
* **Auxiliary Space**: $O(U)$ per frame where $U \le 52$ (size of `unordered_set`).
* **Recursion Stack**: Stack depth is at most $O(n)$ worst-case, or $O(\log n)$ average-case.
* **Total Space Complexity**: $\mathbf{O(n)}$ worst case due to call stack.
