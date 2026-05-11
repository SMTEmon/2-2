## Overview
Dijkstra’s algorithm finds the shortest path from a single source vertex to all other vertices in a weighted graph with **non‑negative edge weights**. It uses a greedy approach, maintaining a set of vertices whose final shortest distance has been determined, and repeatedly selects the vertex with the smallest tentative distance.

> [!NOTE] Non‑negative weights required  
> The algorithm assumes that all edge weights are ≥ 0. If negative edges exist, consider the Bellman‑Ford algorithm instead.

## Step‑by‑Step Description
1. **Initialise distances**  
   Create an array `dist[]` of size `V` (number of vertices) and set every entry to infinity (∞).  
2. **Insert source**  
   Set `dist[source] = 0` and push the pair `(0, source)` into a min‑priority queue.  
3. **Process queue**  
   While the priority queue is not empty:  
   a. Remove the vertex `u` with the smallest distance (the top of the min‑heap).  
   b. **Lazy deletion:** If the popped distance is greater than `dist[u]`, it means `u` has already been processed with a smaller distance. Skip it and continue.  
   c. For every neighbour `v` of `u` (with edge weight `w`):  
      - If `dist[u] + w < dist[v]`, update `dist[v] = dist[u] + w` and push `(dist[v], v)` into the priority queue.  
4. **Termination**  
   When the queue is empty, `dist[]` contains the shortest distance from the source to every vertex.  

## Pseudocode
```python
function Dijkstra(Graph, source):
    V = number of vertices in Graph
    dist = array of size V, filled with INFINITY
    dist[source] = 0
    
    # Min‑heap storing pairs (distance, vertex)
    pq = new PriorityQueue()
    pq.push((0, source))
    
    while pq is not empty:
        d, u = pq.pop()
        
        # Lazy deletion: ignore outdated entries
        if d > dist[u]:
            continue
        
        for each neighbor v, weight w in Graph.adjacency[u]:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                pq.push((dist[v], v))
                
    return dist
```

> [!TIP] Graph representation  
> The pseudocode assumes an adjacency list where `Graph.adjacency[u]` returns a list of `(v, weight)` pairs.

## Complexity
- **Time:** `O((V + E) log V)` with a binary heap.  
  Each vertex can be pushed/popped multiple times, but the lazy deletion ensures at most `E` pushes occur.  
- **Space:** `O(V + E)` for the adjacency list and `O(V)` for the distance array and priority queue.

## Finding the Actual Shortest Paths
To reconstruct the path, maintain an additional `prev` array (size `V`) that stores the predecessor of each vertex. Update it whenever `dist[v]` is updated:
```python
if dist[u] + w < dist[v]:
    dist[v] = dist[u] + w
    prev[v] = u
    pq.push((dist[v], v))
```
After the algorithm finishes, you can trace back from `prev[target]` to the source.

## Example
Consider this graph:  
![Dijkstra example graph](https://upload.wikimedia.org/wikipedia/commons/5/57/Dijkstra_Animation.gif)

| Step | Vertex popped | Distances updated              |
|------|---------------|--------------------------------|
| 0    | –             | `dist[A]=0`, others ∞          |
| 1    | A             | B:7, C:3                       |
| 2    | C             | B:5, D:4                       |
| 3    | D             | B:5 (no change), E:6           |
| 4    | B             | E:5 (updated)                  |
| 5    | E             | – (queue empty)                |

Final distances: `A=0, B=5, C=3, D=4, E=5`.

## References
- Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C. (2009). *Introduction to Algorithms* (3rd ed.). MIT Press.  
- [Dijkstra’s Algorithm – Wikipedia](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)

> [!abstract] Key Points  
> - Always use a **min‑priority queue** for efficiency.  
> - The “lazy deletion” (`if d > dist[u]`) avoids processing stale entries and is essential when using a standard `decrease‑key`‑less priority queue.  
> - Works only with **non‑negative weights**.
