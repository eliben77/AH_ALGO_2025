
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
