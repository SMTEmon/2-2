# Shortest Path Proofs: Intuition & Exam-Ready Versions

---

## 1. Dijkstra's Algorithm

> [!tip] The Logical Proof (The Greedy Contradiction)
> Let's prove this without heavy math. Suppose Dijkstra makes a mistake and extracts node $u$, but there is actually a "secret" shorter path to $u$.
> 1. **The Crossing:** This secret shorter path must start at $s$ (which is finalized) and eventually cross into the unvisited nodes. Let $y$ be the very first unvisited node on this secret path.
> 2. **The Trap (No negative weights):** Because there are no negative weights, the rest of the path from $y$ to $u$ can only *add* more distance. This means the true distance to get to $y$ must be less than or equal to the total distance to get to $u$.
> 3. **The Contradiction:** Dijkstra always greedily picks the unvisited node with the absolute smallest current distance. If $y$ is on a shorter path to $u$, $y$ must be closer (or equal) to the source than $u$. Thus, the algorithm would have extracted $y$ before $u$!
> 
> Since Dijkstra chose $u$, our assumption that a secret shorter path exists must be wrong. $u$'s distance is perfectly calculated.

> [!abstract] 📝 Exam-Ready Writing
> **Claim:** When a node $u$ is extracted from the priority queue, its computed distance $d[u]$ is the absolute shortest path distance.
> **Proof by Contradiction:**
> 1. Assume, for contradiction, that when $u$ is extracted, there exists a strictly shorter true path to $u$.
> 2. This hypothetical shorter path must cross from the set of already "visited" nodes to a first "unvisited" node, let's call it $y$.
> 3. Because all edge weights in the graph are non-negative, the remaining distance from $y$ to $u$ must be $\ge 0$. 
> 4. Therefore, the true optimal distance to $y$ must be less than or equal to the total optimal distance to $u$.
> 5. However, Dijkstra's algorithm always selects the unvisited node with the absolute minimum distance.
> 6. If $y$ were truly on a shorter path, $y$'s distance would be smaller than or equal to $u$'s, meaning the algorithm would have selected $y$ before $u$. 
> 7. This contradicts the fact that $u$ was selected. Therefore, no such shorter path exists, and $d[u]$ is perfectly optimal.

---

## 2. Bellman-Ford Algorithm

> [!tip] The Logical Proof (The Domino Effect)
> Let's prove this without formal induction.
> 1. **The Limit:** In a graph with $V$ vertices, a valid shortest path (with no negative cycles) can never have more than $V-1$ edges. If it had more, it would have to visit a node twice (forming a cycle).
> 2. **The Tracing:** Imagine the absolute shortest path to some destination is the sequence: $s \to a \to b \to c$.
> 3. **Pass 1:** The algorithm blindly relaxes *all* edges. It is guaranteed to evaluate the edge $s \to a$. Now, the distance to $a$ is perfect.
> 4. **Pass 2:** It blindly relaxes all edges again. Since $a$ is already perfect, relaxing the edge $a \to b$ makes $b$'s distance perfect.
> 5. **Pass 3:** Relaxing $b \to c$ makes $c$'s distance perfect.
> 
> **Conclusion:** Like falling dominos, each pass secures at least one more edge along the true shortest path. Since the longest possible path has at most $V-1$ edges, $V-1$ passes mathematically guarantees every domino has fallen, and every node's distance is perfectly calculated.

> [!abstract] 📝 Exam-Ready Writing
> **Claim:** After $V-1$ relaxation passes, the algorithm correctly computes the shortest path to all reachable vertices.
> **Proof by Path Length:**
> 1. In any graph without negative weight cycles, a simple shortest path between any two nodes can contain at most $V-1$ edges. (A path with $\ge V$ edges must contain a cycle).
> 2. Consider the true shortest path to an arbitrary node $v$. Let this path be a sequence of edges: $e_1, e_2, \dots, e_k$, where $k \le V-1$.
> 3. During the 1st pass, the algorithm relaxes all edges, guaranteeing that the first edge $e_1$ is evaluated. The distance to the first intermediate node is now optimal.
> 4. During the 2nd pass, the algorithm again relaxes all edges. Because the first node is optimal, evaluating $e_2$ guarantees the second intermediate node becomes optimal.
> 5. By induction, the $i$-th pass guarantees that the $i$-th edge along the shortest path is successfully relaxed and finalized.
> 6. Since the maximum possible path length is $V-1$ edges, executing $V-1$ passes mathematically guarantees that every edge of every shortest path has been successively relaxed in order.

---

## 3. Floyd-Warshall Algorithm

> [!tip] The Logical Proof (The Expanding Layovers)
> Let's prove this without heavy induction math. The algorithm works by gradually unlocking cities you can use as "layovers".
> 1. **The Setup:** Initially, you are allowed zero layovers. You can only take direct flights.
> 2. **The Decision:** When City $k$ is unlocked as a potential new layover, any optimal route between $i$ and $j$ faces a simple binary choice: either it uses City $k$, or it doesn't.
> 3. **The Trap (No negative cycles):** If the optimal route *does* use City $k$, it will fly from $i$ to $k$, and then from $k$ to $j$. Crucially, because there are no negative cycles to spin around in, the route will only visit City $k$ exactly once.
> 4. **The Guarantee:** Because it only visits $k$ once, the two halves of the trip ($i \to k$ and $k \to j$) can only use the *previously unlocked* cities (cities 1 through $k-1$). Since our matrix already holds the perfect, optimal answers for routes using cities 1 to $k-1$, adding those two optimal halves together mathematically guarantees the perfect answer for the newly expanded map.
> 
> **Conclusion:** By unlocking cities one by one, step $n$ unlocks the final city. Since every optimal route in the graph must just use some combination of the $n$ cities, checking this binary choice at every step mathematically guarantees we find the absolute shortest path for every pair.

> [!abstract] 📝 Exam-Ready Writing
> **Claim:** The algorithm correctly computes all-pairs shortest paths by iteratively building optimal paths using subsets of intermediate vertices.
> **Proof by Subproblems:**
> 1. The algorithm iteratively considers whether to include vertex $k$ as an allowed intermediate node on paths.
> 2. Assume that at step $k$, we have correctly computed the optimal paths for all pairs using only intermediate vertices from the restricted set $\{1, 2, \dots, k-1\}$.
> 3. For any pair of nodes $(i, j)$, the new optimal path allowing intermediate nodes up to $k$ has two mutually exclusive possibilities:
>    - **Case 1:** It does not use vertex $k$. The path remains identical to the previous step.
>    - **Case 2:** It does use vertex $k$. The path breaks down into two segments: a path from $i \to k$, and a path from $k \to j$.
> 4. Because the graph contains no negative cycles, the shortest path will visit vertex $k$ exactly once. 
> 5. Thus, the sub-paths $i \to k$ and $k \to j$ only rely on intermediate vertices up to $k-1$. 
> 6. Since we already computed the optimal distances for those segments in the previous step, the new optimal distance is simply the minimum of the old path (Case 1) and the sum of the two segments (Case 2).
> 7. By step $V$, all vertices are allowed as intermediates, guaranteeing the true shortest path is found for all pairs.
