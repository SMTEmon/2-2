---
tags:
  - algorithms
  - graphs
  - data-structures
  - lecture-04
---

# Lecture 04: Graph - 1

> [!info] Graph Basics
> A **Graph** $G$ is a non-linear data structure composed of a set of vertices (or nodes) and a set of edges that connect them.
> Mathematically, it is represented as **$G = (V, E)$**
> - **$V$**: Set of vertices/nodes (denoted by circles in diagrams).
> - **$E$**: Set of edges (denoted by lines/arrows).
> 
> **Types of Edges:**
> - **Undirected:** $e = \{u, v\}$ $\rightarrow$ Unordered pairs (the connection is two-way).
> - **Directed:** $e = (u, v)$ $\rightarrow$ Ordered pairs (the connection goes from $u$ to $v$).

---

## 1. Graph Representations

To implement graph algorithms, we must represent the graph in memory. There are two primary ways to do this: the **Adjacency Matrix** and the **Adjacency List**.

### A. Adjacency Matrix

> [!abstract] Definition
> An adjacency matrix is a 2D array (matrix) of dimensions $|V| \times |V|$, where each element is either `0` or `1` (or a weight, for weighted graphs).
> 
> **Mathematical Formulation:**
> $$
> adjM[u][v] = 
> \begin{cases} 
> 1 & \text{if } (u, v) \in E \\
> 0 & \text{otherwise}
> \end{cases}
> $$
> *(For undirected graphs, the matrix is symmetric across the diagonal, meaning $adjM[u][v] = adjM[v][u]$)*

#### ⏱️ Complexity Analysis: Adjacency Matrix

> [!math] Space Complexity: $\mathcal{O}(|V|^2)$
> **Step-by-step Explanation:**
> 1. We allocate a 2D array/grid.
> 2. The number of rows equals the total number of vertices, $|V|$.
> 3. The number of columns equals the total number of vertices, $|V|$.
> 4. Total elements stored = $|V| \times |V| = |V|^2$.
> 5. Even if the graph has zero edges (an empty graph), the matrix still takes up $|V|^2$ space, making it highly memory-inefficient for sparse graphs (graphs with few edges).

> [!math] Time Complexity (Basic Operations)
> - **Check if an edge exists between $u$ and $v$**: $\mathcal{O}(1)$ $\rightarrow$ Just check `adjM[u][v]`.
> - **Find all neighbors of a vertex $u$**: $\mathcal{O}(|V|)$ $\rightarrow$ We must scan the entire row `u` from `0` to `|V|-1`.

---

### B. Adjacency List

> [!abstract] Definition
> An adjacency list is an array (or hash map) of size $|V|$, where each element `adjL[u]` contains a linked list (or dynamic array/vector) of all the vertices adjacent to vertex $u$.
> 
> **Mathematical Formulation:**
> $$
> adjL[u] = \{ v \in V \mid (u, v) \in E \}
> $$
> - **Undirected:** If $v$ is in $u$'s list, $u$ will also be in $v$'s list.
> - **Directed:** If $u \rightarrow v$, $v$ is only in $u$'s list.

#### ⏱️ Complexity Analysis: Adjacency List

> [!math] Space Complexity: $\mathcal{O}(|V| + |E|)$
> **Step-by-step Explanation:**
> 1. We create a primary array/list of size $|V|$ to represent every vertex. This costs $\mathcal{O}(|V|)$ space.
> 2. Inside each index $u$, we store a list of its neighbors. 
> 3. **For Directed Graphs:** The total number of items across *all* linked lists is exactly equal to the total number of directed edges, $|E|$.
> 4. **For Undirected Graphs:** Every edge $\{u, v\}$ is stored twice (once in $u$'s list, once in $v$'s list). The total number of items across all lists is $2|E|$.
> 5. Dropping the constant $2$, the space used by the linked lists is $\mathcal{O}(|E|)$.
> 6. Adding the primary array and the linked lists together, the total space is **$\mathcal{O}(|V| + |E|)$**. This is highly efficient for sparse graphs.

> [!math] Time Complexity (Basic Operations)
> - **Check if an edge exists between $u$ and $v$**: $\mathcal{O}(\text{degree}(u))$ $\rightarrow$ We must traverse $u$'s list to find $v$. In the worst case (a dense graph), this is $\mathcal{O}(|V|)$.
> - **Find all neighbors of a vertex $u$**: $\mathcal{O}(\text{degree}(u))$ $\rightarrow$ We just iterate through `adjL[u]`.

## 2. Graph Traversal: Breadth-First Search (BFS)

> [!info] What is BFS?
> Breadth-First Search (BFS) is a graph traversal algorithm that explores nodes level by level. It starts at a given source node, explores all its immediate neighbors first, and then moves on to the neighbors of those neighbors.
> - **Data Structure Used:** BFS inherently relies on a **Queue** (First-In, First-Out / FIFO) to keep track of the next nodes to process.

---

### A. BFS using Adjacency List

> [!example] Pseudocode: Adjacency List Implementation
> ```text
> function BFS_List(Graph, start_node):
>     // Graph is represented as an array of lists: Graph[u] contains neighbors of u
>     
>     // 1. Initialization
>     create a boolean array 'visited' of size |V|, initialized to FALSE
>     create an empty queue 'Q'
>     
>     // 2. Setup starting node
>     visited[start_node] = TRUE
>     enqueue start_node into Q
>     
>     // 3. Traversal loop
>     while Q is not empty do:
>         current = dequeue from Q
>         print/process current
>         
>         // 4. Explore neighbors
>         for each neighbor in Graph[current] do:
>             if visited[neighbor] == FALSE then:
>                 visited[neighbor] = TRUE
>                 enqueue neighbor into Q
> ```

#### ⏱️ Complexity Analysis: BFS (Adjacency List)

> [!math] Time Complexity: $\mathcal{O}(V + E)$
> **Step-by-step Explanation (from the slides):**
> 1. **Queue Operations (Outer Loop):** A node is popped from the queue. Because a node is marked `visited` *before* being pushed to the queue, no node is ever pushed more than once. Therefore, the `while` loop runs exactly $V$ times. $\rightarrow \mathcal{O}(V)$
> 2. **Neighbor Iteration (Inner Loop):** In the inner `for` loop, we look at the neighbors of the `current` node. 
> 3. Over the entire lifetime of the BFS execution, the inner loop will process every single edge in the graph. For a directed graph, it processes $E$ edges. For an undirected graph, it processes $2E$ edges.
> 4. Therefore, the inner loop runs a total of $E$ times across all outer loop iterations combined. $\rightarrow \mathcal{O}(E)$
> 5. **Total Time:** Adding the outer loop and inner loop operations together gives **$\mathcal{O}(V + E)$**.

> [!math] Space Complexity: $\mathcal{O}(V)$
> **Step-by-step Explanation:**
> *(Note: This excludes the space taken to store the graph itself, which we established is $\mathcal{O}(V+E)$)*
> 6. **Visited Array:** We maintain a `visited` array of size $V$ to track which nodes have been seen. This requires $\mathcal{O}(V)$ space.
> 7. **Queue:** In the worst-case scenario (e.g., a star graph where the center node is connected to all other nodes), the queue might hold all other vertices at the same time. The max size of the queue is $V$. This requires $\mathcal{O}(V)$ space.
> 8. **Total Auxiliary Space:** $\mathcal{O}(V) + \mathcal{O}(V)$ = **$\mathcal{O}(V)$**.

---

### B. BFS using Adjacency Matrix

> [!example] Pseudocode: Adjacency Matrix Implementation
> ```text
> function BFS_Matrix(Graph, num_vertices, start_node):
>     // Graph is a 2D matrix of size num_vertices x num_vertices
>     
>     // 1. Initialization
>     create a boolean array 'visited' of size num_vertices, initialized to FALSE
>     create an empty queue 'Q'
>     
>     // 2. Setup starting node
>     visited[start_node] = TRUE
>     enqueue start_node into Q
>     
>     // 3. Traversal loop
>     while Q is not empty do:
>         current = dequeue from Q
>         print/process current
>         
>         // 4. Explore all possible neighbors across the row
>         for i from 0 to num_vertices - 1 do:
>             // Check if an edge exists AND if it hasn't been visited
>             if Graph[current][i] is an edge AND visited[i] == FALSE then:
>                 visited[i] = TRUE
>                 enqueue i into Q
> ```

#### ⏱️ Complexity Analysis: BFS (Adjacency Matrix)

> [!math] Time Complexity: $\mathcal{O}(V^2)$
> **Step-by-step Explanation (from the slides):**
> 1. **Queue Operations (Outer Loop):** Just like the adjacency list, only non-visited nodes are pushed into the queue. This means the `while` loop runs exactly $V$ times. $\rightarrow \mathcal{O}(V)$
> 2. **Matrix Row Iteration (Inner Loop):** In the inner `for` loop, we cannot just jump to the neighbors. We must check *every single column* `i` in the `current` node's row inside the adjacency matrix. 
> 3. This means for *every single node* popped from the queue, we iterate exactly $V$ times. 
> 4. **Total Time:** $V$ outer loop iterations $\times$ $V$ inner loop iterations = **$\mathcal{O}(V \times V) = \mathcal{O}(V^2)$**.

> [!math] Space Complexity: $\mathcal{O}(V)$
> **Step-by-step Explanation:**
> *(Note: This excludes the $\mathcal{O}(V^2)$ space required to store the adjacency matrix graph itself)*
> 1. **Visited Array:** We maintain a `visited` array of size $V$. $\rightarrow \mathcal{O}(V)$ space.
> 2. **Queue:** Just like before, the queue will hold at most $V$ nodes at any given time. $\rightarrow \mathcal{O}(V)$ space.
> 3. **Total Auxiliary Space:** $\mathcal{O}(V) + \mathcal{O}(V)$ = **$\mathcal{O}(V)$**.

## 3. Graph Traversal: Depth-First Search (DFS)

> [!info] What is DFS?
> Depth-First Search (DFS) is a traversal algorithm that dives as deep as possible into a graph's branches before backtracking. 
> - **Data Structure Used:** DFS inherently relies on a **Stack** (Last-In, First-Out / LIFO). This is most commonly implemented implicitly using the **Function Call Stack** via recursion.

*(Note: The slides label the C++ code as "Adjacency Matrix Implementation," but the syntax `for (int i : adj[node])` and the resulting $O(V+E)$ time complexity indicate it is actually traversing an **Adjacency List**. The pseudocode below reflects this Adjacency List logic.)*

### A. DFS Pseudocode (Recursive)

> [!example] Pseudocode: Recursive DFS (Adjacency List)
> ```text
> // Main entry function
> function DFS_Caller(Graph, start_node):
>     // 1. Initialize visited array
>     create a boolean array 'visited' of size |V|, initialized to FALSE
>     
>     // 2. Start recursion
>     DFS_Recursive(Graph, visited, start_node)
> 
> // Recursive helper function
> function DFS_Recursive(Graph, visited, node):
>     // 1. Mark current node as visited
>     visited[node] = TRUE
>     print/process node
>     
>     // 2. Explore all neighbors recursively
>     for each neighbor in Graph[node] do:
>         if visited[neighbor] == FALSE then:
>             DFS_Recursive(Graph, visited, neighbor)
> ```

#### ⏱️ Complexity Analysis: DFS

> [!math] Time Complexity: $\mathcal{O}(V + E)$
> **Step-by-step Explanation (from the slides):**
> 1. **Node Processing:** A node is virtually "popped" from the call stack (or explicit stack) when the function executes. Because of the `visited` check, the `DFS_Recursive` function is called exactly once for each reachable vertex. Total time spent initiating calls is $\mathcal{O}(V)$.
> 2. **Neighbor Checking:** Inside the recursive call, the `for` loop iterates over all adjacent neighbors of the current node. 
> 3. Over the entire execution of the algorithm, this inner loop explores every edge. For directed graphs, it checks $E$ edges. For undirected graphs, it checks $2E$ edges.
> 4. **Total Time:** $\mathcal{O}(V)$ for processing vertices + $\mathcal{O}(E)$ for processing edges = **$\mathcal{O}(V + E)$**.

> [!math] Space Complexity: $\mathcal{O}(V)$
> **Step-by-step Explanation:**
> 1. **Visited Array:** Requires $\mathcal{O}(V)$ space to keep track of visited nodes.
> 2. **Recursion Call Stack:** In the worst-case scenario (e.g., the graph is a single long line of connected nodes), the recursion will dive $V$ levels deep before returning. This places $V$ function calls on the system call stack. Requires $\mathcal{O}(V)$ space.
> 3. **Total Auxiliary Space:** $\mathcal{O}(V) + \mathcal{O}(V)$ = **$\mathcal{O}(V)$**.

---

## 4. Topological Sorting

> [!info] Why do we need to order a Graph?
> Many real-world problems require ordering tasks that have **dependencies** (where task A must be completed before task B). 
> **Examples:**
> - Determining the order to recalculate updated cells in a spreadsheet.
> - Finding an order to recompile code files that have dependencies.
> - Determining the sequence in which university courses (with prerequisites) should be taken.

> [!abstract] Topological Sort Definition
> Given a **Directed Acyclic Graph (DAG)** $G = (V, E)$, topological sorting provides a linear, total ordering of $G$'s vertices such that for every directed edge $(u, v)$ in $E$, **vertex $u$ comes before vertex $v$** in the ordering.
> *(Important: Topological sorting is only possible on graphs with **no cycles**.)*

### A. Topological Sort: DFS Style

To achieve a topological sort, we can slightly modify our standard DFS algorithm. Instead of processing a node when we first see it, we push it to a stack **only after we have completely explored all of its neighbors**.

> [!example] Pseudocode: TopSort DFS Style
> ```text
> function TopologicalSort(Graph):
>     // 1. Initialization
>     create a boolean array 'visited' of size |V|, initialized to FALSE
>     create an empty Stack 'st'
>     
>     // 2. Process all nodes (handles disconnected graphs)
>     for i from 0 to |V| - 1 do:
>         if visited[i] == FALSE then:
>             TopSort_DFS(Graph, visited, st, i)
>             
>     // 3. Print the topological order
>     while st is not empty do:
>         current = pop from st
>         print current
> 
> // Modified Recursive DFS
> function TopSort_DFS(Graph, visited, st, node):
>     visited[node] = TRUE
>     
>     // Visit all neighbors first
>     for each neighbor in Graph[node] do:
>         if visited[neighbor] == FALSE then:
>             TopSort_DFS(Graph, visited, st, neighbor)
>             
>     // Push to stack AFTER visiting all adjacent nodes
>     push node onto st
> ```

#### ⏱️ Complexity Analysis: Topological Sort

> [!math] Time Complexity: $\mathcal{O}(V + E)$
> **Step-by-step Explanation (visualized in Slides 17 & 18):**
> 1. **DFS Traversal (Red Bracket 1):** The first block of code (the `for` loop iterating through all nodes, and the recursive `TopSort_DFS` checking all neighbors) is exactly identical in execution to a standard DFS. We established earlier that a full DFS takes **$\mathcal{O}(V + E)$**.
> 2. **Stack Popping (Red Bracket 2):** After the DFS finishes building the stack, the `while` loop runs. Because exactly $V$ nodes were pushed onto the stack, popping them all takes exactly **$\mathcal{O}(V)$** operations.
> 3. **Total Time:** $\mathcal{O}(V + E) + \mathcal{O}(V)$. Since $\mathcal{O}(V)$ is absorbed by $\mathcal{O}(V+E)$, the final combined time complexity simplifies to **$\mathcal{O}(V + E)$**.

> [!math] Space Complexity: $\mathcal{O}(V)$
> **Step-by-step Explanation:**
> 4. **Visited Array:** $\mathcal{O}(V)$ space.
> 5. **Explicit Stack:** We push every single vertex onto the stack exactly once. Therefore, the stack holds $V$ elements. $\mathcal{O}(V)$ space.
> 6. **Call Stack:** The recursion depth can go up to $V$ in the worst case. $\mathcal{O}(V)$ space.
> 7. **Total Auxiliary Space:** $\mathcal{O}(V) + \mathcal{O}(V) + \mathcal{O}(V)$ = **$\mathcal{O}(V)$**.