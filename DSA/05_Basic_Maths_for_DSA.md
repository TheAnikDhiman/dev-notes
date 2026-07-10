# Basic Maths for DSA — Revision Notes
**Source:** Basic Maths for DSA | Euclidean Algorithm — Striver's A2Z DSA Course

---

## 1. Why Maths Matters in DSA
- A surprising number of DSA problems (especially in arrays, number theory, and pattern questions) reduce to simple math tricks — knowing these speeds up problem-solving significantly.

## 2. Counting Digits in a Number
- Approach 1: keep dividing by 10 until number becomes 0, count iterations.
- Approach 2 (O(1) trick): `digits = floor(log10(n)) + 1`
- Complexity: O(log₁₀ n) for the loop approach — since number of digits is proportional to log of the number.

## 3. Reversing a Number
- Extract last digit using `n % 10`, build reversed number, then remove last digit using `n / 10`, repeat until `n` becomes 0.
- Watch for overflow when reversing large numbers → use `long long`.

## 4. Checking Palindrome Number
- Reverse the number and compare with the original — if equal, it's a palindrome.

## 5. GCD / HCF (Greatest Common Divisor / Highest Common Factor)
- The largest number that divides both given numbers exactly.

### Brute Force
- Loop from `min(a,b)` down to 1, return first number that divides both → O(min(a,b)) time.

### Euclidean Algorithm (Optimized) — the key topic
**Core idea:** `gcd(a, b) = gcd(b, a % b)`, and `gcd(a, 0) = a`.

```cpp
int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}
```
- **Why it works:** any number that divides both `a` and `b` also divides `(a % b)` — so the problem shrinks each step without losing the answer.
- **Time Complexity:** O(log(min(a, b))) — much faster than brute force, because the remainder shrinks roughly exponentially (related to Fibonacci-like decay in worst case).
- Can be written iteratively too (avoids recursion stack usage):
```cpp
int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

## 6. LCM (Least Common Multiple)
- Smallest number that is a multiple of both `a` and `b`.
- **Formula (uses GCD):**
  `LCM(a, b) = (a * b) / GCD(a, b)`
- This is why GCD is computed first — LCM directly depends on it, avoids brute-force multiple checking.
- Watch for overflow: compute as `(a / gcd(a,b)) * b` to reduce risk of overflow with large numbers.

## 7. Checking Prime Numbers
- A number `n` is prime if it has exactly two divisors: 1 and itself.
- **Brute force:** check divisibility from 2 to `n-1` → O(n)
- **Optimized:** only check up to `√n`, because divisors always come in pairs `(i, n/i)` — if no divisor found till √n, none exists beyond it either → O(√n)

## 8. Finding All Divisors of a Number
- Loop `i` from 1 to `√n`; if `i` divides `n`, then both `i` and `n/i` are divisors.
- Time Complexity: O(√n) instead of O(n).

## 9. Overall Pattern Across These Problems
- A recurring theme: **whenever you're checking factors/divisors, looping only till √n instead of n is the standard optimization.**
- **Euclidean Algorithm** is the standout technique here — remember it cold, since GCD shows up as a sub-routine in many other problems (simplifying fractions, LCM, array GCD problems, etc.)

---
### Quick Recall Checklist
- [ ] Euclidean algorithm: `gcd(a,b) = gcd(b, a%b)`, base case `b == 0 → return a`
- [ ] GCD time complexity: O(log(min(a,b)))
- [ ] LCM formula: `(a*b)/gcd(a,b)`
- [ ] Prime check optimization: loop till √n, not n
- [ ] Divisors also found in O(√n) using the pair trick
