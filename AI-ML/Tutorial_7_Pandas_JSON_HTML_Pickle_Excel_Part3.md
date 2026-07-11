# Tutorial 7 — Pandas: Reading JSON, HTML, Pickle & Excel Files (Part 3)

## 1. Reading JSON

> **Definition:** **JSON** (JavaScript Object Notation) is a lightweight, text-based, key-value data format widely used by web APIs. `pd.read_json()` converts JSON data directly into a DataFrame.

```python
import pandas as pd

df = pd.read_json("data.json")
```

### 1.1 Key Parameters
```python
# orient: tells pandas how the JSON is structured
df = pd.read_json("data.json", orient="records")  # list of {col: value} dicts (most common)
df = pd.read_json("data.json", orient="columns")   # {col: {index: value}} - default
df = pd.read_json("data.json", orient="index")      # {index: {col: value}}
df = pd.read_json("data.json", orient="split")       # {"columns":[...], "index":[...], "data":[...]}
```

### 1.2 Reading JSON from a URL / API response
```python
url = "https://api.example.com/data.json"
df = pd.read_json(url)

# if the API returns raw JSON text (e.g. via requests library)
import requests, json
response = requests.get(url)
df = pd.json_normalize(response.json())   # flattens nested JSON into a flat table
```

### 1.3 `json_normalize` for Nested JSON
> **Definition:** `json_normalize()` flattens semi-structured (nested) JSON — like a list of dictionaries containing nested dictionaries — into a flat table, which `read_json()` alone often can't handle well.

```python
data = [
    {"name": "Anik", "address": {"city": "Noida", "pin": 201301}},
    {"name": "Riya", "address": {"city": "Delhi", "pin": 110001}}
]
df = pd.json_normalize(data)
# columns become: name, address.city, address.pin
```

### 1.4 Writing to JSON
```python
df.to_json("output.json")
df.to_json("output.json", orient="records", lines=True)  # one JSON object per line (JSON Lines format)
```

---

## 2. Reading HTML Tables

> **Definition:** `pd.read_html()` scans an HTML page (or raw HTML string) for `<table>` elements and returns a **list of DataFrames**, one per table found — useful for quickly scraping tabular web data (e.g., Wikipedia tables).

```python
tables = pd.read_html("https://en.wikipedia.org/wiki/List_of_countries_by_population")
df = tables[0]     # read_html always returns a LIST -> pick the table you need

print(len(tables))  # how many tables were found on the page
```

### 2.1 Key Parameters
```python
tables = pd.read_html(url, match="Population")       # only tables containing this text
tables = pd.read_html(url, header=0)                     # specify header row
tables = pd.read_html(url, attrs={"id": "table1"})        # target table by HTML attribute
```

**Note:** `read_html` requires the `lxml`, `html5lib`, and `beautifulsoup4` packages installed. It only works for actual `<table>` HTML tags — it can't scrape non-tabular layouts.

### 2.2 Writing to HTML
```python
df.to_html("output.html")
df.to_html("output.html", index=False)
```

---

## 3. Reading Pickle Files

> **Definition:** **Pickle** is Python's native binary serialization format — it saves a Python object (like a DataFrame, with its exact dtypes and index) to disk exactly as-is, and loads it back without any parsing/type-inference overhead, making it faster than CSV for pure Python-to-Python workflows.

```python
df.to_pickle("data.pkl")           # save DataFrame as pickle
df2 = pd.read_pickle("data.pkl")   # load it back exactly as it was
```

**Why use Pickle over CSV?**
- Preserves exact dtypes, index, and even nested Python objects (CSV round-trips everything as text/strings).
- Much faster read/write for large DataFrames.
- **Downside:** Not human-readable, not portable across non-Python systems, and unpickling untrusted files is a **security risk** (can execute arbitrary code).

```python
import pickle
with open("data.pkl", "wb") as f:
    pickle.dump(df, f)             # low-level equivalent using the pickle module directly

with open("data.pkl", "rb") as f:
    df3 = pickle.load(f)
```

---

## 4. Reading Excel Files

> **Definition:** `pd.read_excel()` reads spreadsheet data from `.xls`/`.xlsx` files into a DataFrame, supporting multiple sheets within a single workbook. Requires the `openpyxl` (for .xlsx) or `xlrd` (for legacy .xls) package installed.

```python
df = pd.read_excel("data.xlsx")                  # reads the FIRST sheet by default
df = pd.read_excel("data.xlsx", sheet_name="Sheet2")   # specific sheet by name
df = pd.read_excel("data.xlsx", sheet_name=1)            # specific sheet by position (0-indexed)
df = pd.read_excel("data.xlsx", sheet_name=None)          # dict of ALL sheets: {sheet_name: df}
```

### 4.1 Key Parameters (similar to `read_csv`)
```python
df = pd.read_excel("data.xlsx", header=0)                # header row
df = pd.read_excel("data.xlsx", usecols="A:C")            # Excel-style column range
df = pd.read_excel("data.xlsx", usecols=[0, 2])            # by index
df = pd.read_excel("data.xlsx", skiprows=2, nrows=50)       # skip/limit rows
df = pd.read_excel("data.xlsx", na_values=["N/A", "-"])       # custom NaN markers
```

### 4.2 Writing to Excel
```python
df.to_excel("output.xlsx", index=False)

# writing multiple DataFrames to different sheets in ONE file
with pd.ExcelWriter("output.xlsx") as writer:
    df1.to_excel(writer, sheet_name="Sales", index=False)
    df2.to_excel(writer, sheet_name="Inventory", index=False)
```

---

## 5. Quick Comparison — All File Formats Covered

| Format | Read Function | Write Function | Notes |
|---|---|---|---|
| CSV | `pd.read_csv()` | `df.to_csv()` | Human-readable, universal, text-only |
| JSON | `pd.read_json()` | `df.to_json()` | Web/API standard, key-value structured |
| HTML | `pd.read_html()` | `df.to_html()` | Returns a LIST of tables; needs lxml/bs4 |
| Pickle | `pd.read_pickle()` | `df.to_pickle()` | Fastest, preserves dtypes, Python-only, not portable |
| Excel | `pd.read_excel()` | `df.to_excel()` | Multi-sheet support via `sheet_name` / `ExcelWriter` |

---

## Quick Revision Summary
- `read_json(orient=...)` — orientation must match how the JSON is structured; use `json_normalize()` for nested JSON.
- `read_html()` always returns a **list** of DataFrames — you must index into it (`tables[0]`).
- Pickle preserves exact Python object state and is fastest, but is not human-readable/portable and unpickling untrusted files is unsafe.
- `read_excel(sheet_name=None)` loads every sheet as a dictionary of DataFrames.
- All these readers share common parameters with `read_csv` (`header`, `usecols`, `skiprows`, `na_values`) — learn `read_csv` well and the rest transfer easily.
