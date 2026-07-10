# Solve Any Pattern Question — Revision Notes
**Source:** Solve any Pattern Question - Trick Explained | 22 Patterns in 1 Shot — Striver's A2Z DSA Course

---

## 1. The Core Mindset
- Pattern questions look intimidating but almost all of them follow the **same 3-step framework**:
  1. **Figure out the number of rows** → controlled by an outer loop.
  2. **For each row, figure out the number of columns** (how many things to print) → controlled by an inner loop.
  3. **Figure out what to actually print** in each column (star, number, space, character) → this is the only thing that changes between patterns.
- Once you internalize this, patterns stop being "22 different problems" and become "1 problem with different inner logic."

## 2. The Template
```cpp
for (int i = 1; i <= n; i++) {          // outer loop → rows
    for (int j = 1; j <= <count>; j++) { // inner loop → columns/prints per row
        cout << <what to print>;
    }
    cout << "\n";                        // move to next row
}
```

## 3. How to Find "Number of Columns" for a Row
- Look at the pattern and ask: *as row number `i` increases, does the count of printed characters increase, decrease, or stay the same?*
- Common relationships:
  - Count = `i` (increases with row) → simple triangle
  - Count = `n` (fixed) → square/rectangle
  - Count = `n - i + 1` (decreases with row) → inverted triangle
  - Count = `2*i - 1` (odd numbers) → diamond/pyramid shapes

## 4. Handling Spaces (for pyramid/diamond patterns)
- Many patterns need **leading spaces** before the actual characters (to center the shape).
- Two ways to handle this:
  1. Use a **separate inner loop** just for spaces, then another inner loop for the actual characters.
  2. Print based on condition inside a single loop (whether current column index falls in "space zone" or "character zone").
- Trick: for a pyramid of height `n`, row `i` typically has `(n - i)` spaces before the stars.

## 5. Handling Numbers/Characters Instead of Stars
- Same triangle/pyramid skeleton — just change what's printed:
  - Print `j` instead of `*` → prints column number
  - Print `i` instead of `*` → prints row number (repeated across the row)
  - Print `(char)('A' + j - 1)` → prints letters instead of numbers
- **Core takeaway:** the loop *structure* (rows × columns) rarely changes — only the print statement changes.

## 6. Symmetric / Diamond Patterns
- Usually built by **combining two patterns you already know**:
  - Upper half = increasing triangle
  - Lower half = decreasing triangle (mirror of the upper half)
- Break complex-looking patterns into halves or quadrants and solve each with the basic triangle logic.

## 7. General Debugging Approach When Stuck
1. Write down the pattern for a small `n` (like n = 4) on paper.
2. For each row, count: how many spaces, how many characters.
3. Make a small table: row number → space count → character count.
4. Find the mathematical relationship (usually linear in terms of `i` or `n`).
5. Convert that relationship directly into loop bounds.

## 8. Common Complexity of Pattern Problems
- Almost all pattern problems are **O(n²)** time (since it's rows × columns nested loops) and **O(1)** auxiliary space (no extra data structure used, only printing).

---
### Quick Recall Checklist
- [ ] 3-step framework: rows → columns → what to print
- [ ] Formulas for column count: `i`, `n`, `n-i+1`, `2i-1`
- [ ] Space-loop vs character-loop separation for pyramids
- [ ] Diamond = mirror of two triangles
- [ ] Time complexity of pattern problems is generally O(n²)
