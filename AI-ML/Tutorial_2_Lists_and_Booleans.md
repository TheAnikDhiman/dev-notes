# Tutorial 2 — Python List and Boolean Variables

## 1. What is a List?

> **Definition:** A **list** is an ordered, mutable (changeable), heterogeneous collection of items, defined with square brackets `[]`. Order is preserved, and duplicate values are allowed.

```python
fruits = ["apple", "banana", "cherry"]
mixed = [1, "hello", 3.14, True]
empty = []
nested = [[1, 2], [3, 4]]   # list of lists (matrix-like)
```

---

## 2. Indexing & Slicing

> **Definition:** *Indexing* accesses a single element by position (0-based, negative indices count from the end); *slicing* extracts a sub-list using `start:stop:step`.

```python
nums = [10, 20, 30, 40, 50]

print(nums[0])     # 10  (first element)
print(nums[-1])    # 50  (last element)
print(nums[1:4])   # [20, 30, 40]  (stop is exclusive)
print(nums[:3])    # [10, 20, 30]
print(nums[::2])   # [10, 30, 50]  (every 2nd element)
print(nums[::-1])  # [50, 40, 30, 20, 10]  (reversed)
```

---

## 3. Mutability

> **Definition:** *Mutable* means the object's contents can be changed after creation without creating a new object — lists support in-place modification.

```python
nums = [1, 2, 3]
nums[0] = 100        # modify in place
print(nums)          # [100, 2, 3]
```

---

## 4. Common List Methods

| Method | Purpose | Example |
|---|---|---|
| `append(x)` | add single item at end | `lst.append(5)` |
| `extend(iter)` | add multiple items | `lst.extend([6,7])` |
| `insert(i, x)` | insert at index | `lst.insert(1, 99)` |
| `remove(x)` | remove first matching value | `lst.remove(99)` |
| `pop(i)` | remove & return item at index (default last) | `lst.pop()` |
| `sort()` | sort in place (ascending) | `lst.sort()` |
| `sort(reverse=True)` | descending sort | |
| `reverse()` | reverse order in place | `lst.reverse()` |
| `index(x)` | first index of value | `lst.index(5)` |
| `count(x)` | number of occurrences | `lst.count(5)` |
| `copy()` | shallow copy | `lst2 = lst.copy()` |
| `clear()` | empty the list | `lst.clear()` |

```python
nums = [3, 1, 4, 1, 5]
nums.append(9)          # [3,1,4,1,5,9]
nums.sort()              # [1,1,3,4,5,9]
print(len(nums))         # 6
print(sum(nums))         # 23
print(max(nums), min(nums))  # 9 1
```

---

## 5. List Comprehension

> **Definition:** A concise, Pythonic syntax for building a new list from an iterable in a single line, optionally with a filter condition: `[expr for item in iterable if condition]`.

```python
squares = [x**2 for x in range(6)]           # [0,1,4,9,16,25]
evens = [x for x in range(10) if x % 2 == 0] # [0,2,4,6,8]
```

---

## 6. Boolean Variables

> **Definition:** `bool` is a data type with exactly two values, `True` and `False`, used to represent logical/truth states; internally `True == 1` and `False == 0`.

```python
is_active = True
is_admin = False
print(type(is_active))   # <class 'bool'>
print(True + True)       # 2 (bool behaves like int)
```

### Truthy / Falsy Values
> **Definition:** *Truthy*/*falsy* values are non-boolean objects that evaluate to `True`/`False` in a boolean context (e.g., `if` statement).

- Falsy: `0`, `0.0`, `""`, `[]`, `{}`, `()`, `None`, `False`
- Truthy: everything else (non-empty strings/lists, non-zero numbers)

```python
lst = []
if lst:
    print("has items")
else:
    print("empty list")   # this runs -> [] is falsy
```

### Boolean/Comparison Operators
```python
a, b = 5, 10
print(a == b)       # False
print(a != b)       # True
print(a < b and b < 20)   # True
print(not (a > b))        # True
print(bool(""), bool("x"))  # False True
```

---

## Quick Revision Summary
- List = ordered, mutable, allows duplicates; created with `[]`.
- Indexing = single element; slicing = `[start:stop:step]`, stop is exclusive.
- Common ops: `append, extend, insert, remove, pop, sort, reverse`.
- List comprehension: `[expr for x in iterable if cond]`.
- `bool` has only `True`/`False`; falsy values include `0, "", [], {}, None`.
