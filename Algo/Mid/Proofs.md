# Intuitive Algorithm Proofs

## 1. Dijkstra's Algorithm
> [!tip] The Logical Proof (The Greedy Contradiction)
> Let's prove this without heavy math. Suppose Dijkstra makes a mistake and extracts node $u$, but there is actually a "secret" shorter path to $u$.
> 1. **The Crossing:** This secret shorter path must start at $s$ (which is finalized) and eventually cross into the unvisited nodes. Let $y$ be the very first unvisited node on this secret path.
> 2. **The Trap (No negative weights):** Because there are no negative weights, the rest of the path from $y$ to $u$ can only *add* more distance. This means the true distance to get to $y$ must be less than or equal to the total distance to get to $u$.
> 3. **The Contradiction:** Dijkstra always greedily picks the unvisited node with the absolute smallest current distance. If $y$ is on a shorter path to $u$, $y$ must be closer (or equal) to the source than $u$. Thus, the algorithm would have extracted $y$ before $u$!
> 
> Since Dijkstra chose $u$, our assumption that a secret shorter path exists must be wrong. $u$'s distance is perfectly calculated.

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

## 3. Floyd-Warshall Algorithm
> [!tip] The Logical Proof (The Expanding Layovers)
> Let's prove this without heavy induction math. The algorithm works by gradually unlocking cities you can use as "layovers".
> 1. **The Setup:** Initially, you are allowed zero layovers. You can only take direct flights.
> 2. **The Decision:** When City $k$ is unlocked as a potential new layover, any optimal route between $i$ and $j$ faces a simple binary choice: either it uses City $k$, or it doesn't.
> 3. **The Trap (No negative cycles):** If the optimal route *does* use City $k$, it will fly from $i$ to $k$, and then from $k$ to $j$. Crucially, because there are no negative cycles to spin around in, the route will only visit City $k$ exactly once.
> 4. **The Guarantee:** Because it only visits $k$ once, the two halves of the trip ($i \to k$ and $k \to j$) can only use the *previously unlocked* cities (cities 1 through $k-1$). Since our matrix already holds the perfect, optimal answers for routes using cities 1 to $k-1$, adding those two optimal halves together mathematically guarantees the perfect answer for the newly expanded map.
> 
> **Conclusion:** By unlocking cities one by one, step $n$ unlocks the final city. Since every optimal route in the graph must just use some combination of the $n$ cities, checking this binary choice at every step mathematically guarantees we find the absolute shortest path for every pair.
