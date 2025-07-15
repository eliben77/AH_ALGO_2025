
# Divide and Conquer – Practice Exercises

This problem set covers a range of classic and advanced problems based on the divide and conquer paradigm.

---

## ✦ Basic Level – Understanding the Idea

### 1. Find Maximum and Minimum
Write an algorithm that returns both the **maximum** and the **minimum** in an array of integers using the divide and conquer approach.  
- What is the number of comparisons in the worst case?

---

### 2. Maximum Subarray (Divide and Conquer)
Given an array of integers (positive and negative), find the contiguous subarray with the **maximum sum** using divide and conquer.  
- Analyze the time complexity of your solution.

---

## ✦ Intermediate Level – Classic Algorithms

### 3. Merge Sort
- Explain how Merge Sort is a divide and conquer algorithm.
- What is its time complexity?
- Write pseudocode for the algorithm.

---

### 4. Binary Search
- Implement binary search using divide and conquer.
- Can you generalize this idea to search in a **2D matrix** where each row and each column is sorted?

---

### 5. Counting Inversions
Given an array, count how many **inversions** it contains  
(i.e., how many pairs `i < j` such that `A[i] > A[j]`).  
- Use a modified merge sort to achieve an efficient solution.

---

## ✦ Advanced Level – Deeper Divide and Conquer Ideas

### 6. Closest Pair of Points
Given `n` points in the 2D plane, find the **pair of points** with the smallest Euclidean distance.  
- Use a divide and conquer strategy to improve the naive O(n²) time.  
- Describe how the merge step works geometrically.

---

### 7. Search in a Sorted Matrix
Given an `n × n` matrix where each row and column is sorted in ascending order,  
- Design an efficient algorithm to find a target value using divide and conquer.

---

## ✦ 8. Recurrence Analysis – Master Theorem

Use the **Master Theorem** to analyze the following recurrence relations:

### a)
```
T(n) = 3T(n/2) + n
```
- Identify values of `a`, `b`, and `f(n)`.
- Which case of the Master Theorem applies?
- What is the asymptotic solution?

### b)
```
T(n) = 2T(n/2) + n log n
```
- Apply the Master Theorem and explain which case applies.
- Can you improve this recurrence using a better algorithm?

### c)
```
T(n) = 8T(n/2) + n^2
```
- Analyze the time complexity.
- How does this relate to the standard divide and conquer matrix multiplication algorithm?

### d)
```
T(n) = T(n/2) + 1
```
- What is the solution using a recursion tree or Master Theorem?

### e)
```
T(n) = 4T(n/2) + n^2 log n
```
- Use the Master Theorem with logarithmic factors.
- Determine the dominant term and which case applies.

---

### 📌 Reminder – Master Theorem Format

Given a recurrence of the form:
```
T(n) = aT(n/b) + f(n)
```
Where:
- `a ≥ 1` and `b > 1`
- `f(n)` is asymptotically positive

We compare `f(n)` to `n^log_b(a)`:

- **Case 1**: If `f(n) = O(n^log_b(a) - ε)` for some `ε > 0`, then  
  ➤ `T(n) = Θ(n^log_b(a))`

- **Case 2**: If `f(n) = Θ(n^log_b(a) · log^k(n))` for some `k ≥ 0`, then  
  ➤ `T(n) = Θ(n^log_b(a) · log^{k+1}(n))`

- **Case 3**: If `f(n) = Ω(n^log_b(a) + ε)` for some `ε > 0` **and**  
  if `a f(n/b) ≤ c f(n)` for some `c < 1` and sufficiently large `n`, then  
  ➤ `T(n) = Θ(f(n))`

---

## ✦ Hints

| Exercise | Hint |
|----------|------|
| 1 | Split the array in two, compute max/min recursively, and merge the results. |
| 2 | The max subarray may be fully in left, right, or cross the middle. |
| 3 | Sort left and right recursively, then merge. |
| 4 | Compare to the middle; eliminate half the range. |
| 5 | During the merge step, count how many right elements precede left elements. |
| 6 | Divide by x-coordinates; use a vertical strip to merge. |
| 7 | Start from the top-right element and move accordingly. |
| 8 | Apply Master Theorem carefully—check `a`, `b`, and `f(n)` in each case. |

---

*Prepared for use in academic courses on algorithms and data structures.*
