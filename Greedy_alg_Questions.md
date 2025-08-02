# ✅ In-Depth Solutions – Greedy Algorithms Practice

This document provides highly detailed solutions with formal reasoning and mathematical justification for greedy algorithms.

---

## 🔹 Basic Level

### 1. ⛽ Refueling Scheduling Problem

**Problem:**  
You are given a list `Time[0..n-1]` of refueling times for `n` cars. Determine the order to refuel them so that the **total waiting time** is minimized.

**Greedy Algorithm:**  
Sort the array `Time[]` in non-decreasing order. Refuel cars in that order.

**Why it works:**  
Let `T[i]` be the time of the `i`-th car in the sorted list.  
The total waiting time is:  

\[
\text{Total} = \sum_{i=0}^{n-1} \sum_{j=0}^{i} T[j] = T[0](n) + T[1](n-1) + \dots + T[n-1](1)
\]

**Proof (Exchange Argument):**  
If two cars `i` and `j` are in the wrong order (`T[i] > T[j]` but `i < j`), swapping them reduces the total wait. Applying such swaps leads to the sorted list — the optimal order.

**Time Complexity:**  
- Sorting: \( O(n \log n) \)  
- Summation: \( O(n) \)

---

### 2. 💰 Coin Change

**Problem:**  
Given denominations \( d_1, d_2, \dots, d_k \) (e.g., {1, 2, 5, 10}) and a target amount \( M \), compute the minimum number of coins.

**Greedy Algorithm:**  
While \( M > 0 \):  
- Pick the largest coin \( d_i \leq M \)  
- Subtract from \( M \), repeat

**Counterexample:**  
Coins: \( \{1, 3, 4\} \), Target: 6  
- Greedy: 4 + 1 + 1 = 3 coins  
- Optimal: 3 + 3 = 2 coins

**Conclusion:**  
Greedy works for **canonical systems** but fails otherwise.

---

## 🔹 Intermediate Level

### 3. 🧮 Polynomial Coefficient Maximization

**Problem:**  
Reorder array \( A = [a_0, \dots, a_{n-1}] \) to maximize:

\[
P(X) = A_0 + A_1 X + A_2 X^2 + \dots + A_{n-1} X^{n-1}
\]

for a given \( X > 1 \).

**Greedy Algorithm:**  
Sort \( A \) in increasing order and assign values in that order to increasing powers of \( X \).

**Proof (Exchange Argument):**  
Suppose we have two values \( a > b \) and powers \( i < j \). Then:

\[
\Delta = bX^i + aX^j - (aX^i + bX^j) = (b - a)(X^i - X^j)
\]

Since \( b < a \) and \( X^i < X^j \), we have \( \Delta > 0 \) — swapping improves the result.

---

### 4. 🛍️ Product Pairing

**Problem:**  
Given prices \( p_1, \dots, p_n \), pair them into \( n/2 \) pairs minimizing:

\[
\text{Total} = \sum_{i=1}^{n/2} (a_i + b_i)
\]

**Greedy Algorithm:**  
Sort prices and pair smallest with largest:

\[
(p_1, p_n), (p_2, p_{n-1}), \dots
\]

**Time Complexity:**  
\( O(n \log n) \)

---

## 🔹 Advanced Level

### 5. 🚓 Police and Thieves

**Problem:**  
You are given an array with `'P'` and `'T'`. A police can catch a thief if they are within distance \( K \).

**Greedy Algorithm:**  
Use two pointers or queues to match nearest valid pairs within distance \( K \).

**Time Complexity:**  
\( O(n) \)

---

### 6. 🎒 Fractional Knapsack

**Problem:**  
Given values \( v_i \), weights \( w_i \), and capacity \( C \), maximize:

\[
\text{Value} = \sum_{i} x_i v_i \quad \text{such that } \sum x_i w_i \leq C,\quad 0 \leq x_i \leq 1
\]

**Greedy Algorithm:**  
1. Compute \( v_i / w_i \)  
2. Sort descending  
3. Take full or partial items by order

**Time Complexity:**  
\( O(n \log n) \)

---

### 7. 📡 Huffman Coding

**Problem:**  
Given frequencies, construct prefix-free binary code minimizing:

\[
\sum_{i=1}^n f_i \cdot d_i
\]

where \( d_i \) is the depth (code length) of character \( i \).

**Greedy Algorithm:**  
Use a min-heap to combine smallest frequencies repeatedly.

**Time Complexity:**  
\( O(n \log n) \)

---
