---
tags:
  - algorithms
  - cpp
  - implementations
  - quiz-prep
---

# C++ Implementations

> [!info]- Verified code reference
> Every snippet below was **compiled with g++ 13 (C++17) and executed** — outputs match the Python versions in [[01-Convex-Hull|Convex Hull]], [[02-Divide-and-Conquer|Divide & Conquer]], and [[03-Greedy-Algorithms|Greedy]].
>
> Compile: `g++ -O2 -std=c++17 file.cpp -o file` — run: `./file`
>
> Includes the **lecture's exact Lomuto partition** (D&C-3 slides, slide 12).

---

## 1. Convex Hull

> [!example]- C++: orientation test + Monotone Chain (Andrew)
> ```cpp
> #include <bits/stdc++.h>
> using namespace std;
> using ll = long long;
> using P = pair<ll, ll>;
> 
> // Orientation test: cross product of vectors o->a and o->b
> // >0 left turn (CCW), <0 right turn (CW), =0 collinear
> ll cross(const P& o, const P& a, const P& b) {
>     return (a.first - o.first) * (b.second - o.second)
>          - (a.second - o.second) * (b.first - o.first);
> }
> 
> // Monotone chain: O(n log n), returns hull in CCW order
> vector<P> monotoneChain(vector<P> pts) {
>     sort(pts.begin(), pts.end());                  // sort by (x, y)
>     pts.erase(unique(pts.begin(), pts.end()), pts.end());
>     if (pts.size() <= 1) return pts;
>     vector<P> hull;
>     for (int phase = 0; phase < 2; ++phase) {      // lower then upper hull
>         size_t start = hull.size();
>         for (P p : pts) {
>             while (hull.size() >= start + 2 &&
>                    cross(hull[hull.size()-2], hull.back(), p) <= 0)
>                 hull.pop_back();                   // non-left turn -> pop
>             hull.push_back(p);
>         }
>         hull.pop_back();                           // remove duplicated end
>         reverse(pts.begin(), pts.end());           // upper hull pass
>     }
>     return hull;
> }
> 
> // Test: square + interior + extra points
> // vector<P> pts = {{0,0},{1,0},{1,1},{0,1},{2,2},{3,3},{3,1}};
> // monotoneChain(pts) -> (0,0) (1,0) (3,1) (3,3) (0,1)
> ```

> [!example]- C++: Jarvis March (gift wrapping)
> ```cpp
> ll dist2(const P& a, const P& b) {
>     return (a.first-b.first)*(a.first-b.first) + (a.second-b.second)*(a.second-b.second);
> }
> 
> // O(n*h); h = number of hull vertices. Starts from leftmost point.
> vector<P> jarvisMarch(vector<P> pts) {
>     sort(pts.begin(), pts.end());
>     pts.erase(unique(pts.begin(), pts.end()), pts.end());
>     if (pts.size() < 3) return pts;
>     vector<P> hull;
>     P cur = pts[0];                                // leftmost is on the hull
>     do {
>         hull.push_back(cur);
>         P nxt = pts[0];
>         for (P p : pts) {
>             ll c = cross(cur, nxt, p);
>             // most counter-clockwise; tie -> farthest (avoid dup vertices)
>             if (c > 0 || (c == 0 && dist2(cur, p) > dist2(cur, nxt))) nxt = p;
>         }
>         cur = nxt;
>     } while (cur != hull[0]);                      // wrapped around
>     return hull;
> }
> ```

---

## 2. Divide & Conquer

> [!example]- C++: binary search + merge sort
> ```cpp
> // O(log n) — requires sorted array
> int binarySearch(const vector<int>& a, int target) {
>     int lo = 0, hi = (int)a.size() - 1;
>     while (lo <= hi) {
>         int mid = lo + (hi - lo) / 2;              // avoid overflow
>         if (a[mid] == target) return mid;
>         else if (a[mid] < target) lo = mid + 1;
>         else hi = mid - 1;
>     }
>     return -1;
> }
> 
> void merge(vector<int>& a, int l, int m, int r) {
>     vector<int> L(a.begin()+l, a.begin()+m+1), R(a.begin()+m+1, a.begin()+r+1);
>     int i = 0, j = 0, k = l;
>     while (i < (int)L.size() && j < (int)R.size())
>         a[k++] = (L[i] <= R[j]) ? L[i++] : R[j++]; // stable: <= keeps order
>     while (i < (int)L.size()) a[k++] = L[i++];
>     while (j < (int)R.size()) a[k++] = R[j++];
> }
> 
> // O(n log n); recurrence T(n) = 2T(n/2) + O(n)
> void mergeSort(vector<int>& a, int l, int r) {
>     if (l >= r) return;
>     int m = l + (r - l) / 2;
>     mergeSort(a, l, m);
>     mergeSort(a, m + 1, r);
>     merge(a, l, m, r);
> }
> ```

> [!example]- C++: Quicksort + Lomuto partition (lecture, slide 12)
> ```cpp
> // Lomuto partition: pivot = last element, O(n) per partition
> int partition(vector<int>& a, int low, int high) {
>     int pivot = a[high];              // choose the pivot
>     int i = low - 1;                  // right position of pivot so far
>     for (int j = low; j <= high - 1; ++j)
>         if (a[j] < pivot)             // move all smaller elements left
>             swap(a[++i], a[j]);
>     swap(a[i + 1], a[high]);          // move pivot after smaller elements
>     return i + 1;                     // return its position
> }
> 
> // O(n log n) average, O(n^2) worst (sorted input + last-element pivot)
> void quickSort(vector<int>& a, int low, int high) {
>     if (low < high) {
>         int pi = partition(a, low, high);
>         quickSort(a, low, pi - 1);    // recurse on both sides
>         quickSort(a, pi + 1, high);
>     }
> }
> ```

> [!example]- C++: Kadane (max subarray, O(n)) + closest pair (O(n log n))
> ```cpp
> // Max subarray sum — greedy/DP, beats the D&C O(n log n) version
> int kadane(const vector<int>& a) {
>     int best = a[0], cur = a[0];
>     for (size_t i = 1; i < a.size(); ++i) {
>         cur = max(a[i], cur + a[i]);  // best subarray ENDING at i
>         best = max(best, cur);
>     }
>     return best;
> }
> 
> // Closest pair of points — sweep line, O(n log n)
> double closestPair(vector<P> pts) {
>     sort(pts.begin(), pts.end());
>     double best = DBL_MAX;
>     set<P> active;                    // points within 'best', ordered by y
>     int left = 0;
>     for (const P& p : pts) {
>         while (left < (int)pts.size() && pts[left].first < p.first - best)
>             active.erase(pts[left++]);                 // drop far-left points
>         auto it = active.lower_bound({p.second - best, LLONG_MIN});
>         while (it != active.end() && it->first <= p.second + best) {
>             double d = hypot((double)(p.first - it->second),
>                              (double)(p.second - it->first));
>             best = min(best, d);
>             ++it;
>         }
>         active.insert({p.second, p.first});
>     }
>     return best;
> }
> ```

> [!example]- C++: Karatsuba multiplication
> ```cpp
> // O(n^1.585): 3 recursive multiplications instead of 4
> ll karatsuba(ll x, ll y) {
>     if (x < 10 || y < 10) return x * y;
>     int n = max((int)to_string(x).size(), (int)to_string(y).size());
>     int m = n / 2;
>     ll pow10 = 1; for (int i = 0; i < m; ++i) pow10 *= 10;
>     ll x1 = x / pow10, x0 = x % pow10;   // x = x1*10^m + x0
>     ll y1 = y / pow10, y0 = y % pow10;
>     ll z2 = karatsuba(x1, y1);
>     ll z0 = karatsuba(x0, y0);
>     ll z1 = karatsuba(x1 + x0, y1 + y0) - z2 - z0;
>     return z2 * pow10 * pow10 + z1 * pow10 + z0;
>     // karatsuba(1234, 5678) == 7006652
> }
> ```

---

## 3. Greedy

> [!example]- C++: activity selection + fractional knapsack
> ```cpp
> // Maximize compatible activities — greedy: earliest finish time, O(n log n)
> int activitySelection(vector<pair<int,int>> acts) {
>     sort(acts.begin(), acts.end(),
>          [](auto& a, auto& b) { return a.second < b.second; });  // by finish
>     int cnt = 0, last = INT_MIN;
>     for (auto [s, f] : acts)
>         if (s >= last) { ++cnt; last = f; }
>     return cnt;
> }
> 
> // Fill knapsack maximizing value, fractions allowed — greedy: value/weight
> double fractionalKnapsack(vector<pair<double,double>> items, double cap) {
>     sort(items.begin(), items.end(),
>          [](auto& a, auto& b) { return a.first/a.second > b.first/b.second; });
>     double total = 0;
>     for (auto [v, w] : items) {
>         if (cap >= w) { cap -= w; total += v; }
>         else { total += v * (cap / w); break; }   // take the fraction
>     }
>     return total;
> }
> // fractionalKnapsack({{60,10},{100,20},{120,30}}, 50) == 240
> ```

> [!example]- C++: Huffman coding
> ```cpp
> struct Node { char ch; ll freq; Node *l, *r; };
> struct Cmp { bool operator()(Node* a, Node* b) { return a->freq > b->freq; } };
> map<char,string> huffmanCodes;
> 
> void dfs(Node* root, string code) {                // collect codes
>     if (!root) return;
>     if (root->ch) huffmanCodes[root->ch] = code;   // leaf = symbol
>     dfs(root->l, code + "0");
>     dfs(root->r, code + "1");
> }
> 
> // Greedy: repeatedly merge the two smallest frequencies, O(n log n)
> void huffman(const map<char,ll>& freqs) {
>     priority_queue<Node*, vector<Node*>, Cmp> pq;
>     for (auto [ch, f] : freqs) pq.push(new Node{ch, f, nullptr, nullptr});
>     while (pq.size() > 1) {
>         Node* a = pq.top(); pq.pop();
>         Node* b = pq.top(); pq.pop();
>         pq.push(new Node{0, a->freq + b->freq, a, b});
>     }
>     huffmanCodes.clear();
>     dfs(pq.top(), "");
> }
> // huffman({{'a',45},{'b',13},{'c',12},{'d',16},{'e',9},{'f',5}})
> // -> a:0 b:101 c:100 d:111 e:1101 f:1100
> ```

> [!example]- C++: Dijkstra + Kruskal MST
> ```cpp
> // Shortest paths from src — greedy: smallest tentative distance first
> // O((V+E) log V). REQUIRES non-negative weights.
> vector<ll> dijkstra(const vector<vector<pair<int,ll>>>& g, int src) {
>     vector<ll> dist(g.size(), LLONG_MAX);
>     priority_queue<pair<ll,int>, vector<pair<ll,int>>, greater<>> pq;
>     dist[src] = 0; pq.push({0, src});
>     while (!pq.empty()) {
>         auto [d, u] = pq.top(); pq.pop();
>         if (d > dist[u]) continue;                 // stale heap entry
>         for (auto [v, w] : g[u])
>             if (dist[u] + w < dist[v]) {
>                 dist[v] = dist[u] + w;
>                 pq.push({dist[v], v});
>             }
>     }
>     return dist;
> }
> 
> struct DSU {                                       // union-find
>     vector<int> p;
>     DSU(int n) : p(n) { iota(p.begin(), p.end(), 0); }
>     int find(int x) { return p[x] == x ? x : p[x] = find(p[x]); }
>     bool unite(int a, int b) {
>         a = find(a); b = find(b);
>         if (a == b) return false;
>         p[a] = b; return true;
>     }
> };
> 
> // MST — greedy: lightest edge that doesn't create a cycle, O(E log E)
> ll kruskal(int n, vector<tuple<ll,int,int>> edges) {
>     sort(edges.begin(), edges.end());
>     DSU dsu(n);
>     ll total = 0;
>     for (auto [w, u, v] : edges)
>         if (dsu.unite(u, v)) total += w;
>     return total;
> }
> // kruskal(4, {{10,0,1},{6,0,2},{5,0,3},{15,1,3},{4,2,3}}) == 19
> ```

---

## 4. Quick Reference

> [!summary]- All complexities (C++ same as Python)
>
> | Algorithm | Complexity | Key line |
> |-----------|-----------|----------|
> | Monotone Chain / Graham Scan | O(n log n) | `cross(...) <= 0` → pop |
> | Jarvis March | O(n·h) | pick most CCW point |
> | Merge Sort | O(n log n) | merge two sorted halves |
> | Quicksort (Lomuto) | O(n log n) avg | `swap(a[++i], a[j])` |
> | Binary Search | O(log n) | `mid = lo + (hi-lo)/2` |
> | Kadane | O(n) | `cur = max(a[i], cur+a[i])` |
> | Closest Pair | O(n log n) | strip + set by y |
> | Karatsuba | O(n^1.585) | 3 mults: z2, z0, z1 |
> | Activity Selection | O(n log n) | sort by finish |
> | Fractional Knapsack | O(n log n) | sort by v/w |
> | Huffman | O(n log n) | min-heap, merge 2 smallest |
> | Dijkstra | O((V+E) log V) | `greater<>` min-heap |
> | Kruskal | O(E log E) | DSU union |
>
> Full theory: [[01-Convex-Hull]] · [[02-Divide-and-Conquer]] · [[03-Greedy-Algorithms]] · [[04-Cheat-Sheet]]
