# Greedy Algorithms – 20 Exercises With Full Solutions

 
## Table of Contents

1. Activity Selection (Maximum Non‑Overlapping Intervals)
2. Minimum Rooms / Platforms (Sweep Line)
3. Coin Change — Canonical Systems (+ counterexample)
4. Connecting Ropes / Optimal Merge Pattern
5. Fractional Knapsack
6. Pairing to Minimize Sum of Pair Maxima
7. Minimize Maximum Lateness (EDD rule)
8. Minimize Weighted Sum of Completion Times (Smith’s rule)
9. Unit‑Length Intervals — what Greedy can and can’t do
10. Job Sequencing with Deadlines (Unit‑time jobs)
11. Gas Station / Circular Tour
12. Earliest‑Finish Exchange Argument (and a two‑room note)
13. Minimum Rooms via Priority Queue
14. Huffman Coding
15. Minimum Spanning Tree (Kruskal / Prim)
16. Egyptian Fractions (Greedy)
17. Antenna Placement (Maximize Min Distance via Greedy Feasibility)
18. Scheduling With Linear Tardiness Penalties — limits of Greedy
19. Interval Cover With Capacities — a Greedy that works
20. When Greedy Fails (0/1‑Knapsack; Two‑machine makespan)

---

## 1) Activity Selection (Maximum Non‑Overlapping Intervals)

**Problem.** Intervals (s\_i, f\_i). Select the largest set of pairwise non‑overlapping intervals.

**Greedy algorithm.** Sort by non‑decreasing finish time. Repeatedly pick the first interval that starts after the last chosen finishes.

**Proof (exchange).** Let OPT be an optimal solution. Compare the first finishing interval `A` in the greedy solution to OPT’s first interval `B`. Since `finish(A) <= finish(B)`, replacing `B` by `A` keeps feasibility and size. Repeat to transform OPT into the greedy solution without decreasing size → greedy is optimal.

**Pseudocode.**

```text
sort intervals by finish
S = []
last_end = -inf
for (s,f) in intervals:
  if s >= last_end:
    S.append((s,f))
    last_end = f
return S
```

**Complexity.** Sorting dominates: O(n log n). The scan is O(n).

---

## 2) Minimum Rooms / Platforms (Sweep Line)

**Problem.** Given intervals (start,end), compute the minimum number of rooms (maximum overlap at any time).

**Greedy / sweep.** Create two arrays: all starts and all ends. Sort both. Scan with two pointers: when a start comes before an end, need a room (`++cur`); when an end comes first, free a room (`--cur`). Track `max(cur)`.

**Correctness.** The scan simulates the timeline; `cur` equals the number of active intervals; its maximum is exactly the minimum number of rooms by pigeonhole principle.

**Pseudocode.**

```text
starts = sorted(s_i)
ends   = sorted(f_i)
i=j=0; cur=max_rooms=0
while i < n:
  if starts[i] < ends[j]:
    cur += 1; max_rooms = max(max_rooms, cur); i += 1
  else:
    cur -= 1; j += 1
return max_rooms
```

**Complexity.** Sorting O(n log n); scan O(n).

---

## 3) Coin Change — Canonical Systems (+ counterexample)

**Problem.** Given coin denominations and amount V, minimize number of coins.

**Greedy (canonical system example).** For {1,2,5,10}, the greedy that repeatedly takes the largest coin <= remaining is optimal.

**Why it’s correct for {1,2,5,10}.** In any solution, replacing five 2’s by one 10, two 5’s by one 10, or two 2’s by one 4 (not present) type arguments do not increase coin count; structured exchanges yield that any optimal solution can be transformed into greedy’s choice countwise. (More generally, standard proof uses exchanges by decimal structure.)

**Counterexample system.** Denoms {1,3,4}, V=6. Greedy takes 4+1+1 (3 coins), but optimal is 3+3 (2 coins). So greedy is not universally correct.

**Pseudocode (greedy).**

```text
sort denoms descending
count=0; rem=V
for c in denoms:
  k = rem // c
  count += k
  rem -= k*c
return count   # optimal only in canonical systems
```

**Complexity.** O(m log m) for sorting m denominations; O(m) for the pass.

---

## 4) Connecting Ropes / Optimal Merge Pattern

**Problem.** Given rope lengths L\_i, merging two ropes of lengths x,y costs x+y; the merged rope re‑enters the pool. Minimize total cost to end with one rope.

**Greedy.** Always merge the two shortest ropes first. Implement with a min‑heap.

**Proof.** (Optimal merge/Huffman lemma.) Take an optimal solution; among its first merge, pick two ropes a<=b that are minimal; if not, swapping their first merge with any larger rope(s) does not increase later costs because a and b appear in all subsequent sums anyway. Repeating transforms OPT into the greedy sequence.

**Pseudocode.**

```text
heap = MinHeap(L)
cost = 0
while heap.size() > 1:
  x = heap.pop_min()
  y = heap.pop_min()
  cost += x+y
  heap.push(x+y)
return cost
```

**Complexity.** Each of n-1 merges does 2 pops + 1 push: O(n log n).

---

## 5) Fractional Knapsack

**Problem.** Items i with value v\_i and weight w\_i, capacity W. Fractions allowed. Maximize value.

**Greedy.** Sort by density v\_i / w\_i descending; take as much as possible of each until capacity fills.

**Proof (exchange/convexity).** If two items i,j have densities d\_i >= d\_j but we take a positive fraction of j before saturating i, swapping a small epsilon weight from j to i increases value by (d\_i - d\_j) \* epsilon >= 0. Hence any optimal solution is sorted by density; the unique fractional item is the first that overflows.

**Pseudocode.**

```text
sort items by density = v/w descending
val=0; cap=W
for item in items:
  take = min(cap, item.w)
  val += take * (item.v/item.w)
  cap -= take
  if cap == 0: break
return val
```

**Complexity.** O(n log n) for sorting; O(n) scan.

---

## 6) Pairing to Minimize Sum of Pair Maxima

**Problem.** Given 2n numbers, pair them to minimize sum over pairs of their maxima.

**Greedy.** Sort ascending and pair neighbors: (q1,q2),(q3,q4),...; pay `sum q_{2k}`.

**Proof.** Since sum of all numbers S is fixed and sum max = S - sum min, minimizing sum max is maximizing sum of pairwise minima. By the rearrangement inequality / exchange on quadruples (a<=b<=c<=d), `(a,b)+(c,d)` has min sum `a+c` >= `a+b`? More directly: for any crossing pairing `(a,d),(b,c)`, replacing by `(a,b),(c,d)` does not decrease the sum of minima; applying repeatedly yields the neighbor pairing, which thus maximizes sum of minima and minimizes sum of maxima.

**Complexity.** O(n log n) for sorting; O(n) pairing.

---

## 7) Minimize Maximum Lateness (EDD rule)

**Problem.** Jobs with processing times p\_i and deadlines d\_i on one machine. Lateness of job i is L\_i = C\_i - d\_i. Minimize L\_max = max L\_i.

**Greedy.** Order jobs by non‑decreasing deadlines (EDD: earliest due date).

**Proof (inversion/exchange).** If two adjacent jobs A,B with d\_A > d\_B are in that order, swapping them cannot increase any completion time beyond the larger of the two deadlines; the maximum lateness does not increase. Repeatedly removing all inversions yields EDD without worsening L\_max; thus EDD is optimal.

**Pseudocode.**

```text
sort jobs by deadline ascending
C=0; Lmax= -inf
for j in jobs:
  C += j.p
  Lmax = max(Lmax, C - j.d)
return order, Lmax
```

**Complexity.** O(n log n).

---

## 8) Minimize Weighted Sum of Completion Times (Smith’s rule)

**Problem.** Jobs with processing p\_i and weight w\_i. Minimize sum w\_i \* C\_i on one machine.

**Greedy (Smith’s rule).** Order by non‑increasing key w\_i / p\_i (equivalently, sort by p\_i / w\_i ascending).

**Proof (pairwise interchange).** For two jobs i,j, compare orders \[i,j] vs \[j,i]. The contribution difference is `w_i*(p_i+p_j) + w_j*p_j  -  (w_j*(p_i+p_j) + w_i*p_i) = p_i*w_j - p_j*w_i`. Prefer i before j iff `w_i/p_i >= w_j/p_j`. Applying the pairwise criterion transitive‐closure gives the Smith order, which is optimal.

**Pseudocode.**

```text
sort jobs by key = w/p descending
C=0; total=0
for j in jobs:
  C += j.p
  total += j.w * C
return order, total
```

**Complexity.** O(n log n).

---

## 9) Unit‑Length Intervals — what Greedy can and can’t do

**Problem.** Intervals all have length 1.

**A. Unweighted case (maximize count).** Greedy by earliest finish is optimal (this is the classic activity selection).

**B. Weighted case (maximize total weight).** *In continuous time*, no simple greedy is optimal. Counterexample: intervals [0,1](w=1), [0.5,1.5](100), [1.5,2.5](100). Earliest‑finish picks \[0,1], loses the 100+100 option.

**Special discrete case where a greedy works.** If all starts are integers and intervals are \[t, t+1] exactly, conflicts occur only among intervals with the same t. Then the optimal is: for each integer t, take the max‑weight interval starting at t. (No cross‑slot overlap.)

**Complexity.** Sorting O(n log n) or O(n) bucket by integer t.

---

## 10) Job Sequencing with Deadlines (Unit‑time jobs)

**Problem.** Jobs j take 1 unit, have profit w\_j and deadline d\_j (integer). At most one job per time slot; job must finish by its deadline. Maximize total profit.

**Greedy.** Sort jobs by profit descending. For each job, schedule it in the latest available slot at or before its deadline. Use DSU (union‑find) or a balanced structure to find the latest free slot.

**Proof (exchange).** If a more profitable job is left unscheduled while a less profitable one occupies a slot, swapping increases profit without violating deadlines. Placing each chosen job as late as possible leaves earlier slots available for others. Standard exchange shows optimality.

**Pseudocode (DSU).**

```text
make_set(0..T)   # T = max deadline; parent[t] = greatest free slot <= t
sort jobs by w desc
schedule = []
for job in jobs:
  t = find(d_j)      # latest free slot <= deadline
  if t > 0:
    schedule[t] = job
    union(t, t-1)    # mark t filled; next free is find(t-1)
return schedule
```

**Complexity.** Sorting O(n log n); DSU with path compression gives near‑O(1) per job → O(n log n + T α(T)).

---

## 11) Gas Station / Circular Tour

**Problem.** Stations 0..n-1 around a circle; at i you gain gas\[i] and need cost\[i] to go to i+1. Find a start index to complete the loop (or none).

**Greedy.** Linear scan with two invariants:

1. If the running tank ever drops below 0 at station j, no station in the segment since the last reset can be a valid start. Reset start to j+1, tank=0.
2. If total gas >= total cost, some start exists; otherwise none.

**Pseudocode.**

```text
start=0; tank=0; total=0
for i in 0..n-1:
  diff = gas[i] - cost[i]
  tank += diff; total += diff
  if tank < 0:
    start = i+1
    tank = 0
if total < 0: return "no solution"
return start
```

**Complexity.** O(n). Proof follows from invariant (1) and total balance.

---

## 12) Earliest‑Finish Exchange Argument (and a two‑room note)

**Problem.** Show how the earliest‑finish rule emerges from a local exchange.

**Claim.** Among all feasible schedules of non‑overlapping intervals, there exists an optimal one whose first interval has the earliest finish time. (This is the key greedy‑choice property used in #1.)

**Proof.** Let OPT’s first interval be J. Let G be the earliest‑finishing interval among all. Replace J by G; feasibility is preserved because finish(G) <= finish(J). The remaining part of OPT can only be helped by finishing earlier. Repeat to obtain the greedy solution.

**Two‑room remark.** If you have exactly two parallel rooms and wish to maximize the number of talks scheduled, a simple “run earliest‑finish twice” heuristic is *not* always optimal; the problem reduces to maximum independent set on interval graphs with capacity 2, solvable in O(n log n) by DP. Greedy still plays a role inside the DP via earliest‑finish indexing.

---

## 13) Minimum Rooms via Priority Queue

**Problem.** Same as #2 but with an explicit assignment.

**Greedy / heap.** Sort intervals by start time. Maintain a min‑heap of current room end times. When a meeting starts, if the earliest‑ending room ends <= its start, reuse it (pop+push new end); otherwise open a new room (push). The heap size after processing all intervals equals the minimum number of rooms; the pops give an actual assignment if you track room IDs.

**Pseudocode.**

```text
sort by start
heap = MinHeap()  # stores end times
for (s,f) in intervals:
  if heap and heap.min() <= s:
    heap.pop_min()
  heap.push(f)
return heap.size()
```

**Complexity.** O(n log n).

---

## 14) Huffman Coding

**Problem.** Given symbol frequencies f\_i, build a prefix‑free binary code minimizing expected code length sum f\_i \* depth\_i.

**Greedy.** Repeatedly merge the two least‑frequent trees; make them siblings under a new parent whose weight is the sum. (Min‑heap.)

**Proof sketch.** In some optimal tree, the two least frequent symbols are siblings at maximum depth (sibling property). Merging them reduces the instance and preserves optimality by induction. This is the same exchange used in the optimal‑merge pattern.

**Pseudocode.** As in #4, but nodes are trees; record left/right to output codes.

**Complexity.** O(n log n).

---

## 15) Minimum Spanning Tree (Kruskal / Prim)

**Problem.** Given connected, weighted, undirected graph, find an MST.

**Greedy properties.**

* **Cut property:** For any cut, the minimum‑weight edge crossing it is safe to add to *some* MST.
* **Cycle property:** For any cycle, the maximum‑weight edge on it is not in *some* MST.

**Kruskal.** Sort edges by weight; add an edge if it connects two different components (DSU). Correct by the cut property.

**Prim.** Grow a tree from any root, always adding the lightest edge leaving the current tree (min‑key PQ). Correct by the cut property applied to the cut (tree, rest).

**Complexities.** Kruskal: O(m log m) + near‑linear DSU. Prim: O(m log n) with binary heap; O(m + n log n) with Fibonacci heap.

---

## 16) Egyptian Fractions (Greedy)

**Problem.** For a rational 0 < a/b < 1, represent it as a sum of distinct unit fractions: a/b = sum 1/x\_k.

**Greedy.** At each step take `x = ceil(b/a)` and subtract 1/x: set a/b := a/b - 1/x; repeat until numerator 0.

**Proof idea.** `1/x <= a/b` by definition of ceiling; the remainder (a/b - 1/x) is non‑negative and smaller with strictly increasing denominators, guaranteeing termination. The representation uses distinct unit fractions because denominators strictly grow.

**Pseudocode.**

```text
frac = (a,b)
terms = []
while a != 0:
  x = ceil(b/a)
  terms.append(x)
  a, b = a*x - b, b*x     # reduce fraction by gcd if desired
return terms   # represents sum 1/x
```

**Complexity.** Number of terms is O(log b) in many cases; arithmetic per step is polylog; overall polynomial in input size.

---

## 17) Antenna Placement (Maximize Minimum Distance)

**Problem.** Given sorted candidate positions x\_1 < ... < x\_n on a line, place k antennas to maximize the minimum pairwise distance D.

**Greedy feasibility + parametric search.** For a fixed D, greedy test: place first antenna at x\_1, then always place the next at the leftmost position >= last + D. This maximizes the count placed under distance D. Then binary‑search D over the coordinate range.

**Correctness.** The feasibility greedy is optimal: if we ever skip an available earliest position, we can shift left without hurting feasibility, thus never placing *more* antennas than greedy. Hence greedy achieves the maximum possible count for that D. Binary search finds the greatest feasible D.

**Pseudocode.**

```text
can_place(D):
  count=1; last=x[1]
  for i=2..n:
    if x[i] - last >= D:
      count += 1; last = x[i]
  return count >= k

lo=0; hi=x[n]-x[1]
while lo < hi:
  mid = ceil((lo+hi+1)/2)  # upper mid
  if can_place(mid):
    lo = mid
  else:
    hi = mid-1
return lo
```

**Complexity.** Each feasibility scan O(n); binary search O(log range) steps → O(n log range).

---

## 18) Scheduling With Linear Tardiness Penalties — limits of Greedy

**Problem.** Minimize sum w\_i \* max(0, C\_i - d\_i) on one machine.

**Reality check.** This problem (1||sum w\_i T\_i) is NP‑hard in general; no simple greedy is optimal for all inputs.

**Counterexamples to natural greedies.**

* EDD (by deadlines) can be suboptimal when a very heavy job slightly late outweighs many light on‑time jobs.
* Smith’s rule (by w/p) is for sum w\_i C\_i, not tardiness; it can schedule late heavy jobs early and increase tardiness sum.

**What helps.** Exact algorithms use DP or branch‑and‑bound for small n; approximation/heuristics exist (e.g., local search around EDD, or dyn‑prog on small total processing time). Greedy remains a strong heuristic but without optimality guarantee.

---

## 19) Interval Cover With Capacities — a Greedy that works

**Problem.** Each interval \[s\_i, f\_i) is a guard shift; choosing one guard contributes capacity 1 over its interval. Demand function D(t) specifies how many guards are required at every time t (integer time points). Choose the smallest number of guards (intervals) so that for every integer time t, the number of chosen intervals covering t is at least D(t).

**Greedy.** Sweep t from left to right. Maintain a max‑heap of chosen intervals’ end times that cover t. If current coverage < D(t), add more guards among intervals that start at or before t by choosing those with **latest end times** first (so they help cover future times the most). Also drop from the heap any chosen intervals whose end < t (they no longer cover t).

**Proof (staying‑ahead / exchange).** Suppose at time t we must add r new guards. Picking the r intervals with latest ends gives coverage at least as good as any other r choices at t and never reduces feasibility later; if some solution chose a shorter‑ending interval at t, swapping it with a longer‑ending interval we had available cannot hurt current coverage and only helps later times. Repeating locally yields a globally optimal solution.

**Pseudocode.**

```text
# inputs: intervals sorted by start; discrete times T; demand D[t]
active = MaxHeapByEnd()   # end times of chosen intervals covering t
avail  = MaxHeapByEnd()   # intervals with s_i <= t < f_i not yet chosen
chosen = []
i = 0
for t in T:
  while i < n and intervals[i].s <= t:   # new candidates become available
    if intervals[i].f > t:
      avail.push(intervals[i])
    i += 1
  while active.peek_end() <= t:
    active.pop()
  need = D[t] - active.size()
  while need > 0:
    x = avail.pop_max_end()
    chosen.append(x)
    active.push(x)
    need -= 1
return chosen
```

**Complexity.** Sorting O(n log n) plus O((n + |T|) log n) heap ops.

**Remark.** This is the capacitated analogue of choosing “as long as possible” intervals for point coverage; for D(t)=1 it reduces to the classic greedy for covering points with intervals.

---

## 20) When Greedy Fails

**(a) 0/1‑Knapsack — ratio greedy is not optimal.** Items: (v,w) = (100,51), (100,51), (99,50). Capacity 100. Ratio greedy takes the two 100/51 items (total w=102 > 100) so it picks one 100/51 and then 99/50 cannot fit; value=100. Optimal is the two lighter items (100/51 + 99/50 = 149 value, exactly 101? Adjust weights: use (100,50),(100,50),(99,51) with capacity 100 → ratio greedy picks 100+? A simpler standard: items (v,w): (60,10),(100,20),(120,30), capacity 50: ratio greedy picks all by ratio equal? Better counterexample:
Items: (v,w)=(value,weight) = (100,60),(90,50),(60,30), W=80.
Ratio greedy picks (60,30) (ratio 2.0) then (90,50) (ratio 1.8) => total 150.
Optimal is (100,60)+(60,30)=160 (fits 90) – adjust W to 90; with W=90, ratio greedy 150 vs optimal 160. The point: ratio greedy can be suboptimal; dynamic programming is needed for exact optimum.

**(b) Two‑machine makespan (P2||C\_max).** Greedy LPT (longest processing time first) is **not** always optimal (though it has a 4/3 approximation bound). Counterexample: jobs \[4,3,3,3]. LPT puts 4 on M1, 3 on M2, next 3 on M2 (M2=6), last 3 on M1 (M1=7) → makespan 7. Optimal is (4+3)=7 and (3+3)=6 — here it ties; tweak: jobs \[8,7,6,5,4]. LPT gives (8+5)=13, (7+6+4)=17, makespan 17; optimal is (8+7)=15, (6+5+4)=15. So greedy can lose.

**Takeaway.** Greedy is powerful when a problem has a matroid/greedy‑choice structure (e.g., activity selection, MST, Huffman). When that structure is absent, either careful counterexamples exist or the problem is NP‑hard; then use DP/approximation.
 
