# CSE 4404 Lab Midterm — Practice Sheet

Covers: Sorting (Lab 1–2), Graph traversal / BFS / DFS / topo sort (Lab 3),
Weighted shortest paths — Dijkstra & Bellman-Ford (Lab 4–5), Floyd-Warshall & MST (Lab 6), DP (Lab 7).

**Exam bar (per teacher): "Patient Zero" level ≈ CF 1200–1300.** Easy algorithm, fiddly implementation.

Legend:
- ★ = listed in the official End-of-Lab practice sheet (higher chance the teacher reuses it — don't put its exact solution on your cheatsheet)
- 🎯 = same difficulty band as the lab tasks / Patient Zero (≈ CF 1100–1400) — your main target
- (~NNNN) = approximate Codeforces rating

---

## 🎯 Exam-level band — practice these first (≈ Patient Zero, CF 1100–1400)

The closest matches to what the exam will actually look like:

- 🎯 **Monsters** (CSES 1194, ~1400) — multi-source grid BFS, *the* closest twin of Patient Zero · https://cses.fi/problemset/task/1194
- 🎯★ **Labyrinth** (CSES 1193, ~1200) — grid BFS + reconstruct path · https://cses.fi/problemset/task/1193
- 🎯★ **Building Roads** (CSES 1666, ~1100) — components / DSU · https://cses.fi/problemset/task/1666
- 🎯 **Round Trip** (CSES 1669, ~1300) — cycle detection · https://cses.fi/problemset/task/1669
- 🎯★ **Learning Languages** (CF 277A, ~1400) — DSU / components · https://codeforces.com/contest/277/problem/A
- 🎯★ **High Score** (CSES 1673, ~1400) — Bellman-Ford + neg-cycle → -INF · https://cses.fi/problemset/task/1673
- 🎯 **Shortest Routes II** (CSES 1672, ~1200) — Floyd-Warshall · https://cses.fi/problemset/task/1672
- 🎯★ **Road Reparation** (CSES 1675, ~1200) — Kruskal MST · https://cses.fi/problemset/task/1675
- 🎯★ **Coin Combinations I** (CSES 1631, ~1200) — DP counting · https://cses.fi/problemset/task/1631
- 🎯★ **Frog 2 / K Steps** (AtCoder DP B, ~1050) — sliding-window DP · https://atcoder.jp/contests/dp/tasks/dp_b

---

## 1. Sorting (Lab 1–2)

Know: comparison sorts by hand, counting & radix (non-comparison, for huge n / big values — Lab 2 TLE trap), binary search / lower_bound for range counts, greedy rearrangement (exchange argument).

- ★ Game (CF 984A, ~800) · https://codeforces.com/contest/984/problem/A
- ★ Gravity Flip (CF 405A, ~900) · https://codeforces.com/contest/405/problem/A
- ★ Valid Anagram (LeetCode easy, ~800) · https://leetcode.com/problems/valid-anagram/
- ★ Jagged Swaps (CF 1896A, ~800) · https://codeforces.com/contest/1896/problem/A
- 🎯★ Unforgivable Curse Easy (CF 1800E1, ~1500) · https://codeforces.com/contest/1800/problem/E1
- Unforgivable Curse Hard (CF 1800E2, ~2100) — above exam bar · https://codeforces.com/contest/1800/problem/E2

Extra (not in lab sheets):
- Distinct Numbers (CSES 1621, ~800) · https://cses.fi/problemset/task/1621
- 🎯 Ferris Wheel (CSES 1090, ~1100) · https://cses.fi/problemset/task/1090
- 🎯 Towers (CSES 1073, ~1400) · https://cses.fi/problemset/task/1073

---

## 2. Graph Traversal — BFS / DFS / Topo Sort / DSU (Lab 3 — exam bar lives here)

Know: BFS = unweighted shortest path, multi-source BFS on a grid (= Patient Zero), DFS topo sort + cycle detection, components & cycle counting with DSU.

- 🎯★ Counting Rooms (CSES 1192, ~1100) — grid components · https://cses.fi/problemset/task/1192
- 🎯★ Labyrinth (CSES 1193, ~1200) — grid BFS + path · https://cses.fi/problemset/task/1193
- 🎯★ Building Roads (CSES 1666, ~1100) — DSU · https://cses.fi/problemset/task/1666
- 🎯★ Learning Languages (CF 277A, ~1400) — DSU · https://codeforces.com/contest/277/problem/A
- 🎯★ Kefa and Park (CF 580C, ~1500) — tree DFS · https://codeforces.com/contest/580/problem/C

Extra (not in lab sheets):
- 🎯 Message Route (CSES 1667, ~1100) — BFS + reconstruct · https://cses.fi/problemset/task/1667
- 🎯 Round Trip (CSES 1669, ~1300) — cycle detection · https://cses.fi/problemset/task/1669
- 🎯 Monsters (CSES 1194, ~1400) — multi-source grid BFS (closest to Patient Zero) · https://cses.fi/problemset/task/1194
- 🎯 Course Schedule II (LeetCode medium, ~1400) — topo sort · https://leetcode.com/problems/course-schedule-ii/

---

## 3. Weighted Shortest Paths — Dijkstra & Bellman-Ford (Lab 4–5)

Know: Dijkstra (PQ, non-negative), Bellman-Ford (negative edges), negative-cycle detection & -INF propagation, multi-source, arbitrage = neg cycle after −log.

- 🎯★ High Score (CSES 1673, ~1400) — Bellman-Ford, -INF · https://cses.fi/problemset/task/1673
- 🎯★ Cycle Finding (CSES 1197, ~1500) — detect + print neg cycle · https://cses.fi/problemset/task/1197
- ★ Flight Discount (CSES 1195, ~1600) — Dijkstra with a state · https://cses.fi/problemset/task/1195
- ★ Roads in Berland (CF 25C, ~2000) — incremental all-pairs, above bar · https://codeforces.com/contest/25/problem/C
- ★ Edge Deletion (ABC243 E, ~1500) · https://atcoder.jp/contests/abc243/tasks/abc243_e
- ★ Signal Hill (DMOJ) · https://dmoj.ca/problem/dmpg15s4
- ★ Greg and Graph (CF 295B, ~1700) — = Lab 6 Task A · https://codeforces.com/problemset/problem/295/B

Extra (not in lab sheets):
- 🎯 Shortest Routes I (CSES 1671, ~1100) — plain Dijkstra · https://cses.fi/problemset/task/1671
- Flight Routes (CSES 1196, ~1800) — above bar · https://cses.fi/problemset/task/1196

---

## 4. Floyd-Warshall / All-Pairs (Lab 6 Task A)

Know: triple loop (k outer!), k = intermediate vertex, reverse-deletion trick.

- 🎯 Shortest Routes II (CSES 1672, ~1200) — textbook Floyd · https://cses.fi/problemset/task/1672
- ★ Greg and Graph (CF 295B, ~1700) — Floyd + reverse deletion, = Lab 6 Task A · https://codeforces.com/problemset/problem/295/B

---

## 5. Minimum Spanning Tree — Kruskal / Prim + DSU (Lab 6 Task B–C)

Know: Kruskal (sort edges + DSU), Prim (PQ), forced-order "add only if it merges two components" (= Urgent Cables), detect impossible = fewer than n−1 edges used.

- 🎯★ Road Reparation (CSES 1675, ~1200) — Kruskal, detect impossible · https://cses.fi/problemset/task/1675
- 🎯★ Building Roads (CSES 1666, ~1100) · https://cses.fi/problemset/task/1666
- ★ Minimum Spanning Tree (CF 17B, ~1700) · https://codeforces.com/problemset/problem/17/B
- ★ Edges in MST (CF 160D, ~2100) — above bar · https://codeforces.com/problemset/problem/160/D

---

## 6. Dynamic Programming (Lab 7)

Know: 1-D transitions (staircase/frog with blocked steps + toll), K-step sliding frog, pick-or-skip (house robber). Recurrence → base case → fill order.

- ★ Frog 1 (AtCoder DP A, ~800) · https://atcoder.jp/contests/dp/tasks/dp_a
- 🎯★ Frog 2 / K Steps (AtCoder DP B, ~1050) · https://atcoder.jp/contests/dp/tasks/dp_b
- 🎯★ Minimizing Coins (CSES 1634, ~1100) · https://cses.fi/problemset/task/1634
- 🎯★ Coin Combinations I (CSES 1631, ~1200) · https://cses.fi/problemset/task/1631
- ★ Maximum Sum Increasing Subsequence (GfG) · https://www.geeksforgeeks.org/maximum-sum-increasing-subsequence-dp-14/
- ★ Counting Staircase Ways (HackerRank / step sizes 1,2)

Extra (not in lab sheets):
- Dice Combinations (CSES 1633, ~1000) · https://cses.fi/problemset/task/1633
- Removing Digits (CSES 1637, ~1000) · https://cses.fi/problemset/task/1637
- 🎯 Grid Paths (CSES 1638, ~1100) · https://cses.fi/problemset/task/1638
- 🎯 House Robber (LeetCode, ~1200) — = Lab 7 Task D · https://leetcode.com/problems/house-robber/

---

## Notes for the cheatsheet (teacher's rule)

- ★ problems are the ones the teacher flagged as "popular" — bring **generic templates**, not their exact solutions.
- The exam bar is Patient Zero (~1250): easy algorithm, careful implementation. Drill the 🎯 band until you can code each cold in under 10 min.
- Non-comparison sort when n or values are huge (Lab 2 trap). Floyd loop order = k, i, j. MST impossible = fewer than n−1 edges. Neg cycle → reachable nodes = −INF.
