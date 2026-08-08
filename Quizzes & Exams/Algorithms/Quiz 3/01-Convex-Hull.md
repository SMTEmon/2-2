---
tags:
  - algorithms
  - computational-geometry
  - convex-hull
  - quiz-prep
---

# Convex Hull

> [!note]- Definition (two equivalent ways)
> The **convex hull** of a set of points $S$ is:
>
> 1. **Intersection view:** the smallest convex set containing all points of $S$.
> 2. **Combination view:** the set of all *convex combinations* $\sum \lambda_i p_i$ where $\sum \lambda_i = 1$ and $\lambda_i \ge 0$ for points $p_i \in S$.
>
> **Rubber-band intuition:** place a rubber band around all the points and let it snap tight — the shape it forms is the convex hull.

> [!tip]- Why it matters (applications)
> - Collision detection in games & robotics (hull = simplified bounding shape)
> - Pattern recognition / image processing (boundary of a shape)
> - Geographic information systems (minimum bounding region)
> - Preprocessing step: many geometric algorithms run faster on the hull
> - Gift-wrapping ideas appear in motion planning and path finding

---

## 1. Key Primitive — The Orientation Test

Every convex hull algorithm (and most of computational geometry) is built on **one** operation: the **cross product** of two vectors, which tells you the *turn direction* of three points.

For points $O = (x_1, y_1)$, $A = (x_2, y_2)$, $B = (x_3, y_3)$:

$$
\text{cross}(O, A, B) = (x_2 - x_1)(y_3 - y_1) - (y_2 - y_1)(x_3 - x_1)
$$

| Sign | Meaning |
|------|---------|
| cross > 0 | Counter-clockwise turn (A → B goes **left**) |
| cross < 0 | Clockwise turn (A → B goes **right**) |
| cross = 0 | Collinear (A and B on the same line through O) |

> [!example]- Python: orientation test
> ```python
> def cross(o, a, b):
>     """Cross product of vectors o->a and o->b.
>     Returns >0 for a LEFT turn, <0 for a RIGHT turn, 0 if collinear."""
>     return (a[0] - o[0]) * (b[1] - o[1]) - (a[1] - o[1]) * (b[0] - o[0])
> 
> # Example: points as (x, y) tuples
> o, a, b = (0, 0), (1, 0), (0, 1)
> print(cross(o, a, b))   # +1  -> left turn (CCW)
> print(cross(o, b, a))   # -1  -> right turn (CW)
> print(cross(o, (1, 0), (2, 0)))  # 0  -> collinear
> ```

---

## 2. The Problem

> [!info]- Formal problem statement
> **Input:** a set of $n$ points $P = \{p_1, p_2, \dots, p_n\}$ in the plane, $S = \{(x_i, y_i) \mid i = 1 \dots n\}$.
> **Output:** the vertices of the convex hull of $P$, listed **in counter-clockwise order** (this lecture used *clockwise* — both conventions appear; state yours).
>
> The hull is a **convex polygon** whose vertices are a subset of the input points, and every input point lies inside or on the polygon.
>
> **General-position assumptions** (standard in theory, incl. this lecture): no two points share an x-coordinate, no two share a y-coordinate, and no three are collinear. These simplify tangent-finding (no vertical edges, no ties); real implementations relax them (see [[01-Convex-Hull#5. Edge Cases & Degeneracies|§5]]).

---

## 3. Algorithms

### 3.1 Naive Approach — O(n³)

Check every **ordered pair** of points $(p_i, p_j)$: the directed edge $p_i \to p_j$ is a hull edge **iff all other points lie on the same side** of the line through $p_i, p_j$.

> [!example]- Python: naive O(n³) hull
> ```python
> def naive_hull(points):
>     points = list(set(points))
>     n = len(points)
>     if n < 3:
>         return points
>     hull_edges = set()
>     for i in range(n):
>         for j in range(n):
>             if i == j:
>                 continue
>             # all other points must be on the same side of line i->j
>             sides = set()
>             for k in range(n):
>                 if k == i or k == j:
>                     continue
>                 c = cross(points[i], points[j], points[k])
>                 sides.add(1 if c > 0 else (-1 if c < 0 else 0))
>             if 0 not in sides and len(sides) == 1:
>                 hull_edges.add((i, j))
>     # reconstruct ordered hull from edges (omitted for brevity)
>     return hull_edges  # set of directed edges
> ```
>
> Complexity: **O(n³)** time, O(n) space. Never used in practice — but a classic exam answer for "naive method".

### 3.2 Jarvis March / Gift Wrapping — O(n·h)

Start at the **leftmost** point (guaranteed on the hull). Repeatedly pick the point that makes the **smallest counter-clockwise angle** with the current hull edge — i.e. the most "left-turning" point. Walk around the hull until you return to the start.

- $h$ = number of vertices on the hull
- Time: **O(n·h)** — worst case O(n²) when all points are on the hull
- Very intuitive, easy to trace by hand in an exam

> [!example]- Python: Jarvis March (Gift Wrapping)
> ```python
> def dist2(a, b):
>     return (a[0] - b[0]) ** 2 + (a[1] - b[1]) ** 2
> 
> def jarvis_march(points):
>     points = list(set(points))          # remove duplicates
>     n = len(points)
>     if n < 3:
>         return points
> 
>     hull = []
>     leftmost = min(points)              # smallest x (then y) — always on hull
>     current = leftmost
> 
>     while True:
>         hull.append(current)
>         nxt = points[0]
>         for p in points:
>             c = cross(current, nxt, p)
>             # pick the most counter-clockwise point; tie -> farthest
>             if c > 0 or (c == 0 and dist2(current, p) > dist2(current, nxt)):
>                 nxt = p
>         current = nxt
>         if current == leftmost:         # wrapped around
>             break
>     return hull
> 
> # Test: square + center point
> pts = [(0, 0), (1, 0), (1, 1), (0, 1), (0.5, 0.5)]
> print(jarvis_march(pts))   # [(0,0), (1,0), (1,1), (0,1)]  (CCW order)
> ```

> [!warning]- Exam traps for Jarvis March
> - Complexity is written **O(n·h)**, *not* O(n²) — the $h$ matters (output-sensitive).
> - Ties (collinear points): pick the **farthest** one, or you'll get duplicate/extra hull vertices.
> - The hull is returned in **counter-clockwise** order — state this in your answer.

### 3.3 Graham Scan — O(n log n)

1. Pick the lowest point $p_0$ (by y, then x).
2. Sort all other points by **polar angle** around $p_0$.
3. Push points onto a stack; while the top three points make a **non-left turn**, pop the middle one.

> [!example]- Python: Graham Scan
> ```python
> import math
> 
> def graham_scan(points):
>     points = sorted(set(points))        # sort by (x, y); p0 = lowest-left
>     if len(points) < 3:
>         return points
> 
>     p0 = points[0]
>     def angle_key(p):                   # polar angle of p around p0
>         return math.atan2(p[1] - p0[1], p[0] - p0[0])
> 
>     pts = sorted(points[1:], key=angle_key)
>     stack = [p0, pts[0]]
> 
>     for p in pts[1:]:
>         while len(stack) >= 2 and cross(stack[-2], stack[-1], p) <= 0:
>             stack.pop()                 # non-left turn -> not a hull vertex
>         stack.append(p)
>     return stack
> 
> pts = [(0, 0), (1, 0), (1, 1), (0, 1), (0.5, 0.5)]
> print(graham_scan(pts))    # [(0,0), (1,0), (1,1), (0,1)]
> ```

> [!note]- Why Graham Scan works
> After sorting by angle, the points are in "winding" order around $p_0$. The stack maintains the invariant that consecutive triples always make a **left turn**; any right turn means the middle point is *inside* the growing hull and must be discarded. Each point is pushed and popped **at most once** → O(n) after sorting → **O(n log n)** total.

### 3.4 Monotone Chain (Andrew's Algorithm) — O(n log n)

Sort points by (x, y), build the **lower hull** left→right and the **upper hull** right→left, then concatenate. Avoids trigonometry entirely — only cross products.

> [!example]- Python: Monotone Chain
> ```python
> def monotone_chain(points):
>     points = sorted(set(points))        # lexicographic sort by (x, y)
>     if len(points) < 3:
>         return points
> 
>     def build(seq):                     # standard stack sweep
>         hull = []
>         for p in seq:
>             while len(hull) >= 2 and cross(hull[-2], hull[-1], p) <= 0:
>                 hull.pop()
>             hull.append(p)
>         return hull
> 
>     lower = build(points)               # left -> right
>     upper = build(reversed(points))     # right -> left
>     return lower[:-1] + upper[:-1]      # drop duplicated endpoints
> 
> pts = [(0, 0), (1, 0), (1, 1), (0, 1), (0.5, 0.5)]
> print(monotone_chain(pts))   # [(0,0), (1,0), (1,1), (0,1)]  CCW
> ```

> [!tip]- Monotone Chain vs Graham Scan — which to use?
> **Monotone Chain** is usually preferred: no angle sorting (faster, no floating-point `atan2`), handles collinear points cleanly, and is simpler to code from memory in an exam. Both are **O(n log n)**.

### 3.5 QuickHull — O(n log n) expected

Divide & conquer in the spirit of Quicksort: find the point **farthest from** the line joining two hull points; it is guaranteed to be on the hull and splits the remaining points into two sub-problems.

> [!example]- Python: QuickHull
> ```python
> def quickhull(points):
>     points = list(set(points))
>     if len(points) < 3:
>         return points
> 
>     def build(points, p1, p2):
>         """Hull of points strictly left of directed edge p1 -> p2."""
>         if not points:
>             return []
>         far = max(points, key=lambda p: abs(cross(p1, p2, p)))
>         left1 = [p for p in points if cross(p1, far, p) > 0]
>         left2 = [p for p in points if cross(far, p2, p) > 0]
>         return build(left1, p1, far) + [far] + build(left2, far, p2)
> 
>     leftmost, rightmost = min(points), max(points)
>     above = [p for p in points if cross(leftmost, rightmost, p) > 0]
>     below = [p for p in points if cross(leftmost, rightmost, p) < 0]
> 
>     return ([leftmost]
>             + build(above, leftmost, rightmost)
>             + [rightmost]
>             + build(below, rightmost, leftmost))
> 
> pts = [(0, 0), (1, 0), (1, 1), (0, 1), (0.5, 0.5)]
> print(quickhull(pts))      # [(0,0), (1,0), (1,1), (0,1)]
> ```

> [!warning]- QuickHull worst case
> If all points lie on a circle (or are evenly spread so the "far" point rarely splits), QuickHull degenerates to **O(n²)**. Expected case is O(n log n). Same caveat as Quicksort with bad pivots.

### 3.6 Divide & Conquer Hull — O(n log n)

See [[02-Divide-and-Conquer]] — this is a flagship application of the paradigm (this is exactly what the D&C-3 lecture covers).

1. **Sort:** sort all points by x-coordinate — O(n log n) (Merge Sort) or even **O(n) with Radix Sort** on integer coordinates.
2. **Divide:** split the sorted set into a left half $A$ and right half $B$ (by x).
3. **Conquer:** recursively compute $\text{CH}(A)$ and $\text{CH}(B)$. **Base case:** for a *sufficiently small* set, solve directly with brute force (O(n³) is fine for constant n).
4. **Combine:** merge the two hulls — O(n) — via their **upper and lower common tangents** (bridges).

Recurrence: $T(n) = 2T(n/2) + O(n) \Rightarrow T(n) = O(n \log n)$

#### Finding a tangent — the Two-Finger Algorithm

> [!example]- How the upper tangent is actually found (lecture method)
> The tangent line touches $\text{CH}(A)$ at point $a_i$ and $\text{CH}(B)$ at point $b_j$ and leaves both hulls on the same side. The right tangent maximizes the line's y-intercept with the vertical split line — *picking just the max-y point of each hull is not enough*.
>
> 1. Start with the segment joining **rightmost point of $A$** ($a$) and **leftmost point of $B$** ($b$) — the "two fingers".
> 2. **Move $b$ clockwise** along $\text{CH}(B)$; update if it *improves* $y(i, j)$.
> 3. **Move $a$ counter-clockwise** along $\text{CH}(A)$; update if it *improves* $y(i, j)$.
> 4. Repeat both walks until $y(i, j)$ **converges** — each finger visits each vertex at most once, so the whole walk is O(|A| + |B|) = O(n).
>
> The **lower tangent** is found the same way (mirror: move $b$ counter-clockwise, $a$ clockwise, minimizing instead of maximizing).

#### Merging the hulls — Cut and Paste

> [!example]- Once both tangents are found (lecture method)
> With upper tangent $U$ and lower tangent $L$ known, the merged hull is a single clockwise walk:
>
> 1. Start at the left endpoint of the **upper tangent**.
> 2. Walk clockwise along $\text{CH}(A)$ to the upper tangent's right endpoint (the "top" of the merged hull).
> 3. Cross the upper tangent, keep walking clockwise along $\text{CH}(B)$ **until you reach the lower tangent's right endpoint**.
> 4. Cross the lower tangent back to $\text{CH}(A)$.
> 5. Keep walking clockwise along $\text{CH}(A)$ until back at the start — the interior arcs of both hulls (between the tangents) are *cut out*, hence the name.
>
> Result: every vertex of both hulls is visited once → O(n) merge → $T(n) = 2T(n/2) + O(n) = O(n \log n)$.

> [!note]- The sort step is included in the recurrence
> Sorting once up front costs O(n log n) (or O(n) with radix sort), which the Master Theorem-style analysis absorbs: total stays **O(n log n)**.

### 3.7 Chan's Algorithm — O(n log h) *(advanced / bonus)*

> [!note]- Output-sensitive hull
> Chan's algorithm combines Jarvis March with "mini-hulls" built by Graham Scan on blocks of size $m$, run in $\lceil \log h \rceil$ phases with $m$ doubling each time. Total: **O(n log h)** — optimal for small hulls, and it proves the **lower bound** of hull construction is $\Omega(n \log h)$.
>
> **Lower bound intuition:** (1) reading the input costs Ω(n); (2) if the $h$ hull points are in convex position, the output must list them in **cyclic order** — and sorting $h$ arbitrary numbers reduces to hull construction (map $x_i \mapsto (x_i, x_i^2)$ onto a parabola; the hull order *is* the sorted order), so hull construction is at least as hard as sorting → Ω(h log h). Together: **Ω(n log h)** — exactly what Chan's achieves. Most courses mention Chan's only as an extension — knowing the bound is enough.

---

## 4. Complexity Comparison

> [!summary]- All algorithms at a glance
>
> | Algorithm | Time | Space | Sort-based? | Output-sensitive? |
> |-----------|------|-------|-------------|-------------------|
> | Naive (pair checking) | O(n³) | O(n) | No | No |
> | Jarvis March / Gift Wrapping | **O(n·h)** | O(n) | No | **Yes** |
> | Graham Scan | O(n log n) | O(n) | Yes (polar angle) | No |
> | Monotone Chain (Andrew) | O(n log n) | O(n) | Yes (x, y) | No |
> | QuickHull | O(n log n) exp. | O(n) | No | Yes |
> | Divide & Conquer | O(n log n) | O(n) | Yes (x split) | No |
> | Chan's | **O(n log h)** | O(n) | Yes | **Yes** |
>
> $n$ = number of input points, $h$ = number of hull vertices.
> **Optimal** (comparison-based) is $\Theta(n \log h)$ — achieved by Chan's algorithm.

> [!important]- Choosing an algorithm for an exam
> - Small class / few points, want simple code → **Jarvis March**
> - $n$ large, need guaranteed O(n log n), code by hand → **Monotone Chain**
> - Question mentions "points sorted by angle" → **Graham Scan**
> - Question says "output-sensitive" or "O(n log h)" → **Chan's**
> - Question ties into divide & conquer → **D&C hull with tangents**

---

## 5. Edge Cases & Degeneracies

> [!warning]- Things that break naive implementations
> - **Fewer than 3 distinct points** → hull is the points themselves (handle explicitly).
> - **Duplicate points** → deduplicate first, or hull edges repeat infinitely.
> - **Collinear points on a hull edge** → decide policy: include only the extreme endpoints (standard) or all of them. State your policy in the answer.
> - **All points collinear** → hull degenerates to the segment between the two extremes.
> - **Points on a circle / regular polygon** → every point is a hull vertex; QuickHull and Jarvis degrade to worst case.
> - **Integer vs float coordinates** → cross product is exact on integers; use epsilon comparisons on floats.

---

## 6. Practice Questions

> [!question]- Quick self-check
> 1. Trace **Graham Scan** on the points (0,0), (2,1), (1,2), (3,3), (0,3), (2,0). Which points are popped from the stack and why?
> 2. Why is Jarvis March's worst case O(n²)? Give a point set that achieves it.
> 3. Prove (in one sentence) that the leftmost point is always on the hull.
> 4. Which hull algorithm would you pick for 10⁶ points where you expect only ~20 hull vertices? Justify.
> 5. The D&C hull merges two hulls in O(n). What geometric structure does it search for, and why is it called a "bridge"?
>
> <details><summary>Hints</summary>
>
> 1. Sort by angle around (0,0); points like (2,1) get popped when (1,2) or (3,3) arrives and the triple turns right.
> 2. Put all $n$ points on a convex polygon — then $h = n$ and O(n·h) = O(n²).
> 3. Any convex set containing all points must contain the leftmost point, and no point lies to its left, so it's an extreme point.
> 4. **Chan's (O(n log h))** or Jarvis March (O(n·h) ≈ 2·10⁷); Graham/Monotone would be O(n log n) ≈ 2·10⁷ too — compare constants. Chan's is asymptotically best for tiny $h$.
> 5. The upper/lower **common tangent** — a line touching both hulls that leaves both entirely on one side; the merge "bridges" the gap between the hulls.
> </details>

---

## 7. One-Page Summary

> [!success]- The whole topic in 5 bullets
> - Hull = smallest convex polygon containing all points; computed with one primitive: the **cross-product orientation test**.
> - **Jarvis March (O(n·h))**: walk the hull picking the most CCW point — gift wrapping.
> - **Graham Scan / Monotone Chain (O(n log n))**: sort, then stack-sweep keeping only left turns.
> - **QuickHull (O(n log n) expected)**: recursive farthest-point split — Quicksort's geometric cousin.
> - **Optimal is O(n log h)** (Chan's); D&C hull merges with tangents and is the bridge to [[02-Divide-and-Conquer|Divide & Conquer]].
