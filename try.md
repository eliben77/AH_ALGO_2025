 # Dynamic Programming Practice Problems

This document contains **10 classic Dynamic Programming problems**.  
Each problem includes:

1. Problem Explanation + Example  
2. Recurrence Relation (plain text)  
3. Table Definition  
4. Pseudocode  
5. Complexity Analysis  

---

## 1. Word Segmentation Problem

**Problem Explanation:**  
Given a string `S` of length `n` with no spaces, and a dictionary of valid words (max length `k`), determine if `S` can be segmented into valid words.

**Example:**  
S = "applepenapple"  
Dictionary = {"apple", "pen"}  
Possible segmentation: "apple pen apple"

**Recurrence:**  
dp[i] = true if there exists l (1 <= l <= min(k, i)) such that  
`dp[i-l]` is true and `S[i-l..i-1]` is in dictionary.  
Base: dp[0] = true.

**Table Definition:**  
dp[i] = true if S[0..i-1] can be segmented into dictionary words. Size = n+1.

**Pseudocode:**
```pseudo
WordSegmentation(S, D, k):
    n ← length(S)
    dp[0..n] ← false
    dp[0] ← true
    for i from 1 to n:
        for l from 1 to min(k, i):
            if dp[i-l] and isWord(S[i-l..i-1], D):
                dp[i] ← true
                break
    return dp[n]
````

**Complexity:**
Time: O(n \* k \* q)
Space: O(n)

---

## 2. Longest Increasing Subsequence (LIS)

**Problem Explanation:**
Find the length of the longest subsequence where elements are strictly increasing.

**Example:**
A = \[10, 9, 2, 5, 3, 7, 101, 18]
LIS = \[2, 3, 7, 101] → length = 4.

**Recurrence:**
dp\[i] = 1 + max(dp\[j]) for all j < i where A\[j] < A\[i].
Base: dp\[i] = 1.

**Table Definition:**
dp\[i] = length of LIS ending at index i. Size = n.

**Pseudocode:**

```pseudo
LIS(A):
    n ← length(A)
    dp[1..n] ← 1
    for i from 1 to n:
        for j from 1 to i-1:
            if A[j] < A[i]:
                dp[i] ← max(dp[i], dp[j] + 1)
    return max(dp)
```

**Complexity:**
Time: O(n^2)
Space: O(n)

---

## 3. 0/1 Knapsack

**Problem Explanation:**
Given n items with weights w\[i] and values v\[i], and knapsack capacity W, find the maximum value without exceeding capacity.

**Example:**
W = 5, items = {(2,3), (3,4), (4,5), (5,8)}
Best = {(2,3), (3,4)} → value = 7.

**Recurrence:**
If w\_i > capacity: dp\[i]\[w] = dp\[i-1]\[w]
Else: dp\[i]\[w] = max(dp\[i-1]\[w], dp\[i-1]\[w-w\_i] + v\_i)

**Table Definition:**
dp\[i]\[w] = max value using first i items and capacity w. Size = (n+1) × (W+1).

**Pseudocode:**

```pseudo
Knapsack(W, weights, values, n):
    dp[0..n][0..W] ← 0
    for i from 1 to n:
        for w from 0 to W:
            if weights[i] ≤ w:
                dp[i][w] ← max(dp[i-1][w], dp[i-1][w-weights[i]] + values[i])
            else:
                dp[i][w] ← dp[i-1][w]
    return dp[n][W]
```

**Complexity:**
Time: O(n \* W)
Space: O(n \* W) (can be reduced to O(W))

---

## 4. Coin Change – Minimum Coins

**Problem Explanation:**
Given coin denominations and target sum S, find the minimum number of coins.

**Example:**
Coins = \[1, 3, 4], S = 6 → Best = 2 coins (3+3 or 4+1+1).

**Recurrence:**
dp\[s] = min(dp\[s-c] + 1) for all coins c ≤ s.
Base: dp\[0] = 0.

**Table Definition:**
dp\[s] = minimum coins to form sum s. Size = S+1.

**Pseudocode:**

```pseudo
MinCoins(coins, S):
    dp[0..S] ← ∞
    dp[0] ← 0
    for s from 1 to S:
        for c in coins:
            if c ≤ s:
                dp[s] ← min(dp[s], dp[s-c] + 1)
    return dp[S]
```

**Complexity:**
Time: O(m \* S)
Space: O(S)

---

## 5. Matrix Chain Multiplication

**Problem Explanation:**
Find the optimal way to multiply matrices to minimize operations.

**Example:**
p = \[10, 30, 5, 60]
Best = (A1 × A2) × A3 → cost = 4500.

**Recurrence:**
dp\[i]\[j] = min(dp\[i]\[k] + dp\[k+1]\[j] + p\[i-1] \* p\[k] \* p\[j]) for i ≤ k < j.
Base: dp\[i]\[i] = 0.

**Table Definition:**
dp\[i]\[j] = min multiplication cost from Ai to Aj. Size = n × n.

**Pseudocode:**

```pseudo
MatrixChainOrder(p):
    n ← length(p) - 1
    dp[1..n][1..n] ← 0
    for L from 2 to n:
        for i from 1 to n-L+1:
            j ← i+L-1
            dp[i][j] ← ∞
            for k from i to j-1:
                cost ← dp[i][k] + dp[k+1][j] + p[i-1]*p[k]*p[j]
                if cost < dp[i][j]:
                    dp[i][j] ← cost
    return dp[1][n]
```

**Complexity:**
Time: O(n^3)
Space: O(n^2)

---

## 6. Edit Distance

**Problem Explanation:**
Find the minimum edits (insert, delete, replace) to convert A to B.

**Example:**
A = "kitten", B = "sitting" → 3 edits.

**Recurrence:**
If A\[i-1] == B\[j-1]: dp\[i]\[j] = dp\[i-1]\[j-1]
Else: dp\[i]\[j] = 1 + min(dp\[i-1]\[j], dp\[i]\[j-1], dp\[i-1]\[j-1])

**Table Definition:**
dp\[i]\[j] = min edits to convert first i chars of A to first j chars of B. Size = (n+1) × (m+1).

**Pseudocode:**

```pseudo
EditDistance(A, B):
    n ← length(A), m ← length(B)
    for i from 0 to n: dp[i][0] ← i
    for j from 0 to m: dp[0][j] ← j
    for i from 1 to n:
        for j from 1 to m:
            if A[i-1] == B[j-1]:
                dp[i][j] ← dp[i-1][j-1]
            else:
                dp[i][j] ← 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
    return dp[n][m]
```

**Complexity:**
Time: O(n \* m)
Space: O(n \* m)

---

## 7. Longest Common Subsequence (LCS)

**Problem Explanation:**
Find the length of the longest subsequence present in both sequences.

**Example:**
A = "ABCBDAB", B = "BDCAB" → LCS length = 4.

**Recurrence:**
If A\[i-1] == B\[j-1]: dp\[i]\[j] = dp\[i-1]\[j-1] + 1
Else: dp\[i]\[j] = max(dp\[i-1]\[j], dp\[i]\[j-1])

**Table Definition:**
dp\[i]\[j] = LCS length for A\[0..i-1], B\[0..j-1]. Size = (n+1) × (m+1).

**Pseudocode:**

```pseudo
LCS(A, B):
    n ← length(A), m ← length(B)
    dp[0..n][0..m] ← 0
    for i from 1 to n:
        for j from 1 to m:
            if A[i-1] == B[j-1]:
                dp[i][j] ← dp[i-1][j-1] + 1
            else:
                dp[i][j] ← max(dp[i-1][j], dp[i][j-1])
    return dp[n][m]
```

**Complexity:**
Time: O(n \* m)
Space: O(n \* m)

---

## 8. Minimum Path Sum in a Grid

**Problem Explanation:**
Find min sum path from (0,0) to (m-1,n-1) in a grid, moving only right/down.

**Example:**
Grid = \[\[1,3,1],\[1,5,1],\[4,2,1]] → Min path sum = 7.

**Recurrence:**
dp\[i]\[j] = grid\[i]\[j] + min(dp\[i-1]\[j], dp\[i]\[j-1])
Base: first row/column are cumulative sums.

**Table Definition:**
dp\[i]\[j] = min sum to reach cell (i,j). Size = m × n.

**Pseudocode:**

```pseudo
MinPathSum(grid):
    m ← rows(grid), n ← cols(grid)
    dp[0][0] ← grid[0][0]
    for i from 1 to m-1: dp[i][0] ← dp[i-1][0] + grid[i][0]
    for j from 1 to n-1: dp[0][j] ← dp[0][j-1] + grid[0][j]
    for i from 1 to m-1:
        for j from 1 to n-1:
            dp[i][j] ← grid[i][j] + min(dp[i-1][j], dp[i][j-1])
    return dp[m-1][n-1]
```

**Complexity:**
Time: O(m \* n)
Space: O(m \* n)

---

## 9. Palindrome Partitioning – Min Cuts

**Problem Explanation:**
Partition string into the minimum number of palindromic substrings.

**Example:**
S = "aab" → 1 cut ("aa" | "b").

**Recurrence:**
If substring(0, i) is palindrome: dp\[i] = 0
Else: dp\[i] = min(dp\[j] + 1) for all j < i where substring(j+1, i) is palindrome.

**Table Definition:**
dp\[i] = min cuts for S\[0..i]. Size = n.

**Pseudocode:**

```pseudo
MinCutsPalindrome(S):
    n ← length(S)
    dp[0..n-1] ← ∞
    for i from 0 to n-1:
        if isPalindrome(0, i):
            dp[i] ← 0
        else:
            for j from 0 to i-1:
                if isPalindrome(j+1, i):
                    dp[i] ← min(dp[i], dp[j] + 1)
    return dp[n-1]
```

**Complexity:**
Time: O(n^2) with palindrome precomputation
Space: O(n^2)

---

## 10. Maximum Subarray Sum (Kadane)

**Problem Explanation:**
Find the maximum sum of any contiguous subarray.

**Example:**
A = \[-2,1,-3,4,-1,2,1,-5,4] → Max sum = 6 (\[4,-1,2,1]).

**Recurrence:**
dp\[i] = max(A\[i], A\[i] + dp\[i-1])

**Table Definition:**
dp\[i] = max subarray sum ending at i. Size = n.

**Pseudocode:**

```pseudo
MaxSubarraySum(A):
    n ← length(A)
    dp[0] ← A[0]
    best ← A[0]
    for i from 1 to n-1:
        dp[i] ← max(A[i], A[i] + dp[i-1])
        best ← max(best, dp[i])
    return best
```

**Complexity:**
Time: O(n)
Space: O(n) (can be reduced to O(1))

```
