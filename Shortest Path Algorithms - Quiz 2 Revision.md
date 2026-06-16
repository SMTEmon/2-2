# Shortest Path Algorithms — Quiz 2 Revision

---

## Big Picture

|Feature|Dijkstra|Bellman-Ford|Floyd-Warshall|
|---|---|---|---|
|Source|Single|Single|All-pairs|
|Negative weights|❌|✅|✅|
|Negative cycle detection|❌|✅|✅ (diagonal)|
|Approach|Greedy|DP|DP|
|Time|O((V+E) log V)|O(V·E)|O(V³)|
|Space|O(V+E)|O(V)|O(V²)|
|Real-world use|OSPF (networking)|RIP (routing)|Dense graph analysis|

---

## 1. Dijkstra's Algorithm

### Core Idea

Greedily finalize the vertex with the smallest known distance. Once finalized (added to set S), never revisited.

### Pseudocode

```python
dist[source] = 0; dist[v] = ∞ for all v ≠ source
vis[v] = 0 for all v
PQ.push({0, source})

while PQ not empty:
    d, u = PQ.pop()
    if vis[u]: continue
    vis[u] = 1
    for each neighbor v of u:
        if vis[v]: continue
        if d + weight(u,v) < dist[v]:
            dist[v] = d + weight(u,v)
            PQ.push({dist[v], v})
```

### Correctness — Loop Invariant

> **Invariant:** For every u ∈ S, `d[u] = δ(s, u)` (exact shortest distance).

**Proof (induction on |S|):**

- **Base:** S = ∅, holds vacuously.
    
- **Step (the key argument):** We just extracted vertex u (the minimum in the queue). We want to show d[u] is exact. Suppose it isn't — suppose there's a shorter path to u we missed.
    
    That path starts at s (inside S) and ends at u (outside S). So it _must_ cross the boundary from finalized to unfinalized at some point. Call the first edge that crosses x→y (x is finalized, y is not).
    
    Now ask: could any unfinalized vertex m give a shorter path to u? If the path went S→...→m→...→u with cost < d[u], then d[m] < d[u] (because the m→u portion has non-negative weight, so m's distance alone is already smaller). But if d[m] < d[u], **Extract-Min would have picked m before u** — meaning m would already be finalized. Contradiction: we said m was unfinalized. ✗
    
    So the full chain is:
    
    > alleged shorter path → goes through unfinalized m → d[m] < d[u] → Extract-Min picks m first → m is already finalized → contradiction ✗
    
    ∴ no shorter path exists → d[u] = δ(s,u) ✅
    

### Why Negative Weights Break It — Concrete Example

```
A --5--> B
A --6--> C --(-3)--> B
```

- Dijkstra pops B with d[B]=5, marks it visited.
- Later finds path A→C→B with cost 6+(−3)=**3**, but B is already finalized.
- **Result: wrong answer (5 instead of 3).**

The step "d[m] < d[u] means m gets extracted first" breaks down with negative weights — a path can get _cheaper_ the further you go, so a later vertex m might look expensive now but lead to a cheaper overall path. Extract-Min can no longer guarantee the chosen vertex is settled.

---

## 2. Bellman-Ford Algorithm

### Core Idea

Relax **every** edge, **V−1** times. After pass i, all shortest paths using ≤ i edges are exact. Then do one more pass to detect negative cycles.

### Pseudocode (with negative cycle detection)

```python
dist[source] = 0; dist[v] = ∞ for all v ≠ source

for i in range(V):                        # V passes total
    for each edge (u, v, wt):
        if dist[u] != INF and dist[u] + wt < dist[v]:
            if i == V - 1:                # Vth relaxation → negative cycle
                return [-1]
            dist[v] = dist[u] + wt

return dist
```

> ⚠️ Loop runs **V** times (not V−1) so the Vth iteration acts as the detection pass. Alternatively: loop V−1 times, then do a separate detection pass.

### Simulation Example

Graph: S→A=2, S→B=1, S→C=30, B→C=3, C→D=2, D→B=1

```
Initial:  S=0, A=∞, B=∞, C=∞, D=∞
Pass 1:   S=0, A=2,  B=1,  C=30, D=∞
Pass 2:   S=0, A=2,  B=1,  C=4,  D=∞   (S->B->C = 1+3=4)
Pass 3:   S=0, A=2,  B=1,  C=4,  D=6   (C->D = 4+2)
Pass 4:   no change (converged)
```

### Correctness — Induction on Passes

> **Claim:** After i passes, every vertex whose shortest path uses ≤ i edges has the correct distance.

**Base case (i=0):** d[S]=0=δ(S,S). Only S is reachable in 0 edges. ✅

**Inductive hypothesis:** After i passes, every vertex reachable via ≤ i edges has the exact distance.

**Inductive step:** Take any vertex v whose shortest path uses exactly i+1 edges: S→...→u→v. The sub-path S→...→u uses ≤ i edges, so by the IH, d[u] is already exact after i passes. In pass i+1 we relax every edge — including (u,v) — so:

> d[v] ≤ d[u] + w(u,v) = δ(S,u) + w(u,v) = δ(S,v) ✅

**Why V−1 passes suffice:** No negative cycles → all shortest paths are simple → at most V−1 edges → V−1 passes cover everything.

### Negative-Cycle Detection

If on pass V any edge (u,v) still satisfies `d[u] + w(u,v) < d[v]`:

- Still updating after V−1 passes → path needs >V−1 edges → some vertex repeats → there is a cycle.
- If that cycle were non-negative, removing it gives a shorter or equal path — contradicting that V−1 passes weren't enough.
- **∴ cycle must be negative.** ✅

### Why Negative Weights Are Fine (Unlike Dijkstra)

Dijkstra commits to a vertex being final the moment it's extracted. Bellman-Ford makes no such commitment — it re-relaxes every edge every pass, so an underestimate from an early pass gets corrected in a later pass once a better sub-path is known.

---

## 3. Floyd-Warshall Algorithm

### Core Idea

All-pairs shortest path. DP over intermediate vertices: for every pair (i,j), ask _"is routing through vertex k shorter?"_ — build up by considering vertices 1, 2, …, n as intermediates one by one.

### Formal Definition

$$d^{(k)}[i][j] = \text{weight of shortest path from } i \text{ to } j \text{ with intermediates} \subseteq {1, \ldots, k}$$

Key observation: **d^(n)[i][j] = δ(i,j)** since k=n places no restriction on intermediates.

### Recurrence

$$d^{(k)}[i][j] = \min!\left(d^{(k-1)}[i][j],\ d^{(k-1)}[i][k] + d^{(k-1)}[k][j]\right)$$

### Pseudocode

```python
# Initialization
dist[i][i] = 0          for all i
dist[i][j] = w(i,j)     if edge (i,j) exists
dist[i][j] = ∞          otherwise

# Main loop — k MUST be outermost
for k in range(V):
    for i in range(V):
        for j in range(V):
            if dist[i][k] != INF and dist[k][j] != INF:
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
```

> ⚠️ The `k` loop **must be outermost** — this is the key to correctness.

### Correctness — Loop Invariant

> **Invariant:** After the k-th iteration, `d[i][j] = d^(k)[i][j]` for all pairs (i,j).

**Proof (induction on k):**

- **Base (k=0):** No intermediates → d[i][j] is either 0 (i=j), direct edge weight, or ∞. Matches d^(0). ✅
- **Step:** Fix pair (i,j). A shortest path restricted to intermediates in {1,…,k} either:
    - **Case 1 — doesn't use k:** All intermediates in {1,…,k−1} → weight = d^(k−1)[i][j] = current d[i][j] by IH. ✅
    - **Case 2 — uses k:** Decompose path as p₁ = i→…→k and p₂ = k→…→j.
        - k does **not** repeat as intermediate in p₁ or p₂ (if it did, that sub-walk through k is a closed walk; no negative cycle ⟹ non-negative weight ⟹ removing it gives equal/shorter path, contradicting p being shortest). ✅
        - ∴ intermediates of p₁, p₂ ⊆ {1,…,k−1} → w(p₁) = d^(k−1)[i][k], w(p₂) = d^(k−1)[k][j]
        - Note: d[i][k] and d[k][j] are **not modified** during iteration k (d[k][k]=0 ensures no decrease through k itself).
        - Combined: d^(k)[i][j] = min(d[i][j], d[i][k] + d[k][j]) = exactly the update rule. ✅

After all n iterations: d[i][j] = d^(n)[i][j] = δ(i,j). ✅

### Negative Cycle Detection

```python
for i in range(V):
    if dist[i][i] < 0:
        return "Negative cycle detected"
```

- dist[i][i] starts at 0. After the algorithm, if negative → path i→…→i has negative total weight → negative cycle exists.
- **Why the no-negative-cycle assumption is needed in the proof:** Case 2 uses "no negative cycle" to conclude k doesn't repeat in sub-paths. Without this, δ(i,j) = −∞ for some pairs and the finite matrix d can't represent true distances.

---

## Key Distinctions to Remember

- **Dijkstra** — fast, greedy, non-negative weights only; proof by cut argument
- **Bellman-Ford** — slow, DP, handles negatives; proof by induction on passes; detects neg cycles on Vth pass
- **Floyd-Warshall** — all-pairs, DP over intermediates; k must be outermost; detects neg cycles via diagonal

## Optimal Substructure (foundation of all three proofs)

> A sub-path of a shortest path is itself a shortest path.

## All-Pairs Complexity Comparison

|Approach|Time|
|---|---|
|Dijkstra × V (non-negative)|O(V(V+E) log V)|
|Bellman-Ford × V|O(V²·E)|
|Floyd-Warshall|O(V³)|

Floyd-Warshall wins on dense graphs where E ≈ V².