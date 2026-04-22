- Calculating Mid
	- (lo + hi) / 2 -> Can go out of bounds for input > 10^9
		- the (lo + hi) can overflow
	- lo + (hi - lo) /2 -> Better chance at not going out of bounds
		- lo + (the difference between lo and hi)

[Merge Sort](https://www.geeksforgeeks.org/dsa/merge-sort/)


```cpp
// Merge the temp vectors back 
    // into arr[left..right]
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        }
        else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }


	// Since the above loop terminates whenever L or R either are exhausted/ finished
    // Copy the remaining elements of L[], 
    // if there are any
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    // Copy the remaining elements of R[], 
    // if there are any
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
```

- Not a implace Sort (creates new array/ vector for result)
- Time Complexity
	- O(n) for merging -> going through each element 1 by 1
	- O(log n) for recursion -> Recursing and creating halved tree on each step
	- Total O(n log n) -> call merge on each recursion
- Space Complexity
	- O(n), Additional space is required for the temporary array used during merging.