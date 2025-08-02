# ✅ Detailed Solutions – Greedy Algorithms Practice

This document provides comprehensive solutions and justifications for greedy algorithm problems.

---

## 🔹 Basic Level

### 1. ⛽ Refueling Scheduling Problem

**Problem:**  
You are given `N` cars, each requiring a refueling time `Time[i]`. Schedule them to minimize the **total waiting time** of all cars.

**Greedy Algorithm:**  
Sort cars by increasing refueling time and serve them in that order.

**Why it works:**  
- If shorter refueling tasks are served first, then all later cars wait less.
- This minimizes the **sum of completion times** (classic scheduling problem).

**Proof (Exchange Argument):**  
If two cars `i` and `j` are in the wrong order (`Time[i] > Time[j]` but `i` is scheduled before `j`), swapping them reduces the total waiting time.

**Time Complexity:**  
- Sorting: `O(n log n)`  
- Computing total wait time: `O(n)`

---

### 2. 💰 Coin Change

**Problem:**  
Given coin denominations (e.g., `{1, 2, 5, 10}`) and an amount `n = 38`, compute the minimum number of coins.

**Greedy Algorithm:**  
Use as many coins as possible of the highest denomination ≤ remaining amount.

**Execution for 38:**  
- Take 3 × 10 → 30  
- Take 1 × 5 → 35  
- Take 1 × 2 → 37  
- Take 1 × 1 → 38  
→ Total: 6 coins

**When Greedy Fails:**  
For coins `{1, 3, 4}`, target `6`:  
- Greedy → 4 + 1 + 1 = 3 coins  
- Optimal → 3 + 3 = 2 coins

**Conclusion:**  
Greedy is optimal **only** if the coin system is *canonical* (like real-world currencies).

---

## 🔹 Intermediate Level

### 3. 🧮 Polynomial Coefficients Ordering

**Problem:**  
Maximize:
\[
P(A, X) = A_0 + A_1 X + A_2 X^2 + \dots + A_{n-1} X^{n-1}
\]
by reordering array `A`.

**Greedy Algorithm:**  
Sort `A` in increasing order. Assign smallest value to `A_0`, next to `A_1`, etc.

**Why it works:**  
Since `X > 1`, higher exponents amplify values more. Assign large values to large powers.

**Proof (Exchange Argument):**  
Swapping a smaller `a` with a larger `b` in higher powers increases total sum:
\[
(b - a)(X^j - X^i) > 0 \quad 	ext{if } j > i, b > a
\]

**Time Complexity:**  
- Sorting: `O(n log n)`  
- Evaluation: `O(n)`

---

### 4. 🛍️ Product Pairing for Discount

**Problem:**  
Given `N` product prices, pair them into `N/2` pairs to minimize the **total sum** of all pairwise totals.

**Greedy Algorithm:**  
Sort the prices. Pair smallest with largest, second smallest with second largest, etc.

**Why it works:**  
This “balanced” pairing minimizes extremes and reduces total cost.

**Proof Idea:**  
This minimizes the sum of product sums through symmetric pairing. Similar to minimizing variance.

**Time Complexity:**  
- Sorting: `O(n log n)`

---

## 🔹 Advanced Level

### 5. 🚓 Police and Thieves

**Problem:**  
Array with `'P'` and `'T'`. Each police can catch one thief within `K` distance.

**Greedy Algorithm:**  
Use two queues or pointers:
- When a `'P'` and `'T'` are within distance `K`, match them.
- Advance both pointers.

**Why it works:**  
You always catch the earliest available thief, ensuring no opportunity is missed.

**Time Complexity:**  
- One pass: `O(n)`

---

### 6. 🎒 Fractional Knapsack

**Problem:**  
You can take fractions of `n` items with value `v[i]` and weight `w[i]`, and total capacity `C`. Maximize total value.

**Greedy Algorithm:**  
- Compute value/weight ratio `v[i]/w[i]`.  
- Sort items by descending ratio.  
- Take full items until capacity runs out, then take a fraction.

**Why it works:**  
You always take the most value-efficient item first.

**Proof of Optimality:**  
The greedy choice guarantees the best possible value at every step.

**Time Complexity:**  
- Sorting: `O(n log n)`  
- Greedy fill: `O(n)`

---

### 7. 📡 Huffman Coding

**Problem:**  
Given character frequencies, generate a prefix-free binary code minimizing the total encoded length.

**Greedy Algorithm:**  
- Use a min-heap (priority queue).  
- At each step, merge the two smallest frequencies into one node.  
- Repeat until a single tree is formed.

**Why it works:**  
The most frequent symbols get shorter codes. The two least frequent are placed deepest.

**Proof Sketch:**  
Induction and exchange argument show that combining the two smallest always leads to an optimal tree.

**Time Complexity:**  
- Heap operations: `O(n log n)`

**Example:**  
Frequencies: a:5, b:9, c:12, d:13, e:16, f:45  
→ Optimal total bits = 224

---
