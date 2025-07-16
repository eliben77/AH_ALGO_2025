# Divide and Conquer – Practice Exercises Solutions

---

## 1. Find Maximum and Minimum

**Algorithm (Divide & Conquer):**  
1. If the subarray has size 1, return that element as both min and max.  
2. If size 2, compare the two elements (1 comparison) and return the smaller as min, larger as max.  
3. Otherwise, split the array into two halves, recursively compute (min₁, max₁) on the left and (min₂, max₂) on the right, then:
   - global min = min(min₁, min₂) (1 comparison)  
   - global max = max(max₁, max₂) (1 comparison)

**Worst-case comparisons:**  
3n/2 − 2

---

## 2. Maximum Subarray (Divide and Conquer)

**Problem:** Find a contiguous subarray of maximum sum in an array with positive and negative integers.

**Outline:**  
1. Split the array at midpoint m.  
2. Recursively compute:
   - S_left = best sum in left half  
   - S_right = best sum in right half  
3. Compute best crossing sum S_cross:
   - scan left from m to find max suffix sum  
   - scan right from m+1 to find max prefix sum  
   - S_cross = (max suffix) + (max prefix)  
4. Return max(S_left, S_right, S_cross).

**Pseudocode:**
```plaintext
MaxSubarray(A, l, r):
  if l == r:
    return A[l]
  m = floor((l + r) / 2)
  L = MaxSubarray(A, l, m)
  R = MaxSubarray(A, m+1, r)

  // max suffix in A[l…m]
  sum = 0; left_max = -infinity
  for i from m down to l:
    sum = sum + A[i]
    left_max = max(left_max, sum)

  // max prefix in A[m+1…r]
  sum = 0; right_max = -infinity
  for i from m+1 to r:
    sum = sum + A[i]
    right_max = max(right_max, sum)

  X = left_max + right_max
  return max(L, R, X)
```

**Time complexity:**  
T(n) = 2 T(n/2) + n  
=> T(n) = O(n log n)

---

## 3. Merge Sort

1. **Divide & Conquer structure:**
   - Divide: split array in half.  
   - Conquer: recursively sort each half.  
   - Combine: merge the two sorted halves in linear time.

2. **Time complexity:** O(n log n).

3. **Pseudocode:**
```plaintext
MergeSort(A, l, r):
  if l >= r: return
  m = floor((l + r) / 2)
  MergeSort(A, l, m)
  MergeSort(A, m+1, r)
  Merge(A, l, m, r)

Merge(A, l, m, r):
  let L = A[l…m], R = A[m+1…r]
  i = 0; j = 0; k = l
  while i < |L| and j < |R|:
    if L[i] <= R[j]:
      A[k] = L[i]; i = i + 1
    else:
      A[k] = R[j]; j = j + 1
    k = k + 1
  // copy leftovers
  while i < |L|:
    A[k] = L[i]; i = i + 1; k = k + 1
  while j < |R|:
    A[k] = R[j]; j = j + 1; k = k + 1
```

---

## 4. Binary Search

### a) 1D Binary Search
```plaintext
BinarySearch(A, l, r, x):
  while l <= r:
    m = floor((l + r) / 2)
    if A[m] == x: return m
    else if A[m] < x: l = m + 1
    else: r = m - 1
  return -1
```
**Time:** O(log n).

### b) Search in a 2D sorted matrix
- Staircase search (O(n)): start at top‑right, move left/down based on comparison.  
- D&C quadrant method (O(n^1.585)): split matrix into quadrants, recurse on three.

---

## 5. Counting Inversions

**Idea:** augment merge sort to count cross-inversions.

```plaintext
CountInv(A, l, r):
  if l >= r: return 0
  m = floor((l + r) / 2)
  inv = CountInv(A, l, m) + CountInv(A, m+1, r)

  let L = A[l…m], R = A[m+1…r]
  i = 0; j = 0; k = l
  while i < |L| and j < |R|:
    if L[i] <= R[j]:
      A[k] = L[i]; i = i + 1
    else:
      A[k] = R[j]; j = j + 1
      inv = inv + (|L| - i)
    k = k + 1
  // copy leftovers...
  return inv
```
**Time:** O(n log n).

---

## 6. Closest Pair of Points

1. Sort points by x-coordinate.  
2. Recurse on left/right to get δ_left, δ_right; let δ = min(δ_left, δ_right).  
3. Build “strip” of points within δ of median line, sort by y.  
4. For each point in strip, compare to up to 7 next points in y.  
5. Return global minimum distance.

**Time:** O(n log n).

---

## 7. Search in a Sorted Matrix (recap)

- Staircase: O(n).  
- True D&C: O(n^1.585).

---

## 8. Recurrence Analysis – Master Theorem

| Recurrence                         | a | b | log_b(a) | f(n)                 | Case        | T(n)                   |
|------------------------------------|---|---|----------|----------------------|-------------|------------------------|
| a) T = 3T(n/2) + n                 | 3 | 2 | ≈1.585   | n = O(n^(1.585−ε))   | Case 1      | Θ(n^1.585)             |
| b) T = 2T(n/2) + n log n           | 2 | 2 | 1        | n log n = Θ(n log n) | Case 2 (k=1)| Θ(n log^2 n)           |
| c) T = 8T(n/2) + n^2               | 8 | 2 | 3        | n^2 = O(n^(3−ε))     | Case 1      | Θ(n^3)                 |
| d) T = T(n/2) + 1                  | 1 | 2 | 0        | 1 = Θ(1)             | Case 2 (k=0)| Θ(log n)               |
| e) T = 4T(n/2) + n^2 log n         | 4 | 2 | 2        | n^2 log n = Θ(n^2 log n) | Case 2 (k=1) | Θ(n^2 log^2 n)   |

> Note (b): Removing the extra log factor in the combine step yields mergesort: T(n) = 2T(n/2) + n ⇒ O(n log n).
