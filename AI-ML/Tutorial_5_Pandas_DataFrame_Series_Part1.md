# Tutorial 5 — Pandas, DataFrame and Data Series (Part 1)

## 1. What is Pandas?

> **Definition:** **Pandas** is a Python library built on top of NumPy that provides fast, flexible data structures — primarily **Series** and **DataFrame** — for handling labeled, tabular data (like Excel sheets or SQL tables) along with tools for cleaning, transforming, and analyzing it.

```python
import pandas as pd
```

---

## 2. Series

> **Definition:** A **Series** is a one-dimensional labeled array capable of holding any data type, essentially a single column of data with an associated **index**.

```python
s = pd.Series([10, 20, 30, 40])
print(s)
# 0    10
# 1    20
# 2    30
# 3    40
# dtype: int64

# custom index
s2 = pd.Series([10, 20, 30], index=["a", "b", "c"])
print(s2["b"])   # 20

# from a dictionary (keys become index)
s3 = pd.Series({"x": 1, "y": 2, "z": 3})
```

### Series Attributes & Basic Ops
```python
print(s.values)   # underlying numpy array
print(s.index)    # index object
print(s.dtype)    # data type

print(s * 2)       # vectorized operation, like numpy
print(s[s > 20])   # boolean filtering
```

---

## 3. DataFrame

> **Definition:** A **DataFrame** is a two-dimensional, labeled data structure with rows and columns — conceptually a collection of Series sharing the same index, similar to a spreadsheet or SQL table.

```python
data = {
    "Name": ["Anik", "Riya", "Sam"],
    "Age": [21, 22, 23],
    "Branch": ["CSE", "ECE", "ME"]
}
df = pd.DataFrame(data)
print(df)
```

### Creating DataFrames — Other Ways
```python
df2 = pd.DataFrame([[1,"a"], [2,"b"]], columns=["ID", "Label"])  # from list of lists
df3 = pd.DataFrame(np.random.rand(3,3), columns=["A","B","C"])   # from numpy array
```

---

## 4. Exploring a DataFrame

```python
df.head()       # first 5 rows (default)
df.head(2)      # first 2 rows
df.tail(3)      # last 3 rows
df.shape        # (rows, columns) tuple
df.columns      # column labels
df.index        # row labels
df.info()       # dtypes, non-null counts, memory usage
df.describe()   # summary stats (mean, std, min, max, quartiles) for numeric cols
df.dtypes       # data type of each column
```

---

## 5. Selecting Columns & Rows

> **Definition:** `.loc[]` selects by **label** (row/column names), while `.iloc[]` selects by **integer position** — both work as `[row_selector, column_selector]`.

```python
# select a single column -> returns a Series
print(df["Name"])

# select multiple columns -> returns a DataFrame
print(df[["Name", "Age"]])

# row selection by label
print(df.loc[0])          # first row (label-based)
print(df.loc[0:1])         # rows with label 0 and 1 (inclusive)

# row selection by position
print(df.iloc[0])          # first row (position-based)
print(df.iloc[0:2])        # first 2 rows (exclusive of stop, numpy-style)

# specific cell
print(df.loc[0, "Name"])   # label-based cell access
print(df.iloc[0, 1])       # position-based cell access
```

---

## 6. Adding, Modifying & Dropping Columns

```python
df["Passed"] = True                          # add new column (broadcast)
df["Age_in_2030"] = df["Age"] + 4             # derived column

df.drop("Passed", axis=1, inplace=True)       # drop a column
df.drop(0, axis=0, inplace=True)              # drop a row (by label)
```
> **Note:** `axis=0` refers to rows, `axis=1` refers to columns. `inplace=True` modifies the DataFrame directly instead of returning a copy.

---

## 7. Basic Filtering (Boolean Indexing)

```python
adults = df[df["Age"] > 21]                     # single condition
filtered = df[(df["Age"] > 21) & (df["Branch"] == "ECE")]  # multiple conditions (& / |)
```

---

## Quick Revision Summary
- **Series** = 1D labeled array (single column); **DataFrame** = 2D labeled table (rows + columns).
- `.head()/.tail()/.info()/.describe()/.shape` are the first things to run on any new DataFrame.
- `.loc[]` = label-based selection; `.iloc[]` = position-based selection.
- Boolean indexing (`df[condition]`) is the standard way to filter rows.
- `axis=0` = rows, `axis=1` = columns.
