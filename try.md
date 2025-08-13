 
1. **Full Problem Definition**
2. **Example**
3. **Subproblems & Table Definition**
4. **Recurrence**
5. **Solution (Pseudocode)**
6. **Complexity**
 
````markdown
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

---

```
 
