---
title: Sorting Algorithms – CSE 4403
tags: [algorithms, sorting, cse4403]
date: 2026-07-01
lecturer: Anika Farzana
---

# Sorting Algorithms

> **Sorting** = rearranging items in an array/list into a specified order (ascending or descending).

---

## Key Properties

| Property | Meaning | Examples (In‑place / Stable) |
|----------|---------|-------------------------------|
| **In‑place** | Uses only constant extra space (modifies the given array directly). | ✅ Bubble, Insertion, Selection, Quick, Heap<br>❌ Merge, Counting, Radix |
| **Stable** | Preserves the relative order of equal elements. | ✅ Bubble, Insertion, Counting, Merge<br>❌ Selection, Quick, Heap |

---

## Branch Prediction & Non‑comparison Sorts

Most comparison‑based sorts (Bubble, Insertion, Selection, Merge, Quick) contain many branches:
```cpp
if (arr[j] > key) { ... }
```
Modern CPUs predict the branch; a mis‑prediction flushes the pipeline, wasting time.

**Counting Sort** avoids comparisons – it directly uses array indices:
```cpp
count[arr[i]]++
```

> [!tip] Branchless comparison example
> ```cpp
> int Smaller(int a, int b) {
>     if (a < b) return a; else return b;   // branch
> }
> int Smaller_Branchless(int a, int b) {
>     return a * (a < b) + b * (b <= a);    // no branch, but uses multiplication
> }
> ```

---

# 1. Bubble Sort

**Idea** – Repeatedly swap adjacent elements if they are in wrong order. After each pass, the largest unsorted element “bubbles” to its correct position.

```pseudocode
BubbleSort(arr):
    n = len(arr)
    for i = 0 to n-2:
        for j = 0 to n-i-2:
            if arr[j] > arr[j+1]:
                swap(arr[j], arr[j+1])
```

**Example** – `[5, 6, 1, 3]` → after pass 0: `[5, 1, 3, 6]`, after pass 1: `[1, 3, 5, 6]`.

| Complexity | Value |
|------------|-------|
| Time (worst/avg) | $O(n^2)$ |
| Time (best) | $O(n)$ (if optimised with flag) |
| Space | $O(1)$ |
| In‑place | ✅ |
| Stable | ✅ |

---

# 2. Insertion Sort

**Idea** – Build the sorted array one element at a time, like inserting a card into a sorted hand.

```pseudocode
InsertionSort(arr):
    n = len(arr)
    for i = 1 to n-1:
        key = arr[i]
        j = i-1
        while j >= 0 and arr[j] > key:
            arr[j+1] = arr[j]
            j--
        arr[j+1] = key
```

**Example** – `[12, 11, 13, 5, 6]`  
- i=1: insert 11 before 12 → `[11, 12, 13, 5, 6]`  
- i=2: 13 stays → `[11, 12, 13, 5, 6]`  
- i=3: insert 5 at front → `[5, 11, 12, 13, 6]`  
- i=4: insert 6 → `[5, 6, 11, 12, 13]`

| Complexity | Value |
|------------|-------|
| Time (worst/avg) | $O(n^2)$ |
| Time (best) | $O(n)$ |
| Space | $O(1)$ |
| In‑place | ✅ |
| Stable | ✅ |

---

# 3. Selection Sort

**Idea** – Repeatedly select the smallest element from the unsorted part and swap it with the first unsorted element.

```pseudocode
SelectionSort(arr):
    n = len(arr)
    for i = 0 to n-2:
        min_idx = i
        for j = i+1 to n-1:
            if arr[j] < arr[min_idx]:
                min_idx = j
        swap(arr[i], arr[min_idx])
```

**Example** – `[12, 11, 13, 5]`  
- i=0: min=5 → swap with 12 → `[5, 11, 13, 12]`  
- i=1: min=11 → no swap → `[5, 11, 13, 12]`  
- i=2: min=12 → swap with 13 → `[5, 11, 12, 13]`

| Complexity | Value |
|------------|-------|
| Time (all cases) | $O(n^2)$ |
| Space | $O(1)$ |
| In‑place | ✅ |
| Stable | ❌ |

---

# 4. Counting Sort

**Idea** – Non‑comparison sort. Count the frequency of each distinct value, then use prefix sums to place elements directly into sorted order. **Stable** when we iterate from right to left.

```pseudocode
CountingSort(arr, max_val):
    n = len(arr)
    count = array of size max_val+1, filled with 0

    // 1. Count occurrences
    for i = 0 to n-1:
        count[arr[i]]++

    // 2. Prefix sums (cumulative count)
    for i = 1 to max_val:
        count[i] += count[i-1]

    // 3. Build output array (stable)
    output = array of size n
    for i = n-1 downto 0:
        val = arr[i]
        output[ count[val] - 1 ] = val
        count[val]--

    // 4. Copy back
    for i = 0 to n-1:
        arr[i] = output[i]
```

**Step‑by‑step example** – `arr = [4, 2, 2, 8, 3, 3, 1]`, max = 8

| Step | Array / Count |
|------|---------------|
| Initial counts | `index: 0 1 2 3 4 5 6 7 8`<br>`count: 0 1 2 2 1 0 0 0 1` |
| Prefix sums | `count: 0 1 3 5 6 6 6 6 7` |
| Right‑to‑left placement | i=6, val=1 → output[0]=1, count[1]--<br>i=5, val=3 → output[4]=3, count[3]--<br>… eventually output = `[1,2,2,3,3,4,8]` |

**Complexity Derivation**:
- Counting frequencies: $O(n)$
- Prefix sum loop: $O(m)$ where $m = \text{max\_val}$
- Building output: $O(n)$
- Copy back: $O(n)$  
  **Total** = $O(n + m)$

| Complexity | Value |
|------------|-------|
| Time | $O(n + m)$ |
| Space | $O(n + m)$ |
| In‑place | ❌ |
| Stable | ✅ |

> [!warning] Memory explosion  
> For 32‑bit integers, $m = 2^{32} - 1$ → count array of ~16 GB. For 64‑bit, impossible. Counting Sort is only useful when the value range is small.

---

# 5. Radix Sort

**Idea** – Non‑comparison sort. Process digits from **least significant digit (LSD)** to most significant, using a stable sort (Counting Sort) at each digit.

```pseudocode
RadixSort(arr, base=10):
    // find maximum number to know number of digits
    max_val = max(arr)
    d = number of digits of max_val in given base

    for pos = 0 to d-1:          // pos = 0 (units), 1 (tens), ...
        // use Counting Sort to sort arr by digit at 'pos'
        CountingSortByDigit(arr, pos, base)
```

**Example** – `[538, 915, 036, 633, 233, 170, 102]` (base 10)

| Pass | Sorted by digit |
|------|-----------------|
| Units | `[170, 102, 633, 233, 915, 036, 538]` |
| Tens  | `[102, 915, 633, 233, 036, 538, 170]` |
| Hundreds | `[036, 102, 170, 233, 538, 633, 915]` → **sorted** |

**Complexity Derivation**:
- Each Counting Sort pass costs $O(n + b)$, where $b$ = base.
- We perform $d$ passes, where $d$ = max number of digits.
- **Total** = $O(d \cdot (n + b))$. Usually $b$ is constant, so $O(d \cdot n)$.

**Space** = $O(n + b)$ per pass.

**Base trade‑off**:
- Smaller base (e.g., 2) → less space ($b=2$) but more passes ($d$ increases).
- Larger base (e.g., 256) → more space but fewer passes. Often $b = 256$ is a good balance.

| Complexity | Value |
|------------|-------|
| Time | $O(d \cdot (n + b))$ |
| Space | $O(n + b)$ |
| In‑place | ❌ (uses Counting Sort internally) |
| Stable | ✅ (because Counting Sort is stable) |

---

# 6. Merge Sort

**Idea** – Divide & conquer. Recursively split the array in half, sort each half, then merge the two sorted halves.

```pseudocode
MergeSort(arr, lo, hi):
    if lo < hi:
        mid = lo + (hi - lo) / 2
        MergeSort(arr, lo, mid)
        MergeSort(arr, mid+1, hi)
        Merge(arr, lo, mid, hi)

Merge(arr, lo, mid, hi):
    n1 = mid - lo + 1
    n2 = hi - mid
    left = arr[lo .. mid]
    right = arr[mid+1 .. hi]

    i = 0, j = 0, k = lo
    while i < n1 and j < n2:
        if left[i] <= right[j]:
            arr[k++] = left[i++]
        else:
            arr[k++] = right[j++]

    // copy remaining elements
    while i < n1: arr[k++] = left[i++]
    while j < n2: arr[k++] = right[j++]
```

**Step‑by‑step recurrence derivation**:

Let $T(n)$ be the time to sort $n$ elements.
- Divide: $O(1)$
- Recursively sort two halves: $2 \cdot T(n/2)$
- Merge: $O(n)$

So the recurrence is:
$$T(n) = 2T(n/2) + O(n)$$

**Solve** using the recursion tree (or Master Theorem, case 2):
- At each level, total work = $O(n)$.
- There are $\log_2 n$ levels.
- Hence $T(n) = O(n \log n)$.

**Space**: The `Merge` step requires temporary arrays of total size $n$ per call, but only one merge happens at a time, so auxiliary space is $O(n)$.

| Complexity | Value |
|------------|-------|
| Time (all cases) | $O(n \log n)$ |
| Space | $O(n)$ |
| In‑place | ❌ |
| Stable | ✅ |

---

# 7. Quick Sort (Brief)

**Idea** – Divide & conquer. Pick a pivot, partition the array so that elements smaller than the pivot come before it and larger after, then recursively sort the two partitions.

```pseudocode
QuickSort(arr, lo, hi):
    if lo < hi:
        p = Partition(arr, lo, hi)
        QuickSort(arr, lo, p-1)
        QuickSort(arr, p+1, hi)

Partition(arr, lo, hi):   // Lomuto scheme
    pivot = arr[hi]
    i = lo - 1
    for j = lo to hi-1:
        if arr[j] <= pivot:
            i++
            swap(arr[i], arr[j])
    swap(arr[i+1], arr[hi])
    return i+1
```

| Complexity | Best / Avg | Worst |
|------------|-----------|-------|
| Time | $O(n \log n)$ | $O(n^2)$ (when pivot is always smallest/largest) |
| Space | $O(\log n)$ (recursion stack) | $O(n)$ |
| In‑place | ✅ |
| Stable | ❌ |

---

# Comparison Summary

| Algorithm | Time (Best)  | Time (Avg)   | Time (Worst) | Space       | In‑place | Stable |
| --------- | ------------ | ------------ | ------------ | ----------- | -------- | ------ |
| Bubble    | $O(n)$       | $O(n^2)$     | $O(n^2)$     | $O(1)$      | ✅        | ✅      |
| Insertion | $O(n)$       | $O(n^2)$     | $O(n^2)$     | $O(1)$      | ✅        | ✅      |
| Selection | $O(n^2)$     | $O(n^2)$     | $O(n^2)$     | $O(1)$      | ✅        | ❌      |
| Counting  | $O(n+m)$     | $O(n+m)$     | $O(n+m)$     | $O(n+m)$    | ❌        | ✅      |
| Radix     | $O(d(n+b))$  | $O(d(n+b))$  | $O(d(n+b))$  | $O(n+b)$    | ❌        | ✅      |
| Merge     | $O(n\log n)$ | $O(n\log n)$ | $O(n\log n)$ | $O(n)$      | ❌        | ✅      |
| Quick     | $O(n\log n)$ | $O(n\log n)$ | $O(n^2)$     | $O(\log n)$ | ✅        | ❌      |

> **Note**: $m$ = value range (Counting Sort), $b$ = base, $d$ = max digits (Radix Sort).