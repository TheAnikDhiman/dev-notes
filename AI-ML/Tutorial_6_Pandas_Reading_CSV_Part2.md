# Tutorial 6 — Pandas: Reading CSV Files With Various Parameters (Part 2)

## 1. What is a CSV file?

> **Definition:** **CSV** (Comma-Separated Values) is a plain-text file format where each line is a data record and fields within a record are separated by a delimiter (commonly a comma). It's the most common format for exchanging tabular data.

```python
import pandas as pd

df = pd.read_csv("data.csv")   # simplest read
print(df.head())
```

---

## 2. `read_csv()` — Key Parameters

> **Definition:** `pd.read_csv()` is Pandas' primary function to load a CSV (or any delimited text file) into a DataFrame. It has 50+ optional parameters that control parsing behavior — knowing the common ones is essential for handling messy real-world data.

### 2.1 `sep` / `delimiter`
Controls what character separates fields (default is `,`).
```python
df = pd.read_csv("data.tsv", sep="\t")     # tab-separated file
df = pd.read_csv("data.csv", sep=";")       # semicolon-separated
```

### 2.2 `header`
Which row to use as column names.
```python
df = pd.read_csv("data.csv", header=0)      # default: first row is header
df = pd.read_csv("data.csv", header=None)   # no header row in file -> columns become 0,1,2...
df = pd.read_csv("data.csv", header=2)      # 3rd row (index 2) is the header
```

### 2.3 `names`
Manually assign column names (often paired with `header=None`).
```python
df = pd.read_csv("data.csv", header=None, names=["id", "name", "score"])
```

### 2.4 `index_col`
Use a specific column as the DataFrame's row index instead of default 0,1,2...
```python
df = pd.read_csv("data.csv", index_col=0)         # first column becomes index
df = pd.read_csv("data.csv", index_col="id")        # named column becomes index
```

### 2.5 `usecols`
Load only specific columns (saves memory on large files).
```python
df = pd.read_csv("data.csv", usecols=["name", "score"])
df = pd.read_csv("data.csv", usecols=[0, 2])         # by position
```

### 2.6 `dtype`
Force specific data types on columns (prevents Pandas' auto-inference mistakes, e.g., zip codes becoming int and dropping leading zeros).
```python
df = pd.read_csv("data.csv", dtype={"id": str, "score": float})
```

### 2.7 `skiprows` / `nrows`
Skip rows at the top, or limit how many rows to load.
```python
df = pd.read_csv("data.csv", skiprows=2)     # skip first 2 lines
df = pd.read_csv("data.csv", skiprows=[0,2]) # skip specific line numbers
df = pd.read_csv("data.csv", nrows=100)       # only load first 100 rows (great for big files)
```

### 2.8 `na_values`
Define extra strings that should be treated as missing/NaN.
```python
df = pd.read_csv("data.csv", na_values=["NA", "missing", "?", -1])
```

### 2.9 `parse_dates`
Auto-convert specified columns into `datetime` objects while reading.
```python
df = pd.read_csv("data.csv", parse_dates=["join_date"])
df.dtypes["join_date"]   # datetime64[ns]
```

### 2.10 `encoding`
Character encoding of the file — needed when a CSV throws `UnicodeDecodeError`.
```python
df = pd.read_csv("data.csv", encoding="utf-8")
df = pd.read_csv("data.csv", encoding="latin-1")   # common fallback for older/Windows files
```

### 2.11 `chunksize` — Reading Large Files in Chunks
> **Definition:** `chunksize` returns an iterator of DataFrames instead of loading the whole file into memory at once — essential when a CSV is too large to fit in RAM.

```python
chunks = pd.read_csv("huge_data.csv", chunksize=10000)
for chunk in chunks:
    process(chunk)     # process 10,000 rows at a time
```

### 2.12 `converters`
Apply a custom function to a column's values while parsing.
```python
df = pd.read_csv("data.csv", converters={"price": lambda x: float(x.replace("$", ""))})
```

### 2.13 `error_bad_lines` / `on_bad_lines` (newer Pandas)
Control what happens when a row has more/fewer fields than expected.
```python
df = pd.read_csv("data.csv", on_bad_lines="skip")   # skip malformed rows instead of erroring
```

---

## 3. Writing Back to CSV

```python
df.to_csv("output.csv")                 # writes with index column by default
df.to_csv("output.csv", index=False)    # exclude the index column (common in practice)
df.to_csv("output.csv", columns=["name", "score"])   # only specific columns
```

---

## 4. Reading From a URL

Pandas can read a CSV directly from a web URL, no manual download needed.
```python
url = "https://raw.githubusercontent.com/user/repo/main/data.csv"
df = pd.read_csv(url)
```

---

## Quick Revision Summary

| Parameter | Purpose |
|---|---|
| `sep` | delimiter character |
| `header` | which row is the column header |
| `names` | manually set column names |
| `index_col` | column to use as row index |
| `usecols` | load only selected columns |
| `dtype` | force column data types |
| `skiprows` / `nrows` | skip/limit rows read |
| `na_values` | custom strings treated as NaN |
| `parse_dates` | auto-parse date columns |
| `encoding` | text encoding of the file |
| `chunksize` | iterate over file in memory-safe chunks |
| `on_bad_lines` | handle malformed rows |

**Interview tip:** `chunksize` and `dtype` are the two parameters most commonly asked about for handling large/messy datasets efficiently.
