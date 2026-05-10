- Basically Sort by counting

[Counting Sort](https://www.geeksforgeeks.org/dsa/counting-sort/)

[Stable Sort](https://www.geeksforgeeks.org/dsa/stable-and-unstable-sorting-algorithms/)



- It is particularly efficient when the range of input values is small compared to the number of elements to be sorted. [0, 2, 1, 3, 1, 2] (range 0-3)
- Traverse array arr[] from end and update ans[ cntArr[ arr[i] ] - 1] = arr[i]. Also, update cntArr[ arr[i] ] = cntArr[ arr[i] ]- - .
  
  why do we need to traverse the original array from end to start?
  
  why not just use the original count array for that?
	- Ans-> The main reason is **stability**. Traversing from the end ensures that elements with the same value keep their original relative order in the sorted array.