# ✅ In-Depth Solutions – Greedy Algorithms Practice

This document provides highly detailed solutions with formal reasoning, math, and example analysis for greedy algorithms.

---

## 🔹 Basic Level

### 1. ⛽ Refueling Scheduling Problem

**Problem:**  
You are given a list `Time[0..n-1]` of refueling times for `n` cars. You need to determine the order to refuel them so that the **total waiting time** is minimized.

**Greedy Algorithm:**  
Sort the array `Time[]` in non-decreasing order. Refuel cars in that order.

**Why it works:**  
Let `T[i]` be the time of the `i`-th car in the sorted list.  
Total waiting time is:
\[
\text{Total} = \sum_{i=0}^{n-1} \sum_{j=0}^{i} T[j] = T[0](n) + T[1](n-1) + \dots + T[n-1](1)
\]
Thus, assigning smaller times earlier reduces the impact of larger values.

**Proof (Exchange Argument):**  
If we have a pair of adjacent cars `i` and `j` such that `T[i] > T[j]`, swapping them reduces the total wait. Applying such swaps leads to a sorted list — the optimal order.

**Time Complexity:**  
- Sorting: \( O(n \log n) \)  
- Summation: \( O(n) \)

---

### 2. 💰 Coin Change

**Problem:**  
Given denominations \( d_1, d_2, \dots, d_k \) (e.g., {1, 2, 5, 10}) and a target amount \( M \), compute the minimal number of coins to make \( M \).

**Greedy Algorithm:**  
While \( M > 0 \):  
- Pick the largest coin \( d_i \leq M \)  
- Subtract from \( M \), repeat

**Example (Canonical System):**  
For `M = 38` and `{1, 2, 5, 10}`:
- 3 × 10 → 30  
- 1 × 5 → 35  
- 1 × 2 → 37  
- 1 × 1 → 38  
Total: 6 coins

**Counterexample (Non-Canonical System):**  
Coins: {1, 3, 4}, Target: 6  
- Greedy: 4 + 1 + 1 = 3 coins  
- Optimal: 3 + 3 = 2 coins

**Conclusion:**  
Greedy only works when the coin system is canonical.

---

## 🔹 Intermediate Level

### 3. 🧮 Polynomial Coefficient Maximization

**Problem:**  
Given an array `A[0..n-1]` and a number \( X > 1 \), reorder `A` to maximize:
\[
P = A_0 + A_1 X + A_2 X^2 + \dots + A_{n-1} X^{n-1}
\]

**Greedy Algorithm:**  
Sort `A` in ascending order. Assign smallest values to smallest powers.

**Why it works:**  
Each term is weighted by \( X^i \), which increases with `i`. So we want to place larger values of `A[i]` on larger powers of `X`.

**Proof (Exchange Argument):**  
Suppose we assign \( a > b \) to exponents \( i < j \), respectively.  
Swapping gives:
\[
\Delta = bX^i + aX^j - (aX^i + bX^j) = (b-a)(X^i - X^j)
\]
Since \( b < a \) and \( X^i < X^j \), we have \( \Delta > 0 \), so the swap improves the result.

**Time Complexity:**  
- Sorting: \( O(n \log n) \)

---

### 4. 🛍️ Product Pairing Problem

**Problem:**  
Given a list of `n` prices, pair them into `n/2` pairs so that the **sum of all pair totals** is minimized.

**Greedy Algorithm:**  
Sort the list. Pair the smallest with the largest, second smallest with second largest, etc.

**Why it works:**  
Pairing extremes “balances” large and small values, reducing overall contribution to the total.

**Mathematical Justification:**  
If we denote the sorted list as \( p_1 \leq p_2 \leq \dots \leq p_n \),  
then optimal pairing is:  
\[
(p_1, p_n),\ (p_2, p_{n-1}),\ \dots
\]

**Time Complexity:**  
- Sorting: \( O(n \log n) \)

---

## 🔹 Advanced Level

### 5. 🚓 Police and Thieves

**Problem:**  
You are given an array of `P` (police) and `T` (thieves). A police can catch a thief if they are within distance `K`.

**Greedy Algorithm:**  
Use two pointers:
- One pointer for police, one for thieves
- Match when they are within range
- Advance both pointers accordingly

**Correctness:**  
We always make the earliest possible valid match. Matching any later would risk losing a thief.

**Time Complexity:**  
- One pass over array: \( O(n) \)

---

### 6. 🎒 Fractional Knapsack

**Problem:**  
Given items with value \( v_i \) and weight \( w_i \), and capacity \( C \), maximize total value using fractional items.

**Greedy Algorithm:**  
1. Compute value density: \( \rho_i = \frac{v_i}{w_i} \)  
2. Sort items by \( \rho_i \) in decreasing order  
3. Take as much of each item as fits

**Proof of Optimality:**  
Greedy works because:
- Taking highest \( \rho \) first guarantees locally optimal gain
- There is no dependence between item choices (due to fractional selection)

**Time Complexity:**  
- Sorting: \( O(n \log n) \)

---

### 7. 📡 Huffman Coding

**Problem:**  
Given characters with frequencies, produce prefix-free codes that minimize average code length.

**Greedy Algorithm (Huffman):**
1. Use a min-heap to combine two smallest nodes  
2. Create a new node with combined frequency  
3. Repeat until a single tree remains

**Why it works:**  
Always combining the two least frequent items reduces the depth for common characters.

**Proof (Sketch):**  
Induction and exchange argument:
- Any optimal tree can be transformed into Huffman tree with no increase in cost

**Time Complexity:**  
- \( O(n \log n) \) using a priority queue

---
