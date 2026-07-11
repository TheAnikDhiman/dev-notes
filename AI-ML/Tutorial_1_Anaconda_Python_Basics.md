# Tutorial 1 — Anaconda Installation & Python Basics

## 1. What is Anaconda?

> **Definition:** Anaconda is a free, open-source distribution of Python (and R) that bundles the interpreter with 250+ pre-installed data science/ML packages (NumPy, Pandas, Scikit-learn, Jupyter, etc.) along with a package/environment manager called **conda**.

**Why use it for ML?**
- No manual installation of individual libraries.
- Comes with **Jupyter Notebook**, **Spyder**, **Anaconda Navigator** (GUI).
- Handles dependency conflicts between projects via environments.

**Install:** Download from anaconda.com → run installer → verify with:
```bash
conda --version
python --version
```

---

## 2. Conda Environments

> **Definition:** A *virtual environment* is an isolated Python setup with its own interpreter and packages, so different projects can use different (even conflicting) library versions without interfering with each other.

```bash
# create a new environment named 'ml' with Python 3.10
conda create -n ml python=3.10

# activate it
conda activate ml

# list all environments
conda env list

# install a package inside the active env
conda install numpy pandas

# deactivate
conda deactivate

# remove an environment
conda remove -n ml --all
```

---

## 3. Jupyter Notebook

> **Definition:** Jupyter Notebook is a web-based interactive environment that lets you write and execute code in individual **cells**, mixing code, output, text (Markdown), and visualizations in a single document (`.ipynb`).

```bash
# launch from terminal (inside an activated env)
jupyter notebook
```
- `Shift + Enter` → run cell and move to next
- `Ctrl + Enter` → run cell in place
- Cell modes: **Code** vs **Markdown**
- Kernel = the running Python process behind the notebook (Kernel → Restart to clear all variables).

---

## 4. Python Basics — Variables & Data Types

> **Definition:** A *variable* is a name bound to a value stored in memory; Python is **dynamically typed**, so you don't declare a type — it's inferred at runtime.

```python
x = 10          # int
y = 3.14        # float
name = "Krish"  # str
is_valid = True # bool
z = 2 + 3j      # complex

print(type(x))     # <class 'int'>
```

**Core built-in data types:** `int`, `float`, `complex`, `str`, `bool`, `list`, `tuple`, `set`, `dict`, `NoneType`.

### Type Casting
```python
a = "25"
b = int(a)      # str -> int
c = float(b)    # int -> float
d = str(c)      # float -> str
```

---

## 5. Operators

| Category | Operators | Example |
|---|---|---|
| Arithmetic | `+ - * / // % **` | `7 // 2 = 3`, `7 % 2 = 1` |
| Comparison | `== != > < >= <=` | `5 > 3 -> True` |
| Logical | `and or not` | `True and False -> False` |
| Assignment | `= += -= *= /=` | `x += 1` |
| Membership | `in`, `not in` | `'a' in 'cat' -> True` |
| Identity | `is`, `is not` | `a is b` |

```python
a, b = 10, 3
print(a / b)   # 3.333... (true division)
print(a // b)  # 3 (floor division)
print(a % b)   # 1 (modulus/remainder)
print(a ** b)  # 1000 (exponent)
```

---

## 6. Input/Output & Comments

> **Definition:** `print()` writes output to the console; `input()` reads a line of text from the user as a **string** (must be cast if a number is needed).

```python
name = input("Enter your name: ")   # always returns str
age = int(input("Enter your age: "))
print(f"{name} is {age} years old")  # f-string formatting

# single line comment
"""
multi-line comment /
docstring
"""
```

---

## 7. Indentation & Code Blocks

> **Definition:** Python uses **whitespace indentation** (not braces `{}`) to define code blocks — consistent indentation (commonly 4 spaces) is mandatory and syntactically significant.

```python
if 5 > 3:
    print("5 is greater")   # this indented block belongs to the if
else:
    print("not greater")
```

---

## 8. Conditional Statements

```python
marks = 78

if marks >= 90:
    grade = "A"
elif marks >= 75:
    grade = "B"
elif marks >= 50:
    grade = "C"
else:
    grade = "F"

print(grade)  # B
```

---

## 9. Loops

> **Definition:** A **loop** repeats a block of code until a condition is met — `for` iterates over a known sequence, `while` repeats while a condition remains true.

```python
# for loop
for i in range(5):        # 0,1,2,3,4
    print(i)

# while loop
n = 0
while n < 5:
    print(n)
    n += 1

# loop control
for i in range(10):
    if i == 3:
        continue   # skip this iteration
    if i == 7:
        break      # exit loop entirely
    print(i)
```

---

## Quick Revision Summary
- Anaconda = Python distribution + conda package/env manager, bundles Jupyter and ML libraries.
- `conda create/activate/install` manage isolated environments.
- Jupyter Notebook = cell-based interactive coding (code + markdown + output).
- Python is dynamically typed; use `type()` to check, cast with `int()/float()/str()`.
- Indentation defines blocks — no curly braces.
- `for` for known iterations, `while` for condition-based repetition; `break`/`continue` control flow.
