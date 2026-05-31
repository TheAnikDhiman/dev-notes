# Machine Learning — Pandas Complete Notes
### By Krish Naik | ML Playlist — Tutorial 5, 6 & 7
---

## Table of Contents
1. [Tutorial 5 — Pandas, Series & DataFrame (Part 1)](#tutorial-5--pandas-series--dataframe-part-1)
2. [Tutorial 6 — Reading CSV Files & Parameters (Part 2)](#tutorial-6--reading-csv-files--parameters-part-2)
3. [Tutorial 7 — Reading JSON, HTML, Pickle, Excel (Part 3)](#tutorial-7--reading-json-html-pickle-excel-part-3)
4. [Additional Knowledge — Data Types, Missing Data & Memory](#additional-knowledge--data-types-missing-data--memory)

---

# Tutorial 5 — Pandas, Series & DataFrame (Part 1)

---

## What is Pandas?

Pandas is Python's most important data manipulation library — it is the core tool for loading,
cleaning, transforming, and exploring data before feeding it into any ML model.

- Built on top of NumPy — every Pandas object is backed by NumPy arrays underneath
- The name comes from **Pan**el **Da**ta — a term from econometrics for multi-dimensional data
- Created by Wes McKinney in 2008 while working at a hedge fund — born from real data needs
- Two core data structures: **Series** (1D) and **DataFrame** (2D)
- Think of Pandas as a programmable Excel — but infinitely more powerful and automatable

**Why Pandas for ML specifically?**
- Raw data NEVER comes ready to use — it has missing values, wrong types, inconsistent formatting
- ML models only accept clean, numerical, properly shaped data
- Pandas is the bridge between raw data and model-ready data
- 80% of an ML engineer's time is data wrangling — Pandas is the tool for all of it

---

## Pandas in the ML Workflow

```
Raw Data (CSV, JSON, Excel, DB)
        ↓
   Load with Pandas
        ↓
   Explore (shape, dtypes, describe)
        ↓
   Clean (missing values, outliers, wrong types)
        ↓
   Transform (encode, scale, feature engineer)
        ↓
   Export to NumPy arrays
        ↓
   Feed into Scikit-learn / TensorFlow
```

Every step except the last two is done in Pandas.

---

## What is a Series?

A Series is a **one-dimensional, labeled array** — like a single column from a spreadsheet.

- Every element has both a **value** and a **label (index)**
- The index is what separates a Series from a plain NumPy array — data is always accessible by label
- Can hold any single data type: integers, floats, strings, booleans, Python objects
- Underlying storage is a NumPy array — so all NumPy speed benefits apply
- A Series is essentially a dict that supports vectorised operations

**Analogy:** A Series is like a column in Excel — each cell has a row label (index) and a value.

---

## Creating a Series

```python
import pandas as pd
import numpy as np

# From a list — default index: 0, 1, 2, ...
s = pd.Series([10, 20, 30, 40])

# From a list with custom index
s = pd.Series([10, 20, 30], index=['a', 'b', 'c'])

# From a dictionary — keys become the index automatically
s = pd.Series({'Mumbai': 20.5, 'Delhi': 25.1, 'Chennai': 30.2})

# From a scalar — same value repeated for each index label
s = pd.Series(5, index=['x', 'y', 'z'])   # [5, 5, 5] with labels x, y, z

# From a NumPy array
s = pd.Series(np.arange(5))
```

---

## Series Index

The index is one of Pandas' most powerful features — it gives meaning to your data.

- Default index: `0, 1, 2, 3, ...` (RangeIndex — memory efficient)
- Custom index: any labels — strings, dates, integers, tuples
- The index is itself a Pandas Index object — immutable and hashable
- **Two ways to access a Series element:**
  - By label: `s['Delhi']` — uses the index label
  - By position: `s.iloc[0]` — uses integer position (like a list)
- If index labels ARE integers, `s[0]` refers to the LABEL 0, not position 0 — this causes confusion

---

## Series Operations

Series inherits all of NumPy's vectorised operations:

```python
s = pd.Series([10, 20, 30, 40])

s + 5          # adds 5 to every element
s * 2          # multiplies every element by 2
s[s > 15]      # boolean filtering — returns [20, 30, 40]
s.mean()       # 25.0
s.sum()        # 100
s.max()        # 40
s.describe()   # count, mean, std, min, 25%, 50%, 75%, max
```

**Alignment — a unique Pandas feature:**
When operating on two Series, Pandas automatically aligns by INDEX label, not by position.

```python
a = pd.Series([1, 2, 3], index=['x', 'y', 'z'])
b = pd.Series([10, 20, 30], index=['y', 'z', 'w'])
a + b
# x    NaN   (x not in b)
# y    22.0  (2 + 20)
# z    23.0  (3 + 30... wait — b['z']=30 + a['z']=3 → but b index is y,z,w so b['z']=20)
# w    NaN   (w not in a)
```

- Pandas aligns on matching labels. Non-matching labels produce `NaN` (Not a Number).
- This automatic alignment is incredibly powerful — no need to manually match rows.

---

## Series Attributes & Methods

| Attribute/Method | Returns |
|-----------------|---------|
| `s.index` | The index labels |
| `s.values` | NumPy array of values (no index) |
| `s.dtype` | Data type of elements |
| `s.shape` | Tuple e.g. `(4,)` |
| `s.size` | Total number of elements |
| `s.name` | Name of the Series |
| `s.head(n)` | First n elements (default 5) |
| `s.tail(n)` | Last n elements (default 5) |
| `s.unique()` | Array of unique values |
| `s.nunique()` | COUNT of unique values |
| `s.value_counts()` | Frequency of each unique value |
| `s.isnull()` | Boolean mask — True where NaN |
| `s.notnull()` | Boolean mask — True where not NaN |
| `s.dropna()` | Remove NaN values |
| `s.fillna(x)` | Replace NaN with value x |
| `s.sort_values()` | Sort by values |
| `s.sort_index()` | Sort by index labels |
| `s.reset_index()` | Reset to default integer index |
| `s.rename('newname')` | Rename the Series |
| `s.astype(type)` | Convert to a different dtype |
| `s.apply(func)` | Apply a function to each element |
| `s.map(dict)` | Map values using a dictionary |

---

## What is a DataFrame?

A DataFrame is a **two-dimensional, labeled data structure** — like a spreadsheet or a SQL table.

- Has both **row labels (index)** and **column labels (columns)**
- Each column is a Series — a DataFrame is essentially a dict of Series sharing the same index
- Columns can have different data types — one column strings, another integers, another floats
- All columns must have the SAME length (same number of rows)
- Most ML datasets are loaded as DataFrames

**Analogy:** A DataFrame is a spreadsheet — rows are observations (samples), columns are features (attributes).

---

## Creating a DataFrame

```python
# From a dict of lists — most common
data = {
    'name': ['Anik', 'Rahul', 'Priya'],
    'age':  [20, 22, 21],
    'score': [88.5, 92.1, 79.4]
}
df = pd.DataFrame(data)

# From a list of dicts — each dict is one row
rows = [
    {'name': 'Anik', 'age': 20},
    {'name': 'Rahul', 'age': 22}
]
df = pd.DataFrame(rows)

# From a 2D NumPy array
arr = np.random.rand(4, 3)
df = pd.DataFrame(arr, columns=['A', 'B', 'C'])

# From a Series
s1 = pd.Series([1, 2, 3], name='col1')
s2 = pd.Series([4, 5, 6], name='col2')
df = pd.concat([s1, s2], axis=1)
```

---

## DataFrame Structure Mentally

```
         name    age   score       ← Column labels
    0    Anik    20    88.5        ← Row 0 (index=0)
    1    Rahul   22    92.1        ← Row 1 (index=1)
    2    Priya   21    79.4        ← Row 2 (index=2)
    ↑
  Index (row labels)
```

- Row direction = **axis 0** (vertical)
- Column direction = **axis 1** (horizontal)
- Knowing which axis is which is critical for operations like `drop`, `sum`, `apply`

---

## Essential DataFrame Attributes

```python
df.shape        # (rows, columns) — e.g. (1000, 15)
df.ndim         # 2 always (DataFrame is 2D)
df.size         # total cells = rows × columns
df.columns      # Index of column names
df.index        # Row index
df.dtypes       # Data type of each column
df.values       # Underlying NumPy array (no labels)
```

---

## Inspecting a DataFrame

These are the FIRST commands you run whenever you load new data — always.

```python
df.info()       # column names, non-null counts, dtypes, memory usage — overview
df.describe()   # statistical summary: count, mean, std, min, quartiles, max
df.head(5)      # first 5 rows — quick sanity check
df.tail(5)      # last 5 rows — check end of data
df.sample(5)    # 5 random rows — unbiased preview
df.shape        # dimensions — know your data size immediately
```

**`df.info()` tells you:**
- Which columns have missing values (non-null count < total rows)
- What dtype each column is (object = string, int64, float64, etc.)
- Memory usage — useful for large datasets

**`df.describe()` tells you:**
- Only runs on numeric columns by default
- `describe(include='all')` → also shows categorical stats (unique, top, freq)
- Look at min/max for outliers. Look at mean vs median (50%) for skew.

---

## Selecting Data from a DataFrame

**Select a single column → returns a Series:**
```python
df['age']           # preferred way
df.age              # dot notation — only works if column name is valid Python identifier
```

**Select multiple columns → returns a DataFrame:**
```python
df[['name', 'score']]   # pass a LIST of column names
```

**Select rows by label with `.loc`:**
```python
df.loc[0]              # row with index label 0
df.loc[0:2]            # rows with labels 0, 1, 2 — INCLUSIVE on both ends
df.loc[0, 'name']      # specific cell: row label 0, column 'name'
df.loc[:, 'age']       # all rows, column 'age' — same as df['age']
df.loc[0:2, ['name', 'score']]  # rows 0-2, specific columns
```

**Select rows by position with `.iloc`:**
```python
df.iloc[0]             # first row by POSITION
df.iloc[0:2]           # rows 0 and 1 — EXCLUSIVE on right end (like Python slicing)
df.iloc[0, 1]          # row position 0, column position 1
df.iloc[:, 2]          # all rows, column at position 2
```

---

## `.loc` vs `.iloc` — Critical Distinction

| | `.loc` | `.iloc` |
|--|--------|---------|
| Stands for | Label-based | Integer position-based |
| Row input | Index LABELS | Integer POSITIONS |
| Slice end | **Inclusive** | **Exclusive** |
| When to use | When index has meaningful labels | When index is positional |
| Column input | Column names (strings) | Column positions (integers) |

> This is one of the most common sources of bugs. When in doubt: `.loc` for names/labels, `.iloc` for numbers/positions.

---

## Boolean / Conditional Filtering

Filter rows based on conditions — the most used operation in data cleaning.

```python
df[df['age'] > 21]                          # rows where age > 21
df[df['name'] == 'Anik']                    # rows where name is 'Anik'
df[(df['age'] > 20) & (df['score'] > 85)]   # AND — both conditions true
df[(df['age'] < 21) | (df['score'] > 90)]   # OR — at least one true
df[~(df['age'] > 21)]                       # NOT — negate the condition
df[df['name'].isin(['Anik', 'Priya'])]      # value is in a list
df[df['score'].between(80, 90)]             # value is in a range
```

> Use `&`, `|`, `~` (NOT `and`, `or`, `not`) with Pandas conditions. Always wrap each condition in `()`.

---

## Adding & Removing Columns

```python
# Add a new column
df['grade'] = ['A', 'A', 'B']              # from a list
df['pass'] = df['score'] > 75             # derived from condition — boolean column
df['score_scaled'] = df['score'] / 100    # derived mathematically

# Remove columns
df.drop('grade', axis=1)                  # axis=1 → dropping a column
df.drop(['grade', 'pass'], axis=1)        # drop multiple
df.drop('grade', axis=1, inplace=True)    # modify original — no new variable needed
```

> `inplace=True` modifies the original DataFrame directly. Without it, the result is returned but original stays unchanged. Use with care — can't undo without a backup.

---

## Modifying the Index

```python
df.set_index('name')           # make 'name' column the row index
df.reset_index()               # revert to default 0, 1, 2 integer index
df.rename(columns={'age': 'years', 'score': 'marks'})   # rename columns
df.rename(index={0: 'first', 1: 'second'})              # rename row labels
```

---

# Tutorial 6 — Reading CSV Files & Parameters (Part 2)

---

## What is a CSV File?

CSV = **Comma-Separated Values** — the most universal format for tabular data.

- Plain text file where each line is a row and values are separated by a delimiter (usually comma)
- First row is usually the header (column names)
- No data types stored — everything is plain text until you parse it
- Universal — any software (Excel, Python, R, SQL) can read it
- Large datasets: hundreds of MB or GBs are common in ML

**Why CSV is so common in ML:**
- Kaggle datasets are almost always CSV
- Easy to inspect — open in any text editor
- No proprietary format — no compatibility issues
- Lightweight — no metadata, no formatting

---

## `pd.read_csv()` — The Most Used Pandas Function

Reading a CSV is always the starting point. `read_csv()` has 50+ parameters — knowing the key ones separates beginners from practitioners.

```python
df = pd.read_csv('filename.csv')
```

---

## Key Parameters of `read_csv()`

### `filepath_or_buffer`
- Can be a file path (string), a URL (direct download), or a file-like object
- URLs work directly: `pd.read_csv('https://example.com/data.csv')`

---

### `sep` / `delimiter`
The character that separates values. Default: `,`

```python
pd.read_csv('data.tsv', sep='\t')      # tab-separated
pd.read_csv('data.txt', sep=';')       # semicolon-separated
pd.read_csv('data.txt', sep='\s+')     # any whitespace (regex)
```

- Real-world data often uses tabs, semicolons, pipes (`|`) instead of commas
- `sep=None, engine='python'` → Pandas tries to detect the separator automatically

---

### `header`
Which row to use as column names. Default: `0` (first row).

```python
pd.read_csv('data.csv', header=None)   # no header row — Pandas assigns 0, 1, 2...
pd.read_csv('data.csv', header=2)      # treat row 2 as the header
```

- Use `header=None` when your file has no column names row

---

### `names`
Provide custom column names — used together with `header=None`.

```python
pd.read_csv('data.csv', header=None, names=['id', 'name', 'score'])
```

- If file has a header row but you want to rename: use `header=0, names=[...]`
- `names` list length must match number of columns in file

---

### `index_col`
Which column to use as the row index.

```python
pd.read_csv('data.csv', index_col=0)          # first column as index
pd.read_csv('data.csv', index_col='user_id')  # named column as index
pd.read_csv('data.csv', index_col=False)      # prevent using first col as index
```

- Useful when your data has a meaningful ID column you want as the index
- Without this, Pandas creates a default 0, 1, 2 index AND keeps the ID as a regular column

---

### `usecols`
Load only specific columns — saves memory when file has many columns.

```python
pd.read_csv('data.csv', usecols=['name', 'score'])       # by column names
pd.read_csv('data.csv', usecols=[0, 1, 4])              # by column positions
pd.read_csv('data.csv', usecols=lambda x: x != 'id')   # by condition
```

> Critical for large datasets. If a file has 200 columns and you need 10 — load only those 10. Reduces memory use by 95%.

---

### `dtype`
Force specific data types on columns during loading.

```python
pd.read_csv('data.csv', dtype={'id': int, 'score': float, 'zip_code': str})
```

- Without this: Pandas infers dtypes — sometimes wrong (e.g., zip codes read as integers lose leading zeros)
- Forcing dtype at load time is more efficient than converting after loading
- Force numeric columns that have mixed data to `str` first — clean, then convert

---

### `na_values`
Define custom strings that should be treated as NaN (missing values).

```python
pd.read_csv('data.csv', na_values=['N/A', 'null', '-', 'none', '?', ''])
```

**Default NaN values Pandas already recognises:**
`NaN`, `NA`, `null`, `None`, `n/a`, `N/A`, empty string, `#N/A`

- Real datasets use all kinds of placeholders: `?`, `-`, `99999`, `unknown`
- Always check your data's documentation for how missing data is encoded

---

### `keep_default_na`
Control whether to use Pandas' default NaN recognisers.

```python
pd.read_csv('data.csv', keep_default_na=False, na_values=['missing'])
# ONLY 'missing' is treated as NaN — nothing else
```

---

### `skiprows`
Skip rows at the beginning of the file.

```python
pd.read_csv('data.csv', skiprows=3)          # skip first 3 rows
pd.read_csv('data.csv', skiprows=[0, 2, 5])  # skip specific row numbers
pd.read_csv('data.csv', skiprows=lambda i: i % 2 == 0)  # skip even rows
```

- Useful when files have metadata or comments before the actual data begins
- Commonly seen in exported financial or scientific data files

---

### `skipfooter`
Skip rows at the END of the file.

```python
pd.read_csv('data.csv', skipfooter=2, engine='python')
```

- `engine='python'` is required when using `skipfooter`
- Use when files have summary rows or footnotes at the bottom

---

### `nrows`
Load only the first n rows.

```python
pd.read_csv('data.csv', nrows=1000)   # load only first 1000 rows
```

- Essential for quickly previewing a large file without loading it all into memory
- First thing to do with a new large dataset — `nrows=100` to explore structure

---

### `chunksize`
Read the file in chunks of n rows at a time — returns an iterator.

```python
for chunk in pd.read_csv('big_file.csv', chunksize=10000):
    process(chunk)    # work on 10000 rows at a time
```

**Why chunking?**
- A 10GB file cannot fit in 8GB RAM
- With `chunksize`, you load and process piece by piece
- Each chunk is a normal DataFrame — process it, then discard, load next
- Used for: large data aggregations, batch ML predictions, database ETL pipelines

---

### `encoding`
The character encoding of the file. Default: `utf-8`

```python
pd.read_csv('data.csv', encoding='utf-8')        # most modern files
pd.read_csv('data.csv', encoding='latin-1')      # Windows European files
pd.read_csv('data.csv', encoding='cp1252')       # Windows ANSI
pd.read_csv('data.csv', encoding='ISO-8859-1')   # older European files
```

**Common scenario:** You load a CSV and get `UnicodeDecodeError` → file uses a non-UTF-8 encoding.
Try `latin-1` next — it works for most Western European encoded files.

---

### `parse_dates`
Automatically parse columns as datetime objects.

```python
pd.read_csv('data.csv', parse_dates=['date'])
pd.read_csv('data.csv', parse_dates=['year', 'month', 'day'])  # combine columns
pd.read_csv('data.csv', parse_dates=True)   # try to parse index as date
```

- Without this, dates are loaded as plain strings (object type)
- Once parsed as datetime, you can extract year, month, day, do time arithmetic
- Pandas is very good at guessing date formats — `dayfirst=True` if day comes before month

---

### `infer_datetime_format`
Speed up date parsing by inferring the format once and reusing it.

```python
pd.read_csv('data.csv', parse_dates=['date'], infer_datetime_format=True)
```

---

### `true_values` / `false_values`
Map custom string values to Python booleans.

```python
pd.read_csv('data.csv', true_values=['yes', 'Yes', 'TRUE'], false_values=['no', 'No', 'FALSE'])
```

---

### `comment`
Ignore everything after a specific character on a line.

```python
pd.read_csv('data.csv', comment='#')   # lines starting with # are ignored
```

---

### `squeeze` → deprecated in newer Pandas
If CSV has only one column, return a Series instead of DataFrame.

---

### `low_memory`
Controls how Pandas reads the file internally.

```python
pd.read_csv('data.csv', low_memory=False)
```

- Default `low_memory=True` → reads file in chunks to infer dtypes, can give mixed-type warnings
- `low_memory=False` → reads whole file at once for dtype inference — more accurate, uses more memory
- Use `low_memory=False` to silence mixed-type warnings, or better: specify `dtype` explicitly

---

### `memory_map`
Map file directly into memory — faster for large files.

```python
pd.read_csv('data.csv', memory_map=True)
```

---

## Writing a CSV

```python
df.to_csv('output.csv', index=False)   # index=False — don't write row numbers as a column
df.to_csv('output.csv', sep='\t')      # write as tab-separated
df.to_csv('output.csv', encoding='utf-8', na_rep='NULL')  # custom NA representation
```

> Always use `index=False` when saving unless your index has meaningful labels. Otherwise you get an unwanted column of row numbers when you reload.

---

# Tutorial 7 — Reading JSON, HTML, Pickle, Excel (Part 3)

---

## Why Multiple File Formats?

Different sources store data differently:

| Format | Common Source | Best For |
|--------|--------------|---------|
| CSV | Kaggle, databases, spreadsheets | Tabular data, universal compatibility |
| JSON | APIs, web scraping, NoSQL | Nested/hierarchical data |
| HTML | Web pages, Wikipedia tables | Quick web scraping |
| Excel | Business reports, finance | Formatted spreadsheets with multiple sheets |
| Pickle | Python workflows, model saving | Python-specific fast serialisation |
| Parquet | Big data, cloud storage | Large-scale analytics, columnar storage |
| SQL | Relational databases | Production data |

---

## Reading JSON Files

JSON = **JavaScript Object Notation** — standard format for APIs and web data.

**JSON vs CSV:**
- CSV is flat/tabular. JSON can be nested — objects inside objects, arrays inside objects.
- JSON preserves data types (numbers are numbers, not strings).
- JSON is the native format of REST APIs.

```python
df = pd.read_json('data.json')
df = pd.read_json('https://api.example.com/data')   # read directly from URL
```

---

## JSON Orientations — Critical Concept

JSON can be structured in many ways. The `orient` parameter tells Pandas how your JSON is shaped.

| Orient | JSON Structure | Description |
|--------|---------------|-------------|
| `'records'` | `[{col: val}, {col: val}]` | List of dicts — most common from APIs |
| `'columns'` | `{col: {index: val}}` | Default Pandas to_json format |
| `'index'` | `{index: {col: val}}` | Row-oriented with index as outer key |
| `'values'` | `[[val, val], [val, val]]` | Just the values, no labels |
| `'split'` | `{index:[..], columns:[..], data:[..]}` | Separated index, columns, data |
| `'table'` | `{schema: {...}, data: [...]}` | Full schema + data |

```python
# API response typically in 'records' format
df = pd.read_json('api_response.json', orient='records')
```

> When loading JSON from an API, always check the structure first. `orient='records'` handles the most common API response pattern.

---

## Handling Nested JSON

APIs often return deeply nested JSON — Pandas' `json_normalize` flattens it.

```python
from pandas import json_normalize
import json

with open('nested.json') as f:
    data = json.load(f)

df = json_normalize(data)               # flatten one level
df = json_normalize(data, record_path='results')   # extract a nested list
df = json_normalize(data, sep='_')      # nested keys become 'parent_child'
```

**Example — what json_normalize does:**
```
Before: {'user': {'name': 'Anik', 'age': 20}, 'score': 88}
After:  {'user_name': 'Anik', 'user_age': 20, 'score': 88}
```

> In real ML projects, most API data is nested JSON. `json_normalize` is essential.

---

## Writing JSON

```python
df.to_json('output.json', orient='records', indent=2)
```

- `orient='records'` → most readable, compatible with APIs
- `indent=2` → pretty-printed with 2-space indentation (human readable)

---

## Reading HTML Tables

`pd.read_html()` scrapes ALL tables from a webpage and returns a **list of DataFrames**.

```python
tables = pd.read_html('https://en.wikipedia.org/wiki/List_of_countries')
df = tables[0]   # first table on the page
```

**Key facts:**
- Requires `lxml` or `html5lib` or `beautifulsoup4` installed
- Returns a LIST — always index into it even if there's only one table
- Very useful for: Wikipedia data, sports stats, financial tables, government data
- Data quality varies — HTML tables are messy, always clean after scraping

---

## Parameters for `read_html()`

```python
tables = pd.read_html(
    url,
    match='Population',     # only return tables containing this text
    header=0,               # which row is the header
    index_col=0,            # column to use as index
    skiprows=2,             # skip rows
    attrs={'id': 'mytable'}  # select table by HTML attribute
)
```

- `match` is very useful — filter to only tables containing specific text
- `attrs` lets you target a specific table by its HTML `id`, `class`, etc.

---

## Reading Pickle Files

Pickle is Python's native object serialisation — saves ANY Python object to disk exactly as-is.

**What is Pickle?**
- Serialisation = converting a Python object to a byte stream for storage
- De-serialisation = loading that byte stream back into a Python object
- Pickle preserves the EXACT Python object — types, index, dtypes, everything
- File extension: `.pkl` or `.pickle`

```python
# Save a DataFrame as Pickle
df.to_pickle('data.pkl')

# Load it back — exactly the same DataFrame, no parsing needed
df = pd.read_pickle('data.pkl')
```

**Why use Pickle?**
- **Speed** — no parsing, no type inference — just load the object directly. Much faster than CSV for large DataFrames.
- **Preserves types** — dtypes, index, categorical columns all preserved. No re-converting after load.
- **Saves ML models** — `pickle.dump(model, file)` to save trained Scikit-learn models
- **Saves preprocessing pipelines** — save your fitted scaler/encoder along with the model

**Pickle limitations:**
- Not human readable — can't open in Excel or text editor
- Python-specific — can't use in R, Excel, SQL
- Version issues — Pickle files from an old Python/library version may not load in newer versions
- **Security risk** — NEVER load a Pickle file from an untrusted source (it can execute arbitrary code)

---

## Pickle for Saving ML Models

This is the most important use of Pickle in ML:

```python
import pickle

# After training your model
with open('model.pkl', 'wb') as f:
    pickle.dump(model, f)

# To load and use
with open('model.pkl', 'rb') as f:
    loaded_model = pickle.load(f)

predictions = loaded_model.predict(X_test)
```

- `'wb'` = write binary mode. `'rb'` = read binary mode. Always binary for Pickle.
- Alternative: `joblib` library — better than pickle for large NumPy arrays (used in Scikit-learn)

---

## Reading Excel Files

Excel is the most common format in business and finance.

```python
df = pd.read_excel('data.xlsx')                      # first sheet
df = pd.read_excel('data.xlsx', sheet_name='Sales') # specific sheet by name
df = pd.read_excel('data.xlsx', sheet_name=0)        # first sheet by position
df = pd.read_excel('data.xlsx', sheet_name=None)     # ALL sheets → returns dict of DataFrames
```

- Requires: `pip install openpyxl` (for .xlsx) or `pip install xlrd` (for .xls old format)
- `sheet_name=None` → loads every sheet as a dictionary: `{'Sheet1': df1, 'Sheet2': df2}`

---

## Parameters Shared with `read_csv()`

These parameters work EXACTLY the same as in `read_csv()`:

```python
pd.read_excel(
    'data.xlsx',
    header=0,              # which row is the header
    index_col=0,           # which column is the index
    usecols='A:E',         # Excel column range (A to E)
    usecols=[0, 1, 4],     # by position
    skiprows=3,            # skip first 3 rows
    nrows=100,             # load only 100 rows
    dtype={'id': int},     # force dtypes
    na_values=['N/A']      # custom NaN strings
)
```

- `usecols='A:E'` is Excel-style column range — unique to `read_excel()`

---

## Writing Excel

```python
df.to_excel('output.xlsx', index=False, sheet_name='Results')

# Write multiple sheets to same file
with pd.ExcelWriter('output.xlsx') as writer:
    df1.to_excel(writer, sheet_name='Sales', index=False)
    df2.to_excel(writer, sheet_name='Inventory', index=False)
    df3.to_excel(writer, sheet_name='Summary', index=False)
```

- `ExcelWriter` is a context manager — all writes to the same file, one `save()` at the end
- Requires `openpyxl`

---

## Other Important File Formats (Additional Knowledge)

---

## Parquet Files

Parquet is a **columnar storage format** — the industry standard for big data.

```python
df = pd.read_parquet('data.parquet')
df.to_parquet('data.parquet', index=False)
```

**Why Parquet is better than CSV for large data:**
- **Columnar** → stored column by column (not row by row). Reading only 5 columns from 500-column file reads only 5/500 of the data.
- **Compressed** → automatically compressed — a 1GB CSV might be 100MB in Parquet
- **Type-preserving** → dtypes are stored — no re-inference on load
- **Splittable** → easy to read in parallel across multiple machines
- Used by: AWS S3, Google BigQuery, Apache Spark, Databricks

> For production ML pipelines with large datasets — always use Parquet, not CSV.

---

## SQL Databases

```python
import sqlalchemy
engine = sqlalchemy.create_engine('postgresql://user:pass@localhost/db')

df = pd.read_sql('SELECT * FROM users WHERE active = true', engine)
df = pd.read_sql_table('users', engine)
df = pd.read_sql_query('SELECT id, name FROM users LIMIT 100', engine)

df.to_sql('new_table', engine, if_exists='replace', index=False)
```

- `if_exists='replace'` → drop and recreate table
- `if_exists='append'` → add to existing table
- `if_exists='fail'` → raise error if table exists (default)

---

# Additional Knowledge — Data Types, Missing Data & Memory

---

## Pandas Data Types — Deep Dive

Understanding dtypes is essential — wrong dtypes waste memory and break models.

| Pandas dtype | Python equivalent | Used for |
|-------------|------------------|---------|
| `int64` | int | Whole numbers (default integer) |
| `float64` | float | Decimal numbers (default float) |
| `object` | str (usually) | Text data, mixed types |
| `bool` | bool | True/False |
| `datetime64[ns]` | datetime | Dates and timestamps |
| `timedelta64[ns]` | timedelta | Time differences/durations |
| `category` | Categorical | Low-cardinality text (ML friendly) |
| `Int64` | nullable int | Integers WITH NaN support |
| `Float32` | float32 | Memory-efficient float |

**`object` dtype:**
- Catch-all type — Pandas uses it for strings, mixed types, anything it can't categorise
- Least memory efficient — stores Python objects with all their overhead
- If a column SHOULD be numeric but has a few non-numeric values, it becomes `object`

**`category` dtype:**
- Perfect for columns with few repeated values (e.g., gender, country, size: S/M/L/XL)
- Stores values as integers internally, maps to strings — huge memory savings
- Required for ordinal encoding, efficient groupby operations

```python
df['size'] = df['size'].astype('category')
```

---

## Missing Data — The Most Important Data Quality Issue

Missing data is the norm in real-world ML datasets — almost no dataset is complete.

**Types of missing data (from statistics):**
- **MCAR** (Missing Completely At Random) → no pattern — safe to drop or impute
- **MAR** (Missing At Random) → missing depends on OTHER observed variables — impute carefully
- **MNAR** (Missing Not At Random) → missing depends on the missing value itself — hardest case

**Why missing data matters for ML:**
- Most ML algorithms CANNOT handle NaN — will crash or give wrong results
- The strategy you choose for handling missing data significantly affects model performance
- Always understand WHY data is missing before deciding how to handle it

---

## Detecting Missing Data

```python
df.isnull()               # DataFrame of True/False — True where NaN
df.isnull().sum()         # count of nulls PER column
df.isnull().sum() / len(df) * 100  # percentage missing per column

df.notnull()              # opposite — True where NOT null
df.info()                 # shows non-null count per column at a glance

# Find rows where ANY column has null
df[df.isnull().any(axis=1)]

# Find rows where ALL values are null
df[df.isnull().all(axis=1)]
```

---

## Handling Missing Data — Strategies

**Option 1: Drop rows/columns with missing data**
```python
df.dropna()                    # drop rows with ANY null
df.dropna(how='all')           # only drop rows where ALL values are null
df.dropna(subset=['score'])    # only drop rows where 'score' is null
df.dropna(axis=1)              # drop COLUMNS with any null
df.dropna(thresh=3)            # keep rows with at least 3 non-null values
```
- Use when: missing data is < 5% and MCAR
- Don't use: if missing data carries information (MNAR)

**Option 2: Fill/Impute missing data**
```python
df.fillna(0)                           # fill all NaN with 0
df['age'].fillna(df['age'].mean())     # fill with column mean
df['city'].fillna(df['city'].mode()[0]) # fill with most frequent value
df['score'].fillna(method='ffill')     # forward fill — use previous row's value
df['score'].fillna(method='bfill')     # backward fill — use next row's value
df.fillna(df.median())                 # fill each column with its median
```
- **Mean imputation** → for normally distributed numeric data
- **Median imputation** → for skewed numeric data (more robust to outliers)
- **Mode imputation** → for categorical data
- **Forward/backward fill** → for time-series data where adjacent values are related

**Option 3: Interpolation (for time-series)**
```python
df['temperature'].interpolate(method='linear')   # estimate missing values between known points
```

> Rule of thumb: >50% missing in a column → consider dropping the column. 5-50% → impute carefully. <5% → drop rows or impute, either is fine.

---

## Memory Optimization — Essential for Large Datasets

By default, Pandas uses conservative (large) dtypes. You can significantly reduce memory usage.

```python
# Check memory usage
df.memory_usage(deep=True)          # memory per column in bytes
df.memory_usage(deep=True).sum()    # total

# Convert integers to smaller types
df['age'] = df['age'].astype('int8')      # if values -128 to 127
df['count'] = df['count'].astype('int16') # if values -32768 to 32767
df['id'] = df['id'].astype('int32')       # if values up to ~2 billion

# Convert floats to smaller types
df['price'] = df['price'].astype('float32')   # half the memory of float64

# Convert low-cardinality strings to category
df['country'] = df['country'].astype('category')    # massive savings

# Typical result: 50-70% memory reduction on a real dataset
```

**Integer ranges (for choosing the right type):**

| dtype | Range | Memory |
|-------|-------|--------|
| `int8` | -128 to 127 | 1 byte |
| `int16` | -32,768 to 32,767 | 2 bytes |
| `int32` | ~-2.1B to ~2.1B | 4 bytes |
| `int64` | very large | 8 bytes (default) |
| `float32` | 7 decimal digits | 4 bytes |
| `float64` | 15 decimal digits | 8 bytes (default) |

---

## Useful Pandas Utility Functions

```python
pd.to_numeric(df['col'], errors='coerce')    # convert to number; invalid → NaN
pd.to_datetime(df['date'])                   # convert to datetime
pd.cut(df['age'], bins=[0,18,35,60,100])     # bin numeric data into categories
pd.qcut(df['score'], q=4)                    # bin into equal-frequency quartiles
pd.get_dummies(df['color'])                  # one-hot encode a categorical column

df['col'].str.lower()       # string operations via .str accessor
df['col'].str.strip()       # remove leading/trailing whitespace
df['col'].str.contains('x') # boolean — does string contain 'x'
df['col'].str.split(',')    # split string by delimiter
df['col'].dt.year           # extract year from datetime via .dt accessor
df['col'].dt.month          # extract month
df['col'].dt.dayofweek      # 0=Monday, 6=Sunday
```

---

## Quick Reference — File Reading Functions

| Function | File Type | Key Parameters |
|----------|-----------|---------------|
| `pd.read_csv()` | .csv, .tsv, .txt | sep, header, usecols, dtype, na_values, chunksize, encoding, parse_dates |
| `pd.read_json()` | .json | orient, lines, dtype |
| `pd.read_html()` | .html, URL | match, header, attrs, index_col |
| `pd.read_pickle()` | .pkl | compression |
| `pd.read_excel()` | .xlsx, .xls | sheet_name, usecols, skiprows |
| `pd.read_parquet()` | .parquet | columns, engine |
| `pd.read_sql()` | SQL DB | query, connection engine |

---

## Quick Reference — Series vs DataFrame

| Operation | Series | DataFrame |
|-----------|--------|-----------|
| Select | `s[label]` | `df['col']` or `df[['col1','col2']]` |
| By label | `s.loc[label]` | `df.loc[row, col]` |
| By position | `s.iloc[n]` | `df.iloc[row, col]` |
| Filter | `s[s > 10]` | `df[df['col'] > 10]` |
| Stats | `s.describe()` | `df.describe()` |
| Missing | `s.isnull()` | `df.isnull().sum()` |
| Apply fn | `s.apply(fn)` | `df.apply(fn)` or `df['col'].apply(fn)` |
| Shape | `s.shape → (n,)` | `df.shape → (r, c)` |
| Type | `s.dtype` | `df.dtypes` |

---

## Where Pandas Fits in the Full ML Pipeline

```
pd.read_csv()          → Load raw data
df.info() / describe() → Understand data
df.isnull()            → Find missing data
df.fillna() / dropna() → Handle missing data
df.astype()            → Fix data types
df[condition]          → Filter bad rows
df['new'] = ...        → Feature engineering
pd.get_dummies()       → Encode categories
df.to_numpy()          → Convert to NumPy
sklearn.fit(X, y)      → Train the model
```

> Every ML project follows this pipeline. Pandas covers everything from load to the final `.to_numpy()` call.

---
*Notes by Anik | Krish Naik — Machine Learning Playlist (Tutorial 5, 6, 7)*