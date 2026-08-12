# Algorithm Quiz Syllabus

---

## 1. Greedy Algorithms

### Big Picture
> **Core Idea:** At each step, make the choice that looks best right now, the locally optimal choice, without reconsidering earlier decisions, hoping it leads to a globally optimal solution.

Greedy algorithms are fast and simple but do not work for every optimization problem. For a greedy algorithm to work, the problem must exhibit two properties:
1. **Greedy Choice Property:** A globally optimal solution can be reached by choosing the locally optimal option at each step, without revisiting past choices.
2. **Optimal Substructure:** An optimal solution to the problem contains within it optimal solutions to its subproblems.

### The General Strategy
1. **Define the greedy criterion:** Pick a measure that ranks choices (e.g., a ratio, a finish time, a frequency).
2. **Sort or order the input:** Arrange candidates by that criterion, usually taking $O(n \log n)$ time.
3. **Iterate and choose:** Walk through candidates in order; take each one if it keeps the solution feasible.
4. **Never reconsider:** Once a choice is made (or rejected), it is final and no backtracking is needed.

---

### Fractional Knapsack
**Problem Statement:** Given $n$ items, each with a weight and a value, and a knapsack of capacity $W$, choose how much of each item to carry. The items may be broken into fractions to maximize total value without exceeding capacity.
*(Contrast with 0/1 Knapsack where greedy fails and DP is required).*

**Greedy Algorithm:**
1. **Compute ratios:** For each item $i$, compute $v_i / w_i$ ("value per unit weight").
2. **Sort descending:** Order all items by ratio, highest first.
3. **Fill greedily:** Take whole items in order while they fit in the remaining capacity.
4. **Take a fraction:** When the next item doesn't fully fit, take just enough of it to fill the knapsack, then stop.

---

### Interval Scheduling (Activity Selection)
**Problem Statement:** Given a single resource used by $n$ activities, each with a start time and a finish time, select the largest possible set of activities such that no two selected activities overlap.

**Why other sorting options fail (Counterexamples):**
- **Earliest start time:** Fails if one very long activity starts early and blocks many short activities.
- **Smallest request (shortest interval):** Fails if a short activity overlaps and blocks two non-overlapping larger activities.
- **Minimum overlaps/conflicts:** Fails in complex arrangements where picking the one with fewest conflicts blocks a denser independent sequence.

**Correct Greedy Strategy:**
1. **Sort by finish time:** Order all activities by finish time, ascending in $O(n \log n)$.
2. **Select the first:** Always take the activity that finishes earliest overall.
3. **Scan and compare:** For each next activity, compare its start time to the finish time of the last selected one.
4. **Select if compatible:** If `start >= last finish`, select it and update "last finish"; otherwise skip.

> [!abstract] Proof of Correctness (Activity Selection)
> Let $G$ = the set of intervals selected by the greedy algorithm, and $O$ = the set of intervals in some optimal solution. Assume $G \neq O$.
> Order both by finish time:
> $G = \{g_1, g_2, \ldots, g_m\}$
> $O = \{r_1, r_2, \ldots, r_n\}$ with $n \ge m$.
> 
> Let $k$ be the first index where they differ: $O = \{g_1, g_2, \ldots, g_{k-1}, r_k, \ldots, r_n\}$. 
> Since $g_k$ finishes before or at the same time as $r_k$, $g_k$ is compatible with $g_{k-1}$ and everything after $r_k$. We can replace $r_k$ with $g_k$ in $O$ without creating a conflict. Repeating this process converts $O$ to $G$, proving $G$ is also optimal.

---

### Job Sequencing
**Problem Statement:** Given $n$ jobs, each with a deadline and a profit, and each taking **exactly one unit of time**, schedule jobs (at most one at a time) to maximize total profit. A job earns profit only if completed by its deadline.

**Greedy Strategy:**
1. **Sort by profit:** Order all jobs by profit, descending, in $O(n \log n)$.
2. **Scan in profit order:** Take each job in turn, most profitable first.
3. **Find the latest free slot:** Search for the latest available time slot $i$ with $i \le \text{deadline}$.
4. **Place or skip:** If such a slot exists, schedule the job there; otherwise it earns no profit, so skip it.

```cpp
// Step 2: scan jobs in profit order
for (auto& j : jobs) {
    // Step 3: find the latest free slot i with i <= deadline (and i >= 1)
    for (int i = min(maxDeadline, j.deadline); i >= 1; i--) {
        if (slot[i] == -1) {
            // Step 4: place it
            slot[i] = j.id;
            totalProfit += j.profit;
            jobsScheduled++;
            break;
        }
        // else: slot taken, try the next earlier slot
    }
    // if no free slot was found, the job is simply skipped
}
```

---

## 2. Divide and Conquer

### Big Picture
> **Core Idea:** Break a big problem into smaller problems, solve those, and combine the results.

| Feature | Dynamic Programming | Divide & Conquer |
| :--- | :--- | :--- |
| **Subproblems** | Overlap | Independent |
| **Same problem twice?** | Yes — cache it | No — solve once |
| **Direction** | Bottom-up table | Top-down recursion |
| **Examples** | Coin change, Knapsack, LCS | Merge Sort, Binary Search |

### The General Template
Every D&C algorithm is just a different answer to: *"How do you split?"* and *"How do you combine?"*
- **Split:** Divide into independent subproblems. (Usually the dumb part, e.g., cut array in half).
- **Solve:** Recurse on each part.
- **Combine:** Merge results into the final answer. (Usually the smart part where the real work lives).

```python
def solve(problem):
    if problem is small enough:
        return base_case_solution
    
    left, right = split(problem)
    
    left_ans = solve(left)
    right_ans = solve(right)
    
    return combine(left_ans, right_ans)
```

---

### Master Theorem
For recurrences of the form:
$$T(n) = aT\left(\frac{n}{b}\right) + f(n)$$
where $a \ge 1$ and $b > 1$.

1. If $f(n) = O(n^c)$ where $c < \log_b a$, then $T(n) = \Theta(n^{\log_b a})$
2. If $f(n) = \Theta(n^c)$ where $c = \log_b a$, then $T(n) = \Theta(n^c \log n)$
3. If $f(n) = \Omega(n^c)$ where $c > \log_b a$, then $T(n) = \Theta(f(n))$

**Examples:**
- **Example 1:** $T(n) = 2T(n/2) + cn \implies \Theta(n \log_2 n)$ (Equal amount of work done in each level)
- **Example 2:** $T(n) = 2T(n/2) + c \implies \Theta(n)$ (Most of the work done in leaves)
- **Example 3:** $T(n) = 2T(n/2) + cn^2 \implies \Theta(n^2)$ (Most of the work done in root)

---

### Key Algorithms

#### 1. Binary Search
- **Split:** Find midpoint (trivial).
- **Combine:** No combine step. Just pick one half and discard the other.
```cpp
int solve(vector<int>& arr, int l, int r, int target) {
    if (l > r) return -1; // base case: not found
    int mid = (l + r) / 2;
    if (arr[mid] == target) return mid; // found!
    else if (arr[mid] > target) return solve(arr, l, mid-1, target); // left half
    else return solve(arr, mid+1, r, target); // right half
}
```

#### 2. Merge Sort
- **Split:** Cut array in half (trivial).
- **Combine:** Merge two sorted halves in $O(n)$ work.
- **Recurrence:** $T(n) = 2T(n/2) + O(n) \implies O(n \log n)$.
```cpp
void mergeSort(vector<int>& arr, int l, int r) {
    if (l >= r) return; // base case
    int mid = (l + r) / 2;
    mergeSort(arr, l, mid);     // sort left half
    mergeSort(arr, mid+1, r);   // sort right half
    merge(arr, l, mid, r);      // combine step
}
```

#### 3. Quick Sort (Lomuto's Partition)
- **Split:** Partition the array around a pivot (smart step). 
- **Combine:** No combine step. Sub-arrays are sorted in place.
```cpp
int partition(int arr[], int low, int high) {
    int pivot = arr[high]; // Choose the pivot
    int i = low - 1; // Index of smaller element
    for (int j = low; j <= high - 1; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(&arr[i], &arr[j]);
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return i + 1;
}

void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

#### 4. Maximum Sub-array Sum
- **Problem:** Given an array of integers (can be negative), find the contiguous sub-array with the largest sum.
- **D&C Approach:** Split the array at the midpoint. The maximum sub-array must be in one of three places: entirely in the left half, entirely in the right half, or crossing the midpoint.
```cpp
int findCrossing(vector<int>& arr, int l, int mid, int r) {
    int left_sum = INT_MIN, sum = 0;
    for (int i = mid; i >= l; i--) {
        sum += arr[i];
        left_sum = max(left_sum, sum);
    }
    int right_sum = INT_MIN; sum = 0;
    for (int i = mid+1; i <= r; i++) {
        sum += arr[i];
        right_sum = max(right_sum, sum);
    }
    return left_sum + right_sum;
}
```

#### 5. Longest Nice Substring
- **Problem:** A string is nice if, for every letter it contains, it appears in both uppercase and lowercase (e.g., `aAbBAAA`).
- **D&C Approach:** Scan the string. If a character is missing its pair, it **cannot** be part of any nice substring. The valid answer must reside entirely to the left or entirely to the right of this invalid character.
```cpp
string longestNiceSubstring(string s) {
    return solve(s, 0, s.size() - 1);
}

string solve(string& s, int l, int r) {
    if (r - l < 1) return ""; // need at least 2 chars
    set<char> present(s.begin() + l, s.begin() + r + 1);
    for (int i = l; i <= r; i++) {
        char c = s[i];
        // check if its pair is missing
        if (present.find(tolower(c)) == present.end() || 
            present.find(toupper(c)) == present.end()) {
            string left = solve(s, l, i - 1);
            string right = solve(s, i + 1, r);
            return left.size() >= right.size() ? left : right;
        }
    }
    return s.substr(l, r - l + 1); // whole segment is nice
}
```

---

## 3. Convex Hull

### Problem Statement
In geometry, the **convex hull** (or convex envelope) of a shape is the smallest convex set that contains it, much like a rubber band stretched tightly around a group of pins on a board.
- **Given:** $n$ points in a plane, $S = \{(x_i, y_i) \mid i = 1 \dots n\}$. No two have the same x or y, and no three are in a line.
- **Find:** The smallest convex polygon containing all points in $S$, outputted as a sequence of boundary points in clockwise order.

### Approaches

#### Brute Force
- **Idea:** For each pair of points, check if the line segment between them is on the convex hull. A line segment is on the hull if all other points fall on one side of the line.
- **Complexity:** $O(n^3)$ because there are $O(n^2)$ pairs of points, and we check all remaining $O(n)$ points against each pair.

#### Divide & Conquer
- **Idea:**
  1. Sort the points by x-coordinates.
  2. **Split:** Divide into left half $A$ and right half $B$.
  3. **Solve:** Compute $CH(A)$ and $CH(B)$ recursively.
  4. **Combine:** Find the upper and lower tangents that connect $CH(A)$ and $CH(B)$, and merge them into a single hull.
- **Two-Finger Algorithm (Merge step):**
  - Start with a line segment using the rightmost point of $A$ and the leftmost point of $B$.
  - Move clockwise on $B$ until it improves the tangent.
  - Move anti-clockwise on $A$ until it improves the tangent.
  - Repeat until the tangent converges (finds the upper tangent). Similarly find the lower tangent.
- **Time Complexity:** 
  - Sorting: $O(n \log n)$
  - Divide & Conquer: $T(n) = 2T(n/2) + O(n) \implies O(n \log n)$
  - Total Time: **$O(n \log n)$**

---
