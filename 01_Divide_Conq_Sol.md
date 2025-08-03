# Divide and Conquer – Practice Exercises Solutions

---

## 1. Find Maximum and Minimum

**Algorithm (Divide & Conquer):**  
1. If the subarray has size 1, return that element as both min and max.  
2. If size 2, compare the two elements (1 comparison) and return the smaller as min, larger as max.  
3. Otherwise, split the array into two halves, recursively compute `(min₁, max₁)` on the left and `(min₂, max₂)` on the right, then:  
   - global min = `min(min₁, min₂)` (1 comparison)  
   - global max = `max(max₁, max₂)` (1 comparison)

**Worst-case comparisons:**  
```
C(n) = 3n/2 - 2
```

---

## 2. Maximum Subarray (Divide and Conquer)

**Problem:** Find a contiguous subarray of maximum sum in an array that may contain positive and negative integers.

**Outline:**  
1. Split the array at midpoint `m`.  
2. Recursively compute:  
   - `S_left` = best sum in left half  
   - `S_right` = best sum in right half  
3. Compute best crossing sum `S_cross`:  
   - scan left from `m` toward the start to find the maximum suffix sum  
   - scan right from `m+1` toward the end to find the maximum prefix sum  
   - `S_cross = (max suffix) + (max prefix)`  
4. Return `max(S_left, S_right, S_cross)`.

**Pseudocode:**
```plaintext
MaxSubarray(A, l, r):
  if l == r:
    return A[l]
  m = floor((l + r) / 2)
  left_max = MaxSubarray(A, l, m)
  right_max = MaxSubarray(A, m+1, r)

  // compute max suffix in A[l…m]
  sum = 0
  best_left = -infinity
  for i from m down to l:
    sum = sum + A[i]
    best_left = max(best_left, sum)

  // compute max prefix in A[m+1…r]
  sum = 0
  best_right = -infinity
  for i from m+1 to r:
    sum = sum + A[i]
    best_right = max(best_right, sum)

  cross_max = best_left + best_right
  return max(left_max, right_max, cross_max)
```

**Time complexity:**  
```
T(n) = 2 T(n/2) + Θ(n)   ⇒   O(n log n)
```

---

## 3. Merge Sort

1. **Divide & Conquer structure:**  
   - **Divide:** split the array in half.  
   - **Conquer:** recursively sort each half.  
   - **Combine:** merge the two sorted halves in linear time.

2. **Time complexity:**  
```
T(n) = 2 T(n/2) + Θ(n)   ⇒   O(n log n)
```

3. **Pseudocode:**
```plaintext
MergeSort(A, l, r):
  if l >= r: return
  m = floor((l + r) / 2)
  MergeSort(A, l, m)
  MergeSort(A, m+1, r)
  Merge(A, l, m, r)

Merge(A, l, m, r):
  // assume L = A[l..m], R = A[m+1..r]
  i = 0; j = 0; k = l
  while i < length(L) and j < length(R):
    if L[i] <= R[j]:
      A[k] = L[i]
      i = i + 1
    else:
      A[k] = R[j]
      j = j + 1
    k = k + 1
  // copy any remaining elements
  while i < length(L):
    A[k] = L[i]; i = i + 1; k = k + 1
  while j < length(R):
    A[k] = R[j]; j = j + 1; k = k + 1
```

---

## 4. Binary Search

### a) 1D Binary Search
```plaintext
BinarySearch(A, l, r, x):
  while l <= r:
    m = floor((l + r) / 2)
    if A[m] == x:
      return m
    else if A[m] < x:
      l = m + 1
    else:
      r = m - 1
  return -1   // not found
```
**Time complexity:** `O(log n)`

### b) Search in a 2D Sorted Matrix

Given an `n × n` matrix `A` where each row and each column is sorted in ascending order, there are two efficient methods:

#### 1. Staircase (Step‐wise Linear) Search – O(n)

1. Start at the top‐right corner: `row = 0`, `col = n - 1`.  
2. While `row < n` and `col >= 0`:  
   - If `A[row][col] == x`, return `(row, col)`.  
   - If `A[row][col] > x`, move left: `col = col - 1`.  
   - Else, move down: `row = row + 1`.  
3. If the loop ends, `x` is not in the matrix.

```plaintext
StaircaseSearch(A, n, x):
  row = 0
  col = n - 1
  while row < n and col >= 0:
    if A[row][col] == x:
      return (row, col)
    else if A[row][col] > x:
      col = col - 1
    else:
      row = row + 1
  return "Not found"
```

#### 2. Divide & Conquer Search – O(n^log₂(3)) ≈ O(n^1.585)

1. Let `size` be the current submatrix dimension; `mid = floor(size/2)`.  
2. Compare `x` with the middle element `A[r0 + mid][c0 + mid]`.  
3. If equal, return `(r0+mid, c0+mid)`.  
4. If `x < val`, recurse on three submatrices:  
   - top‐left:    `Search2D(A, r0,     c0,     mid, x)`  
   - top‐right:   `Search2D(A, r0,     c0+mid, mid, x)`  
   - bottom‐left: `Search2D(A, r0+mid, c0,     mid, x)`  
5. If `x > val`, recurse on:  
   - bottom‐right:`Search2D(A, r0+mid, c0+mid, mid, x)`  
   - top‐right:   `Search2D(A, r0,     c0+mid, mid, x)`  
   - bottom‐left: `Search2D(A, r0+mid, c0,     mid, x)`  
6. Collect results; return the first successful match.

```plaintext
Search2D(A, r0, c0, size, x):
  if size == 0:
    return "Not found"
  mid = size // 2
  val = A[r0 + mid][c0 + mid]
  if val == x:
    return (r0 + mid, c0 + mid)

  results = []
  if x < val:
    results.append(Search2D(A, r0,     c0,     mid, x))
    results.append(Search2D(A, r0,     c0+mid, mid, x))
    results.append(Search2D(A, r0+mid, c0,     mid, x))
  else:
    results.append(Search2D(A, r0+mid, c0+mid, mid, x))
    results.append(Search2D(A, r0,     c0+mid, mid, x))
    results.append(Search2D(A, r0+mid, c0,     mid, x))

  for res in results:
    if res != "Not found":
      return res
  return "Not found"
```

---

## 5. Counting Inversions

**Problem:** Count the number of inversions in array `A` (pairs `i < j` with `A[i] > A[j]`).

```plaintext
CountInv(A, l, r):
  if l >= r:
    return 0
  m = floor((l + r) / 2)
  inv = CountInv(A, l, m) + CountInv(A, m+1, r)

  // merge step with counting
  let L = A[l..m], R = A[m+1..r]
  i = 0; j = 0; k = l
  while i < length(L) and j < length(R):
    if L[i] <= R[j]:
      A[k] = L[i]; i = i + 1
    else:
      A[k] = R[j]; j = j + 1
      inv = inv + (length(L) - i)
    k = k + 1
  while i < length(L):
    A[k] = L[i]; i = i + 1; k = k + 1
  while j < length(R):
    A[k] = R[j]; j = j + 1; k = k + 1

  return inv
```

**Time complexity:**  
```
T(n) = 2 T(n/2) + Θ(n)   ⇒   O(n log n)
```

---

## 6. Closest Pair of Points

**Problem:** Given `n` points in the plane, find the pair with minimum Euclidean distance faster than O(n²).

```plaintext
ClosestPair(P):
  sort P by x-coordinate
  return Recurse(P)

Recurse(P):
  if |P| ≤ 3: // brute force
    return brute_force(P)
  mid = |P| // 2
  L = P[0..mid-1], R = P[mid..]
  dL = Recurse(L), dR = Recurse(R)
  d = min(dL, dR)
  strip = points within d of median line, sorted by y
  best = d
  for i in range(len(strip)):
    for j in range(i+1, min(i+7, len(strip))):
      best = min(best, dist(strip[i], strip[j]))
  return best
```

**Time complexity:**  
```
O(n log n)
```

---

## 7. Search in a Sorted Matrix (Recap)

- **Staircase search:** O(n)  
- **True D&C:** O(n^log₂3) ≈ O(n^1.585)

---

## 8. Recurrence Analysis – Master Theorem

For recurrences of the form `T(n) = a T(n/b) + f(n)`, compare `f(n)` to `n^log_b(a)`:

| Recurrence                   | a | b | log_b(a) | f(n)                   | Case          | T(n)                 |
|------------------------------|---|---|----------|------------------------|---------------|----------------------|
| a) 3 T(n/2) + n              | 3 | 2 | ≈1.585   | n = O(n^(1.585−ε))     | Case 1        | Θ(n^1.585)           |
| b) 2 T(n/2) + n·log n        | 2 | 2 | 1        | n·log n = Θ(n log n)   | Case 2 (k=1)  | Θ(n log² n)          |
| c) 8 T(n/2) + n²             | 8 | 2 | 3        | n² = O(n^(3−ε))        | Case 1        | Θ(n³)                |
| d) T(n/2) + 1                | 1 | 2 | 0        | 1 = Θ(1)               | Case 2 (k=0)  | Θ(log n)             |
| e) 4 T(n/2) + n²·log n       | 4 | 2 | 2        | n²·log n = Θ(n² log n) | Case 2 (k=1)  | Θ(n² log² n)         |

> **Note (b):** Removing the extra log factor in combine gives `T(n) = 2 T(n/2) + n` ⇒ O(n log n)
