---
tags:
  - algorithms
  - cheatsheet
  - quiz-prep
---

# Algorithms Quiz — Cheat Sheet

> [!info]- One page, everything you need
> Full notes: [[00-Algorithms-Quiz-Index|Index]] · [[01-Convex-Hull|Convex Hull]] · [[02-Divide-and-Conquer|Divide & Conquer]] · [[03-Greedy-Algorithms|Greedy]]
>
> Format: **one line per algorithm → complexity → why it works**. Memorize the tables, then practice tracing.

---

## 1. Convex Hull

> [!note]- Core facts
> - Hull = smallest convex polygon containing all points. h = #hull vertices.
> - **One primitive:** orientation test → $cross(O,A,B) = (A_x{-}O_x)(B_y{-}O_y) - (A_y{-}O_y)(B_x{-}O_x)$; **>0 left/CCW, <0 right/CW, =0 collinear**.
> - Lower bound: **Ω(n log h)**; optimal = Chan's.
> - Leftmost point is always on the hull. Deduplicate points; handle n < 3; decide collinear policy.

| Algorithm | Time | Idea | Exam cue |
|-----------|------|------|----------|
| Naive pair-check | O(n³) | pair is hull edge iff all others on one side | "brute force" |
| **Jarvis March** | **O(n·h)** | walk hull, always pick most CCW point (gift wrapping) | "output-sensitive" |
| Graham Scan | O(n log n) | sort by polar angle around lowest point, stack-sweep left turns | "sorted by angle" |
| **Monotone Chain** | **O(n log n)** | sort by (x,y); lower hull + upper hull; no trig — easiest to code | "Andrew's" |
| QuickHull | O(n log n) exp., O(n²) worst | recurse on points left of line to farthest point | "Quicksort of geometry" |
| D&C Hull | O(n log n) | 2T(n/2) + O(n); merge with **upper/lower common tangents** | ties to [[02-Divide-and-Conquer\|D&C]] |
| Chan's | **O(n log h)** | mini-hulls + Jarvis phases, m doubles | "optimal" |

> [!warning]- Exam traps
> - Jarvis is **O(n·h)** not O(n²) — the h matters.
> - Ties (collinear): pick the **farthest** point.
> - QuickHull & Jarvis degrade to O(n²) when all points are on a circle / convex position.

---

## 2. Divide & Conquer

> [!note]- Core facts
> - **Divide → Conquer → Combine**; recurrence $T(n) = aT(n/b) + f(n)$.
> - Solve with: substitution (guess+induction), recursion tree (sum per level), **Master Theorem**.
> - Overlapping subproblems → **DP**, not D&C.

> [!important]- Master Theorem ($c_{crit} = \log_b a$)
> | Case | Condition on $f(n)$ | Result |
> |------|--------------------|--------|
> | 1 | $f(n) = O(n^{c_{crit}-\epsilon})$ | $\Theta(n^{c_{crit}})$ — leaves win |
> | 2 | $f(n) = \Theta(n^{c_{crit}} \log^k n)$ | $\Theta(n^{c_{crit}} \log^{k+1} n)$ — tie |
> | 3 | $f(n) = \Omega(n^{c_{crit}+\epsilon})$ + regularity | $\Theta(f(n))$ — root wins |
>
> Not for unbalanced splits ($T(n) = T(n-1) + n$ → Θ(n²)) — use recursion tree.

| Algorithm | Recurrence | $c_{crit}$ | Complexity |
|-----------|-----------|-----------|------------|
| Binary Search | $T(n/2) + O(1)$ | 0 | Θ(log n) |
| Merge Sort | $2T(n/2) + Θ(n)$ | 1 | Θ(n log n) — stable, O(n) space |
| Quicksort avg | $2T(n/2) + Θ(n)$ | 1 | Θ(n log n); worst Θ(n²), O(log n) stack |
| Max Subarray (D&C) | $2T(n/2) + Θ(n)$ | 1 | Θ(n log n); Kadane = O(n) |
| Closest Pair | $2T(n/2) + Θ(n)$ | 1 | Θ(n log n); strip 2δ, ≤7 comparisons |
| D&C Hull | $2T(n/2) + Θ(n)$ | 1 | Θ(n log n) |
| Karatsuba | $3T(n/2) + Θ(n)$ | log₂3 ≈ 1.585 | Θ(n^1.585) — 3 mults |
| Strassen | $7T(n/2) + Θ(n²)$ | log₂7 ≈ 2.807 | Θ(n^2.807) — 7 mults |
| Naive MatMul | $8T(n/2) + Θ(n²)$ | 3 | Θ(n³) |

> [!tip]- Where the work happens
> Merge sort works during **combine** (merge); Quicksort during **divide** (partition). Closest pair's combine is the strip trick; Karatsuba/Strassen trade extra additions for fewer multiplications.

---

## 3. Greedy Algorithm

> [!note]- Core facts
> - Locally optimal, **irreversible** choice. Needs **greedy-choice property + optimal substructure**.
> - Proof toolkit: **exchange argument** (swap first difference, still optimal) or **staying ahead** (induction).
> - No greedy-choice property → **DP**.

| Problem | Greedy rule | Time | Proof |
|---------|-------------|------|-------|
| Activity Selection | earliest finish time | O(n log n) | exchange |
| Fractional Knapsack | best value/weight | O(n log n) | exchange; 0/1 version → DP O(n·W) |
| Huffman | merge 2 smallest freqs | O(n log n) | sibling lemma |
| Dijkstra | smallest tentative distance | O((V+E) log V) | induction on finalized set; **no negative edges** |
| Kruskal MST | lightest edge, no cycle | O(E log E) | cut property |
| Prim MST | cheapest edge across cut | O(E log V) | cut property |
| Job Sequencing | max profit → latest free slot | O(n log n + n·d) | exchange |
| Coin Change | largest coin first | O(#coins) | only for canonical systems! |

> [!danger]- Counterexamples to memorize
> - **0/1 Knapsack:** (60,10),(100,20),(120,30), W=50 → greedy ratio = 160, optimal = 220 (items 2+3).
> - **Coin change:** {1,3,4}, amount 6 → greedy 3 coins (4+1+1), optimal 2 (3+3).
> - **Shortest duration fails:** (1,5),(5,8),(4,6) → picks (4,6), gets 1; optimal = (1,5)+(5,8) = 2.
> - **Dijkstra + negative edge:** A→B=2, A→C=5, C→B=−4 → B finalized at 2 before the negative relaxation (1) arrives → wrong answer 2, truth 1.
> - **Earliest start / shortest duration / fewest conflicts** all fail for activity selection — only **earliest finish** is optimal.

---

## 4. Greedy vs DP (10-second version)

| | Greedy | DP |
|---|---|---|
| Choices kept | **one** (committed) | all (table) |
| Needs | greedy-choice property | overlapping subproblems |
| Example | Huffman, MST, Dijkstra | 0/1 knapsack, LCS, edit distance |

---

## 5. 60-Second Recap

> [!success]- Final sprint
> - **Hull:** orientation test → Jarvis O(n·h) / Graham & Monotone O(n log n) / Chan O(n log h) optimal.
> - **D&C:** Master Theorem compares $f(n)$ with $n^{\log_b a}$; Merge Sort/Closest Pair/D&C Hull all $2T(n/2)+Θ(n)$ = Θ(n log n); Karatsuba 3 mults, Strassen 7 mults.
> - **Greedy:** prove with exchange arguments; activity selection = earliest finish; knapsack fractional yes / 0/1 no; Huffman merge smallest; MST cut property; Dijkstra no negative edges.
> - **If you remember 3 numbers:** log₂3 ≈ **1.585**, log₂7 ≈ **2.807**, O(n log h) for hull.
