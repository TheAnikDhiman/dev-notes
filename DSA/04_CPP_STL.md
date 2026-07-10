# Complete C++ STL — Revision Notes
**Source:** Complete C++ STL in 1 Video | Time Complexity and Notes — Striver's A2Z DSA Course

---

## 1. What is STL
- **Standard Template Library** — a collection of pre-built, template-based classes and functions for common data structures and algorithms.
- 4 main components: **Containers**, **Iterators**, **Algorithms**, **Functors**.
- Why it matters for DSA: saves you from writing linked lists, stacks, hashmaps etc. from scratch — lets you focus on logic.

## 2. Iterators (quick concept)
- Act like pointers that let you traverse a container.
- `container.begin()` → points to first element
- `container.end()` → points to one-past-the-last element
- Used heavily with algorithms like `sort(v.begin(), v.end())`.

## 3. Containers Overview

### A. Sequence Containers
| Container | Description | Key Operations & Complexity |
|---|---|---|
| `vector` | dynamic array | access O(1), push_back O(1) amortized, insert/delete at middle O(n) |
| `list` | doubly linked list | insert/delete O(1) (given iterator), no random access |
| `deque` | double-ended queue | push/pop from both ends O(1), random access O(1) |

### B. Container Adapters (built on top of other containers)
| Container | Description | Key Operations |
|---|---|---|
| `stack` | LIFO | push/pop/top all O(1) |
| `queue` | FIFO | push/pop/front all O(1) |
| `priority_queue` | max-heap by default | push/pop O(log n), top O(1) |

- `priority_queue<int, vector<int>, greater<int>>` → makes it a **min-heap** instead of default max-heap.

### C. Associative Containers (ordered, based on balanced BST internally)
| Container | Description | Complexity |
|---|---|---|
| `set` | unique elements, sorted | insert/delete/find O(log n) |
| `multiset` | duplicates allowed, sorted | O(log n) |
| `map` | key-value pairs, unique keys, sorted by key | O(log n) |
| `multimap` | key-value, duplicate keys allowed | O(log n) |

### D. Unordered Associative Containers (based on hashing)
| Container | Description | Complexity |
|---|---|---|
| `unordered_set` | unique elements, no order | O(1) average, O(n) worst case |
| `unordered_map` | key-value, no order | O(1) average, O(n) worst case |

**When to choose which:**
- Need sorted order → `set`/`map`
- Need fastest average lookup, order doesn't matter → `unordered_set`/`unordered_map`
- Need duplicates → `multiset`/`multimap`

## 4. Pair
```cpp
pair<int, int> p = {1, 2};
p.first;   // 1
p.second;  // 2
```
- Useful for storing two related values together (e.g., coordinates, (value, index) pairs).

## 5. Commonly Used Algorithms (`<algorithm>` header)
| Function | Purpose | Complexity |
|---|---|---|
| `sort(v.begin(), v.end())` | sorts ascending | O(n log n) |
| `reverse(v.begin(), v.end())` | reverses container | O(n) |
| `max_element()` / `min_element()` | finds max/min | O(n) |
| `binary_search()` | checks presence in sorted range | O(log n) |
| `lower_bound()` | first element ≥ given value | O(log n) |
| `upper_bound()` | first element > given value | O(log n) |
| `accumulate()` (from `<numeric>`) | sum of range | O(n) |
| `count()` | counts occurrences | O(n) |
| `next_permutation()` | generates next lexicographic permutation | O(n) |

## 6. Vector — Deep Dive (most-used container)
- Underlying: dynamic array that **doubles capacity** when it runs out of space → this is why `push_back` is O(1) *amortized* (occasionally O(n) when resizing, but averages out to O(1)).
- `.size()` → current number of elements
- `.capacity()` → allocated memory (≥ size)
- `.resize()`, `.clear()`, `.empty()`

## 7. Map vs Unordered_map — Key Interview Point
- `map` maintains sorted key order (internally a Red-Black Tree) → O(log n) operations.
- `unordered_map` uses hashing → O(1) average, but can degrade to O(n) in worst case (hash collisions) → still usually preferred when order doesn't matter, due to better average performance.

---
### Quick Recall Checklist
- [ ] vector vs list vs deque — when to use which
- [ ] set/map vs unordered_set/unordered_map trade-off
- [ ] priority_queue → max-heap by default, use `greater<int>` for min-heap
- [ ] push_back is O(1) amortized, not strictly O(1)
- [ ] lower_bound vs upper_bound difference
