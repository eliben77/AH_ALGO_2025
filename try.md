 
1. **Full Problem Definition**
2. **Example**
3. **Subproblems & Table Definition**
4. **Recurrence**
5. **Solution (Pseudocode)**
6. **Complexity**
 

# Dynamic Programming — 18 Classic Problems with Full Solutions

## 1) 0/1 Knapsack Problem
**Problem.**  
Given `n` items, each with weight `w[i]` and value `v[i]`, and capacity `W`, find the maximum total value without exceeding the capacity. Each item can be taken at most once.

**Example.**  
`w = [2, 3, 4], v = [4, 5, 10], W = 6` → Best choice is items `{0, 2}` with total value `14`.

**Subproblems & Table.**  
`dp[i][c]` = max value using items `0..i-1` with capacity `c`.  
Size: `(n+1) × (W+1)`.

**Recurrence.**
- Base: `dp[0][c] = 0`
- Step:  
  If `w[i-1] > c` → `dp[i][c] = dp[i-1][c]`  
  Else: `dp[i][c] = max(dp[i-1][c], dp[i-1][c-w[i-1]] + v[i-1])`

**Pseudocode.**
```python
def knapsack(w, v, W):
    n = len(w)
    dp = [[0]*(W+1) for _ in range(n+1)]
    for i in range(1, n+1):
        for c in range(W+1):
            dp[i][c] = dp[i-1][c]
            if w[i-1] <= c:
                dp[i][c] = max(dp[i][c], dp[i-1][c-w[i-1]] + v[i-1])
    return dp[n][W]
````

**Complexity.**
Time: `O(nW)`
Space: `O(nW)` (or `O(W)` with optimization).

---

## 2) Coin Change — Minimum Coins

**Problem.**
Given denominations `coins` and amount `A`, find the minimum number of coins to make `A`. Return `-1` if impossible.

**Example.**
`coins = [1, 3, 4], A = 6` → `2` coins (3+3).

**Subproblems & Table.**
`dp[x]` = min coins for amount `x`. Size: `A+1`.

**Recurrence.**

* Base: `dp[0] = 0`
* Step: `dp[x] = min(dp[x-c] + 1)` for all `c ≤ x`.

**Pseudocode.**

```python
def min_coins(coins, A):
    INF = 10**9
    dp = [INF]*(A+1)
    dp[0] = 0
    for x in range(1, A+1):
        for c in coins:
            if c <= x:
                dp[x] = min(dp[x], dp[x-c] + 1)
    return dp[A] if dp[A] != INF else -1
```

**Complexity.**
Time: `O(A·|coins|)`
Space: `O(A)`

---

## 3) Longest Common Subsequence (LCS)

**Problem.**
Given strings `s` and `t`, find the length of their longest subsequence common to both.

**Example.**
`s = "abcde"`, `t = "ace"` → LCS length `3`.

**Subproblems & Table.**
`dp[i][j]` = LCS length of `s[:i]` and `t[:j]`.

**Recurrence.**

* Base: `dp[i][0] = dp[0][j] = 0`
* Step:
  If `s[i-1] == t[j-1]`: `dp[i][j] = dp[i-1][j-1] + 1`
  Else: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`

**Pseudocode.**

```python
def lcs_len(s, t):
    n, m = len(s), len(t)
    dp = [[0]*(m+1) for _ in range(n+1)]
    for i in range(1, n+1):
        for j in range(1, m+1):
            if s[i-1] == t[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    return dp[n][m]
```

**Complexity.**
Time: `O(nm)`
Space: `O(nm)`

---

## 4) Longest Increasing Subsequence (LIS)

**Problem.**
Find the length of the longest strictly increasing subsequence in array `a`.

**Example.**
`a = [10,9,2,5,3,7,101,18]` → length `4` (`[2,3,7,18]`).

**Subproblems & Table.**
`dp[i]` = LIS length ending at `i`.

**Recurrence.**
`dp[i] = 1 + max(dp[j])` for `j < i` and `a[j] < a[i]`, else `1`.

**Pseudocode.**

```python
def lis_len(a):
    n = len(a)
    dp = [1]*n
    for i in range(n):
        for j in range(i):
            if a[j] < a[i]:
                dp[i] = max(dp[i], dp[j]+1)
    return max(dp, default=0)
```

**Complexity.**
Time: `O(n²)`
Space: `O(n)`

---

## 5) Edit Distance (Levenshtein)

**Problem.**
Transform `s` into `t` with minimum insert, delete, or replace operations.

**Example.**
`s = "intention"`, `t = "execution"` → distance `5`.

**Subproblems & Table.**
`dp[i][j]` = edit distance between `s[:i]` and `t[:j]`.

**Recurrence.**

* Base: `dp[i][0] = i`, `dp[0][j] = j`
* Step:
  If `s[i-1] == t[j-1]`: `dp[i][j] = dp[i-1][j-1]`
  Else: `dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])`

**Pseudocode.**

```python
def edit_distance(s, t):
    n, m = len(s), len(t)
    dp = [[0]*(m+1) for _ in range(n+1)]
    for i in range(n+1): dp[i][0] = i
    for j in range(m+1): dp[0][j] = j
    for i in range(1, n+1):
        for j in range(1, m+1):
            if s[i-1] == t[j-1]:
                dp[i][j] = dp[i-1][j-1]
            else:
                dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
    return dp[n][m]
```

**Complexity.**
Time: `O(nm)`
Space: `O(nm)`

---

## 6) Matrix Chain Multiplication

**Problem.**
Given matrices `(p0×p1), (p1×p2), ..., (p_{n-1}×p_n)`, find the order of multiplication that minimizes scalar multiplications.

**Example.**
`p = [10,30,5,60]` → cost `4500`.

**Subproblems & Table.**
`dp[i][j]` = min cost to multiply matrices `i..j`.

**Recurrence.**
`dp[i][j] = min(dp[i][k] + dp[k+1][j] + p[i-1]*p[k]*p[j])` for `i ≤ k < j`.

**Pseudocode.**

```python
def matrix_chain(p):
    n = len(p) - 1
    dp = [[0]*(n+1) for _ in range(n+1)]
    for len_ in range(2, n+1):
        for i in range(1, n-len_+2):
            j = i+len_-1
            dp[i][j] = float('inf')
            for k in range(i, j):
                dp[i][j] = min(dp[i][j], dp[i][k] + dp[k+1][j] + p[i-1]*p[k]*p[j])
    return dp[1][n]
```

**Complexity.**
Time: `O(n³)`
Space: `O(n²)`

---

## 7) Fibonacci (DP)

**Problem.**
Compute nth Fibonacci number efficiently.

**Example.**
`n = 7` → `13`.

**Recurrence.**
`dp[0] = 0`, `dp[1] = 1`, `dp[i] = dp[i-1] + dp[i-2]`.

**Pseudocode.**

```python
def fib(n):
    if n <= 1: return n
    a, b = 0, 1
    for _ in range(2, n+1):
        a, b = b, a+b
    return b
```

**Complexity.**
Time: `O(n)`
Space: `O(1)`

---

## 8) Subset Sum (Decision)

**Problem.**
Given `S` and target `T`, determine if a subset sums to `T`.

**Example.**
`S = [3,34,4,12,5,2], T = 9` → `True`.

**Subproblems.**
`dp[i][t]` = `True` if some subset of first `i` numbers sums to `t`.

**Recurrence.**
`dp[i][t] = dp[i-1][t] or (t >= S[i-1] and dp[i-1][t-S[i-1]])`

**Pseudocode.**

```python
def subset_sum(S, T):
    n = len(S)
    dp = [[False]*(T+1) for _ in range(n+1)]
    dp[0][0] = True
    for i in range(1, n+1):
        for t in range(T+1):
            dp[i][t] = dp[i-1][t]
            if t >= S[i-1]:
                dp[i][t] |= dp[i-1][t-S[i-1]]
    return dp[n][T]
```

**Complexity.**
Time: `O(nT)`
Space: `O(nT)`

---

## 9) Rod Cutting

**Problem.**
Maximize profit cutting rod of length `n` with given prices.

**Example.**
`n=4, price=[1,5,8,9]` → `10` (cut into two length-2).

**Subproblems.**
`dp[x]` = max profit for rod length `x`.

**Recurrence.**
`dp[x] = max(price[k-1] + dp[x-k])` for `1 ≤ k ≤ x`.

**Pseudocode.**

```python
def rod_cut(price, n):
    dp = [0]*(n+1)
    for x in range(1, n+1):
        best = 0
        for k in range(1, x+1):
            best = max(best, price[k-1] + dp[x-k])
        dp[x] = best
    return dp[n]
```

**Complexity.**
Time: `O(n²)`
Space: `O(n)`

## 10) Word Break (Decision)
**Problem.**  
Given a string `s` and a dictionary `D`, determine if `s` can be segmented into one or more dictionary words.

**Example.**  
`s = "leetcode"`, `D = {"leet","code"}` → `True`.

**Subproblems & Table.**  
`dp[i]` = `True` if `s[:i]` can be segmented.  
Size: `n+1`.

**Recurrence.**  
`dp[0] = True`  
`dp[i] = any(dp[j] and s[j:i] in D for j < i)`

**Pseudocode.**
```python
def word_break(s, D):
    n = len(s)
    dp = [False]*(n+1)
    dp[0] = True
    for i in range(1, n+1):
        for j in range(i):
            if dp[j] and s[j:i] in D:
                dp[i] = True
                break
    return dp[n]
````

**Complexity.**
Time: `O(n²)`
Space: `O(n)`

---

## 11) Coin Change — Counting Ways

**Problem.**
Count the number of combinations of coins that sum to `A` (order doesn't matter).

**Example.**
`coins = [1,2,5], A = 5` → `4` ways: `(5)`, `(2+2+1)`, `(2+1+1+1)`, `(1×5)`.

**Subproblems.**
`dp[x]` = number of ways to make `x`.

**Recurrence.**
`dp[0] = 1`
For each coin `c`:
`for x in range(c, A+1): dp[x] += dp[x-c]`

**Pseudocode.**

```python
def coin_change_count(coins, A):
    dp = [0]*(A+1)
    dp[0] = 1
    for c in coins:
        for x in range(c, A+1):
            dp[x] += dp[x-c]
    return dp[A]
```

**Complexity.**
Time: `O(A·|coins|)`
Space: `O(A)`

---

## 12) Word Break — Counting Segmentations

**Problem.**
Count how many ways `s` can be segmented into dictionary words.

**Example.**
`s = "catsanddog"`, `D = {"cat","cats","and","sand","dog"}` → `2` ways.

**Subproblems.**
`dp[i]` = number of segmentations for `s[:i]`.

**Recurrence.**
`dp[0] = 1`
`dp[i] = sum(dp[j] for j < i if s[j:i] in D)`

**Pseudocode.**

```python
def word_break_count(s, D):
    n = len(s)
    dp = [0]*(n+1)
    dp[0] = 1
    for i in range(1, n+1):
        for j in range(i):
            if s[j:i] in D:
                dp[i] += dp[j]
    return dp[n]
```

**Complexity.**
Time: `O(n²)`
Space: `O(n)`

---

## 13) Unique Paths in a Grid

**Problem.**
Count the number of unique paths in an `m×n` grid moving only right or down.

**Example.**
`m = 3, n = 3` → `6` paths.

**Subproblems.**
`dp[i][j]` = paths to `(i,j)`.

**Recurrence.**
`dp[i][j] = (dp[i-1][j] if i>0 else 0) + (dp[i][j-1] if j>0 else 0)`
Base: `dp[0][0] = 1`.

**Pseudocode.**

```python
def unique_paths(m, n):
    dp = [[0]*n for _ in range(m)]
    dp[0][0] = 1
    for i in range(m):
        for j in range(n):
            if i > 0: dp[i][j] += dp[i-1][j]
            if j > 0: dp[i][j] += dp[i][j-1]
    return dp[m-1][n-1]
```

**Complexity.**
Time: `O(mn)`
Space: `O(mn)` (or `O(n)` optimized).

---

## 14) Unique Paths with Obstacles

**Problem.**
Same as #13 but some cells have obstacles (cannot be visited).

**Example.**

```
0 0 0
0 1 0
0 0 0
```

→ `2` paths.

**Recurrence.**
If obstacle: `dp[i][j] = 0` else same as #13.

**Pseudocode.**

```python
def unique_paths_obstacles(grid):
    m, n = len(grid), len(grid[0])
    dp = [[0]*n for _ in range(m)]
    if grid[0][0] == 0: dp[0][0] = 1
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 1:
                dp[i][j] = 0
            else:
                if i > 0: dp[i][j] += dp[i-1][j]
                if j > 0: dp[i][j] += dp[i][j-1]
    return dp[m-1][n-1]
```

**Complexity.**
Time: `O(mn)`
Space: `O(mn)`

---

## 15) Partition Equal Subset Sum — Counting Ways

**Problem.**
Count how many ways an array can be split into two subsets with equal sum.

**Example.**
`[1,5,11,5]` → `1` way.

**Subproblems.**
If total sum is odd → `0`.
Target = `S/2`, `dp[t]` = number of subsets with sum `t`.

**Recurrence.**
`dp[0] = 1`
For each num:
`for t from Target down to num: dp[t] += dp[t-num]`

**Pseudocode.**

```python
def partition_equal_count(nums):
    S = sum(nums)
    if S % 2: return 0
    T = S // 2
    dp = [0]*(T+1)
    dp[0] = 1
    for num in nums:
        for t in range(T, num-1, -1):
            dp[t] += dp[t-num]
    return dp[T]
```

**Complexity.**
Time: `O(nT)`
Space: `O(T)`

---

## 16) Paths in a Grid with Diagonal Moves

**Problem.**
Count unique paths from `(0,0)` to `(m-1,n-1)` moving right, down, or diagonally down-right.

**Example.**
`m = 2, n = 2` → `3` paths.

**Recurrence.**
`dp[i][j] = dp[i-1][j] + dp[i][j-1] + dp[i-1][j-1]` (with bounds checks).
Base: `dp[0][0] = 1`.

**Pseudocode.**

```python
def unique_paths_diag(m, n):
    dp = [[0]*n for _ in range(m)]
    dp[0][0] = 1
    for i in range(m):
        for j in range(n):
            if i > 0: dp[i][j] += dp[i-1][j]
            if j > 0: dp[i][j] += dp[i][j-1]
            if i > 0 and j > 0: dp[i][j] += dp[i-1][j-1]
    return dp[m-1][n-1]
```

**Complexity.**
Time: `O(mn)`
Space: `O(mn)`

---

## 17) Counting Binary Strings without Consecutive 1’s

**Problem.**
Count binary strings of length `n` with no two consecutive 1’s.

**Example.**
`n = 3` → `5` valid strings.

**Subproblems.**
`dp0[i]` = count ending with 0, `dp1[i]` = count ending with 1.

**Recurrence.**
`dp0[i] = dp0[i-1] + dp1[i-1]`
`dp1[i] = dp0[i-1]`

**Pseudocode.**

```python
def count_bin_no_consec1(n):
    dp0, dp1 = 1, 1
    for _ in range(2, n+1):
        dp0, dp1 = dp0+dp1, dp0
    return dp0 + dp1
```

**Complexity.**
Time: `O(n)`
Space: `O(1)`

---

## 18) Bitmask DP — Traveling Salesman Problem (TSP)

**Problem.**
Given cost matrix `cost[i][j]`, find min tour visiting all cities exactly once and returning to start.

**Example.**
Small example with `n=4`.

**Subproblems.**
`dp[mask][i]` = min cost to visit cities in `mask` ending at `i`.

**Recurrence.**
`dp[mask][i] = min(dp[mask^(1<<i)][j] + cost[j][i])`

**Pseudocode.**

```python
def tsp(cost):
    n = len(cost)
    dp = [[float('inf')]*n for _ in range(1<<n)]
    dp[1][0] = 0
    for mask in range(1<<n):
        for i in range(n):
            if not (mask & (1<<i)): continue
            for j in range(n):
                if mask & (1<<j): continue
                dp[mask|(1<<j)][j] = min(dp[mask|(1<<j)][j], dp[mask][i] + cost[i][j])
    return min(dp[(1<<n)-1][j] + cost[j][0] for j in range(1, n))
```

**Complexity.**
Time: `O(n²·2^n)`
Space: `O(n·2^n)`

---

## 📊 Summary Table

| #  | Problem                | State Definition                                | Time      | Space    |
| -- | ---------------------- | ----------------------------------------------- | --------- | -------- |
| 1  | 0/1 Knapsack           | `dp[i][c]`: max value with first i items, cap c | O(nW)     | O(nW)    |
| 2  | Coin Change Min        | `dp[x]`: min coins for amount x                 | O(A·k)    | O(A)     |
| 3  | LCS                    | `dp[i][j]`: LCS len of s\[:i], t\[:j]           | O(nm)     | O(nm)    |
| 4  | LIS                    | `dp[i]`: LIS ending at i                        | O(n²)     | O(n)     |
| 5  | Edit Distance          | `dp[i][j]`: min edits for s\[:i], t\[:j]        | O(nm)     | O(nm)    |
| 6  | Matrix Chain           | `dp[i][j]`: min mult cost for Ai..Aj            | O(n³)     | O(n²)    |
| 7  | Fibonacci              | prev two values                                 | O(n)      | O(1)     |
| 8  | Subset Sum             | `dp[i][t]`: subset sum possible?                | O(nT)     | O(nT)    |
| 9  | Rod Cutting            | `dp[x]`: max profit length x                    | O(n²)     | O(n)     |
| 10 | Word Break             | `dp[i]`: s\[:i] segmentable?                    | O(n²)     | O(n)     |
| 11 | Coin Change Count      | `dp[x]`: #ways to make x                        | O(A·k)    | O(A)     |
| 12 | Word Break Count       | `dp[i]`: #ways segment s\[:i]                   | O(n²)     | O(n)     |
| 13 | Unique Paths           | `dp[i][j]`: paths to (i,j)                      | O(mn)     | O(mn)    |
| 14 | Unique Paths Obstacles | same as 13 with check                           | O(mn)     | O(mn)    |
| 15 | Partition Equal Count  | `dp[t]`: #subsets sum t                         | O(nT)     | O(T)     |
| 16 | Unique Paths Diag      | `dp[i][j]`: paths to (i,j)                      | O(mn)     | O(mn)    |
| 17 | Bin Strings no 11      | counts ending with 0/1                          | O(n)      | O(1)     |
| 18 | TSP Bitmask            | `dp[mask][i]`: min cost                         | O(n²·2^n) | O(n·2^n) |

```
---

  
