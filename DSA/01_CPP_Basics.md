# C++ Basics — Revision Notes
**Source:** C++ Basics in One Shot — Striver's A2Z DSA Course - L1

---

## 1. Why C++ for DSA
- Fast execution, gives control over memory, has STL (built-in data structures + algorithms) — saves implementation time in interviews/contests.

## 2. Structure of a C++ Program
```cpp
#include <bits/stdc++.h>   // includes almost all standard libraries
using namespace std;       // avoids writing std:: everywhere

int main() {
    // code
    return 0;
}
```
- `main()` is the entry point — execution always starts here.
- `return 0;` tells the OS the program ended successfully.

## 3. Input / Output
- `cout << "text";` → prints output
- `cin >> variable;` → takes input
- `endl` or `"\n"` → moves to next line (`\n` is generally faster since it doesn't flush the buffer)

## 4. Data Types
| Type | Use | Approx Size |
|---|---|---|
| `int` | whole numbers | 4 bytes |
| `long long` | very large whole numbers | 8 bytes |
| `float` | decimal numbers | 4 bytes |
| `double` | decimal numbers, more precision | 8 bytes |
| `char` | single character | 1 byte |
| `bool` | true/false | 1 byte |
| `string` | sequence of characters | variable |

**Key idea:** Use `long long` whenever numbers can exceed ~10^9 (int overflows around 2.1 × 10^9).

## 5. Variables & Constants
- Declared as `dataType variableName = value;`
- `const` keyword → value cannot change after initialization.

## 6. Operators
- **Arithmetic:** `+ - * / %` (`%` = modulo, only works on integers)
- **Relational:** `== != > < >= <=` → return boolean
- **Logical:** `&& || !`
- **Assignment:** `= += -= *= /=`
- **Increment/Decrement:** `++i` (pre) vs `i++` (post) — pre changes value before use, post changes after use.

## 7. Conditional Statements
- `if / else if / else`
- `switch-case` — used when checking one variable against many fixed values; needs `break` to prevent fall-through.

## 8. Loops
| Loop | When to use |
|---|---|
| `for` | when number of iterations is known |
| `while` | when condition-based, iterations unknown |
| `do-while` | when the loop body must run at least once |

- `break` → exits the loop immediately.
- `continue` → skips current iteration, moves to next.

## 9. Functions
```cpp
returnType functionName(parameters) {
    // logic
    return value;
}
```
- **Why use functions:** code reusability, modularity, easier debugging.
- **Pass by Value:** a copy of the variable is passed → changes inside function don't affect original.
- **Pass by Reference (`&`):** the actual variable is passed → changes inside function DO affect original. Used when a function needs to modify the caller's variable (very common in DSA, e.g., modifying an array or swapping values).

## 10. Arrays
- Collection of elements of the same type stored in contiguous memory.
- Zero-indexed: first element at index `0`.
- Fixed size, declared as `int arr[5];`
- Passing an array to a function automatically passes it by reference (as a pointer) — no need for explicit `&`.

## 11. Strings
- `string` is a class in C++ STL representing a sequence of characters.
- Common operations: `.length()`, `.size()`, `s[i]` (indexing), `+` (concatenation), `.substr()`.

## 12. Pointers
- A pointer stores the **memory address** of a variable.
```cpp
int a = 10;
int* p = &a;   // p stores address of a
cout << *p;    // *p dereferences → gives value at that address (10)
```
- `&` → address-of operator
- `*` → dereference operator (used differently in declaration vs usage — be careful)
- Why pointers matter in DSA: foundation for linked lists, trees, dynamic memory.

## 13. References
- An alias (another name) for an existing variable.
```cpp
int a = 10;
int &b = a;   // b is now another name for a
```
- Difference from pointer: a reference must be initialized at declaration and cannot be reassigned to refer to another variable; a pointer can be reassigned and can be null.

## 14. Object-Oriented Basics (Intro)
- **Class:** blueprint for creating objects (defines properties + behavior).
- **Object:** instance of a class.
```cpp
class Student {
public:
    string name;
    int age;
};

Student s1;   // s1 is an object of class Student
s1.name = "Anik";
```
- `public` members are accessible outside the class; `private` members are not (used for encapsulation).

## 15. Time Taken Consideration
- In competitive/interview coding, ~10^8 operations run in roughly 1 second — this rough benchmark helps decide if a brute-force approach will pass time limits (covered in depth in the Time Complexity video).

---
### Quick Recall Checklist
- [ ] Difference between pass by value vs pass by reference
- [ ] Pointer vs reference
- [ ] When to use `long long`
- [ ] `for` vs `while` vs `do-while`
- [ ] Class vs Object basics
