# Sorting Algorithms — Complete Notes

_CSE 4403: Algorithms | Based on lecture slides + supplementary material_

---

## Table of Contents

- [[#Key Concepts]]
- [[#Bubble Sort]]
- [[#Insertion Sort]]
- [[#Selection Sort]]
- [[#Counting Sort]]
- [[#Radix Sort]]
- [[#Merge Sort]]
- [[#Quick Sort]]
- [[#Complexity Comparison]]

---

## Key Concepts

> **Sorting** = Rearranging items in ascending or descending order.

|Property|Definition|Examples|
|---|---|---|
|**In-place**|Uses O(1) extra space (modifies array directly)|Bubble, Insertion, Selection, Quick, Heap|
|**Not in-place**|Requires extra space|Merge, Counting, Radix|
|**Stable**|Equal elements maintain their original relative order|Bubble, Insertion, Counting, Merge|
|**Not stable**|Equal elements may be reordered|Selection, Quick, Heap|

### Two Categories

```
Sorting
├── Comparison-Based          → Lower bound: Ω(n log n)
│   ├── Bubble Sort
│   ├── Insertion Sort
│   ├── Selection Sort
│   ├── Quick Sort
│   ├── Merge Sort
│   └── Heap Sort
└── Non-Comparison-Based      → Can beat O(n log n)
    ├── Counting Sort
    └── Radix Sort
```

**Why non-comparison sorts?**  
Comparison-based sorts must compare elements with `if (a > b)` — these are _branches_. Modern CPUs use **branch prediction**; a wrong prediction flushes the pipeline and wastes cycles. Non-comparison sorts (like Counting Sort) use direct indexing `count[arr[i]]++` — no branches.

---

## Bubble Sort

### Idea

Make multiple passes. In each pass, compare adjacent pairs and swap if out of order. After pass `i`, the `i` largest elements are in their final positions at the end.

### Dry Run — `[5, 6, 1, 3]`

**Pass 1 (i=0), j runs 0..2:**

```
[5, 6, 1, 3]  j=0: 5<6, no swap
[5, 6, 1, 3]  j=1: 6>1, swap → [5, 1, 6, 3]
[5, 1, 6, 3]  j=2: 6>3, swap → [5, 1, 3, 6]  ← 6 settled
```

**Pass 2 (i=1), j runs 0..1:**

```
[5, 1, 3, 6]  j=0: 5>1, swap → [1, 5, 3, 6]
[1, 5, 3, 6]  j=1: 5>3, swap → [1, 3, 5, 6]  ← 5,6 settled
```

### Implementation (C++)

```cpp
// Basic version
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1])
                swap(arr[j], arr[j + 1]);
        }
    }
}

// Optimized: early exit if already sorted
void bubbleSortOptimized(vector<int>& arr) {
    int n = arr.size();
    bool swapped;
    for (int i = 0; i < n - 1; i++) {
        swapped = false;
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if (!swapped) break;  // Array already sorted
    }
}
```

### Complexity

|Case|Time|Reasoning|
|---|---|---|
|Best|O(n)|Already sorted → 1 pass, 0 swaps (with `swapped` flag)|
|Average|O(n²)|~n²/4 comparisons on average|
|Worst|O(n²)|Reverse sorted → n(n-1)/2 comparisons|
|Space|O(1)|In-place|

**Derivation:**  
Pass `i` does `(n - i - 1)` comparisons.  
Total = (n-1) + (n-2) + ... + 1 = **n(n-1)/2** = O(n²)

---

## Insertion Sort

### Idea

Build the sorted portion one element at a time. Take element at index `i`, shift all larger elements in the sorted portion one step right, then insert at the correct position. Like sorting playing cards in hand.

### Dry Run — `[12, 11, 13, 5, 6]`

```
i=1, key=11: [12, 11, 13, 5, 6] → insert → [11, 12, 13, 5, 6]
i=2, key=13: 12≤13, no shift     →          [11, 12, 13, 5, 6]
i=3, key=5:  shift 13,12,11      →          [5, 11, 12, 13, 6]
i=4, key=6:  shift 13,12,11      →          [5, 6, 11, 12, 13]
```

### Implementation (C++)

```cpp
void insertionSort(vector<int>& arr, int n) {
    for (int i = 1; i < n; ++i) {
        int key = arr[i];
        int j = i - 1;
        // Shift elements greater than key one position ahead
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```

### Complexity

|Case|Time|Reasoning|
|---|---|---|
|Best|O(n)|Already sorted → inner while never executes|
|Average|O(n²)|~n²/4 shifts|
|Worst|O(n²)|Reverse sorted → i shifts at step i|
|Space|O(1)|In-place|

**Derivation (worst case):**  
At step `i`, up to `i` shifts needed.  
Total = 1 + 2 + ... + (n-1) = **n(n-1)/2** = O(n²)

> **Practical note:** Insertion Sort is fast for small arrays (n ≤ ~20) and nearly-sorted data. Many real-world sort implementations (like `std::sort`) use it as a base case.

---

## Selection Sort

### Idea

In each pass `i`, find the minimum element in `arr[i..n-1]` and swap it into position `i`. After `i` passes, the first `i` elements are sorted.

### Dry Run — `[12, 11, 13, 5]`

```
i=0: min at index 3 (value 5)  → swap(arr[0], arr[3]) → [5, 11, 13, 12]
i=1: min at index 1 (value 11) → swap(arr[1], arr[1]) → [5, 11, 13, 12]
i=2: min at index 3 (value 12) → swap(arr[2], arr[3]) → [5, 11, 12, 13]
```

### Implementation (C++)

```cpp
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; ++i) {
        int min_idx = i;
        for (int j = i + 1; j < n; ++j) {
            if (arr[j] < arr[min_idx])
                min_idx = j;
        }
        swap(arr[i], arr[min_idx]);
    }
}
```

### Complexity

|Case|Time|Reasoning|
|---|---|---|
|All cases|O(n²)|Always scans entire unsorted portion|
|Space|O(1)|In-place|

**Note:** Selection Sort always does **exactly n(n-1)/2 comparisons** regardless of input — no best/average/worst distinction for comparisons. However, it does at most **n-1 swaps** (better than Bubble Sort's O(n²) swaps). Useful when write operations are expensive.

**Why not stable?** When we swap `arr[i]` with `arr[min_idx]`, it can move an element past an equal element.

What does not stable mean? 
- (<1, a>, <1, b>) might become (<1, b>, <1, a>) after the swap.

---

## Counting Sort

### Idea

Instead of comparing elements, count how many times each value appears, then use prefix sums to find each element's final sorted position. Exploit the fact that values are integers in a known range `[0, maxVal]`.

### Step-by-Step for `[1, 0, 1, 2, 4, 2]`

**Step 1 — Count frequency:**

```
Value:  0  1  2  3  4
Count:  1  2  2  0  1
```

**Step 2 — Prefix sum (each entry = count of elements ≤ that value):**

```
Value:  0  1  2  3  4
Prefix: 1  3  5  5  6
```

`prefix[k]` = number of elements ≤ k = 1-based index of last occurrence of k in sorted output.

**Step 3 — Build output (traverse input RIGHT TO LEFT for stability):**

```
i=5, arr[5]=2: place at prefix[2]-1 = 4, decrement prefix[2] → output[4]=2
i=4, arr[4]=4: place at prefix[4]-1 = 5, decrement prefix[4] → output[5]=4
i=3, arr[3]=2: place at prefix[2]-1 = 3, decrement prefix[2] → output[3]=2
... and so on
Final: [0, 1, 1, 2, 2, 4]
```

> **Why right-to-left = stable?** Elements equal in value appear in the same left-to-right order in output as in input, because we process from the back.

### Implementation (C++)

```cpp
vector<int> countSort(vector<int>& arr) {
    int n = arr.size();

    // Find max to size the count array
    int maxval = *max_element(arr.begin(), arr.end());

    vector<int> count(maxval + 1, 0);

    // Count frequency
    for (int i = 0; i < n; i++)
        count[arr[i]]++;

    // Compute prefix sum
    for (int i = 1; i <= maxval; i++)
        count[i] += count[i - 1];

    // Build output (right to left for stability)
    vector<int> ans(n);
    for (int i = n - 1; i >= 0; i--) {
        ans[count[arr[i]] - 1] = arr[i];
        count[arr[i]]--;
    }

    return ans;
}
```

### Complexity

| |Complexity|Reasoning|
|---|---|---|
|Time|O(n + m)|n = array size, m = range of values (maxval+1)|
|Space|O(n + m)|count array of size m, output array of size n|

**Derivation:**

- Finding max: O(n)
- Filling count: O(n)
- Prefix sum: O(m)
- Building output: O(n)
- Total: O(n + m)

### When NOT to use Counting Sort

|Integer size|Count array memory needed|
|---|---|
|16-bit|65,536 entries × 4B = 256 KB ✅|
|32-bit|4,294,967,296 entries × 4B = **16 GB** ❌|
|64-bit|2⁶⁴ entries × 8B ≈ **138 Exabytes** ❌❌|

> Rule of thumb: Only use when `m` (range) is O(n) or smaller.

---

## Radix Sort

### Idea

Sort integers digit by digit, from the **least significant digit (LSD) to most significant digit (MSD)**, using Counting Sort at each digit level. Because Counting Sort is stable, earlier passes' orderings are preserved.

### Why LSD and not MSD?

With LSD, later passes (on more significant digits) "override" earlier passes correctly. Stability ensures that when two numbers tie on the current digit, their relative order from the previous pass (on a less significant digit) is preserved.

### Example — `[53, 89, 150, 36, 633, 233, 170, 102]`

```
Pass 1 (ones digit):
  Input:  053 089 150 036 633 233 170 102
  Output: 150 170 102 053 633 233 036 089

Pass 2 (tens digit):
  Input:  150 170 102 053 633 233 036 089
  Output: 102 633 233 036 150 053 170 089

Pass 3 (hundreds digit):
  Input:  102 633 233 036 150 053 170 089
  Output: 036 053 089 102 150 170 233 633  ✅ Sorted
```

### Implementation (C++)

```cpp
int getMax(int arr[], int n) {
    int mx = arr[0];
    for (int i = 1; i < n; i++)
        if (arr[i] > mx) mx = arr[i];
    return mx;
}

// Counting sort by digit at position represented by exp (1, 10, 100, ...)
void countSort(int arr[], int n, int exp) {
    int output[n];
    int count[10] = {0};

    // Count occurrences of current digit
    for (int i = 0; i < n; i++)
        count[(arr[i] / exp) % 10]++;

    // Prefix sum
    for (int i = 1; i < 10; i++)
        count[i] += count[i - 1];

    // Build output (right to left for stability)
    for (int i = n - 1; i >= 0; i--) {
        output[count[(arr[i] / exp) % 10] - 1] = arr[i];
        count[(arr[i] / exp) % 10]--;
    }

    for (int i = 0; i < n; i++)
        arr[i] = output[i];
}

void radixSort(int arr[], int n) {
    int m = getMax(arr, n);
    // exp = 1, 10, 100, ... while m has that digit place
    for (int exp = 1; m / exp > 0; exp *= 10)
        countSort(arr, n, exp);
}
```

### Space-Time Tradeoff (Base Selection)

The base `b` used for digit grouping creates a tradeoff:

| Base `b`     | Count array size | Passes `d`        | Notes                     |
| ------------ | ---------------- | ----------------- | ------------------------- |
| 2 (binary)   | 2                | log₂(maxval)      | Fewest space, most passes |
| 10 (decimal) | 10               | log₁₀(maxval)     | Familiar, moderate        |
| 256 (byte)   | 256              | 4 for 32-bit ints | Good practical balance    |
| 65536        | 65536            | 2 for 32-bit ints | Fast but memory heavy     |

Example: 129 in base 10 needs 3 digits; in base 2 (binary) needs 8 digits. More passes = more time, but smaller count array = less space.

**b = 256 is often the best practical choice** for 32-bit integers.

### Complexity

| |Complexity|Notes|
|---|---|---|
|Time|O((n + b) × d) ≈ O(dn)|d = #digits in max value using base b; b is constant|
|Space|O(n + b)|output array + count array per pass|

**Derivation:**

- Each call to `countSort` = O(n + b) where b = base (10 here = constant)
- Number of calls = d = number of digits in max element = O(log_b(maxval))
- Total time = O(d × (n + b)) = O(dn) when b is a constant

**vs Comparison sorts:** When d is constant (e.g., all elements < 1000 → d=3), Radix Sort is O(n) — better than O(n log n).

---

## Merge Sort

### Idea

Divide and Conquer:

1. **Divide** the array in half
2. **Recursively sort** both halves
3. **Merge** the two sorted halves

```
[70, 30, 50, 10]
       ↓ divide
  [70,30]     [50,10]
   ↓ ↓          ↓ ↓
  [70] [30]  [50] [10]
   ↓ merge     ↓ merge
  [30,70]     [10,50]
       ↓ merge
  [10, 30, 50, 70]
```

### Implementation (C++)

```cpp
void merge(int arr[], int lo, int mid, int hi) {
    int n1 = mid - lo + 1;   // size of left half
    int n2 = hi - mid;        // size of right half

    vector<int> left(n1), right(n2);

    for (int i = 0; i < n1; i++) left[i] = arr[lo + i];
    for (int j = 0; j < n2; j++) right[j] = arr[mid + 1 + j];

    int i = 0, j = 0, k = lo;

    // Merge the two halves
    while (i < n1 && j < n2) {
        if (left[i] <= right[j])   // <= preserves stability
            arr[k++] = left[i++];
        else
            arr[k++] = right[j++];
    }

    while (i < n1) arr[k++] = left[i++];  // leftover left
    while (j < n2) arr[k++] = right[j++]; // leftover right
}

void mergeSort(int arr[], int lo, int hi) {
    if (lo < hi) {
        int mid = lo + (hi - lo) / 2;  // avoids overflow vs (lo+hi)/2
        mergeSort(arr, lo, mid);
        mergeSort(arr, mid + 1, hi);
        merge(arr, lo, mid, hi);
    }
}
```

> **`mid = lo + (hi - lo) / 2`** — prevents integer overflow that would occur with `(lo + hi) / 2` when both are large.

### Complexity

|Case|Time|Space|
|---|---|---|
|All cases|O(n log n)|O(n) auxiliary|

**Derivation (Recurrence):**

```
T(n) = 2T(n/2) + O(n)
```

- Divide into 2 subproblems of size n/2 → 2T(n/2)
- Merging two halves of total size n → O(n)

Using the **Master Theorem** (a=2, b=2, f(n)=n):  
log_b(a) = log₂(2) = 1 = degree of f(n)  
→ Case 2: T(n) = **O(n log n)**

**Recursion tree view:**

```
Level 0:       n            → n work
Level 1:    n/2 + n/2       → n work
Level 2:  n/4+n/4+n/4+n/4  → n work
...
Level log n:  n leaves      → n work
Total levels = log n → Total = n × log n = O(n log n)
```

**Space:** Each merge creates temporary arrays. At any point, the total auxiliary memory across the call stack = O(n).

---

## Quick Sort

### Idea

Divide and Conquer using a **pivot**:

1. Choose a pivot element
2. **Partition**: move elements < pivot to left, elements > pivot to right
3. Recursively sort left and right partitions

Unlike Merge Sort, Quick Sort sorts **in-place** (no auxiliary arrays).

### Lomuto Partition Scheme

```cpp
int partition(vector<int>& arr, int lo, int hi) {
    int pivot = arr[hi];  // last element as pivot
    int i = lo - 1;       // index of smaller element

    for (int j = lo; j < hi; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[hi]);  // place pivot in correct position
    return i + 1;
}

void quickSort(vector<int>& arr, int lo, int hi) {
    if (lo < hi) {
        int pi = partition(arr, lo, hi);
        quickSort(arr, lo, pi - 1);
        quickSort(arr, pi + 1, hi);
    }
}
```

### Complexity

|Case|Time|When|
|---|---|---|
|Best|O(n log n)|Pivot always splits array in half|
|Average|O(n log n)|Random data|
|Worst|O(n²)|Already sorted / all same elements (pivot always min or max)|
|Space|O(log n)|Recursion stack (best/average), O(n) worst|

**Derivation:**

- Best/average: Recurrence T(n) = 2T(n/2) + O(n) → O(n log n) (same as merge sort)
- Worst: Recurrence T(n) = T(n-1) + O(n) → T(n) = n + (n-1) + ... + 1 = **O(n²)**

**Mitigating worst case:** Use **random pivot** (`swap(arr[lo + rand()%(hi-lo+1)], arr[hi])` before partitioning) to make O(n²) astronomically unlikely.

**Why preferred over Merge Sort in practice?**  
Better cache locality (in-place), lower constant factors, O(log n) stack space vs O(n).

---

## Complexity Comparison

|Algorithm|Best|Average|Worst|Space|Stable|In-place|
|---|---|---|---|---|---|---|
|Bubble Sort|O(n)|O(n²)|O(n²)|O(1)|✅|✅|
|Insertion Sort|O(n)|O(n²)|O(n²)|O(1)|✅|✅|
|Selection Sort|O(n²)|O(n²)|O(n²)|O(1)|❌|✅|
|Counting Sort|O(n+m)|O(n+m)|O(n+m)|O(n+m)|✅|❌|
|Radix Sort|O(dn)|O(dn)|O(dn)|O(n+b)|✅|❌|
|Merge Sort|O(n log n)|O(n log n)|O(n log n)|O(n)|✅|❌|
|Quick Sort|O(n log n)|O(n log n)|O(n²)|O(log n)|❌|✅|

> **Lower bound theorem:** Any comparison-based sort requires Ω(n log n) comparisons in the worst case. This is why Counting/Radix sort (non-comparison) can be faster.

### Choosing the Right Sort

```
Is the range of values small (m ≈ n)?
├── Yes → Counting Sort or Radix Sort
└── No  → Is n small (≤ ~20)?
          ├── Yes → Insertion Sort
          └── No  → Is stability required?
                    ├── Yes  → Merge Sort
                    └── No   → Quick Sort (random pivot)
```

---

_Tags: #algorithms #sorting #complexity #cpp #DSA