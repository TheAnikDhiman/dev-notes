# Tutorial 4 — NumPy and Inbuilt Functions

## 1. What is NumPy?

> **Definition:** **NumPy** (Numerical Python) is a library that provides the **ndarray** — a fast, memory-efficient, multi-dimensional array object — plus vectorized mathematical operations, forming the numerical foundation for Pandas, Scikit-learn, and most of the ML/DL stack.

```python
import numpy as np
```

**Why NumPy over Python lists?**
- Arrays are stored in contiguous memory → much faster.
- Supports **vectorized operations** (no explicit loops).
- Supports **broadcasting** (operations between different shapes).

---

## 2. Creating Arrays

> **Definition:** An `ndarray` (n-dimensional array) is a grid of values, all of the **same data type**, indexed by a tuple of non-negative integers (its shape).

```python
a = np.array([1, 2, 3])                 # 1D array
b = np.array([[1, 2], [3, 4]])          # 2D array (matrix)

zeros = np.zeros((2, 3))                # 2x3 array of 0.0
ones = np.ones((3, 3))                  # 3x3 array of 1.0
identity = np.eye(3)                    # 3x3 identity matrix
rng = np.arange(0, 10, 2)               # [0,2,4,6,8] (like range())
lin = np.linspace(0, 1, 5)              # 5 evenly spaced values 0 to 1
rand = np.random.rand(2, 2)             # random floats [0,1)
rand_int = np.random.randint(1, 10, 5)  # 5 random ints [1,10)
```

---

## 3. Array Attributes

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

print(arr.shape)   # (2, 3)  -> rows, columns
print(arr.ndim)    # 2       -> number of dimensions
print(arr.size)    # 6       -> total number of elements
print(arr.dtype)   # int64   -> data type of elements
```

---

## 4. Indexing & Slicing

```python
arr = np.array([[10, 20, 30], [40, 50, 60]])

print(arr[0, 1])     # 20   (row 0, col 1)
print(arr[:, 1])     # [20, 50]  (entire column 1)
print(arr[1, :])     # [40, 50, 60]  (entire row 1)
print(arr[0:1, 1:3])  # [[20, 30]]  (sub-matrix)

# boolean/fancy indexing
mask = arr > 25
print(arr[mask])     # [30, 40, 50, 60]
```

---

## 5. Reshaping

> **Definition:** `reshape()` changes an array's shape (dimensions) without changing its underlying data, as long as the total number of elements stays the same.

```python
arr = np.arange(12)          # 12 elements, 1D
reshaped = arr.reshape(3, 4)  # 3 rows, 4 cols
flat = reshaped.flatten()     # back to 1D
```

---

## 6. Arithmetic Operations & Broadcasting

> **Definition:** **Broadcasting** is NumPy's mechanism for applying operations between arrays of different shapes by automatically "stretching" the smaller array without copying data.

```python
a = np.array([1, 2, 3])
b = np.array([10, 20, 30])

print(a + b)     # [11, 22, 33]  element-wise
print(a * 2)     # [2, 4, 6]     scalar broadcast
print(a * b)     # [10, 40, 90]  element-wise multiply
print(a.dot(b))  # 140  (dot product)

matrix = np.array([[1,2],[3,4]])
print(matrix + np.array([10, 20]))  # broadcasts row-wise
```

---

## 7. Aggregate / Statistical Functions

```python
arr = np.array([1, 2, 3, 4, 5])

print(arr.sum())     # 15
print(arr.mean())    # 3.0
print(arr.std())     # standard deviation
print(arr.var())     # variance
print(arr.min(), arr.max())   # 1 5
print(arr.argmin(), arr.argmax())  # index of min/max -> 0 4

matrix = np.array([[1,2],[3,4]])
print(matrix.sum(axis=0))   # [4, 6]  column-wise sum
print(matrix.sum(axis=1))   # [3, 7]  row-wise sum
```

---

## 8. Useful Inbuilt Functions

```python
arr = np.array([1, 4, 9, 16])

print(np.sqrt(arr))        # [1, 2, 3, 4]
print(np.exp([1, 2]))      # e^1, e^2
print(np.log([1, np.e]))   # natural log
print(np.unique([1,1,2,3,3]))  # [1,2,3]
print(np.where(arr > 5))   # indices where condition True

# stacking / joining
a = np.array([1,2]); b = np.array([3,4])
print(np.concatenate([a, b]))  # [1,2,3,4]
print(np.vstack([a, b]))       # stack vertically -> 2x2
print(np.hstack([a, b]))       # stack horizontally -> [1,2,3,4]
```

---

## Quick Revision Summary
- NumPy's core object = `ndarray`; faster than lists via contiguous memory + vectorization.
- Key attributes: `.shape`, `.ndim`, `.size`, `.dtype`.
- `reshape()` changes shape without changing data; total elements must match.
- **Broadcasting** lets differently-shaped arrays interact without manual loops.
- Aggregates: `sum, mean, std, var, min, max, argmin, argmax` — use `axis=0/1` for rows/columns.
