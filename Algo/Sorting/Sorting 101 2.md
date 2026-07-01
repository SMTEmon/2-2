***

# Sorting Algorithms & Complexity Analysis

> [!info] Key Sorting Concepts
> - **In-place Sorting:** An algorithm that uses $O(1)$ constant auxiliary space (modifies the original array directly).
> - **Stable Sorting:** When two equal elements appear in the same relative order in the sorted array as they did in the original unsorted array.
> - **Comparison vs. Non-Comparison Based:** 
>   - *Comparison-based* sorts (e.g., Merge Sort, Bubble Sort) compare elements (e.g., `if A[i] < A[j]`). They are generally bounded by an $\Omega(n \log n)$ time limit. Modern CPUs try to use *branch prediction* for these `if/else` conditions. If the CPU guesses incorrectly, the pipeline clears, wasting time.
>   - *Non-Comparison* sorts (e.g., Counting Sort) avoid conditional branches by using element values as direct indices or keys, making them faster under the right conditions.

---

## 1. Bubble Sort
Repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. Larger elements "bubble" to the end of the list.

> [!example] Pseudocode
> ```text
> function BubbleSort(A, n):
>     for i = 0 to n - 1:
>         swapped = false
>         // The last i elements are already sorted
>         for j = 0 to n - i - 2:
>             if A[j] > A[j + 1]:
>                 swap(A[j], A[j + 1])
>                 swapped = true
>         
>         // If no elements were swapped, the array is sorted
>         if not swapped:
>             break
> ```

> [!abstract] Complexity Analysis
> **Time Complexity:**
> - **Step 1:** The outer loop runs $n$ times.
> - **Step 2:** The inner loop runs $n - i - 1$ times.
> - **Step 3:** The total number of comparisons is the sum of the inner loop iterations: $(n-1) + (n-2) + \dots + 1 = \frac{n(n-1)}{2}$.
> - **Worst/Average Case:** $\frac{n^2 - n}{2} \implies O(n^2)$ (when the array is reverse sorted).
> - **Best Case:** Because of the `swapped` flag, if the array is already sorted, the inner loop runs once without swapping, and the algorithm breaks. $\implies O(n)$.
> 
> **Space Complexity:**
> - Only a few temporary variables (`swapped`, `i`, `j`) are allocated.
> - **Total Space:** $O(1)$ (In-place).

---

## 2. Insertion Sort
Builds the sorted array one element at a time, much like sorting a hand of playing cards. It takes the current element and inserts it into the correct position in the already sorted left-hand portion of the array.

> [!example] Pseudocode
> ```text
> function InsertionSort(A, n):
>     for i = 1 to n - 1:
>         key = A[i]
>         j = i - 1
>         
>         // Shift elements greater than key to the right
>         while j >= 0 and A[j] > key:
>             A[j + 1] = A[j]
>             j = j - 1
>             
>         A[j + 1] = key
> ```

> [!abstract] Complexity Analysis
> **Time Complexity:**
> - **Step 1:** The outer loop runs from $i = 1$ to $n-1$.
> - **Step 2:** In the **worst case** (reverse sorted), the `while` loop has to shift every element to the left of `i`. The number of shifts is $1 + 2 + 3 + \dots + (n-1) = \frac{n(n-1)}{2} \implies O(n^2)$.
> - **Step 3:** In the **best case** (already sorted), `A[j] > key` is immediately false. The `while` loop runs 0 times per outer iteration. Total comparisons = $n-1 \implies O(n)$.
> 
> **Space Complexity:**
> - Only the `key` and loop variables are stored.
> - **Total Space:** $O(1)$ (In-place).

---

## 3. Selection Sort
Divides the array into a sorted and unsorted region. Repeatedly scans the unsorted region to find the minimum element and swaps it into the first position of the unsorted region.

> [!example] Pseudocode
> ```text
> function SelectionSort(A, n):
>     for i = 0 to n - 2:
>         min_idx = i
>         
>         // Find the index of the minimum element in the unsorted part
>         for j = i + 1 to n - 1:
>             if A[j] < A[min_idx]:
>                 min_idx = j
>                 
>         // Swap it with the first element of the unsorted part
>         if min_idx != i:
>             swap(A[i], A[min_idx])
> ```

> [!abstract] Complexity Analysis
> **Time Complexity:**
> - **Step 1:** The outer loop runs $n-1$ times.
> - **Step 2:** The inner loop scans the remaining elements to find the minimum. Regardless of the array's initial order, it will *always* perform $(n-1) + (n-2) + \dots + 1$ comparisons.
> - **Step 3:** Total comparisons = $\frac{n(n-1)}{2} \implies O(n^2)$.
> - **Best/Worst/Average Case:** All are strictly $O(n^2)$ because there is no way to exit early.
> 
> **Space Complexity:**
> - Only index tracking variables are used.
> - **Total Space:** $O(1)$ (In-place).

---

## 4. Merge Sort
A classic Divide and Conquer algorithm. It recursively splits the array into two halves until arrays of size 1 are reached, then carefully merges the halves back together in sorted order.

> [!example] Pseudocode
> ```text
> function MergeSort(A, left, right):
>     if left < right:
>         mid = left + (right - left) / 2
>         MergeSort(A, left, mid)
>         MergeSort(A, mid + 1, right)
>         Merge(A, left, mid, right)
> 
> function Merge(A, left, mid, right):
>     // Create temporary arrays for the two halves
>     L = A[left ... mid]
>     R = A[mid+1 ... right]
>     
>     i = 0, j = 0, k = left
>     
>     // Compare elements and overwrite A with the smaller one
>     while i < length(L) and j < length(R):
>         if L[i] <= R[j]:
>             A[k++] = L[i++]
>         else:
>             A[k++] = R[j++]
>             
>     // Copy any remaining elements
>     while i < length(L): A[k++] = L[i++]
>     while j < length(R): A[k++] = R[j++]
> ```

> [!abstract] Complexity Analysis
> **Time Complexity:**
> - **Step 1 (Divide):** Finding the midpoint takes $O(1)$ time.
> - **Step 2 (Conquer):** We recursively solve 2 subproblems, each of size $n/2$. This yields the term $2T(n/2)$.
> - **Step 3 (Combine):** Merging two sorted arrays of combined size $n$ takes $O(n)$ time (one pass through both arrays).
> - **Recurrence Relation:** $T(n) = 2T(n/2) + O(n)$.
> - **Solving the Tree:**
>   - At level 0, we do $n$ work.
>   - At level 1, we have 2 nodes doing $n/2$ work $\implies 2 \times (n/2) = n$ work.
>   - The tree halves the array until size 1. The depth of the tree is $\log_2(n)$.
>   - Total work = (work per level) $\times$ (number of levels) $= n \times \log n \implies O(n \log n)$.
> - **Best/Worst/Average:** Always $O(n \log n)$.
> 
> **Space Complexity:**
> - The `Merge` function creates temporary arrays `L` and `R` that, combined, equal the size of the subarray being merged. 
> - At the top level, this requires an array of size $n$. 
> - **Total Space:** $O(n)$ (Not in-place).

---

## 5. Counting Sort
A non-comparison algorithm that works by counting the frequency of each distinct element, calculating prefix sums, and using them to place elements directly into their correct sorted positions.

> [!warning] A Big NO for large values!
> If sorting 64-bit integers, the count array would need size $2^{64}$, requiring about **138 Exabytes** of RAM! Counting Sort is only viable when the maximum value ($m$) is reasonably small compared to $n$.

> [!example] Pseudocode
> ```text
> function CountingSort(A, n):
>     // Step 1: Find max value to determine count array size
>     max_val = max(A)
>     
>     // Step 2: Initialize count array and result array
>     count = array of size (max_val + 1) initialized to 0
>     result = array of size n
>     
>     // Step 3: Store the frequency of each element
>     for i = 0 to n - 1:
>         count[A[i]]++
>         
>     // Step 4: Compute cumulative (prefix) sum
>     // This tells us the actual index of the element in the result array
>     for i = 1 to max_val:
>         count[i] += count[i - 1]
>         
>     // Step 5: Build result array iterating backwards (to maintain stability)
>     for i = n - 1 down to 0:
>         result[count[A[i]] - 1] = A[i]
>         count[A[i]]--
>         
>     return result
> ```

> [!abstract] Complexity Analysis
> Let $n$ be the number of elements and $m$ be `max_val`.
> **Time Complexity:**
> - **Step 1:** Finding the max value scans the array once: $O(n)$.
> - **Step 2:** Initializing arrays takes $O(m + n)$.
> - **Step 3:** Frequency counting takes $O(n)$.
> - **Step 4:** Prefix sum iterates through the count array: $O(m)$.
> - **Step 5:** Building output scans the original array once: $O(n)$.
> - **Total Time:** $O(n + n + m + n) \implies O(n + m)$.
> 
> **Space Complexity:**
> - We allocate `count` of size $m + 1$ and `result` of size $n$.
> - **Total Space:** $O(n + m)$.

---

## 6. Radix Sort
Sorts elements digit by digit, starting from the least significant digit (LSD) to the most significant digit (MSD). It uses Counting Sort as a stable subroutine for sorting each digit.

> [!example] Pseudocode
> ```text
> function RadixSort(A, n):
>     // Find the max number to know the number of digits (d)
>     max_val = max(A)
>     
>     // Run counting sort for every digit. 
>     // exp is 10^i (1, 10, 100, etc.)
>     exp = 1
>     while (max_val / exp) > 0:
>         CountingSortByDigit(A, n, exp)
>         exp = exp * 10
> 
> function CountingSortByDigit(A, n, exp):
>     // Same as Counting sort, but instead of counting A[i], 
>     // we count (A[i] / exp) % 10 (the specific digit)
>     // Count array size is fixed at 10 (base 10)
>     ...
> ```

> [!abstract] Complexity Analysis
> Let $n$ be the number of elements, $b$ be the base (e.g., base 10), and $d$ be the maximum number of digits in the elements.
> **Time Complexity:**
> - **Step 1:** The `while` loop runs exactly $d$ times.
> - **Step 2:** Inside the loop, `CountingSort` takes $O(n + b)$ time.
> - **Total Time:** $d \times O(n + b) \implies O(d \cdot (n + b))$. 
> - *Note: If $d$ is constant and $b$ is small, it essentially behaves as $O(n)$.*
> 
> **Space Complexity:**
> - The underlying `CountingSort` requires an output array of size $n$ and a count array of size $b$.
> - **Total Space:** $O(n + b)$.

> [!info] Space-Time Tradeoff
> We can change the base $b$. Instead of base 10, we could use base 2 (binary) or base 256. 
> - A smaller base (e.g., base 2) means the count array is small $\implies$ **Low Space**. But the number of digits $d$ increases drastically $\implies$ **High Time (more iterations)**.
> - A larger base (e.g., base 256) means $d$ shrinks $\implies$ **Low Time (fewer iterations)**. But the count array becomes larger $\implies$ **High Space**. Base 2^8 (256) is often a great balance.

---

## Summary Table

| Algorithm | Best Time | Avg Time | Worst Time | Space Complexity | Stable? | In-Place? |
| :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **Bubble Sort** | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Yes | Yes |
| **Insertion Sort** | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Yes | Yes |
| **Selection Sort** | $O(n^2)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | No | Yes |
| **Merge Sort** | $O(n \log n)$ | $O(n \log n)$| $O(n \log n)$| $O(n)$ | Yes | No |
| **Counting Sort** | $O(n + m)$ | $O(n + m)$ | $O(n + m)$ | $O(n + m)$ | Yes | No |
| **Radix Sort** | $O(d(n + b))$| $O(d(n + b))$| $O(d(n + b))$| $O(n + b)$ | Yes | No |

*(Note: $m$ = maximum value in the array. $d$ = max number of digits. $b$ = base system used).*