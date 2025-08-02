# 🧠 Greedy Algorithms – Practice Questions

This practice set covers classic and theoretical problems using greedy strategies, with a focus on correctness and runtime analysis.

---

## 🔹 Basic Level – Understanding Greedy Strategy

### 1. ⛽ Refueling Scheduling Problem  
You are given `N` cars waiting to be refueled. Each car `i` requires a refueling time `Time[i]`.  
- Describe a greedy algorithm that minimizes the total waiting time.  
- Prove its correctness by showing:
  - The greedy choice property
  - Optimal substructure  
- Analyze the time complexity.

---

### 2. 💰 Coin Change  
You are given coin denominations `{1, 2, 5, 10}` and a total amount `n = 38`.  
- Use a greedy algorithm to compute the minimal number of coins to make change.  
- Provide a counterexample where the greedy solution fails.  
- Explain why greedy does not work for arbitrary coin systems.

---

## 🔹 Intermediate Level – Greedy with Proofs

### 3. 🧮 Polynomial Coefficients Ordering  
Given an array `A` of non-negative integers and a number `X > 1`,  
you must order the elements of `A` to maximize the polynomial:  
\[
A_0 + A_1 X + A_2 X^2 + \dots + A_{n-1} X^{n-1}
\]  
- Propose a greedy algorithm.  
- Prove the greedy choice property and optimal substructure.  
- Analyze the time complexity.

---

### 4. 🛍️ Product Pairing for Discount  
Given `N` product prices, pair the products into `N/2` pairs to minimize the total price of all pairs.  
- Design a greedy algorithm for optimal pairing.  
- Prove why this greedy strategy works.  
- What is the time complexity?

---

## 🔹 Advanced Level – Structural Greedy Problems

### 5. 🚓 Police and Thieves  
You are given an array of length `N`, each element either `'P'` (police) or `'T'` (thief).  
Each police officer can catch at most one thief within distance `K`.  
- Design a greedy algorithm to maximize the number of caught thieves.  
- Prove the correctness of your algorithm.  
- Analyze time and space complexity.

---

### 6. 🎒 Fractional Knapsack  
Given `n` items, each with value `v[i]` and weight `w[i]`, and a knapsack of capacity `C`.  
- Items can be broken into fractions.  
- Design a greedy algorithm that maximizes the total value.  
- Prove that the greedy approach is optimal.  
- What is the time complexity?

---

### 7. 📡 Huffman Coding  
You are given characters and their frequencies.  
- Describe the Huffman coding algorithm.  
- Prove why choosing the two smallest frequencies at each step leads to an optimal prefix code.  
- What is the runtime complexity?

---
