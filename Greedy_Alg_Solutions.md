# ✅ Detailed Solutions – Greedy Algorithms Practice

This document provides in-depth solutions and explanations for each greedy algorithm problem.

---

## 🔹 Basic Level

### 1. ⛽ Refueling Scheduling Problem

**Problem:**  
You are given `N` cars, each requiring a refueling time `Time[i]`. You want to schedule their refueling to minimize the total waiting time.

**Greedy Algorithm:**  
Sort the cars by their refueling times in ascending order and refuel in that order.

**Justification:**  
The total waiting time is minimized if short tasks are scheduled first. This is equivalent to the Shortest Job First (SJF) scheduling algorithm.

**Proof Sketch:**  
- Suppose two cars i and j are scheduled such that Time[i] > Time[j] but i is before j.  
- Swapping them reduces the total waiting time for j and may reduce the total sum.  
- By applying such swaps greedily, we reach an optimal order.

**Time Complexity:**  
- Sorting: `O(n log n)`  
- Final computation: `O(n)`

---

### 2. 💰 Coin Change

**Problem:**  
Given denominations `{1, 2, 5, 10}` and a total amount `n = 38`, compute the minimal number of coins.

**Greedy Algorithm:**  
Always pick the largest denomination ≤ remaining amount.

**Execution:**  
- Use 3 coins of 10 → 30  
- 1 coin of 5 → 35  
- 1 coin of 2 → 37  
- 1 coin of 1 → 38  
→ Total = 6 coins

**When it Fails:**  
For coin denominations `{1, 3, 4}` and amount `6`:
- Greedy picks 4 → 2 left → 1 + 1 = 3 coins  
- But optimal is 3 + 3 = 2 coins

**Conclusion:**  
Greedy works for canonical coin systems (like real currencies), but not for all.

---

## 🔹 Intermediate Level

### 3. 🧮 Polynomial Coefficients Ordering

**Problem:**  
You want to maximize the polynomial  
\[
A_0 + A_1 X + A_2 X^2 + \dots + A_{n-1} X^{n-1}
\]  
by reordering `A`.

**Greedy Algorithm:**  
Sort `A` in increasing order. Assign smallest to the smallest power.

**Why it works:**  
Higher powers of `X` grow faster. To maximize the sum, assign larger values of `A` to higher powers.

**Proof:**  
Exchange Argument:  
If a smaller value is assigned to a higher power than a larger one, swapping them increases the result.

**Time Complexity:**  
- Sorting: `O(n log n)`  
- Evaluation: `O(n)`

---

### 4. 🛍️ Product Pairing for Discount

**Problem:**  
Given `N` prices, pair them to minimize the total cost of all pairs.

**Greedy Algorithm:**  
Sort prices. Pair smallest with largest, second smallest with second largest, and so on.

**Why this works:**  
Balancing large and small values reduces extremes in pair sums.

**Formal Insight:**  
This strategy minimizes the L1 norm of pair differences (similar to minimizing squared error).

**Time Complexity:**  
- Sorting: `O(n log n)`

---

## 🔹 Advanced Level

### 5. 🚓 Police and Thieves

**Problem:**  
Array of `'P'` and `'T'`. A police can catch a thief within distance `K`.

**Greedy Algorithm:**  
- Use two lists/queues: one for police, one for thieves.  
- Traverse array, when a police and thief are within distance `K`, match them.

**Why Greedy Works:**  
We always make the earliest valid match, ensuring no opportunity is wasted.

**Time Complexity:**  
`O(n)`, since each character is visited at most once.

**Implementation Detail:**  
You can also use two pointers instead of queues.

---

### 6. 🎒 Fractional Knapsack

**Problem:**  
Maximize value of items placed into a knapsack of capacity `C`. Fractional items allowed.

**Greedy Algorithm:**  
- Compute `value/weight` ratio for each item.  
- Sort by descending ratio.  
- Take full items until capacity, then take fraction of next item.

**Why it works:**  
Taking the item with highest value per unit weight gives most efficient usage of space.

**Proof of Optimality:**  
This problem satisfies:
- Greedy choice property  
- Optimal substructure  
Because items can be split, there's no need to consider future consequences.

**Time Complexity:**  
- Sorting: `O(n log n)`  
- Filling: `O(n)`

---

### 7. 📡 Huffman Coding

**Problem:**  
Given frequencies of characters, produce a prefix-free binary code with minimum average length.

**Greedy Algorithm:**  
- Use a min-heap to repeatedly combine the two smallest frequencies into a new node.  
- Repeat until one tree remains.

**Correctness:**  
- Always combine least frequent symbols to minimize deep tree nodes.  
- Proven using exchange argument (any optimal tree can be transformed into Huffman tree without increasing cost).

**Time Complexity:**  
`O(n log n)` using a priority queue.

**Example:**  
Frequencies: a:5, b:9, c:12, d:13, e:16, f:45  
Huffman tree yields total cost = 224 bits (minimum possible).

---
