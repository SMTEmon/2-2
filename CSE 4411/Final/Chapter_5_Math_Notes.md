---
title: "Chapter 5: Network Layer - Control Plane (Math & Algorithms)"
course: "CSE 4411"
chapter: 5
tags:
  - cse4411
  - networking
  - math
  - final
---

# Chapter 5: Control Plane Math & Algorithms

This note provides a detailed guide on the mathematical and computational problems from Chapter 5, specifically focusing on Dijkstra's Link-State algorithm and the Bellman-Ford Distance-Vector algorithm.

---

## 1. Dijkstra’s Link-State Routing Algorithm

Dijkstra's algorithm is a centralized algorithm used to compute the least-cost path from a single source node to all other nodes in a network.

### Notation and Setup
*   **$N'$:** The set of nodes whose least-cost path from the source is *definitively* known.
*   **$c(x,y)$:** The direct link cost from node $x$ to node $y$. If $x$ and $y$ are not direct neighbors, $c(x,y) = \infty$.
*   **$D(v)$:** The current estimate of the least-cost path from the source to destination $v$.
*   **$p(v)$:** The predecessor node along the current least-cost path from the source to $v$.

### Algorithm Steps

**Initialization:**
1. Add the starting source node (let's call it $u$) to $N'$ ($N' = \{u\}$).
2. For all nodes $v$ in the network:
   * If $v$ is a direct neighbor of $u$, set $D(v) = c(u,v)$.
   * Otherwise, set $D(v) = \infty$.

**Loop (Iterative Phase):**
1. Find a node $w$ that is **not** in $N'$ such that $D(w)$ is a minimum.
2. Add $w$ to $N'$.
3. Update $D(v)$ for all neighbors $v$ of $w$ that are **not** in $N'$:
   $$D(v) = \min \Big( D(v), D(w) + c(w,v) \Big)$$
   *(Meaning: Is the current path to $v$ cheaper, or is it cheaper to go through the newly added node $w$ and then to $v$?)*
4. Repeat the loop until all nodes are in $N'$.

### Example Problem
*Consider a network where you need to find the shortest paths from source node `u`.*

1. **Step 0 (Init):** Look at $u$'s direct neighbors. Suppose it connects to $v$ (cost 2), $x$ (cost 1), and $w$ (cost 5). 
   - $N'$ = `{u}`
   - $D(v)=2$, $D(x)=1$, $D(w)=5$, all others $\infty$.
2. **Step 1:** The node with the smallest $D$ value not in $N'$ is $x$ ($D(x)=1$). 
   - Add $x$ to $N'$. $N'$ = `{u, x}`.
   - Look at $x$'s neighbors. If going through $x$ is cheaper than the current $D$ value, update it. For example, if $x$ connects to $w$ with a cost of 3: $D(w) = \min(5, D(x) + c(x,w)) = \min(5, 1+3) = 4$. So $D(w)$ updates to 4, and $p(w)$ becomes $x$.
3. **Continue:** Select the next smallest $D$ value not in $N'$ and repeat until all nodes are included.

### Complexity
- **Time Complexity:** $O(n^2)$ for $n$ nodes, because in the $k$-th iteration, you must search through $n-k$ nodes to find the minimum. This can be optimized to $O(n \log n)$ using a min-priority queue (heap).
- **Message Complexity:** $O(n^2)$ total messages sent across the network, because each of the $n$ nodes broadcasts link-state info to all other $n$ nodes.

> [!warning] Oscillations
> If link costs depend on traffic volume (load-sensitive), Dijkstra's algorithm can cause route oscillations. If everyone switches to an empty "cheap" path, it becomes congested and expensive, causing everyone to switch back in the next iteration.

---

## 2. Distance-Vector (DV) Routing Algorithm

The DV algorithm is distributed and iterative. Nodes only exchange information with their immediate neighbors.

### The Bellman-Ford Equation
The algorithm relies heavily on this dynamic programming equation.
Let $d_x(y)$ be the cost of the least-cost path from $x$ to $y$.

$$d_x(y) = \min_v \Big\{ c(x,v) + d_v(y) \Big\}$$

*(The minimum is taken over all direct neighbors $v$ of node $x$. It asks: "For all my neighbors, what is my cost to reach that neighbor PLUS that neighbor's cost to reach the destination? I will choose the minimum.")*

### Algorithm Steps
1. **Initialization:** Each node $x$ initializes its Distance Vector $D_x$. It knows the cost to its direct neighbors. It sets the cost to all other nodes as $\infty$.
2. **Exchange:** Each node sends its distance vector to its neighbors.
3. **Update:** When node $x$ receives a new DV from a neighbor $v$, it updates its own estimates using the Bellman-Ford equation.
4. **Trigger:** If node $x$'s DV changes as a result of the update, it sends its new DV to all its neighbors.
5. **Wait:** The algorithm stops and waits if no DVs change (self-stopping). It wakes up if a local link cost changes or if an update message is received.

### Example Problem
*Suppose node `y` receives an updated distance vector from its neighbor `z`.*

- `y` wants to know its cheapest path to `x`.
- `y` looks at its direct neighbors (say, $w$ and $z$).
- Cost to reach $x$ via $w$: $c(y,w) + D_w(x)$
- Cost to reach $x$ via $z$: $c(y,z) + D_z(x)$
- Node `y` calculates $D_y(x) = \min( c(y,w) + D_w(x), c(y,z) + D_z(x) )$.
- If $D_y(x)$ changes, `y` updates its routing table and sends its new DV to its neighbors.

### The Count-to-Infinity Problem
This is a critical vulnerability of the DV algorithm.

- **Good news travels fast:** If a link cost drops, the new, cheaper cost is quickly adopted and propagated.
- **Bad news travels slow:** If a link fails or its cost increases drastically, a routing loop can form.
  - *Example:* $x$ and $y$ are neighbors. $y$ reaches destination $A$ through $x$. If the link to $A$ fails, $x$ thinks it can route to $A$ through $y$ (because $y$ previously advertised a path to $A$). $y$ thinks it can route to $A$ through $x$.
  - They continuously update each other, steadily incrementing their costs ($C+1, C+2, C+3...$) to infinity before realizing the path is actually dead.

#### Poisoned Reverse Solution
To prevent 2-node loops, if $z$ routes through $y$ to get to $x$, $z$ will advertise to $y$ that its distance to $x$ is infinity ($D_z(x) = \infty$). This prevents $y$ from routing back through $z$. 
*(Note: This does not solve loops involving 3 or more nodes).*
