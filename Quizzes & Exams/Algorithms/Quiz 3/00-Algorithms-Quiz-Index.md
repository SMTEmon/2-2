---
tags:
  - algorithms
  - index
  - quiz-prep
---

# Algorithms Quiz — Master Index

> [!info]- How to use these notes
> Three syllabus topics, one note each. Every note follows the same exam-ready structure:
> **definition → key idea → algorithms → complexity table → Python code → pitfalls → practice questions**.
>
> - [[01-Convex-Hull|Convex Hull]]
> - [[02-Divide-and-Conquer|Divide & Conquer]]
> - [[03-Greedy-Algorithms|Greedy Algorithm]]
> - [[04-Cheat-Sheet|🚀 One-Page Cheat Sheet]]
> - [[05-CPP-Implementations|⚙️ C++ Implementations (compiled & verified)]]
>
> Code examples live in collapsible `[!example]` callouts — click to expand.
> Key points use `[!note]`, exam tips use `[!tip]`, and common mistakes use `[!warning]`.

## Syllabus Overview

| # | Topic | Core Idea | Must-Know Algorithms | Typical Exam Question |
|---|-------|-----------|----------------------|-----------------------|
| 1 | [[01-Convex-Hull\|Convex Hull]] | Smallest convex polygon containing all points | Jarvis March, Graham Scan, Monotone Chain, QuickHull | Trace an algorithm on given points; state complexity |
| 2 | [[02-Divide-and-Conquer\|Divide & Conquer]] | Split → solve → merge; solve recurrences | Merge Sort, Binary Search, Closest Pair, Karatsuba, Strassen | Write recurrence; solve with Master Theorem |
| 3 | [[03-Greedy-Algorithms\|Greedy Algorithm]] | Locally optimal choice at every step | Activity Selection, Fractional Knapsack, Huffman, Dijkstra, MST | Prove correctness via exchange argument; find counterexample |

## Complexity Cheat Sheet

> [!summary]- Everything at a glance
>
> | Algorithm | Time Complexity | Space | Notes |
> |-----------|-----------------|-------|-------|
> | **Convex Hull — Jarvis March** | O(n·h) | O(n) | h = number of hull vertices; O(n²) worst case |
> | **Convex Hull — Graham Scan** | O(n log n) | O(n) | sort by polar angle, then stack |
> | **Convex Hull — Monotone Chain** | O(n log n) | O(n) | sort by (x, y); simplest to implement |
> | **Convex Hull — QuickHull** | O(n log n) expected | O(n) | worst case O(n²) (all points on a circle) |
> | **Convex Hull — Divide & Conquer** | O(n log n) | O(n) | merges hulls with common tangents |
> | **Convex Hull — Chan's** | O(n log h) | O(n) | output-sensitive; advanced |
> | **Binary Search** | O(log n) | O(1) | requires sorted array |
> | **Merge Sort** | Θ(n log n) | O(n) | stable; recurrence 2T(n/2) + Θ(n) |
> | **Quicksort** | Θ(n log n) avg | O(log n) | worst Θ(n²); recurrence T(n) = T(n−1) + Θ(n) |
> | **Max Subarray (D&C)** | Θ(n log n) | O(log n) | Kadane's algorithm is O(n) — greedy/DP |
> | **Closest Pair of Points** | Θ(n log n) | O(n) | 2T(n/2) + Θ(n); strip trick |
> | **Karatsuba Multiplication** | Θ(n^1.585) | O(n) | 3T(n/2) + Θ(n) — 3 mults instead of 4 |
> | **Strassen Matrix Multiply** | Θ(n^2.807) | O(n²) | 7T(n/2) + Θ(n²) — 7 mults instead of 8 |
> | **Activity Selection** | O(n log n) | O(n) | greedy: sort by finish time |
> | **Fractional Knapsack** | O(n log n) | O(1) | greedy: sort by value/weight |
> | **Huffman Coding** | O(n log n) | O(n) | greedy: min-heap, merge two smallest |
> | **Dijkstra** | O((V + E) log V) | O(V) | greedy: smallest distance first; no negative edges |
> | **Kruskal MST** | O(E log E) | O(V) | greedy: smallest edge, union-find |
> | **Prim MST** | O(E log V) | O(V) | greedy: closest vertex to tree |
> | **Job Sequencing w/ Deadlines** | O(n log n + n·d) | O(d) | greedy: profit descending, latest free slot |

## The Big Picture

> [!important]- How the three topics fit together
> - **Divide & Conquer** is a *problem-solving paradigm*: recursion + combination. It gives you Merge Sort, Closest Pair, and even a Convex Hull algorithm.
> - **Greedy** is a *strategy for optimization problems*: pick the best-looking option now, never look back. It works only when the problem has the **greedy-choice property**.
> - **Convex Hull** is a *specific computational geometry problem* with many algorithms — and one of them (D&C hull) is literally a divide-and-conquer application, while Jarvis March is the "greedy-like" one (always pick the next extreme point).
>
> Quiz tip: examiners love asking **"which technique fits this problem and why?"** — be ready to name the property that justifies each choice (optimal substructure, greedy-choice property, or geometry).

## Suggested Study Order

1. **[[02-Divide-and-Conquer|Divide & Conquer]]** first — recurrences and the Master Theorem are the mathematical backbone of all three topics.
2. **[[03-Greedy-Algorithms|Greedy Algorithm]]** second — shortest proofs, most counterexample questions.
3. **[[01-Convex-Hull|Convex Hull]]** last — builds on D&C (hull merge) and on sorting (Graham, Monotone Chain).
