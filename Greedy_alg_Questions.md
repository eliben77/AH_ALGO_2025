# ✅ Greedy Algorithms Solutions 

## 🔹 Basic Level

### 1. ⛽ Refueling Scheduling Problem

**Problem Description:**  
You are given an array `Time[0..n-1]`, where `Time[i]` is the amount of time it takes to refuel the i-th car.  
You must schedule the cars so that the **total waiting time** is minimized.  
Each car waits for the sum of all cars before it in the schedule.

**Objective:** Minimize total waiting time:  
Total = T[0] * n + T[1] * (n - 1) + ... + T[n-1] * 1

**Greedy Strategy:**  
Sort the array in ascending order and serve the cars in that order.

**Why It Works (Intuition):**  
Shorter refuels should go first, as they create less delay for the rest.  
Placing long refuels earlier causes a cascading penalty on all following cars.

**Formal Proof (Exchange Argument):**  
Suppose two cars i and j are out of order: Time[i] > Time[j] but i < j.  
Swapping them reduces total waiting time.  
Applying this idea repeatedly leads to the optimal (sorted) order.

**Example:**  
Time = [5, 3, 1]  
Sorted = [1, 3, 5]  
Waiting time = 0 + 1 + (1+3) = 0 + 1 + 4 = 5  
Total = 5 + 3 + 1 = 9 (much less than if 5 is first)

**Time Complexity:**  
- Sorting: O(n log n)  
- Computation: O(n)

---

### 2. 💰 Coin Change Problem

**Problem Description:**  
You are given coin denominations and a target amount `M`.  
Find the smallest number of coins that sum to `M`.

**Greedy Strategy:**  
Always take the largest coin ≤ remaining amount.  
Repeat until the sum reaches `M`.

**Why It Works:**  
If the coin system is **canonical**, greedy will always yield an optimal result.  
That is, it works for coin sets like {1, 5, 10, 25}.

**Counterexample:**  
Coins = {1, 3, 4}, M = 6  
- Greedy: 4 + 1 + 1 = 3 coins  
- Optimal: 3 + 3 = 2 coins  
Thus, greedy is not guaranteed to work on arbitrary denominations.

**Conclusion:**  
Use greedy only for specific coin systems (e.g., modern currencies).

**Time Complexity:** O(n), assuming constant number of coin types.

---

## 🔹 Intermediate Level

### 3. 🧮 Polynomial Coefficient Maximization

**Problem Description:**  
Given a list A = [a0, a1, ..., an-1], and a constant X > 1,  
maximize the polynomial:  
P(X) = A0 + A1*X + A2*X^2 + ... + An-1*X^(n-1)

**Goal:** Rearrange A to maximize P(X)

**Greedy Strategy:**  
Sort A in increasing order. Assign the smallest values to the smallest powers of X.

**Why It Works (Core Idea):**  
Larger values of X^i occur for higher i.  
So we want to multiply the largest numbers by the highest powers of X.

**Proof Idea:**  
If a > b and assigned to lower power than b, we miss the opportunity to "amplify" a.  
Swapping improves total value.

**Example:**  
A = [1, 3, 5], X = 10  
→ Assign: 1*X^0 + 3*X^1 + 5*X^2 = 1 + 30 + 500 = 531

**Time Complexity:**  
- Sorting: O(n log n)

---

### 4. 🛍️ Product Pairing (Minimize Cost)

**Problem Description:**  
Given prices for N items, pair them into N/2 pairs to minimize the total cost of all pairs  
(i.e., sum of each pair's values).

**Greedy Strategy:**  
Sort prices and pair the smallest with the largest.

**Why It Works (Insight):**  
This pairing minimizes the impact of the extremes.  
Balancing high and low values avoids pairing two large numbers together.

**Example:**  
Prices = [1, 3, 6, 10]  
- Pairs: (1,10), (3,6) → total = 11 + 9 = 20  
- Bad: (1,3), (6,10) → total = 4 + 16 = 20 (equal here, but not always)

**Time Complexity:** O(n log n)

---

## 🔹 Advanced Level

### 5. 🚓 Police and Thieves

**Problem Description:**  
You are given an array with characters 'P' and 'T' for police and thief.  
A police can catch a thief if they are at most K positions away.

**Greedy Strategy:**  
- Use two queues (or indices): one for police, one for thieves.  
- Always match the leftmost valid pair (within range K).  
- Once matched, move both pointers forward.

**Why It Works:**  
This strategy ensures the earliest available police always gets the earliest catchable thief.

**Time Complexity:** O(n)

---

### 6. 🎒 Fractional Knapsack

**Problem Description:**  
You are given items with (value, weight) and a knapsack with capacity C.  
You can take fractions of items. Maximize total value in the knapsack.

**Greedy Strategy:**  
- Compute value/weight for each item.  
- Sort items by value/weight descending.  
- Take whole items until you cannot, then take a fraction of the next.

**Why It Works:**  
Taking the best value-per-weight first is always optimal when fractions are allowed.

**Proof Sketch:**  
Because you can split items, there's no dependency between choices.

**Example:**  
Item1: v=60, w=10 → v/w = 6  
Item2: v=100, w=20 → v/w = 5  
→ Take Item1 fully, then as much of Item2 as fits.

**Time Complexity:** O(n log n)

---

### 7. 📡 Huffman Coding

**Problem Description:**  
Given character frequencies, build a prefix-free binary encoding minimizing the total encoded length.

**Greedy Strategy:**  
- Use a min-heap (priority queue).  
- Repeatedly remove two smallest frequencies and merge into one node.  
- Push merged frequency back into heap. Repeat.

**Why It Works:**  
Characters with smaller frequencies should have longer codes, and vice versa.  
The greedy method ensures this.

**Proof (Sketch):**  
The optimality of Huffman coding is proven using a "greedy choice" and "optimal substructure" argument.

**Time Complexity:** O(n log n)

---
