# Tutorial 3 — Python Sets, Dictionaries and Tuples

## 1. Sets

> **Definition:** A **set** is an unordered, mutable collection of **unique** elements, defined with `{}` or `set()`. Duplicates are automatically removed; elements must be hashable (immutable types).

```python
s = {1, 2, 3, 3, 2}
print(s)            # {1, 2, 3} - duplicates removed

empty_set = set()    # NOT {} (that creates an empty dict!)
```

### Set Methods
| Method | Purpose |
|---|---|
| `add(x)` | add single element |
| `update(iter)` | add multiple elements |
| `remove(x)` | remove element (error if missing) |
| `discard(x)` | remove element (no error if missing) |
| `pop()` | remove & return an arbitrary element |
| `clear()` | empty the set |

```python
s = {1, 2, 3}
s.add(4)
s.update([5, 6])
s.discard(10)   # no error even though 10 not present
```

### Set Operations (Math-style)
> **Definition:** Sets support mathematical set theory operations — union (all elements), intersection (common elements), difference (elements only in first set), symmetric difference (elements not shared).

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

print(a | b)   # union            {1,2,3,4,5,6}
print(a & b)   # intersection     {3,4}
print(a - b)   # difference       {1,2}
print(a ^ b)   # symmetric diff   {1,2,5,6}
print(a.issubset(b))    # False
print(a.issuperset({1,2}))  # True
```

---

## 2. Dictionaries

> **Definition:** A **dictionary** is an unordered (insertion-ordered since Python 3.7), mutable collection of **key-value pairs**, defined with `{key: value}`. Keys must be unique and hashable.

```python
student = {"name": "Anik", "age": 21, "branch": "CSE"}
empty = {}
```

### Accessing & Modifying
```python
print(student["name"])          # Anik  (KeyError if missing)
print(student.get("gpa"))       # None  (safe access, no error)
print(student.get("gpa", 0))    # 0     (default value)

student["age"] = 22             # update
student["cgpa"] = 8.5           # add new key
del student["cgpa"]             # remove key
```

### Dictionary Methods
| Method | Purpose |
|---|---|
| `keys()` | view of all keys |
| `values()` | view of all values |
| `items()` | view of (key, value) pairs |
| `update(dict2)` | merge another dict in |
| `pop(key)` | remove key & return its value |
| `popitem()` | remove & return last inserted pair |

```python
for key, value in student.items():
    print(key, "->", value)

d1 = {"a": 1}
d2 = {"b": 2}
d1.update(d2)   # {"a":1, "b":2}
```

### Dictionary Comprehension
```python
squares = {x: x**2 for x in range(5)}   # {0:0, 1:1, 2:4, 3:9, 4:16}
```

---

## 3. Tuples

> **Definition:** A **tuple** is an ordered, **immutable** collection of items, defined with parentheses `()`. Once created, elements cannot be added, removed, or changed.

```python
t = (1, 2, 3)
single = (5,)        # comma required for single-element tuple
mixed = (1, "a", 3.5)
```

### Why Tuples?
- Faster than lists (lower memory overhead).
- Immutability makes them safe as dictionary keys or for fixed data (e.g., coordinates).

```python
t = (10, 20, 30)
print(t[0])       # 10 (indexing works)
print(t[1:])      # (20, 30) (slicing works)
# t[0] = 99       # TypeError: tuples don't support item assignment
```

### Tuple Packing & Unpacking
> **Definition:** *Packing* groups multiple values into a tuple; *unpacking* extracts tuple values into separate variables in one line.

```python
point = 3, 4              # packing (parentheses optional)
x, y = point               # unpacking
print(x, y)                # 3 4

a, *rest = (1, 2, 3, 4)    # a=1, rest=[2,3,4]
```

### Tuple Methods
```python
t = (1, 2, 2, 3)
print(t.count(2))   # 2
print(t.index(3))   # 3
```

---

## 4. List vs Tuple vs Set vs Dict — Quick Comparison

| Feature | List | Tuple | Set | Dict |
|---|---|---|---|---|
| Syntax | `[]` | `()` | `{}`/`set()` | `{k:v}` |
| Ordered | Yes | Yes | No (insertion order not guaranteed) | Yes (3.7+) |
| Mutable | Yes | No | Yes | Yes |
| Duplicates | Allowed | Allowed | Not allowed | Keys unique |
| Use case | general sequence | fixed/constant data | uniqueness, membership tests | key-based lookup |

---

## Quick Revision Summary
- **Set** = unique, unordered, mutable; supports union/intersection/difference.
- **Dict** = key-value pairs; use `.get()` for safe access; `.items()` to iterate.
- **Tuple** = ordered, immutable; faster than list; supports unpacking.
- Use `set()` (not `{}`) for an empty set — `{}` makes an empty dict.
