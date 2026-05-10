
> **Sources:**  
> *Lecture slides by Anika Farzana, IUT*  

---

## 1. Key Concepts

| Term | Definition |
|------|------------|
| **In‑place sort** | Uses constant extra space; modifies the input array directly. |
| **Stable sort** | Preserves the relative order of equal elements. |
| **Comparison‑based** | Sorts by comparing elements (e.g., `if (a > b)`). Lower bound: Ω(n log n) |
| **Non‑comparison‑based** | Uses special properties of keys (e.g., frequencies, digits) to sort in linear time (when range is small). |

### Which algorithm is what?

| Algorithm | In‑place | Stable | Comparison‑based |
|-----------|----------|--------|------------------|
| Bubble Sort | Yes | Yes | Yes |
| Insertion Sort | Yes | Yes | Yes |
| Selection Sort | Yes | No | Yes |
| Merge Sort | No (needs O(n)) | Yes | Yes |
| Quick Sort | Yes (uses recursion stack) | No (naïve) | Yes |
| Heap Sort | Yes | No | Yes |
| Counting Sort | No (needs O(n+m)) | Yes | No |
| Radix Sort | No (needs O(n+b)) | Yes | No |

---

## 2. Bubble Sort

### Idea
Repeatedly step through the list, compare adjacent elements and swap them if they are in the wrong order.  
After each full pass, the largest unsorted element “bubbles up” to its correct position.

### Implementation (C++)
```cpp
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        // last i elements are already in place
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1])
                swap(arr[j], arr[j + 1]);
        }
    }
}
```

### Complexity Calculation
- **Outer loop** runs \(n-1\) times.
- **Inner loop** runs \(n-i-1\) times per outer iteration.
- Total comparisons: \((n-1)+(n-2)+\dots+1 = \frac{n(n-1)}{2} = O(n^2)\).
- **Space**: No extra memory beyond a few variables → \(O(1)\).

| Case | Time Complexity |
|------|----------------|
| Best (already sorted) | O(n) (with early‑exit optimization) |
| Average | O(n²) |
| Worst (reverse sorted) | O(n²) |
| Space | O(1) |

> **Stable:** Yes, because we only swap adjacent elements and only when strictly greater.

---

## 3. Insertion Sort

### Idea
Build the sorted array one element at a time.  
At each step, take the next unsorted element and insert it into its correct position among the previously sorted elements (like sorting a hand of playing cards).

### Implementation (C++)
```cpp
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        // Shift elements of arr[0..i-1] that are greater than key
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```

### Complexity Calculation
- **Outer loop** runs \(n-1\) times.
- **Inner loop** shifts elements; in the worst case (reverse sorted) it shifts \(i\) elements for each \(i\).
- Total shifts: \(1+2+\dots+(n-1) = O(n^2)\).
- **Space**: \(O(1)\) (in‑place).

| Case | Time Complexity |
|------|----------------|
| Best (already sorted) | O(n) |
| Average (random) | O(n²) |
| Worst (reverse sorted) | O(n²) |
| Space | O(1) |

> **Stable:** Yes (shifting preserves order of equal elements).

---

## 4. Selection Sort

### Idea
Repeatedly find the minimum element from the unsorted part and place it at the beginning of the unsorted part.

### Implementation (C++)
```cpp
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx])
                minIdx = j;
        }
        swap(arr[i], arr[minIdx]);
    }
}
```

### Complexity Calculation
- Two nested loops: \(n-1\) passes, each scanning the remaining (\(n-i\)) elements.
- Comparisons: \(\frac{n(n-1)}{2} = O(n^2)\) **always**, regardless of input order.
- **Space**: \(O(1)\).

| Case | Time Complexity |
|------|----------------|
| All cases | O(n²) |
| Space | O(1) |

> **Stable:** No (the swap can disrupt the order of equal keys, e.g., swapping the first with a far away equal element).

---

## 5. Counting Sort (Non‑comparison)

### Idea
Count the frequency of each distinct value. Compute prefix sums to know exactly where each element should go in the output array.  
Excellent when the range of possible values \(m\) is small relative to \(n\).

### Implementation (C++)
```cpp
void countingSort(vector<int>& arr, int maxVal) {
    int n = arr.size();
    vector<int> count(maxVal + 1, 0);
    vector<int> output(n);

    // frequencies
    for (int x : arr)
        count[x]++;

    // prefix sums (cumulative)
    for (int i = 1; i <= maxVal; i++)
        count[i] += count[i-1];

    // build output (iterate backwards for stability)
    for (int i = n-1; i >= 0; i--) {
        int val = arr[i];
        output[ count[val] - 1 ] = val;
        count[val]--;
    }

    arr = output;
}
```

### Complexity Calculation
- Counting loop: \(O(n)\)
- Prefix sum loop: \(O(m)\) where \(m = \max(\text{arr})\)
- Output loop: \(O(n)\)
- **Total time:** \(O(n + m)\)
- **Space:** Count array \(O(m)\) + output array \(O(n)\) → \(O(n+m)\)

| Case | Time Complexity |
|------|----------------|
| All cases | O(n + m) |
| Space | O(n + m) |

> **Stable:** Yes (when iterated backwards and placed with decreasing prefix sums).  
> **In‑place:** No.  
> **Branchless advantage:** No `if` comparisons between elements (just indexing), so it avoids branch mispredictions.

### Why not always?
For 32‑bit integers \(m = 2^{32}\) → count array needs 16 GB; for 64‑bit it’s infeasible. Use only when \(m\) is small.

---

## 6. Radix Sort (Non‑comparison)

### Idea
Sort numbers digit‑by‑digit from least significant digit (LSD) to most significant digit (MSD).  
Uses a stable sorting subroutine (usually Counting Sort) for each digit position.

### Implementation Sketch (C++)
```cpp
void radixSort(vector<int>& arr) {
    // Find max to know number of digits
    int m = *max_element(arr.begin(), arr.end());
    // Do counting sort for every digit (exp = 1, 10, 100, ...)
    for (int exp = 1; m/exp > 0; exp *= 10)
        countingSortByDigit(arr, exp);   // sorts arr based on (arr[i]/exp)%10
}
```
Inside `countingSortByDigit`:
```cpp
void countingSortByDigit(vector<int>& arr, int exp) {
    int n = arr.size();
    vector<int> output(n), count(10, 0);   // base = 10 (digits 0-9)

    // count digit occurrences
    for (int i = 0; i < n; i++)
        count[ (arr[i]/exp) % 10 ]++;

    // prefix sum
    for (int i = 1; i < 10; i++)
        count[i] += count[i-1];

    // build output (backwards for stability)
    for (int i = n-1; i >= 0; i--) {
        int digit = (arr[i]/exp) % 10;
        output[ count[digit] - 1 ] = arr[i];
        count[digit]--;
    }
    arr = output;
}
```

### Complexity Calculation
- Each digit pass uses Counting Sort with base \(b\) (here \(b = 10\)).
- That pass takes \(O(n + b)\).
- Number of passes \(d\) = number of digits of the maximum element in base \(b\).
- **Time:** \(O\left( d \cdot (n + b) \right)\).
- **Space:** \(O(n+b)\) auxiliary for count + output array.

| Parameter | Meaning |
|-----------|---------|
| b | base (radix), e.g., 10, 256, … |
| d | max digits = ⌈log_b(maxVal)⌉ |
| Time | O(d·n) if b is constant; O((n+b)·d) |
| Space | O(n+b) |

**Trade‑off**: larger \(b\) → fewer passes but larger count array. Often \(b = 2^8 = 256\) gives a good balance.

> **Stable:** Yes (because the underlying counting sort is stable).  
> **In‑place:** No.

---

## 7. Merge Sort (Divide & Conquer)

### Idea
1. Divide the array into two halves recursively until each sub‑array has one element.
2. Merge the sorted halves back together.

### Implementation (C++)
```cpp
void merge(vector<int>& arr, int lo, int mid, int hi) {
    int n1 = mid - lo + 1;
    int n2 = hi - mid;
    vector<int> left(n1), right(n2);
    for (int i = 0; i < n1; i++) left[i] = arr[lo + i];
    for (int j = 0; j < n2; j++) right[j] = arr[mid + 1 + j];

    int i = 0, j = 0, k = lo;
    while (i < n1 && j < n2) {
        if (left[i] <= right[j])
            arr[k++] = left[i++];
        else
            arr[k++] = right[j++];
    }
    while (i < n1) arr[k++] = left[i++];
    while (j < n2) arr[k++] = right[j++];
}

void mergeSort(vector<int>& arr, int lo, int hi) {
    if (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        mergeSort(arr, lo, mid);
        mergeSort(arr, mid + 1, hi);
        merge(arr, lo, mid, hi);
    }
}
```

### Complexity Calculation
- Recurrence: \(T(n) = 2T(n/2) + O(n)\) (merge step touches every element once).
- Using the Master Theorem or recursion tree: depth = \(\log_2 n\), each level does \(O(n)\) work → \(O(n \log n)\).
- **Space:** The temporary arrays `left` and `right` consume \(O(n)\) auxiliary memory at the top level (the recursion stack adds \(O(\log n)\) but total auxiliary is \(O(n)\)).

| Case | Time Complexity |
|------|----------------|
| All cases | Θ(n log n) |
| Space | O(n) |

> **Stable:** Yes (we use `<=` in the merge comparison, thus preserving order).  
> **In‑place:** No (requires additional arrays).

---

## 8. Quick Sort (Divide & Conquer)

### Idea
1. Pick a **pivot** element.
2. **Partition** the array so that all elements less than pivot are on the left, all greater on the right.
3. Recursively apply the same to left and right sub‑arrays.

### Implementation (Lomuto partition, C++)
```cpp
int partition(vector<int>& arr, int lo, int hi) {
    int pivot = arr[hi];               // last element as pivot
    int i = lo - 1;                    // index of smaller element
    for (int j = lo; j < hi; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[hi]);
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

### Complexity Calculation
- **Best / Average:** Partition always splits the array into two roughly equal halves.  
  Recurrence: \(T(n) = 2T(n/2) + O(n)\) → \(O(n \log n)\).
- **Worst:** Pivot is always the smallest (or largest) element → one sub‑array of size \(n-1\), the other 0.  
  Recurrence: \(T(n) = T(n-1) + O(n)\) → \(O(n^2)\).
- **Space:** In‑place. Recursion depth: \(O(\log n)\) on average, \(O(n)\) in worst case (can be mitigated with tail‑recursion elimination or iterative approach).

| Case | Time Complexity |
|------|----------------|
| Best | O(n log n) |
| Average | O(n log n) |
| Worst | O(n²) |
| Space | O(log n) average, O(n) worst (stack) |

> **Stable:** Not stable in the basic version (partition swaps can disturb order of equal keys).  
> **In‑place:** Yes (modifies the array in‑place, uses only a few local variables + recursion stack).

---

## 9. A Note on Branch Prediction

Modern CPUs try to predict the outcome of an `if` statement.  
Comparison‑based sorts (Bubble, Insertion, Selection, Quick, Merge) are full of such branches, e.g., `if (arr[j] > arr[j+1])`. Mispredictions slow them down.

Counting Sort replaces comparisons with:
```cpp
count[arr[i]]++
```
This is a direct index operation – no data‑dependent branches. Radix Sort inherits this branchless property because it uses Counting Sort as its subroutine. This can make them extremely fast for suitable data.

---

## 10. Summary Table

| Algorithm     | Time (Worst) | Time (Average) | Time (Best) | Space (Aux) | In‑place | Stable |
|---------------|--------------|----------------|-------------|-------------|----------|--------|
| Bubble        | O(n²)        | O(n²)          | O(n)        | O(1)        | Yes      | Yes    |
| Insertion     | O(n²)        | O(n²)          | O(n)        | O(1)        | Yes      | Yes    |
| Selection     | O(n²)        | O(n²)          | O(n²)       | O(1)        | Yes      | No     |
| Counting      | O(n+m)       | O(n+m)         | O(n+m)      | O(n+m)      | No       | Yes    |
| Radix         | O(d·(n+b))   | O(d·(n+b))     | O(d·(n+b))  | O(n+b)      | No       | Yes    |
| Merge         | O(n log n)   | O(n log n)     | O(n log n)  | O(n)        | No       | Yes    |
| Quick         | O(n²)        | O(n log n)     | O(n log n)  | O(log n)*   | Yes      | No     |

*\* auxiliary space for recursion stack; \(O(n)\) in worst‑case*

---