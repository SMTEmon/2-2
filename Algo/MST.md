---
tags:
  - computer-science
  - algorithms
  - graph-theory
  - revision
---

# 🌳 Minimum Spanning Trees (MST) — Ultimate Revision Note

## 1. Core Definitions & Concepts

### Spanning Tree
[cite_start]A **Spanning Tree** is a subgraph of an undirected, connected graph $G = (V, E)$ that[cite: 2, 3]:
* [cite_start]🟩 **Connects ALL vertices** together[cite: 2].
* [cite_start]🚫 **Contains NO cycles** (it is acyclic)[cite: 2].
* [cite_start]📐 Has exactly **$V - 1$ edges**[cite: 2].

### Minimum Spanning Tree (MST)
[cite_start]An MST of a weighted, connected, undirected graph is a spanning tree whose **sum of total edge weights is minimized**.
* [cite_start]It satisfies: $\text{Weight}(T) \le \text{Weight}(T')$ for any other possible spanning tree $T'$[cite: 3].
* [cite_start]Used extensively to connect network components at the lowest cost[cite: 3].

---

## 2. Prim's Algorithm (Vertex-Centric Approach)

### 💡 Core Concept
[cite_start]Grows a single tree structure node-by-node starting from an arbitrary root vertex.
1. [cite_start]Pick any starting vertex and mark it as visited.
2. [cite_start]Look at all edges extending from the currently visited tree to unvisited vertices.
3. [cite_start]Pick the edge with the **minimum weight** connecting to an unvisited vertex.
4. [cite_start]Add that vertex/edge to the tree and repeat until all vertices are visited.

### 💻 Pseudocode Variants

#### Variant A: Competitive Programming / Priority Queue Style
```cpp
Prims(G, source):
    for each vertex v in G:
        cost[v] ← ∞
        vis[v] ← 0
    cost[source] ← 0
    PQ.push({0, source}) // Stores pairs of {weight, vertex}
    res ← 0

    while PQ is not empty:
        node = PQ.pop()
        wt = node.first
        u = node.second
        
        if vis[u] == 1: continue
        vis[u] ← 1
        res += wt       // Accumulate total MST weight
        
        for each neighbor v of u:
            if vis[v] == 1: continue
            if G[u][v] < cost[v]:
                cost[v] = G[u][v]
                PQ.push({G[u][v], v})
    return res
````

#### Variant B: CLRS / Decrease-Key Style

Plaintext

```
PRIM(G, w, r):
    for each u in G:
        u.key = INF
        u.p = NIL
    r.key = 0
    Q = G             // Min-Priority Queue tracking all vertices V
    while Q is not empty:
        u = EXTRACT-MIN(Q)
        for each v in Adj[u]:
            if v in Q and w(u,v) < v.key:
                v.p = u
                v.key = w(u,v) // Triggers Decrease-Key operation
```

> [!warning] **Prim's vs. Dijkstra: The Critical Exam Distinction**
> 
> - **Dijkstra** selects an edge $(u, v)$ based on global path distance from the source:
>     
>     $$\text{If } \text{cost}[u] + \text{weight}(u, v) < \text{cost}[v]$$
>     
> - **Prim's** selects an edge $(u, v)$ based solely on local cost to connect to the existing cut:
>     
>     $$\text{If } \text{weight}(u, v) < \text{cost}[v]$$
>     

### ⏱️ Complexity

- **Time Complexity:** $O((V + E) \log V)$ using a Binary Heap. Can be optimized to $O(E + V \log V)$ using a Fibonacci Heap.
    
- **Space Complexity:** $O(V + E)$.
    

## 3. Kruskal's Algorithm (Edge-Centric Approach)

### 💡 Core Concept

Grows a forest of trees and merges them into a single tree by processing sorted edges.

1. **Sort** all edges in non-decreasing order of their weights.
    
2. Iterate through the sorted list and pick the smallest edge.
    
3. Check if adding this edge forms a **cycle** with the edges chosen so far.
    
    - 💡 _Crucial tool used for cycle detection:_ **Disjoint Set Union (DSU)**.
        
4. If no cycle is formed, include it in the MST. Otherwise, discard it.
    
5. Stop when you have successfully added exactly **$V - 1$ edges**.
    

### 💻 Pseudocode

Plaintext

```
KRUSKAL(G):
    A = ∅                        // Will contain the final MST edges
    for each vertex v in G.V:
        MAKE-SET(v)
    
    SORT the edges of G.E into non-decreasing order by weight w
    
    for each edge (u, v) in sorted G.E:
        if FIND-SET(u) ≠ FIND-SET(v):   // If sets are disjoint, no cycle forms
            A = A ∪ {(u, v)}
            UNION(u, v)                 // Merge the two components
            if |A| == V - 1:            // Early stop condition
                break
    return A
```

### ⏱️ Complexity

- **Time Complexity:** $O(E \log E + E \log V) = O(E \log V)$.
    
    - _Reasoning:_ Sorting edges takes $O(E \log E)$. DSU operations take near-linear time $O(E \cdot \alpha(V))$. Since $E \le V^2$, $\log E \le 2\log V$, making sorting the dominating term.
        
- **Space Complexity:** $O(V + E)$ to store graph edges and DSU tracking arrays.
    

## 4. Disjoint Set Union (DSU) — The Engine Behind Kruskal's

Since cycle testing needs to be fast, Kruskal's relies on DSU to maintain non-overlapping subsets of nodes.

### Three Core Operations

1. `MAKE-SET(v)`: Initializes a new set containing only element $v$, making $v$ its own parent.
    
2. `FIND-SET(v)`: Follows parent pointers up to determine the "representative" (root) of the set containing $v$.
    
3. `UNION(u, v)`: Merges the sets containing elements $u$ and $v$ if they belong to different roots.
    

### Essential Optimizations

To avoid worst-case $O(V)$ chain lookups, we implement two core strategies:

- **Path Compression (in `FIND`):** Flattens the tree structure by pointing every looked-up node directly to the root, making subsequent lookups $O(1)$.
    
- **Union by Rank/Size (in `UNION`):** Attaches the shorter tree under the root of the taller tree, keeping the tree flat.
    

C++

```
// DSU Implementation Framework
struct DSU {
    vector<int> parent, rank;
    
    DSU(int n) {
        parent.resize(n);
        rank.resize(n, 0);
        for(int i = 0; i < n; i++) parent[i] = i; // MAKE-SET
    }
    
    int find_set(int v) {
        if (v == parent[v]) return v;
        return parent[v] = find_set(parent[v]);   // Path Compression
    }
    
    void union_sets(int a, int b) {
        a = find_set(a);
        b = find_set(b);
        if (a != b) {
            if (rank[a] < rank[b]) swap(a, b);    // Union by Rank
            parent[b] = a;
            if (rank[a] == rank[b]) rank[a]++;
        }
    }
};
```

## 5. Cheatsheet Summary Table

|**Metric / Property**|**Prim's Algorithm PDF+ 2**|**Kruskal's Algorithm PDF+ 1**|
|---|---|---|
|**Strategy Approach**|**Vertex-centric:** Spreads outward from a node.|**Edge-centric:** Sorts global list of edges.|
|**Graph Structure**|Grows a single coherent tree structure.|Builds an independent forest that merges together.|
|**Data Structures**|Priority Queue (Min-Heap) + Visited Array.|Array List + Disjoint Set Union (DSU).|
|**Time Complexity**|$O((V + E) \log V)$|$O(E \log V)$|
|**Best Used On...**|**Dense Graphs** ($E \approx V^2$)|**Sparse Graphs** ($E \approx V$)|