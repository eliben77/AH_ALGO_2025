# Greedy Algorithms — 12 Problems
( Problem Statements + Pseudocode + Examples)

 
## 1) Minimum Rooms / Platforms (Maximum Overlap)

**Problem.**

* **Input.** `n` half‑open intervals `[sᵢ, fᵢ)` with `sᵢ < fᵢ`, representing usage of a resource (meeting room, train platform). Times can be integers or reals.
* **Overlap model.** Two intervals overlap iff their intersection has positive length. Thus `[a,b)` and `[b,c)` do **not** overlap (touching endpoints allowed). If your domain treats touching as overlap, say so (we note the change below).
* **Goal (output).** The **minimum number of identical rooms/platforms** needed so that every interval is assigned to a room and no two overlapping intervals share the same room. (You only need the number here; #8 gives the actual assignment.)
* **Real‑world analogy.** The peak number of simultaneous meetings determines how many rooms you must rent.

**Greedy solution (sweep line).** Sort all **start times** and all **end times** separately; scan with two pointers. On a start < next end → need an extra room; on an end ≤ next start → a room frees up. Track the maximum concurrent usage.

**Pseudocode.**

```text
starts = sorted(s_i)
ends   = sorted(f_i)
i = j = 0
cur = max_rooms = 0
while i < n:                   # process all starts
  if starts[i] < ends[j]:      # touching is non-overlap
    cur += 1
    max_rooms = max(max_rooms, cur)
    i += 1
  else:                        # ends[j] <= starts[i]
    cur -= 1
    j += 1
return max_rooms
```

**Correctness.** At any time `t`, the number of active intervals is a lower bound on rooms (pigeonhole). The scan **immediately** reuses a room when an interval ends (we process `end` when it’s ≤ next `start`), so it attains that lower bound. Hence the maximum concurrent count is **necessary and sufficient**.

**Example:** Intervals: `[0,30), [5,10), [15,20), [25,40), [35,50)` → starts `[0,5,15,25,35]`, ends `[10,20,30,40,50]`. Peak concurrency is 2 ⇒ **2 rooms**.

**Complexity.** Sort `O(n log n)`; scan `O(n)`.

**Clarifications & variants.** If touching should count as overlap, change the condition to `starts[i] <= ends[j]`.

---

## 2) Connecting Ropes / Optimal Merge Pattern

**Problem.**

* **Input.** A multiset of positive lengths `L₁..Lₙ`.
* **Action/cost.** Merging two ropes of lengths `x,y` costs `x+y` and yields a rope of length `x+y` that re‑enters the pool.
* **Goal (output).** The **minimum total merge cost** to end with a single rope.
* **Analogy.** Merging sorted files where merge cost is proportional to sizes; minimize total disk I/O.

**Greedy solution.** Always merge the **two shortest** items first. Use a min‑heap to fetch them fast.

**Pseudocode.**

```text
heap = MinHeap(L_1..L_n)
cost = 0
while heap.size() > 1:
  x = heap.pop_min()
  y = heap.pop_min()
  cost += x + y
  heap.push(x + y)
return cost
```

**Correctness.** (Optimal‑merge/Huffman lemma.) If a shortest rope waits, it is added into **more** later merges and its length is counted more times. Exchanging merges to pair the two shortest **never** increases cost; iterating until every step takes the two smallest gives an optimal sequence.

**Example:** `[8,4,6,12]`: merge `4+6=10` (cost 10) → `[8,10,12]`; merge `8+10=18` (cost 28) → `[12,18]`; merge `12+18=30` (cost **58**). Any other first merge is ≥ 58.

**Complexity.** `O(n log n)`.

**Notes.** This is the same structure as **Huffman coding** (merge least‑frequent symbols first).

---

## 3) Pairing to Minimize Sum of Pair Maxima

**Problem.**

* **Input.** `2n` numbers. You must partition them into `n` pairs.
* **Cost.** A pair `(a,b)` costs `max(a,b)`.
* **Goal (output).** A pairing of **minimum total cost** `Σ max over pairs`.
* **Why this model appears.** “Buy one, pay for the pricier” promotions; pairing students for tests where cost is defined by the stronger student’s score, etc.

**Greedy solution.** Sort ascending and pair **neighbors**: `(q₁,q₂), (q₃,q₄), …`. Then cost is `Σ q₂k`.

**Pseudocode.**

```text
q = sorted(A)
cost = 0
pairs = []
for k in range(0, len(q), 2):
  pairs.append((q[k], q[k+1]))
  cost += q[k+1]
return pairs, cost
```

**Correctness.** Since `Σ max = (sum of all numbers) − Σ min`, minimizing `Σ max` ≡ maximizing the sum of the pairwise **minima**. For `a≤b≤c≤d`, crossing pairs `(a,d),(b,c)` have min‑sum `a+b`; neighbor pairs `(a,b),(c,d)` have min‑sum `a+c ≥ a+b`. Replacing crossings with neighbors never reduces min‑sum. Eliminating all crossings yields the neighbor pairing, hence optimal.

**Example:** `A=[1,2,2,7,8,10]` → neighbor pairs `(1,2),(2,7),(8,10)` cost `2+7+10=19`. A crossing pairing costs ≥ 25.

**Complexity.** Sort `O(n log n)`; pair in `O(n)`.

**Edge cases.** Duplicates are fine; proof is unaffected.

---

## 4) Minimize Total Wait Time (Minimize `Σ Cᵢ`)

**Problem.**

* **Input.** Single machine. All jobs available at time 0; each job `i` has processing time `pᵢ>0` (no preemption).
* **Timing.** If jobs are run in order `π`, completion times are `Cᵢ = Σ_{j up to i in π} p_j`.
* **Objective.** Minimize `Σ Cᵢ` (equivalently average response/wait). Note: average **waiting** time `Σ(Cᵢ − pᵢ)` differs from `Σ Cᵢ` by the constant `Σ pᵢ`, so minimizing one minimizes the other.
* **Analogy.** Minimize average time customers spend in a non‑preemptive service queue.

**Greedy solution (SPT).** Run jobs in **Shortest Processing Time first** order.

**Pseudocode.**

```text
jobs = sorted(jobs, key = p_i)
C = 0
sumC = 0
for job in jobs:
  C += job.p
  sumC += C
return jobs, sumC
```

**Correctness.** For adjacent jobs `i,j` with `pᵢ ≤ pⱼ`, order `[i,j]` contributes `pᵢ + (pᵢ+pⱼ)`; order `[j,i]` contributes `pⱼ + (pⱼ+pᵢ)`, larger by `(pⱼ − pᵢ) > 0`. Swapping any inversion (longer before shorter) strictly reduces `ΣC`. Removing all inversions yields SPT, the global optimum.

**Example:** `p = [3,1,2]`: SPT order `[1,2,3]` → `C = [1,3,6]`, `ΣC=10`. Order `[3,1,2]` → `C=[3,4,6]`, `ΣC=13` (worse).

**Complexity.** `O(n log n)`.

**Edge notes.** With **release times** or **preemption**, optimal rule is **SRPT** (shortest remaining processing time), not plain SPT.

---

## 5) Smith’s Rule (Minimize `Σ wᵢ Cᵢ`)

**Problem.**

* **Input.** Single machine. Job `i` has processing `pᵢ` and weight `wᵢ` (importance/penalty per unit completion time).
* **Objective.** Minimize the **weighted** sum of completion times `Σ wᵢ Cᵢ`.
* **Analogy.** Weighted customers where delays of high‑priority customers are more costly.

**Greedy solution.** Sort by **`wᵢ/pᵢ` descending** (equivalently `pᵢ/wᵢ` ascending), then run jobs.

**Pseudocode.**

```text
jobs = sorted(jobs, key = (w_i / p_i), reverse=True)
C = 0
obj = 0
for job in jobs:
  C += job.p
  obj += job.w * C
return jobs, obj
```

**Correctness.** Two‑job exchange: compare `[i,j]` vs `[j,i]`. Objective difference `Δ = pᵢ wⱼ − pⱼ wᵢ`. Prefer `i` before `j` iff `Δ ≤ 0 ⇔ wᵢ/pᵢ ≥ wⱼ/pⱼ`. Sorting by this key makes every adjacent pair optimal; transitivity yields a global optimum (Smith 1956).

**Example:** `(p,w) = (3,6), (2,3), (1,5)` → ratios `(2, 1.5, 5)` → order `(1,5),(3,6),(2,3)` → `C=[1,4,6]`, `ΣwC=47`, minimal among permutations.

**Complexity.** `O(n log n)`.

**Caution.** This does **not** minimize tardiness (#11).

---

## 6) Unit‑Length Intervals — What Greedy Can and Can’t Do

**Problem.** Intervals are all of length 1.

* **Unweighted variant.** Maximize the **number** of non‑overlapping intervals.
* **Weighted variant.** Each interval `j` has value `vⱼ`; maximize total value without overlap.
* **Continuous vs discrete.** In continuous time, starts can be arbitrary reals; in the **discrete** special case, starts are integers and each interval is **exactly** `[t, t+1]`.

**Greedy solution.**

* **Unweighted:** earliest‑finish (finish ↑) is optimal (classic activity selection argument).
* **Weighted, continuous time:** no simple global greedy is optimal (small counterexamples exist).
* **Discrete special case:** conflicts occur **only** among intervals with the same `t`. So pick, for each `t`, the interval with **maximum value**; choices at different `t` are independent.

**Pseudocode (unweighted; discrete weighted).**

```text
# Unweighted
I = sorted(intervals, key=finish)
S=[]; last=-inf
for (s,f) in I:
  if s >= last: S.append((s,f)); last=f
return S

# Discrete weighted (all intervals [t,t+1])
for each integer t:
  choose interval with start t and maximum value (if exists)
return union over t
```

**Correctness.** Unweighted: exchange—there exists an optimal solution whose first interval has **minimum finish**; iterate. Discrete weighted: because intervals at different `t` do not overlap across `t`, you can maximize locally at each `t` to get a global optimum. The general weighted case needs DP (e.g., weighted interval scheduling).

**Examples:**

* Unweighted: `[0,1], [0.5,1.5], [1.5,2.5]` → greedy picks `[0,1]` and `[1.5,2.5]` (2 intervals).
* Weighted counterexample (continuous): `[0,1](1), [0.5,1.5](100), [1.5,2.5](100)` → picking `[0,1]` blocks the two 100‑valued intervals.

**Complexity.** `O(n log n)` (or `O(n)` with bucketing by integer `t`).

---

## 7) Job Sequencing with Deadlines (Unit‑Time Jobs)

**Problem.**

* **Input.** `n` jobs; each job `j` takes **one** unit, has profit `wⱼ ≥ 0` and a **deadline** `dⱼ ∈ {1..T}`. All jobs available at time 0; preemption not allowed.
* **Feasibility.** At most one job per unit time slot. A job completed at or before its deadline earns its profit; jobs scheduled after their deadline earn **0**.
* **Goal (output).** A schedule (subset and times) with **maximum total profit**.

**Greedy solution.** Sort jobs by **profit descending**. For each job, schedule it in the **latest available** slot at or before its deadline (use DSU to find the latest free slot quickly).

**Pseudocode.**

```text
# DSU helpers
make_set(0..T): parent[t]=t for all t; parent[0]=0
find(t): if parent[t]==t return t else parent[t]=find(parent[t]); return parent[t]
union(a,b): parent[find(a)] = find(b)

jobs = sorted(jobs, key=profit, reverse=True)
schedule = [EMPTY] * (T+1)
for job in jobs:
  t = find(job.deadline)
  if t > 0:
    schedule[t] = job
    union(t, t-1)
return schedule
```

**Correctness.** (i) If a less profitable job occupies a slot that a more profitable job could use, swapping them increases profit and keeps feasibility. (ii) Placing a chosen job as **late as possible** preserves opportunities for others (it doesn’t block any job with an earlier deadline). Repeating these exchanges yields the greedy schedule.

**Example:** `(profit,deadline) = (100,2),(50,1),(20,2),(10,1)` → choose `100@t=2`, `50@t=1`, others drop; total **150**.

**Complexity.** `O(n log n)` + near‑`O(1)` DSU operations.

---

## 8) Minimum Rooms via Priority Queue (Constructive)

**Problem.** Same input as #1, but **also output an assignment** of intervals to the **minimum** number of rooms.

**Greedy solution.** Sort intervals by **start**. Maintain a **min‑heap of `(end_time, room_id)`**. For each new interval `(s,f)`, if the earliest‑ending room has `end ≤ s`, reuse it; otherwise open a new room.

**Pseudocode.**

```text
I = sorted(intervals, key=start)
heap = MinHeap()  # (end, room)
rooms = 0
assignment = []   # (interval_id -> room)
for I_k in I:
  if heap and heap.min().end <= I_k.start:
    (e, r) = heap.pop_min()
    assignment.append((I_k.id, r))
    heap.push((I_k.finish, r))
  else:
    rooms += 1
    r = rooms
    assignment.append((I_k.id, r))
    heap.push((I_k.finish, r))
return rooms, assignment
```

**Correctness.** A room becomes reusable **exactly** when its current meeting ends. The min‑heap gives the earliest such room; reusing whenever possible attains the lower bound from #1. Opening a new room happens **only** when all rooms are still busy.

**Example:** `[0,30]→R1`, `[5,10]→R2` (R1 busy), `[15,20]→R2` (R2 free at 10). Total **2 rooms**.

**Complexity.** `O(n log n)`.

---

## 9) Egyptian Fractions (Greedy)

**Problem.**

* **Input.** A reduced fraction `0 < a/b < 1`.
* **Output.** A list of **distinct** denominators `(x₁, x₂, …)` such that `a/b = Σ 1/xₖ`.
* **Why distinct?** Classical Egyptian representation requires distinct unit fractions.

**Greedy solution.** At each step take `x = ceil(b/a)` and subtract `1/x` from the fraction; reduce and repeat until numerator zero.

**Pseudocode.**

```text
function egyptian(a,b):
  terms = []
  while a > 0:
    x = ceil(b / a)
    terms.append(x)
    a, b = a*x - b, b*x
    g = gcd(a,b); a//=g; b//=g
  return terms
```

**Correctness.** Because `x = ceil(b/a)`, `1/x ≤ a/b`. The remainder is non‑negative and strictly smaller. Denominators strictly increase, so fractions are distinct. The telescoping sum reconstructs `a/b`.

**Example:** `7/15` → `x=3` ⇒ `7/15 − 1/3 = 2/15`; `x=8` ⇒ `2/15 − 1/8 = 1/120`; `x=120` ⇒ done. So `7/15 = 1/3 + 1/8 + 1/120`.

**Complexity.** Polynomial in bit‑length of inputs (though expansions can be long in worst cases).

---

## 10) Antenna Placement (Maximize Minimum Distance)

**Problem.**

* **Input.** Sorted positions `x₁ < … < xₙ` on a line and an integer `k` (1 ≤ k ≤ n).
* **Output.** Place `k` antennas on some of these positions to **maximize** the minimum distance `D` between any two consecutively placed antennas.
* **Monotonicity.** If a distance `D` is feasible (you can place ≥ k), then any smaller distance is also feasible.

**Greedy solution (feasibility + binary search).** For a fixed `D`, place at `x₁`, then always at the **leftmost position** `≥ last + D`. If you place `≥ k` antennas, `D` is feasible; otherwise not. Binary‑search the largest feasible `D`.

**Pseudocode.**

```text
function can_place(D):
  count = 1
  last = x[1]
  for i in 2..n:
    if x[i] - last >= D:
      count += 1
      last = x[i]
  return count >= k

lo = 0
hi = x[n] - x[1]
while lo < hi:
  mid = ceil((lo + hi) / 2)
  if can_place(mid): lo = mid
  else: hi = mid - 1
return lo
```

**Correctness.** (**Staying‑ahead**) The greedy chooser never reduces the number placed for a fixed `D`: if you ever skip the earliest feasible spot, you cannot place **more** later. Because feasibility in `D` is monotone, binary search finds the maximal `D`.

**Example:** `x=[1,2,4,8,9], k=3` → `D=4` infeasible (only 2 placed), `D=3` feasible (place at 1,4,8). Best `D = 3`.

**Complexity.** `O(n log (xₙ−x₁))`.

---

## 11) Scheduling with Linear Tardiness (`Σ wᵢ·max(0, Cᵢ−dᵢ)`) — Limits of Greedy

**Problem.**

* **Input.** One machine; job `i` has processing `pᵢ>0`, deadline `dᵢ`, weight `wᵢ≥0`.
* **Timing.** A schedule yields completion times `Cᵢ`.
* **Key terms.**

  * **Lateness:** `Lᵢ = Cᵢ − dᵢ` (can be negative if early).
  * **Tardiness:** `Tᵢ = max(0, Lᵢ) = max(0, Cᵢ − dᵢ)` → only counts positive lateness; being early gives 0, not negative reward.
* **Objective.** Minimize `Σ wᵢ Tᵢ`.

**Reality.** The problem `1||ΣwᵢTᵢ` is **NP‑hard**. Natural greedies fail:

* **EDD** (deadline↑) optimizes `L_max`, not `ΣT`.
* **Smith’s rule** (w/p↓) optimizes `Σ wᵢ Cᵢ`, not `ΣT`.
  Small instances can be constructed where either order is suboptimal for `ΣT`.

**Example (compute tardiness):** Suppose `(p,d,w)` = A(3,3,10), B(2,2,1), C(2,4,1). Order `A,B,C`: completion `C=[3,5,7]` → `T=[max(0,0), max(0,3), max(0,3)] = [0,3,3]` → `ΣwT = 0*10 + 1*3 + 1*3 = 6`. Try `B,A,C`: `C=[2,5,7]` → `T=[0,2,3]` → `ΣwT = 0 + 20 + 3 = 23` (worse). This shows sensitivity to order and weights; neither EDD nor Smith’s rule systematically wins for `ΣT`.

**What actually works.** DP in `O(n·Σpᵢ)` for small `Σpᵢ`, branch‑and‑bound, or heuristics (e.g., local search around EDD, Wilkerson–Irwin) with no global guarantee.

**Complexity.** NP‑hard ⇒ no known polytime exact algorithm unless P=NP.

---

## 12) Interval Cover with Capacities (Discrete Time)

**Problem.**

* **Input.** Discrete time points `T = {t₁ < … < t_m}`. Each shift `i` is an interval `[sᵢ, fᵢ)` contributing **capacity 1** at every `t` with `sᵢ ≤ t < fᵢ`. A demand function `D(t)` (nonnegative integers) specifies how many units of capacity are required at each `t`.
* **Feasibility.** The instance is feasible only if, for every `t`, at least `D(t)` shifts exist that cover `t`. (The greedy below assumes feasibility; otherwise it will discover no available shift at some deficit.)
* **Goal (output).** Choose the **fewest shifts** so that, for every `t ∈ T`, the number of chosen shifts covering `t` is at least `D(t)`.
* **Analogy.** Staffing: choose the smallest number of worker shifts so that each hour has enough workers on duty.

**Greedy solution (latest‑end at each deficit).** Sweep `t` from left to right. Maintain:

* `active`: max‑heap by end time of **chosen** shifts that currently cover `t`;
* `avail`: max‑heap by end time of **candidate** shifts with `sᵢ ≤ t < fᵢ` not yet chosen.
  If `|active| < D(t)`, repeatedly **choose** from `avail` the shift with the **latest end** (maximizes coverage of future times) until coverage meets `D(t)`. As `t` advances, remove from `active` any chosen shift that no longer covers `t`.

**Pseudocode.**

```text
I = sorted(intervals, key=start)
active = MaxHeapByEnd()
avail  = MaxHeapByEnd()
chosen = []
i = 0
for t in T:
  # add new candidates that have started but not yet finished
  while i < n and I[i].start <= t:
    if I[i].finish > t: avail.push(I[i])
    i += 1
  # drop chosen shifts that have expired
  while active and active.max_end <= t:
    active.pop()
  # fill any deficit using latest-ending candidates
  need = D[t] - active.size()
  while need > 0:
    x = avail.pop_max()      # if none exists, instance is infeasible
    chosen.append(x)
    active.push(x)
    need -= 1
return chosen
```

**Correctness.** (**Staying‑ahead/exchange**) Suppose at time `t` you must add `r` shifts. Choosing the `r` **latest‑ending** candidates gives coverage at least as good now and for all future times `u ≥ t` as any other `r` choices. If some optimal solution picked a shorter‑ending shift while a longer one was available, swapping them cannot decrease coverage at any `u ≥ t` and may increase it; repeating such swaps transforms any optimum into the greedy choice without increasing cardinality. Thus the greedy set is minimum.

**Example:** `T={1..6}`, `D=[1,2,1,2,1,0]`. Shifts: `A=[1,4)`, `B=[2,5)`, `C=[3,6)`, `D=[1,3)`, `E=[4,6)`. At `t=1`, need 1 → choose latest of `{A,D}` → `A(4)`. At `t=2`, need 2 → add latest of `{B,D}` → `B(5)`. At `t=4`, `A` expires; need 2 → choose latest of `{B,E}` → `E(6)`. Final chosen `{A,B,E}` (3 shifts) meets all demands and is minimal.

**Complexity.** Sort `O(n log n)`; across all `t`, heap ops `O((n+m) log n)`.
