# Time and Space Complexity — Revision Notes
**Source:** Time and Space Complexity — Striver's A2Z DSA Course

---

## 1. Why Complexity Analysis Matters
- Tells us **how an algorithm's runtime/memory grows as input size (n) grows**, independent of the machine it runs on.
- Helps compare two different approaches to the same problem without actually running the code.

## 2. Time Complexity
- Not the *actual* time in seconds — it's a measure of **number of operations** relative to input size `n`.
- Expressed using **Big-O notation** → describes the **worst-case** growth rate.

### Common Complexities (fastest → slowest)
| Notation | Name | Example |
|---|---|---|
| O(1) | Constant | accessing arr[i] |
| O(log n) | Logarithmic | binary search |
| O(n) | Linear | single loop over n elements |
| O(n log n) | Linearithmic | merge sort, quick sort |
| O(n²) | Quadratic | nested loop (two levels) |
| O(n³) | Cubic | triple nested loop |
| O(2ⁿ) | Exponential | recursive subsets/subsequences |
| O(n!) | Factorial | permutations |

## 3. How to Calculate Time Complexity
- **Single loop** running `n` times → O(n)
- **Nested loop** (both running `n` times) → O(n × n) = O(n²)
- **Loop that halves input each time** (e.g., `i = i * 2` or binary search) → O(log n)
- **Sequential (not nested) blocks of code** → add complexities: O(n) + O(n) = O(n) (constants dropped)
- **Nested but independent loops** (one after another, not inside each other) → still additive, not multiplicative.

### Rules of Thumb
1. Drop constants: O(2n) → O(n)
2. Drop lower-order terms: O(n² + n) → O(n²)
3. Only the dominant term matters as n → ∞

## 4. Best, Average, Worst Case
- **Best Case:** minimum time taken (e.g., linear search finds element at index 0) — rarely the focus.
- **Worst Case:** maximum time taken — this is what Big-O usually represents, and is the most important for interviews since it guarantees an upper bound.
- **Average Case:** expected time over all possible inputs.

## 5. Other Notations (good to know)
- **Big-Ω (Omega):** best-case lower bound.
- **Big-Θ (Theta):** tight bound — when best and worst case growth are the same.
- In practice, interviews mostly care about Big-O (worst case).

## 6. Space Complexity
- Measures **extra memory** used by the algorithm relative to input size (excluding input itself, usually).
- **Auxiliary Space:** extra space used other than the input — this is usually what's actually meant when people say "space complexity" in interviews.
- Examples:
  - Using a few variables → O(1) space
  - Creating a new array of size n → O(n) space
  - Recursive calls → each call adds a frame to the call stack → recursion depth = space used (e.g., recursion going n levels deep → O(n) space)

## 7. Recursion & Complexity (quick note)
- Time complexity of recursion = (number of recursive calls) × (work done per call)
- Space complexity of recursion = depth of the recursion tree (call stack usage)

## 8. Estimating Feasibility from Constraints
A useful trick: given time limit ~1 second, the machine executes roughly **10^8 operations/second**.

| n (input size) | Acceptable Complexity |
|---|---|
| ≤ 10 | O(n!), O(2ⁿ) |
| ≤ 20 | O(2ⁿ) |
| ≤ 500 | O(n³) |
| ≤ 10⁴ | O(n²) |
| ≤ 10⁶ | O(n log n) |
| ≤ 10⁸ | O(n) |
| very large n | O(log n), O(1) |

This lets you predict which approach (brute force vs optimized) will pass, just by looking at the constraints in a problem before writing code.

---
### Quick Recall Checklist
- [ ] Big-O vs Big-Ω vs Big-Θ
- [ ] How nested loops translate to O(n²)
- [ ] Why we drop constants and lower-order terms
- [ ] Auxiliary space vs total space
- [ ] Constraint → complexity estimation table
