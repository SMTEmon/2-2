***

# Sorting Algorithms & Complexity Analysis

> [!info] Key Sorting Concepts
> - **In-place Sorting:** An algorithm that uses $O(1)$ constant auxiliary space. It modifies the original array directly rather than creating a new one (e.g., Bubble, Insertion, Selection).
> - **Stable Sorting:** When two equal elements appear in the same relative order in the sorted array as they did in the original unsorted array.
> - **Comparison vs. Non-Comparison Based:** 
>   - *Comparison-based* sorts (e.g., Merge Sort, Bubble Sort) compare elements using conditions like `if (A[i] < A[j])`. Modern CPUs use **branch prediction** for these `if/else` statements. If the CPU guesses wrong, the pipeline clears, wasting time.
>   - *Non-Comparison* sorts (e.g., Counting Sort) avoid conditional branches by using element values directly as array indices (e.g., `count[arr[i]]++`). This avoids branch prediction penalties entirely.

---

## 1. Bubble Sort
Repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. In each pass, the largest unsorted element "bubbles" to its correct position at the end.

> [!example] Pseudocode (Optimized)
> ```text
> function BubbleSort(A, n):
>     for i = 0 to n - 1:
>         swapped = false
>         // The last i elements are already in place
>         for j = 0 to n - i - 2:
>             if A[j] > A[j + 1]:
>                 swap(A[j], A[j + 1])
>                 swapped = true
>         
>         // If no elements were swapped in this pass, array is sorted
>         if not swapped:
>             break
> ```

> [!abstract] Complexity Analysis
> **Time Complexity:**
> - **Step-by-step:** The outer loop runs up to $n$ times. The inner loop runs $n - i - 1$ times. The total number of comparisons is an arithmetic series: $(n-1) + (n-2) + \dots + 1 = \frac{n(n-1)}{2}$.
> - **Worst/Average Case:** $\frac{n^2 - n}{2} \implies \mathbf{O(n^2)}$ (Occurs when the array is reverse sorted).
> - **Best Case:** Thanks to the `swapped` boolean flag, if the array is already sorted, the inner loop runs exactly once without making any swaps, and the algorithm breaks immediately. $\implies \mathbf{O(n)}$.
> 
> **Space Complexity:**
> - Only a few temporary variables (`swapped`, `i`, `j`) are allocated.
> - **Total Space:** $\mathbf{O(1)}$ (In-place).

---

## 2. Insertion Sort
Builds the sorted array one element at a time, exactly like sorting a hand of playing cards. It takes the current element and shifts all larger elements in the sorted left-hand portion to the right to make room for it.

> [!example] Pseudocode
> ```text
> function InsertionSort(A, n):
>     for i = 1 to n - 1:
>         key = A[i]
>         j = i - 1
>         
>         // Move elements greater than key one position ahead
>         while j >= 0 and A[j] > key:
>             A[j + 1] = A[j]
>             j = j - 1
>             
>         A[j + 1] = key
> ```

> [!abstract] Complexity Analysis
> **Time Complexity:**
> - **Step-by-step:** The outer `for` loop runs from $i = 1$ to $n-1$. The inner `while` loop checks and shifts elements.
> - **Worst/Average Case:** In reverse order, every element must be shifted to the very beginning. The number of shifts is $1 + 2 + 3 + \dots + (n-1) = \frac{n(n-1)}{2} \implies \mathbf{O(n^2)}$.
> - **Best Case:** If the array is already sorted, `A[j] > key` evaluates to false immediately. The `while` loop runs 0 times per outer iteration. $\implies \mathbf{O(n)}$.
> 
> **Space Complexity:**
> - Only the `key` and index variables are stored.
> - **Total Space:** $\mathbf{O(1)}$ (In-place).
> 
> *Note:* Insertion sort is preferred over Bubble sort for small or nearly sorted arrays because it minimizes actual full swapping operations in favor of simple overwrites (shifts).

---

## 3. Selection Sort
Divides the array into a sorted and unsorted region. Repeatedly scans the unsorted region to find the exact minimum element, then swaps it with the first element of the unsorted region.

> [!example] Pseudocode
> ```text
> function SelectionSort(A, n):
>     for i = 0 to n - 2:
>         min_idx = i
>         
>         // Iterate through unsorted portion to find the actual minimum
>         for j = i + 1 to n - 1:
>             if A[j] < A[min_idx]:
>                 min_idx = j
>                 
>         // Swap minimum element to its correct position
>         if min_idx != i:
>             swap(A[i], A[min_idx])
> ```

> [!abstract] Complexity Analysis
> **Time Complexity:**
> - **Step-by-step:** The outer loop runs $n-1$ times. The inner loop *always* scans the remaining elements to find the minimum. There is no way to exit early.
> - Comparisons = $(n-1) + (n-2) + \dots + 1 = \frac{n(n-1)}{2} \implies \mathbf{O(n^2)}$.
> - **Best/Worst/Average Case:** All cases are strictly $\mathbf{O(n^2)}$.
> 
> **Space Complexity:**
> - Only index tracking variables (`i`, `j`, `min_idx`) are used.
> - **Total Space:** $\mathbf{O(1)}$ (In-place).

---

## 4. Merge Sort
A highly efficient, stable Divide & Conquer algorithm. It recursively splits the array into two halves until subarrays of size 1 are reached, then meticulously merges the halves back together in ascending order.

> [!example] Pseudocode
> ```text
> function MergeSort(A, left, right):
>     if left < right:
>         mid = left + (right - left) / 2
>         
>         // Recursively divide
>         MergeSort(A, left, mid)
>         MergeSort(A, mid + 1, right)
>         
>         // Conquer and merge
>         Merge(A, left, mid, right)
> 
> function Merge(A, left, mid, right):
>     // Create temporary arrays for the left and right halves
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
>     // Copy any remaining elements left over
>     while i < length(L): A[k++] = L[i++]
>     while j < length(R): A[k++] = R[j++]
> ```

> [!abstract] Complexity Analysis
> **Time Complexity:**
> - **Step 1 (Divide):** Calculating `mid` takes $O(1)$.
> - **Step 2 (Conquer):** We recursively solve 2 subproblems of size $n/2$. Math representation: $2T(n/2)$.
> - **Step 3 (Merge):** Merging two arrays of total size $n$ takes $O(n)$ time (one full linear pass).
> - **Recurrence Relation:** $T(n) = 2T(n/2) + O(n)$.
> - **Tree Solving:** The recursion tree has a depth of $\log_2(n)$ (because we halve $n$ until we reach 1). At every level of the tree, merging all subproblems takes exactly $O(n)$ operations. 
> - Total work = $n \text{ (work per level)} \times \log n \text{ (levels)} \implies \mathbf{O(n \log n)}$.
> - **Best/Worst/Average:** Always $\mathbf{O(n \log n)}$.
> 
> **Space Complexity:**
> - The `Merge` function allocates temporary vectors (`L` and `R`). Combined, at the highest level of recursion, these arrays hold $n$ elements.
> - **Total Space:** $\mathbf{O(n)}$ (Not in-place).

---

## 5. Counting Sort
A non-comparison-based algorithm that operates by counting the exact frequency of each distinct element. It then transforms this frequency array into a cumulative prefix-sum array, allowing it to map elements directly to their correct sorted index.

> [!warning] A Big NO for large values!
> Counting Sort's memory relies on the *maximum value* in the array. If sorting 32-bit integers, the count array would be size $4,294,967,296$ (16 GB). For 64-bit integers, it requires **138 Exabytes** of RAM! Use only when the max value ($m$) is roughly close to $n$.

> [!example] Pseudocode
> ```text
> function CountingSort(A, n):
>     max_val = max(A)
>     
>     // Initialize arrays
>     count = array of size (max_val + 1) filled with 0s
>     result = array of size n
>     
>     // 1. Store the frequency of each element
>     for i = 0 to n - 1:
>         count[A[i]]++
>         
>     // 2. Compute cumulative (prefix) sum
>     for i = 1 to max_val:
>         count[i] = count[i] + count[i - 1]
>         
>     // 3. Build result array going BACKWARDS to ensure stability
>     for i = n - 1 down to 0:
>         result[count[A[i]] - 1] = A[i]
>         count[A[i]]--
>         
>     return result
> ```

> [!abstract] Complexity Analysis
> Let $n$ be the number of elements and $m$ be the maximum value (`max_val`).
> **Time Complexity:**
> - **Step 1:** Finding the max value takes $O(n)$.
> - **Step 2:** Frequency counting takes $O(n)$.
> - **Step 3:** Prefix sum iterates through the `count` array of size $m \implies O(m)$.
> - **Step 4:** Building output scans the original array once $\implies O(n)$.
> - **Total Time:** $O(n + n + m + n) \implies \mathbf{O(n + m)}$.
> 
> **Space Complexity:**
> - We create a `count` array of size $m + 1$ and a `result` array of size $n$.
> - **Total Space:** $\mathbf{O(n + m)}$.

---

## 6. Radix Sort
Sorts elements digit by digit, strictly from the least significant digit (LSD) to the most significant digit (MSD). It loops and calls **Counting Sort** as a stable subroutine to sort the array based on one digit at a time.

> [!example] Pseudocode
> ```text
> function RadixSort(A, n):
>     max_val = max(A)
>     
>     // exp is 10^i (1, 10, 100...). 
>     // We keep going until max_val / exp is 0.
>     exp = 1
>     while (max_val / exp) > 0:
>         CountingSortByDigit(A, n, exp)
>         exp = exp * 10
> 
> function CountingSortByDigit(A, n, exp):
>     // Same logic as Counting sort, but the index we count is:
>     // digit = (A[i] / exp) % 10
>     // Count array size is fixed at the base (e.g., 10 for base-10).
>     count = array of size 10 filled with 0s
>     result = array of size n
>     
>     // 1. Store the frequency of each element based on the current digit
>     for i = 0 to n - 1:
>         digit = (A[i] / exp) % 10
>         count[digit]++
>         
>     // 2. Compute cumulative (prefix) sum
>     for i = 1 to 9:
>         count[i] = count[i] + count[i - 1]
>         
>     // 3. Build result array going BACKWARDS to ensure stability
>     for i = n - 1 down to 0:
>         digit = (A[i] / exp) % 10
>         result[count[digit] - 1] = A[i]
>         count[digit]--
>         
>     // 4. Copy sorted elements back into the original array
>     for i = 0 to n - 1:
>         A[i] = result[i]
> ```

> [!abstract] Complexity Analysis
> Let $n$ be the number of elements, $b$ be the base (e.g., 10), and $d$ be the max number of digits.
> **Time Complexity:**
> - The outer `while` loop runs exactly $d$ times (once per digit).
> - Inside the loop, `CountingSortByDigit` takes $O(n + b)$ time.
> - **Total Time:** $d \times O(n + b) \implies \mathbf{O(d \cdot (n + b))}$. 
> - *(If $d$ is treated as a constant and $b$ is small, it simplifies to $O(n)$).*
> 
> **Space Complexity:**
> - The subroutine needs an output array of size $n$ and a count array of size $b$.
> - **Total Space:** $\mathbf{O(n + b)}$.

> [!info] The Space-Time Tradeoff
> You are not forced to use Base-10 ($b = 10$). We could represent numbers in Base-2 (Binary) or Base-256. 
> - **Small Base (Base 2):** Count array is very small (size 2), so **Space is extremely low**. However, a number takes many more digits to represent in binary, so $d$ gets massive $\implies$ **Time is much higher** (many loop iterations).
> - **Large Base (Base 256):** $d$ shrinks to just a few iterations (e.g., a 32-bit integer is just 4 bytes, so $d=4$) $\implies$ **Time is very low**. However, the count array takes up more memory $\implies$ **Space is higher**. Base-256 ($2^8$) is widely considered the optimal balance.

---

## 7. Master Summary Table

| Algorithm | Best Time | Avg Time | Worst Time | Space Complexity | Stable? | In-Place? |
| :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **Bubble Sort** | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Yes | Yes |
| **Insertion Sort** | $O(n)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | Yes | Yes |
| **Selection Sort** | $O(n^2)$ | $O(n^2)$ | $O(n^2)$ | $O(1)$ | No | Yes |
| **Merge Sort** | $O(n \log n)$ | $O(n \log n)$| $O(n \log n)$| $O(n)$ | Yes | No |
| **Counting Sort** | $O(n + m)$ | $O(n + m)$ | $O(n + m)$ | $O(n + m)$ | Yes | No |
| **Radix Sort** | $O(d(n + b))$| $O(d(n + b))$| $O(d(n + b))$| $O(n + b)$ | Yes | No |

*(Legend: $m$ = max value in array | $d$ = max number of digits | $b$ = numeric base system).*