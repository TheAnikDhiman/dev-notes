# Machine Learning — Python Fundamentals
### By Krish Naik | ML Playlist — Tutorial 2, 3 & 4
---

## Table of Contents
1. [Tutorial 2 — Lists & Boolean Variables](#tutorial-2--lists--boolean-variables)
2. [Tutorial 3 — Sets, Dictionaries & Tuples](#tutorial-3--sets-dictionaries--tuples)
3. [Tutorial 4 — NumPy & Inbuilt Functions](#tutorial-4--numpy--inbuilt-functions)

---

# Tutorial 2 — Lists & Boolean Variables

---

## Boolean Variables

A Boolean is the simplest data type in Python — it holds only one of two values: `True` or `False`.

- Named after mathematician George Boole
- Booleans are the foundation of all decision-making in code
- In Python, `True` and `False` are case-sensitive — capital T and F always
- Under the hood, `True = 1` and `False = 0` (they are subclasses of `int`)
- Any value in Python can be evaluated as a Boolean — this is called **truthiness**

**Truthy vs Falsy:**

| Falsy (evaluates to False) | Truthy (evaluates to True) |
|---------------------------|---------------------------|
| `0`, `0.0` | Any non-zero number |
| `""` (empty string) | Any non-empty string |
| `[]` (empty list) | Any non-empty list |
| `{}` (empty dict/set) | Any non-empty dict/set |
| `None` | Any object |
| `False` | `True` |

> Why does this matter in ML? You'll constantly check: "is this list empty?", "did this operation return anything?", "is this value zero?" — Python's truthiness makes these checks clean.

---

## Comparison Operators (Return Booleans)

Comparison operators compare two values and always return `True` or `False`.

| Operator | Meaning | Example |
|----------|---------|---------|
| `==` | Equal to | `5 == 5` → True |
| `!=` | Not equal to | `5 != 3` → True |
| `>` | Greater than | `7 > 3` → True |
| `<` | Less than | `3 < 7` → True |
| `>=` | Greater or equal | `5 >= 5` → True |
| `<=` | Less or equal | `4 <= 5` → True |

- `=` is assignment (set a value). `==` is comparison (check if equal). Don't confuse them.

---

## Logical Operators

Logical operators combine multiple boolean expressions.

| Operator | Meaning | Returns True when... |
|----------|---------|----------------------|
| `and` | Both must be True | Left AND Right are both True |
| `or` | At least one True | Left OR Right (or both) is True |
| `not` | Flips the value | The original value is False |

**Short-circuit evaluation:**
- `and` → if the LEFT side is False, Python doesn't even check the right side (already False)
- `or` → if the LEFT side is True, Python doesn't even check the right side (already True)
- This is a performance optimization Python does automatically

---

## What is a List?

A List is Python's most versatile data structure — an **ordered, mutable collection** that can hold any mix of data types.

- **Ordered** → items have a fixed position (index). The order is preserved.
- **Mutable** → you can change, add, or remove items after creation
- **Allows duplicates** → same value can appear multiple times
- **Heterogeneous** → can mix strings, numbers, booleans, even other lists
- Created with square brackets `[]`

> In ML, lists are everywhere — storing feature values, labels, file paths, column names. Understanding them deeply is foundational.

---

## Indexing

Every item in a list has a position number called an **index**.

- Indexing starts at **0**, not 1 (zero-based indexing)
- First item → index 0. Second → index 1. Last item → index `len(list) - 1`
- **Negative indexing** → count from the end. `-1` is the last item, `-2` is second last, and so on.

```python
fruits = ['apple', 'banana', 'cherry', 'date']
fruits[0]    # 'apple'  — first item
fruits[2]    # 'cherry' — third item
fruits[-1]   # 'date'   — last item
fruits[-2]   # 'cherry' — second from last
```

> Negative indexing is very useful — `list[-1]` gives the last element without needing to know the list's length.

---

## Slicing

Slicing extracts a portion (sub-list) from a list. It returns a NEW list.

**Syntax:** `list[start : stop : step]`
- `start` → index to start from (included). Default: 0
- `stop` → index to stop at (NOT included). Default: end of list
- `step` → how many positions to jump. Default: 1

```python
nums = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

nums[2:6]     # [2, 3, 4, 5]   — index 2 to 5 (6 not included)
nums[:4]      # [0, 1, 2, 3]   — from start to index 3
nums[5:]      # [5, 6, 7, 8, 9] — from index 5 to end
nums[::2]     # [0, 2, 4, 6, 8] — every 2nd item
nums[::-1]    # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0] — reversed!
```

> `list[::-1]` to reverse a list is one of Python's most used tricks. Memorise it.

---

## List Methods

Methods are actions you can perform on a list. They modify the list in-place (no new list created).

| Method | What it does |
|--------|-------------|
| `append(x)` | Adds item `x` to the END of the list |
| `insert(i, x)` | Inserts `x` at index `i`, shifts rest right |
| `extend([x, y])` | Appends multiple items from another iterable |
| `remove(x)` | Removes FIRST occurrence of value `x` |
| `pop(i)` | Removes AND returns item at index `i` (default: last) |
| `sort()` | Sorts list in-place (ascending by default) |
| `reverse()` | Reverses the list in-place |
| `index(x)` | Returns index of first occurrence of `x` |
| `count(x)` | Counts how many times `x` appears |
| `clear()` | Removes all items — empty list remains |
| `copy()` | Returns a shallow copy of the list |

**append vs extend — Common confusion:**
- `append([4, 5])` → adds the WHOLE list as ONE item: `[1, 2, 3, [4, 5]]`
- `extend([4, 5])` → adds each item individually: `[1, 2, 3, 4, 5]`

---

## List Functions (Built-in)

These are Python built-in functions that work on lists (not methods — no dot notation).

| Function | What it returns |
|----------|----------------|
| `len(list)` | Number of items |
| `sum(list)` | Sum of all numeric items |
| `min(list)` | Smallest value |
| `max(list)` | Largest value |
| `sorted(list)` | Returns a NEW sorted list (original unchanged) |
| `list(range(n))` | Creates a list of numbers 0 to n-1 |
| `reversed(list)` | Returns an iterator of the reversed list |

**`sort()` vs `sorted()`:**
- `list.sort()` → modifies the ORIGINAL list. Returns None.
- `sorted(list)` → returns a NEW list. Original is untouched.
- In ML, use `sorted()` when you need to preserve the original order.

---

## List Comprehension

A compact way to create a new list by applying an expression to each item.

**Syntax:** `[expression for item in iterable if condition]`

```python
# Without comprehension
squares = []
for x in range(10):
    squares.append(x ** 2)

# With comprehension — same result, one line
squares = [x ** 2 for x in range(10)]

# With condition — only even squares
even_squares = [x ** 2 for x in range(10) if x % 2 == 0]
```

> List comprehensions are faster than regular loops and much more common in ML/data science code. You'll see them everywhere in NumPy, Pandas workflows.

---

## Nested Lists (2D Lists)

A list can contain other lists — this creates a 2D structure (like a matrix/table).

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

matrix[1][2]   # 6 — row 1 (0-indexed), column 2
```

- Access with double indexing: `[row][column]`
- This is the manual version of a NumPy 2D array — understanding this first makes NumPy clearer later

---

## Copying Lists — Shallow vs Deep

This is a common source of bugs. Know this well.

**Assignment (`=`) does NOT copy — it creates a reference:**
```python
a = [1, 2, 3]
b = a           # b and a point to the SAME list
b.append(4)
print(a)        # [1, 2, 3, 4] — a changed too!
```

**Shallow copy — creates a new list, same content:**
```python
b = a.copy()    # or: b = a[:]  or: b = list(a)
```

**Deep copy — needed for nested lists:**
```python
import copy
b = copy.deepcopy(a)   # completely independent, even for nested lists
```

> In ML, you often transform datasets. Always copy before modifying — never mutate original data.

---

# Tutorial 3 — Sets, Dictionaries & Tuples

---

## What is a Tuple?

A Tuple is an **ordered, immutable** collection. Like a list that you can never change after creation.

- **Immutable** → once created, you CANNOT add, remove, or change items
- **Ordered** → items have a fixed position, accessible by index
- **Allows duplicates** → same value can appear multiple times
- Created with parentheses `()` or just comma-separated values (parentheses are optional)
- A single-item tuple NEEDS a trailing comma: `(5,)` — without it, `(5)` is just the number 5

**Why use a tuple instead of a list?**
- Tuples are faster than lists (Python optimizes immutable structures)
- Use tuple when data should NOT change — coordinates, RGB color, database records, function return values
- Tuples can be used as dictionary keys (lists cannot — immutability is required for keys)
- Signals intent: "this data is fixed, don't modify it"

---

## Tuple Packing & Unpacking

**Packing:** creating a tuple by putting values together
```python
point = (10, 20)        # packed into a tuple
```

**Unpacking:** extracting tuple values into individual variables
```python
x, y = (10, 20)         # x = 10, y = 20
a, b, c = 1, 2, 3       # works without parentheses too
```

**Unpacking with `*` (extended unpacking):**
```python
first, *rest = (1, 2, 3, 4, 5)
# first = 1, rest = [2, 3, 4, 5]
```

> Unpacking is used heavily in ML — returning multiple values from a function, unpacking coordinate pairs, extracting features and labels from datasets.

---

## Tuple Methods

Tuples have only TWO methods (because they're immutable — no modifications allowed):

| Method | What it does |
|--------|-------------|
| `count(x)` | Returns how many times `x` appears |
| `index(x)` | Returns index of first occurrence of `x` |

---

## Named Tuples

A named tuple lets you access fields by name instead of just index — more readable.

```python
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
p.x   # 10 — more readable than p[0]
p.y   # 20 — more readable than p[1]
```

> Named tuples are used in ML for things like storing model metrics (accuracy, loss, f1) with readable names.

---

## What is a Set?

A Set is an **unordered collection of unique elements**.

- **Unordered** → items have NO position, NO index. You can't do `set[0]`
- **Unique** → duplicates are automatically ignored
- **Mutable** → you can add or remove items
- Created with `{}` but empty set MUST use `set()` — `{}` creates an empty dictionary, not a set
- Very fast membership testing — `x in set` is O(1) regardless of size

> Sets are perfect when you need uniqueness — removing duplicates from data, finding common elements between datasets, checking if a value already exists.

---

## Set Operations — Mathematical Set Theory

This is where sets shine. These come directly from mathematics.

| Operation | Symbol | Method | Returns |
|-----------|--------|--------|---------|
| Union | `\|` | `.union()` | All elements from both sets |
| Intersection | `&` | `.intersection()` | Only elements in BOTH sets |
| Difference | `-` | `.difference()` | Elements in A but NOT in B |
| Symmetric Difference | `^` | `.symmetric_difference()` | Elements in either but NOT both |

```python
A = {1, 2, 3, 4, 5}
B = {4, 5, 6, 7, 8}

A | B    # {1, 2, 3, 4, 5, 6, 7, 8}  — all elements
A & B    # {4, 5}                     — common elements
A - B    # {1, 2, 3}                  — in A but not B
A ^ B    # {1, 2, 3, 6, 7, 8}        — not in both
```

> In ML: find common features between two datasets (intersection), find features unique to one dataset (difference), merge feature sets (union).

---

## Set Methods

| Method | What it does |
|--------|-------------|
| `add(x)` | Adds a single element |
| `update([x, y])` | Adds multiple elements |
| `remove(x)` | Removes `x` — raises error if not found |
| `discard(x)` | Removes `x` — NO error if not found (safer) |
| `pop()` | Removes and returns a RANDOM element |
| `clear()` | Empties the set |
| `issubset(B)` | True if all elements of A are in B |
| `issuperset(B)` | True if A contains all elements of B |
| `isdisjoint(B)` | True if A and B share NO elements |

**`remove` vs `discard`:**
- `remove` → use when you KNOW the element exists (crash if it doesn't)
- `discard` → use when unsure (safe — no error)

---

## Frozenset

A frozenset is an **immutable set** — like a tuple version of a set.

- Created with `frozenset([1, 2, 3])`
- Can't add or remove elements after creation
- Can be used as a dictionary key (unlike regular sets)
- Supports all set operations (union, intersection, etc.) but no modification methods

---

## What is a Dictionary?

A Dictionary is an **unordered collection of key-value pairs** — like a real-world dictionary where you look up a word (key) to find its definition (value).

- **Key-value pairs** → every item has a unique key and an associated value
- **Keys must be unique** → if you assign the same key twice, the second value overwrites the first
- **Keys must be immutable** → strings, numbers, tuples can be keys. Lists CANNOT be keys.
- **Values can be anything** → any data type, including other dicts (nested)
- **Ordered since Python 3.7+** → insertion order is preserved
- Created with `{}` containing `key: value` pairs

---

## Accessing Dictionary Values

```python
person = {'name': 'Anik', 'age': 20, 'city': 'Delhi'}

person['name']           # 'Anik' — direct access, KeyError if not found
person.get('name')       # 'Anik' — safe access, returns None if not found
person.get('phone', 'N/A')  # 'N/A' — fallback value if key missing
```

**`[ ]` vs `.get()`:**
- `dict['key']` → crashes with KeyError if key doesn't exist
- `dict.get('key')` → returns None silently — use this when key might not exist

---

## Dictionary Methods

| Method | What it does |
|--------|-------------|
| `keys()` | Returns all keys as a view |
| `values()` | Returns all values as a view |
| `items()` | Returns all (key, value) pairs as a view |
| `get(k, default)` | Safe access with optional fallback |
| `update({k: v})` | Merge another dict into this one |
| `pop(k)` | Remove and return value for key `k` |
| `popitem()` | Remove and return the LAST inserted (key, value) pair |
| `setdefault(k, v)` | Returns value for k; if not present, sets k=v first |
| `clear()` | Empties the dictionary |
| `copy()` | Returns a shallow copy |

---

## Iterating Over a Dictionary

```python
for key in person:               # iterates over keys only
for value in person.values():    # iterates over values only
for key, value in person.items():  # iterates over key-value pairs together
```

> `.items()` is the most common in ML — used when you need both the feature name (key) and its value at the same time.

---

## Dictionary Comprehension

Like list comprehension, but creates dictionaries.

```python
# Square of each number as key-value
squares = {x: x**2 for x in range(6)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# Filter: only even keys
even_squares = {x: x**2 for x in range(10) if x % 2 == 0}
```

> Dict comprehensions are heavily used in ML for mapping labels, creating feature dictionaries, and building encoding maps.

---

## Nested Dictionaries

A dictionary whose values are themselves dictionaries.

```python
students = {
    'student1': {'name': 'Anik', 'grade': 'A'},
    'student2': {'name': 'Rahul', 'grade': 'B'},
}

students['student1']['name']    # 'Anik'
```

- Common in ML for storing model configs, hyperparameter grids, results per model

---

## Comparison: List vs Tuple vs Set vs Dict

| Feature | List | Tuple | Set | Dictionary |
|---------|------|-------|-----|------------|
| Ordered | ✅ | ✅ | ❌ | ✅ (3.7+) |
| Mutable | ✅ | ❌ | ✅ | ✅ |
| Duplicates | ✅ | ✅ | ❌ | Keys: ❌ |
| Indexed | ✅ | ✅ | ❌ | By key |
| Use case | Sequence of items | Fixed data | Unique items | Key-value pairs |

---

# Tutorial 4 — NumPy & Inbuilt Functions

---

## Why NumPy?

NumPy (Numerical Python) is the core library for numerical computing in Python.

**The problem with Python lists for ML:**
- Python lists can hold any type — flexible but slow
- Operations on lists work element by element (loop) — slow for large data
- No built-in support for math operations like matrix multiplication, dot products

**What NumPy gives you:**
- **ndarray** — a fast, multi-dimensional array where all elements are the SAME type
- Operations work on the ENTIRE array at once (no loops needed) — this is called **vectorisation**
- Vectorised operations are 50x–100x faster than Python loops on large datasets
- Built-in mathematical, statistical, and linear algebra functions
- Foundation of Pandas, Scikit-learn, TensorFlow, PyTorch — everything ML uses NumPy underneath

> NumPy is to ML what a calculator is to mathematics. You could do it without, but you never would.

---

## NumPy Array vs Python List

| Feature | Python List | NumPy Array |
|---------|-------------|-------------|
| Data types | Mixed | All same type |
| Speed | Slow | Very fast |
| Memory | More (stores type info per element) | Less (fixed type = compact) |
| Math operations | Manual loops | Vectorised (operates on whole array) |
| Dimensions | Nested lists | Native multi-dimensional |
| Broadcasting | Not supported | Supported |

```python
import numpy as np

list_a = [1, 2, 3]
arr_a = np.array([1, 2, 3])

# Python list — no math on the whole list
list_a * 2           # [1, 2, 3, 1, 2, 3]  — list repetition, not math!

# NumPy array — math works element-wise
arr_a * 2            # [2, 4, 6]  — multiplies every element by 2
```

---

## ndarray — The Core Object

`ndarray` = N-dimensional array. The central object in NumPy.

- **1D array** → a vector: `[1, 2, 3, 4]`
- **2D array** → a matrix (rows × columns): like a spreadsheet
- **3D array** → a cube (depth × rows × columns): like stacked spreadsheets
- **Shape** → tuple describing dimensions: `(3,)` for 1D, `(3, 4)` for 2D
- **dtype** → the data type of all elements: `int32`, `float64`, `bool`, etc.
- **ndim** → number of dimensions
- **size** → total number of elements (product of shape values)

---

## Creating Arrays

**From Python structures:**
```python
np.array([1, 2, 3])              # 1D from list
np.array([[1, 2], [3, 4]])       # 2D from nested list
```

**Special arrays:**
```python
np.zeros((3, 4))        # 3×4 array of all 0.0
np.ones((2, 3))         # 2×3 array of all 1.0
np.full((2, 2), 7)      # 2×2 array filled with 7
np.eye(4)               # 4×4 identity matrix (diagonal = 1, rest = 0)
np.empty((3, 3))        # uninitialized array (garbage values, fast to create)
```

**Range-based:**
```python
np.arange(0, 10, 2)     # [0, 2, 4, 6, 8] — like range(), but returns array
np.linspace(0, 1, 5)    # [0.0, 0.25, 0.5, 0.75, 1.0] — 5 evenly spaced between 0 and 1
```

> `arange` gives you control over the step. `linspace` gives you control over the COUNT of points. Use linspace when you want exactly n points between two values.

**Random arrays:**
```python
np.random.rand(3, 4)       # 3×4 array, uniform random [0, 1)
np.random.randn(3, 4)      # 3×4 array, standard normal distribution (mean=0, std=1)
np.random.randint(0, 10, (3, 3))  # 3×3 array of random ints from 0 to 9
np.random.seed(42)         # fix seed for reproducibility — same result every run
```

> `random.seed()` is essential in ML — ensures your random splits, initializations, and augmentations are reproducible. Always set it.

---

## Array Properties

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

arr.shape    # (2, 3)  — 2 rows, 3 columns
arr.ndim     # 2       — 2 dimensions
arr.size     # 6       — total 6 elements
arr.dtype    # int64   — data type of elements
```

---

## Indexing & Slicing NumPy Arrays

**1D — exactly like lists:**
```python
arr = np.array([10, 20, 30, 40, 50])
arr[2]       # 30
arr[1:4]     # [20, 30, 40]
arr[-1]      # 50
```

**2D — use comma for row and column:**
```python
matrix = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

matrix[1, 2]      # 6 — row 1, column 2
matrix[0, :]      # [1, 2, 3] — entire first row
matrix[:, 1]      # [2, 5, 8] — entire second column
matrix[0:2, 1:3]  # [[2, 3], [5, 6]] — sub-matrix
```

> Column selection `matrix[:, i]` is one of the most used operations in ML — extracting a feature column from a dataset.

---

## Boolean Indexing (Fancy Indexing)

Filter array elements based on a condition — no loop needed.

```python
arr = np.array([15, 8, 23, 4, 42, 11])

arr[arr > 10]       # [15, 23, 42, 11] — only values greater than 10
arr[arr % 2 == 0]   # [8, 4, 42]       — only even values
```

- The condition inside `[]` creates a boolean array (True/False for each element)
- NumPy uses that boolean array to filter
- This is the direct foundation of Pandas boolean filtering and Scikit-learn masking

---

## Reshaping Arrays

Reshaping changes the DIMENSIONS of an array without changing its data.

```python
arr = np.arange(12)        # [0, 1, 2, ..., 11]  — shape (12,)
arr.reshape(3, 4)          # shape (3, 4) — 3 rows, 4 columns
arr.reshape(2, 2, 3)       # shape (2, 2, 3) — 3D array

arr.flatten()              # always returns 1D — copy of data
arr.ravel()                # returns 1D — view where possible (faster)
arr.reshape(-1)            # also flattens — -1 means "figure it out"
arr.reshape(-1, 1)         # column vector — shape (12, 1)
arr.reshape(1, -1)         # row vector — shape (1, 12)
```

> `reshape(-1, 1)` is used constantly in Scikit-learn — ML models expect a 2D array where each row is one sample and each column is one feature.

---

## Array Operations — Vectorisation

All arithmetic operations apply **element-wise** on the entire array at once — no loops.

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

a + b        # [5, 7, 9]   — element-wise addition
a * b        # [4, 10, 18] — element-wise multiplication
a ** 2       # [1, 4, 9]   — element-wise square
a + 10       # [11, 12, 13] — scalar broadcast (adds 10 to every element)
```

**This is vectorisation** — the operation runs on the entire array without a Python loop, using optimised C code underneath. This is why NumPy is so fast.

---

## Broadcasting

Broadcasting is how NumPy handles operations between arrays of DIFFERENT shapes.

**Rule:** NumPy compares shapes from the right side. Dimensions are compatible if they're equal OR one of them is 1.

```python
matrix = np.ones((3, 4))   # shape (3, 4)
row    = np.array([1, 2, 3, 4])  # shape (4,) — treated as (1, 4)

matrix + row   # row is broadcast across all 3 rows — shape (3, 4)
```

> Broadcasting is what lets you add a bias vector to a matrix, or normalise each feature column without loops. It's everywhere in deep learning.

---

## Mathematical & Statistical Functions

These operate on the array (or along a specific axis).

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

np.sum(arr)          # 21 — sum of ALL elements
np.sum(arr, axis=0)  # [5, 7, 9] — sum down each COLUMN
np.sum(arr, axis=1)  # [6, 15]   — sum across each ROW

np.mean(arr)         # 3.5 — average
np.median(arr)       # 3.5 — middle value
np.std(arr)          # standard deviation
np.var(arr)          # variance

np.min(arr)          # 1
np.max(arr)          # 6
np.argmin(arr)       # index of minimum value
np.argmax(arr)       # index of maximum value

np.sort(arr)         # sort along last axis
np.argsort(arr)      # indices that would sort the array
np.unique(arr)       # unique values
```

> `axis=0` = operate DOWN the rows (column-wise result). `axis=1` = operate ACROSS the columns (row-wise result). This trips people up constantly — remember: axis is the DIRECTION you collapse.

---

## Linear Algebra with NumPy

Essential for ML — understanding weights, transformations, projections.

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

np.dot(A, B)         # matrix multiplication (dot product)
A @ B                # same as np.dot — cleaner syntax (Python 3.5+)

np.transpose(A)      # flip rows and columns — same as A.T
A.T                  # shorthand for transpose

np.linalg.inv(A)     # matrix inverse (A × A_inv = Identity)
np.linalg.det(A)     # determinant of matrix
np.linalg.eig(A)     # eigenvalues and eigenvectors
np.linalg.norm(A)    # L2 norm (Euclidean magnitude)
```

> Matrix multiplication (`@`), transpose (`.T`), and norm are used in nearly every ML algorithm — gradient descent, PCA, neural network forward pass.

---

## Stacking & Concatenating Arrays

Combining multiple arrays into one.

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

np.concatenate([a, b])          # [1, 2, 3, 4, 5, 6] — join along existing axis
np.stack([a, b])                # [[1,2,3],[4,5,6]] — creates new axis
np.vstack([a, b])               # stack vertically (as rows)
np.hstack([a, b])               # stack horizontally (side by side)
```

> `vstack` is commonly used in ML to combine training batches or concatenate feature matrices row-wise.

---

## Useful NumPy Utilities

```python
np.where(arr > 3, 1, 0)    # if condition: 1, else: 0 — like a vectorised if-else
np.clip(arr, 0, 5)         # cap values: below 0 → 0, above 5 → 5
np.abs(arr)                # absolute value of each element
np.sqrt(arr)               # square root of each element
np.log(arr)                # natural log — used in loss functions
np.exp(arr)                # e^x — used in sigmoid, softmax
np.round(arr, 2)           # round to 2 decimal places
```

> `np.log` and `np.exp` are used in logistic regression, cross-entropy loss, and activation functions. `np.clip` is used to prevent numerical instability (like log(0)).

---

## Python Inbuilt Functions Relevant to ML

These are standard Python functions you'll use constantly alongside NumPy.

---

## `range()`

Generates a sequence of numbers — does NOT create a list, creates a lazy iterator.

```python
range(5)          # 0, 1, 2, 3, 4
range(2, 10, 2)   # 2, 4, 6, 8
list(range(5))    # [0, 1, 2, 3, 4] — convert to list when needed
```

---

## `enumerate()`

Iterates over a sequence and gives you BOTH the index and the value.

```python
fruits = ['apple', 'banana', 'cherry']
for index, fruit in enumerate(fruits):
    print(index, fruit)
# 0 apple  1 banana  2 cherry
```

> Used in ML when you loop over labels or class names and need both the position and value.

---

## `zip()`

Pairs up elements from multiple iterables simultaneously.

```python
names = ['Anik', 'Rahul', 'Priya']
scores = [85, 92, 78]

for name, score in zip(names, scores):
    print(name, score)
# Anik 85, Rahul 92, Priya 78
```

> `zip` is used to pair features with labels, combine predictions with actuals, or merge two lists.

---

## `map()`

Applies a function to EVERY element of an iterable — returns a map object (lazy).

```python
nums = [1, 2, 3, 4]
squared = list(map(lambda x: x**2, nums))   # [1, 4, 9, 16]
```

- More functional-style alternative to a list comprehension
- `map()` is lazy — doesn't compute until you consume it (wrap in `list()`)

---

## `filter()`

Filters elements from an iterable based on a function that returns True/False.

```python
nums = [1, 2, 3, 4, 5, 6]
evens = list(filter(lambda x: x % 2 == 0, nums))  # [2, 4, 6]
```

---

## `lambda` Functions

Anonymous (nameless) functions written in a single line.

**Syntax:** `lambda parameters: expression`

```python
square  = lambda x: x ** 2
add     = lambda x, y: x + y
```

- Used when you need a small function once — especially with `map`, `filter`, `sorted`
- `sorted(data, key=lambda x: x['score'])` → sort list of dicts by a specific key
- Overusing lambda for complex logic → use a real `def` instead

---

## `sorted()` with `key`

The `key` parameter defines HOW to sort — pass a function that extracts the sort value.

```python
students = [('Anik', 85), ('Rahul', 92), ('Priya', 78)]
sorted(students, key=lambda s: s[1])           # sort by score
sorted(students, key=lambda s: s[1], reverse=True)  # descending
```

> This pattern is used in ML to sort model results, rank features by importance, order classes by frequency.

---

## `any()` and `all()`

Work on iterables of booleans (or values with truthiness).

- `any(iterable)` → returns True if AT LEAST ONE element is truthy
- `all(iterable)` → returns True only if ALL elements are truthy

```python
any([False, True, False])    # True  — at least one True
all([True, True, True])      # True  — all are True
all([True, False, True])     # False — not all True
```

> Used for validation checks — "are all required fields filled?", "does any value exceed threshold?"

---

## `abs()`, `round()`, `type()`, `isinstance()`

```python
abs(-7.5)          # 7.5   — absolute value
round(3.14159, 2)  # 3.14  — round to 2 decimal places

type(42)           # <class 'int'>
type([1, 2, 3])    # <class 'list'>

isinstance(42, int)          # True  — is 42 an int?
isinstance([1, 2], list)     # True
isinstance(42, (int, float)) # True  — check against multiple types
```

> `isinstance()` is preferred over `type() ==` because it respects inheritance.

---

## Quick Reference — Data Structures

| Operation | List | Tuple | Set | Dict |
|-----------|------|-------|-----|------|
| Create | `[1, 2]` | `(1, 2)` | `{1, 2}` | `{'a': 1}` |
| Add item | `.append()` | ❌ | `.add()` | `d['k'] = v` |
| Remove item | `.remove()` | ❌ | `.discard()` | `.pop('k')` |
| Access | `[index]` | `[index]` | ❌ | `['key']` |
| Check membership | `in` | `in` | `in` (fast) | `in` (keys) |
| Length | `len()` | `len()` | `len()` | `len()` |
| Iteration | `for x in` | `for x in` | `for x in` | `.items()` |
| Mutable | ✅ | ❌ | ✅ | ✅ |

---

## Quick Reference — NumPy Essentials

| Task | NumPy Code |
|------|-----------|
| Create array | `np.array([1, 2, 3])` |
| Zeros / Ones | `np.zeros((r, c))` / `np.ones((r, c))` |
| Range | `np.arange(start, stop, step)` |
| Evenly spaced | `np.linspace(start, stop, n)` |
| Random | `np.random.rand(r, c)` |
| Shape | `arr.shape` |
| Reshape | `arr.reshape(r, c)` |
| Flatten | `arr.flatten()` |
| Transpose | `arr.T` |
| Dot product | `A @ B` |
| Boolean filter | `arr[arr > 5]` |
| Element-wise ops | `arr * 2`, `arr + arr` |
| Stats | `np.mean()`, `np.std()`, `np.sum()` |
| Axis-wise | `np.sum(arr, axis=0)` |
| Conditional | `np.where(cond, x, y)` |

---
*Notes by Anik | Krish Naik — Machine Learning Playlist (Tutorial 2, 3, 4)*