# Build 12 Data Science Apps with Python and Streamlit
**By Chanin Nantasenamat (Data Professor) | freeCodeCamp | ~3 Hours**

> 🔗 Course: [youtube.com/watch?v=JwSS70SZdyM](https://youtu.be/JwSS70SZdyM)
> 💻 Code Repo: [github.com/dataprofessor/streamlit_freecodecamp](https://github.com/dataprofessor/streamlit_freecodecamp)

---

## Table of Contents
0. [Introduction — What is Streamlit?](#0-introduction--what-is-streamlit)
1. [App 1 — Simple Stock Price](#1-app-1--simple-stock-price-254)
2. [App 2 — Simple Bioinformatics DNA Count](#2-app-2--simple-bioinformatics-dna-count-1324)
3. [App 3 — EDA Basketball](#3-app-3--eda-basketball-2944)
4. [App 4 — EDA Football](#4-app-4--eda-football-5039)
5. [App 5 — EDA S&P 500 Stock Price](#5-app-5--eda-sp500-stock-price-10048)
6. [App 6 — EDA Cryptocurrency](#6-app-6--eda-cryptocurrency-12403)
7. [App 7 — Classification Iris](#7-app-7--classification-iris-15047)
8. [App 8 — Classification Penguins](#8-app-8--classification-penguins-15858)
9. [App 9 — Regression Boston Housing](#9-app-9--regression-boston-housing-21608)
10. [App 10 — Regression Bioinformatics Solubility](#10-app-10--regression-bioinformatics-solubility-22753)
11. [App 11 — Deploy to Heroku](#11-app-11--deploy-to-heroku-25427)
12. [App 12 — Deploy to Streamlit Sharing](#12-app-12--deploy-to-streamlit-sharing-30437)
13. [Streamlit Widget Cheatsheet](#13-streamlit-widget-cheatsheet)

---

# 0. Introduction — What is Streamlit?

## 📖 Theory

Traditional data science workflows have a gap: you build something powerful in Python (a trained ML model, an EDA script, a visualization), but sharing it with non-technical stakeholders requires either a Jupyter notebook (messy, requires Python knowledge) or building a full web app (requires frontend skills — HTML, CSS, JS). Streamlit closes this gap completely.

Streamlit is a Python library that **turns a plain Python script into an interactive web app with almost no extra code**. There's no frontend, no server configuration, no routing. You write Python top-to-bottom, and Streamlit re-runs the entire script every time a user interacts with a widget. This makes it the fastest way to demo data science work — a model that took weeks to build can be wrapped in a shareable web app in under an hour.

---

## What is Streamlit?
- An **open-source Python library** for building interactive data science and ML web apps.
- Write pure Python — Streamlit handles the entire frontend (HTML, CSS, JS) behind the scenes.
- Auto-reruns the script top-to-bottom on every user interaction (widget change, button press).
- Built-in support for: data tables, charts, maps, file uploads, sliders, dropdowns, and more.

## Installation & Running

```bash
# Install
pip install streamlit

# Run your app
streamlit run app.py

# App opens automatically at http://localhost:8501
```

## Core Execution Model

```
User interacts with widget
        ↓
Streamlit re-runs entire script from top
        ↓
New output is rendered in the browser
```

This is different from a traditional web framework. There are no callbacks or event handlers — the whole script just runs again. This simplicity is both Streamlit's greatest strength and the thing to keep in mind when debugging.

## Essential Streamlit Commands

```python
import streamlit as st

# Text display
st.title("My App")
st.header("Section Header")
st.subheader("Sub Header")
st.text("Plain text")
st.markdown("**Bold**, *italic*, `code`")
st.write("Magic function — handles text, dataframes, plots, anything")

# Data display
st.dataframe(df)          # interactive sortable table
st.table(df)              # static table
st.json({"key": "value"}) # formatted JSON

# Layout
st.sidebar.title("Sidebar")   # everything prefixed st.sidebar.* goes to sidebar
st.columns([1, 2])            # split layout into columns
st.expander("Click to expand")

# Media
st.image("image.png", caption="Caption")
st.video("video.mp4")

# Status
st.success("Done!")
st.warning("Watch out!")
st.error("Something failed!")
st.info("FYI")
st.spinner("Loading...")

# Cache (important for performance)
@st.cache_data
def load_data():
    return pd.read_csv("large_file.csv")  # only runs once, then cached
```

## Project Setup (Every App)

```bash
# 1. Create project folder
mkdir app_name && cd app_name

# 2. Create virtual environment
python -m venv venv
source venv/bin/activate   # Mac/Linux
venv\Scripts\activate      # Windows

# 3. Create the app file
touch app.py

# 4. Install dependencies
pip install streamlit pandas numpy ...

# 5. Create requirements.txt
pip freeze > requirements.txt

# 6. Run
streamlit run app.py
```

---

# 1. App 1 — Simple Stock Price `(2:54)`

## 📖 Theory

This first app introduces two key ideas: fetching **live data from an external API** and rendering it as an **interactive chart** — all in under 20 lines of Python. Financial data (stock prices) is time-series data — a sequence of values indexed by date. The `yfinance` library wraps Yahoo Finance's API, making it trivially easy to download historical price data for any listed company. The `st.line_chart()` call then renders that data as an interactive chart. The point of this app is to see how little code is needed to make something genuinely useful.

---

## What It Does
- Fetches **Google (GOOGL)** stock price history from Yahoo Finance.
- Displays the **Open** and **Close** prices as an interactive line chart.

## Libraries Used

```bash
pip install streamlit yfinance
```

| Library | Purpose |
|---------|---------|
| `streamlit` | Web app framework |
| `yfinance` | Fetches stock data from Yahoo Finance |

## Full App Code

```python
import yfinance as yf
import streamlit as st

# --- Page header ---
st.write("""
# Simple Stock Price App
Shown are the stock **closing price** and **volume** of Google!
""")

# --- Fetch data ---
tickerSymbol = 'GOOGL'
tickerData = yf.Ticker(tickerSymbol)

# Get historical prices: from 2010 to today
tickerDf = tickerData.history(period='1d', start='2010-5-31', end='2020-5-31')
# tickerDf columns: Open, High, Low, Close, Volume, Dividends, Stock Splits

# --- Display charts ---
st.write("## Closing Price")
st.line_chart(tickerDf.Close)

st.write("## Volume")
st.line_chart(tickerDf.Volume)
```

## Key Concepts

**`yf.Ticker(symbol)`** — creates a Ticker object for any stock symbol (AAPL, MSFT, TSLA, etc.)

**`.history(period, start, end)`** — downloads historical OHLCV data as a pandas DataFrame:
- `period` — '1d', '5d', '1mo', '3mo', '6mo', '1y', '2y', '5y', '10y', 'ytd', 'max'
- `start` / `end` — date strings `'YYYY-MM-DD'`

**`st.line_chart(series)`** — renders an interactive line chart. Accepts a pandas Series or DataFrame column.

**`st.write()`** — Streamlit's "magic" function. Pass it a string and it renders markdown. Pass it a DataFrame and it renders a table. Pass it a plot and it renders the chart.

---

# 2. App 2 — Simple Bioinformatics DNA Count `(13:24)`

## 📖 Theory

This app introduces two Streamlit features that make apps interactive: the **text area widget** (for user input) and the **bar chart** (for output visualization). It also demonstrates that Streamlit isn't just for finance — any domain that involves counting, transforming, or visualizing data is a candidate. DNA is made of four nucleotides — A, T, G, C. Counting their frequency is a fundamental bioinformatics operation. In code, this is just counting characters in a string — but wrapping it in a Streamlit app makes it accessible to biologists who don't code.

---

## What It Does
- User inputs a **DNA sequence** string.
- App counts the frequency of each nucleotide (A, T, G, C).
- Displays results as: dictionary → dataframe → bar chart.

## Libraries Used

```bash
pip install streamlit pandas altair
```

## Full App Code

```python
import pandas as pd
import streamlit as st
import altair as alt

# --- Header ---
st.write("""
# DNA Nucleotide Count Web App
This app counts the nucleotide composition of query DNA!
""")

# --- User Input ---
st.header('Enter DNA sequence')

sequence_input = ">DNA Query 2\nGAACACGTGGAGGCAAACAGGAAGGTGAAGAAGAACTTATCCTATCAGGACGGAAGGTCCTGTGCTCGG"

sequence = st.text_area("Sequence input", sequence_input, height=250)

# Strip the header line (lines starting with ">")
sequence = sequence.splitlines()
sequence = sequence[1:]          # remove first line (FASTA header)
sequence = ''.join(sequence)     # join remaining lines into single string

st.write("""---""")   # horizontal divider

# --- DNA Count Function ---
st.header('1. Print Dictionary')

def DNA_nucleotide_count(seq):
    d = {
        'A': seq.count('A'),
        'T': seq.count('T'),
        'G': seq.count('G'),
        'C': seq.count('C')
    }
    return d

X = DNA_nucleotide_count(sequence)
X   # dict: {'A': 20, 'T': 12, 'G': 15, 'C': 14}

st.write(X)

# --- Display as DataFrame ---
st.header('2. Print DataFrame')

df = pd.DataFrame.from_dict(X, orient='index')
df = df.rename(columns={0: 'count'})
df.reset_index(inplace=True)
df = df.rename(columns={'index': 'nucleotide'})

st.write(df)

# --- Bar Chart with Altair ---
st.header('3. Display Bar Chart')

p = alt.Chart(df).mark_bar().encode(
    x='nucleotide',
    y='count'
)
p = p.properties(width=alt.Step(80))   # bar width
st.write(p)
```

## Key Concepts

**`st.text_area(label, value, height)`** — a multi-line text input. `value` is the default text shown. Returns whatever the user types as a string.

**`sequence.splitlines()`** — splits string on newlines into a list.

**FASTA format** — biology's standard sequence format. First line starts with `>` and is a header/description. Remaining lines are the actual sequence. That's why we do `sequence[1:]` to strip the header.

**`pd.DataFrame.from_dict(dict, orient='index')`** — converts a dictionary to a DataFrame where dict keys become the index.

**`altair`** — a declarative visualization library. More customizable than `st.bar_chart()` for styling.

---

# 3. App 3 — EDA Basketball `(29:44)`

## 📖 Theory

**Exploratory Data Analysis (EDA)** is the process of examining a dataset to understand its structure, patterns, and interesting subsets — before building any models. This app introduces the standard EDA workflow in Streamlit: load data from a web source (Wikipedia in this case), let users filter it interactively via sidebar widgets, display the filtered data, and provide a download option. The `@st.cache_data` decorator is critical here — it prevents the data from being re-fetched from the web on every single widget interaction, which would be slow and wasteful.

---

## What It Does
- Scrapes **NBA player stats** by year from Wikipedia.
- User selects **year** and filters by **team** and **position**.
- Displays filtered data + heatmap of stat correlations.
- Provides CSV download of filtered data.

## Libraries Used

```bash
pip install streamlit pandas numpy matplotlib seaborn
```

## Full App Code

```python
import streamlit as st
import pandas as pd
import base64
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np

st.title('NBA Player Stats Explorer')
st.markdown("""
This app performs simple webscraping of NBA player stats data!
* **Data source:** [Basketball-reference.com](https://www.basketball-reference.com/)
""")

# --- Sidebar filters ---
st.sidebar.header('User Input Features')
selected_year = st.sidebar.selectbox('Year', list(reversed(range(1950, 2020))))

# --- Load & cache data ---
@st.cache_data
def load_data(year):
    url = f"https://www.basketball-reference.com/leagues/NBA_{year}_per_game.html"
    html = pd.read_html(url, header=0)   # reads all HTML tables on the page
    df = html[0]
    raw = df.drop(df[df.Age == 'Age'].index)   # remove repeated header rows
    raw = raw.fillna(0)
    playerstats = raw.drop(['Rk'], axis=1)
    return playerstats

playerstats = load_data(selected_year)

# --- Sidebar: multi-select filters ---
sorted_unique_team = sorted(playerstats.Tm.unique())
selected_team = st.sidebar.multiselect(
    'Team', sorted_unique_team, sorted_unique_team   # default = all selected
)

unique_pos = ['C', 'PF', 'SF', 'PG', 'SG']
selected_pos = st.sidebar.multiselect(
    'Position', unique_pos, unique_pos
)

# --- Filter data ---
df_selected_team = playerstats[
    (playerstats.Tm.isin(selected_team)) &
    (playerstats.Pos.isin(selected_pos))
]

st.header('Display Player Stats of Selected Team(s)')
st.write(f'Data Dimension: {df_selected_team.shape[0]} rows and {df_selected_team.shape[1]} columns.')
st.dataframe(df_selected_team)

# --- CSV Download ---
def filedownload(df):
    csv = df.to_csv(index=False)
    b64 = base64.b64encode(csv.encode()).decode()   # encode to base64
    href = f'<a href="data:file/csv;base64,{b64}" download="playerstats.csv">Download CSV File</a>'
    return href

st.markdown(filedownload(df_selected_team), unsafe_allow_html=True)

# --- Heatmap ---
if st.button('Intercorrelation Heatmap'):
    st.header('Intercorrelation Matrix Heatmap')
    df_selected_team.to_csv('output.csv', index=False)
    df = pd.read_csv('output.csv')

    corr = df.corr()
    mask = np.zeros_like(corr)
    mask[np.triu_indices_from(mask)] = True   # mask upper triangle
    with sns.axes_style("white"):
        f, ax = plt.subplots(figsize=(7, 5))
        ax = sns.heatmap(corr, mask=mask, vmax=1, square=True)
    st.pyplot(f)
```

## Key Concepts

**`@st.cache_data`** — caches the function's return value. If called again with the same arguments, returns cached result instantly instead of re-running. Critical for expensive operations like web scraping or loading large files.

**`pd.read_html(url)`** — scrapes all HTML tables from a URL into a list of DataFrames. Returns the first table with `[0]`.

**`st.sidebar.selectbox(label, options)`** — dropdown widget in the sidebar. Returns selected value.

**`st.sidebar.multiselect(label, options, default)`** — checkbox-style multi-select. Returns list of selected items.

**`df.isin(list)`** — boolean mask that's True where column value is in the list.

**`base64` download trick** — Streamlit doesn't have a built-in file download button (at the time of this course). The workaround: encode the CSV as base64 and render it as an HTML download link using `st.markdown(..., unsafe_allow_html=True)`. Modern Streamlit has `st.download_button()` which is cleaner.

**`st.button(label)`** — renders a button. Returns `True` when clicked. The code inside the `if st.button(...)` block only runs when the button is pressed.

**Correlation Heatmap** — `df.corr()` computes pairwise correlation between all numeric columns (Pearson by default, range -1 to 1). `np.triu_indices_from(mask)` masks the upper triangle to avoid redundancy.

---

# 4. App 4 — EDA Football `(50:39)`

## 📖 Theory

This app follows the same EDA pattern as App 3 but with a different sport and data source — reinforcing that the Streamlit + pandas EDA template is reusable across domains. The core workflow (load → filter → display → download) is a pattern you'll reuse constantly in data science work. This app also scrapes data from a web URL, demonstrating that web-scraped data can feed directly into a Streamlit app without any intermediate database.

---

## What It Does
- Scrapes **NFL rushing stats** from Pro Football Reference.
- Sidebar: filter by **year** and **team**.
- Displays filtered player stats + download button.

## Libraries Used

```bash
pip install streamlit pandas numpy matplotlib seaborn
```

## Core Code Pattern (Same as Basketball)

```python
import streamlit as st
import pandas as pd
import base64
import matplotlib.pyplot as plt
import seaborn as sns

st.title('NFL Football Stats Explorer')

# Sidebar
st.sidebar.header('User Input Features')
selected_year = st.sidebar.selectbox('Year', list(reversed(range(1990, 2020))))

@st.cache_data
def load_data(year):
    url = f"https://www.pro-football-reference.com/years/{year}/rushing.htm"
    html = pd.read_html(url, header=1)
    df = html[0]
    raw = df.drop(df[df.Age == 'Age'].index)   # drop repeated headers
    raw = raw.fillna(0)
    playerstats = raw.drop(['Rk'], axis=1)
    return playerstats

playerstats = load_data(selected_year)

# Team filter
unique_team = sorted(playerstats.Tm.unique())
selected_team = st.sidebar.multiselect('Team', unique_team, unique_team)

# Filter
df_filtered = playerstats[playerstats.Tm.isin(selected_team)]

st.dataframe(df_filtered)
```

## What's New vs App 3

- `header=1` in `pd.read_html()` — some tables have a two-row header; `header=1` takes the second row as the column names.
- Same download and heatmap pattern — confirms the template is reusable.

---

# 5. App 5 — EDA S&P 500 Stock Price `(1:00:48)`

## 📖 Theory

The S&P 500 is a stock market index tracking the 500 largest US companies. This app goes deeper than App 1 by letting users **select which companies to compare** and showing multiple line charts. It also introduces scraping from **Wikipedia** (not an API — raw HTML table parsing) and using `matplotlib` for custom plot rendering instead of Streamlit's built-in charts. The key data concept is **pivoting** — reshaping a DataFrame from long format (one row per company per date) to wide format (one column per company, indexed by date) so all company price lines can be plotted on one chart.

---

## What It Does
- Reads S&P 500 company list from Wikipedia.
- Fetches historical prices for selected companies via `yfinance`.
- User selects companies via sidebar multiselect.
- Plots each selected company's closing price as a separate chart.

## Libraries Used

```bash
pip install streamlit pandas numpy matplotlib seaborn yfinance
```

## Full App Code

```python
import streamlit as st
import pandas as pd
import base64
import matplotlib.pyplot as plt
import seaborn as sns
import yfinance as yf
import numpy as np

st.title('S&P 500 App')
st.markdown("""
This app retrieves the list of the **S&P 500** (from Wikipedia) and its corresponding
**stock closing price** (year-to-date).
""")

# --- Sidebar ---
st.sidebar.header('User Input Features')

# Scrape S&P 500 company list from Wikipedia
@st.cache_data
def load_data():
    url = 'https://en.wikipedia.org/wiki/List_of_S%26P_500_companies'
    html = pd.read_html(url, header=0)
    df = html[0]
    return df

df = load_data()

# Filter by sector
sector = df.groupby('GICS Sector')
sorted_sector_unique = sorted(df['GICS Sector'].unique())
selected_sector = st.sidebar.multiselect(
    'Sector', sorted_sector_unique, sorted_sector_unique
)

# Filter companies by selected sectors
df_selected_sector = df[df['GICS Sector'].isin(selected_sector)]

st.header('Display Companies in Selected Sector')
st.write(f'Data Dimension: {df_selected_sector.shape[0]} rows and {df_selected_sector.shape[1]} columns.')
st.dataframe(df_selected_sector)

# --- Download ---
def filedownload(df):
    csv = df.to_csv(index=False)
    b64 = base64.b64encode(csv.encode()).decode()
    href = f'<a href="data:file/csv;base64,{b64}" download="SP500.csv">Download CSV File</a>'
    return href

st.markdown(filedownload(df_selected_sector), unsafe_allow_html=True)

# --- Fetch stock prices via yfinance ---
data = yf.download(
    tickers=list(df_selected_sector[:10].Symbol),  # limit to 10 tickers
    period="ytd",
    interval="1d",
    group_by='ticker',
    auto_adjust=True,
    prepost=True,
    threads=True,
    proxy=None
)

# --- Plot closing price per company ---
def price_plot(symbol):
    df = pd.DataFrame(data[symbol].Close)
    df['Date'] = df.index
    plt.fill_between(df.Date, df.Close, alpha=0.3)
    plt.plot(df.Date, df.Close, color='skyblue', alpha=0.8)
    plt.xticks(rotation=90)
    plt.title(symbol, fontweight='bold')
    plt.xlabel('Date', fontweight='bold')
    plt.ylabel('Closing Price', fontweight='bold')
    return st.pyplot(plt)

num_company = st.sidebar.slider('Number of Companies', 1, 5)
if st.button('Show Plots'):
    st.header('Stock Closing Price')
    for i in list(df_selected_sector.Symbol)[:num_company]:
        price_plot(i)
```

## Key Concepts

**`yf.download(tickers=list, period, interval)`** — bulk download for multiple tickers at once. Returns a hierarchical DataFrame indexed by `(ticker, field)`.

**`st.sidebar.slider(label, min, max)`** — renders a slider widget. Returns current value as integer.

**`plt.fill_between(x, y)`** — fills the area under the line chart with semi-transparent color (`alpha=0.3`).

**`st.pyplot(fig)`** — renders a matplotlib figure in Streamlit.

---

# 6. App 6 — EDA Cryptocurrency `(1:24:03)`

## 📖 Theory

Cryptocurrency markets are highly volatile and heavily data-driven. This app introduces **web scraping with `requests` and `BeautifulSoup`** (instead of `pd.read_html`) for more fine-grained scraping control. It also demonstrates **percentage change calculations** — a common financial metric — and how to color-code output (green = positive, red = negative) to make data instantly readable. The currency selector shows how a single widget choice can cascade to filter multiple parts of the UI.

---

## What It Does
- Scrapes top 100 cryptocurrency data from CoinMarketCap.
- Sidebar: filter by **currency** (USD, BTC, ETH) and **coin name**.
- Shows **7-day % change** bar chart with color coding (green/red).
- Displays price data table.

## Libraries Used

```bash
pip install streamlit pandas numpy matplotlib requests beautifulsoup4
```

## Core Code Patterns

```python
import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt
import requests
from bs4 import BeautifulSoup
import json

# --- Scrape CoinMarketCap ---
@st.cache_data
def load_data():
    cmc = requests.get('https://coinmarketcap.com')
    soup = BeautifulSoup(cmc.content, 'html.parser')
    data = soup.find('script', id='__NEXT_DATA__', type='application/json')
    coins = {}
    coin_data = json.loads(data.contents[0])
    listings = coin_data['props']['initialState']['cryptocurrency']['listingLatest']['data']
    for i in listings:
        coins[str(i['id'])] = i['slug']
    return coins

# --- Color coding % change ---
def color_df(val):
    color = 'green' if val > 0 else 'red'
    return f'color: {color}'

# Apply style to specific column
df.style.applymap(color_df, subset=['percent_change_7d'])

# --- Sidebar currency selector ---
price_unit = st.sidebar.selectbox(
    'Select currency for price',
    ('USD', 'BTC', 'ETH')
)

# --- Bar chart of % change ---
col_name = f'percent_change_7d'
df_change = df[[col_name]].copy()
df_change['positive'] = df_change[col_name] > 0

plt.figure(figsize=(5, 25))
plt.subplots_adjust(top=1, bottom=0)
df_change[col_name].plot(
    kind='barh',
    color=df_change.positive.map({True: 'forestgreen', False: 'red'})
)
st.pyplot(plt)
```

## Key Concepts

**`requests.get(url)`** — fetches raw HTML from any URL. Returns a `Response` object; use `.content` for bytes or `.text` for string.

**`BeautifulSoup(html, 'html.parser')`** — parses HTML into a searchable tree. Use `.find()`, `.find_all()` to locate elements.

**`df.style.applymap(func, subset=[col])`** — applies a CSS style function to specific DataFrame cells for colored display.

**`plot(kind='barh', color=list)`** — horizontal bar chart with per-bar colors mapped from a boolean column.

---

# 7. App 7 — Classification Iris `(1:50:47)`

## 📖 Theory

This is the first **machine learning app** in the course — shifting from EDA to prediction. The Iris dataset is the "hello world" of classification ML: 150 flower samples, 4 features (sepal/petal length/width), 3 classes (species). The app demonstrates the core ML app pattern: collect user input via sidebar sliders → run model prediction → display result. The model itself is `RandomForestClassifier` from scikit-learn — an ensemble method that builds multiple decision trees and takes a majority vote. Since this is a small, static dataset, the model is trained fresh on every app run (acceptable here; for larger models, you'd serialize with `pickle` or `joblib`).

---

## What It Does
- User adjusts **4 sliders** (sepal/petal dimensions).
- App feeds inputs to a **Random Forest classifier** trained on the Iris dataset.
- Displays **predicted species** and **prediction probability** for all 3 classes.

## Libraries Used

```bash
pip install streamlit pandas scikit-learn
```

## Full App Code

```python
import streamlit as st
import pandas as pd
from sklearn import datasets
from sklearn.ensemble import RandomForestClassifier

st.write("""
# Simple Iris Flower Prediction App
This app predicts the **Iris flower** type!
""")

# --- Sidebar: user input ---
st.sidebar.header('User Input Parameters')

def user_input_features():
    sepal_length = st.sidebar.slider('Sepal length', 4.3, 7.9, 5.4)
    sepal_width  = st.sidebar.slider('Sepal width',  2.0, 4.4, 3.4)
    petal_length = st.sidebar.slider('Petal length', 1.0, 6.9, 1.3)
    petal_width  = st.sidebar.slider('Petal width',  0.1, 2.5, 0.2)

    data = {
        'sepal_length': sepal_length,
        'sepal_width': sepal_width,
        'petal_length': petal_length,
        'petal_width': petal_width
    }
    features = pd.DataFrame(data, index=[0])
    return features

df = user_input_features()

# --- Display user input ---
st.subheader('User Input parameters')
st.write(df)

# --- Load dataset and train model ---
iris = datasets.load_iris()
X = iris.data       # features
Y = iris.target     # labels (0=setosa, 1=versicolor, 2=virginica)

clf = RandomForestClassifier()
clf.fit(X, Y)

# --- Predict ---
prediction = clf.predict(df)
prediction_proba = clf.predict_proba(df)

# --- Display results ---
st.subheader('Class labels and their corresponding index number')
st.write(iris.target_names)   # ['setosa', 'versicolor', 'virginica']

st.subheader('Prediction')
st.write(iris.target_names[prediction])

st.subheader('Prediction Probability')
st.write(prediction_proba)
# Shows probability for each class: [[0.02, 0.05, 0.93]]
```

## Key Concepts

**`st.sidebar.slider(label, min, max, default)`** — renders a slider. Args: label, min value, max value, default value. Returns current numeric value.

**`pd.DataFrame(dict, index=[0])`** — creates a single-row DataFrame from a dictionary. `index=[0]` gives it row index 0.

**`RandomForestClassifier()`** — ensemble of decision trees. Default: 100 trees. Each tree votes; majority class wins.

**`clf.fit(X, Y)`** — trains the model on the full dataset.

**`clf.predict(df)`** — returns predicted class index (0, 1, or 2).

**`clf.predict_proba(df)`** — returns probability for each class. Sum = 1.0. E.g., `[[0.02, 0.05, 0.93]]` means 93% confidence it's class 2.

**`iris.target_names[prediction]`** — maps numeric prediction to species name string.

---

# 8. App 8 — Classification Penguins `(1:58:58)`

## 📖 Theory

This app upgrades App 7 in two critical ways. First, it uses a **real-world dataset from a CSV file** (not a clean scikit-learn toy dataset) — introducing data preprocessing: handling categorical variables (species, island, sex) via **label encoding** and dealing with missing values. Second, it introduces **`pickle`** — Python's model serialization format. Instead of training the model from scratch on every run, the model is pre-trained, saved to disk as a `.pkl` file, and simply loaded at runtime. This is how real ML apps work — training is separate from serving.

---

## What It Does
- User inputs penguin measurements via sidebar.
- App loads a **pre-trained** Random Forest model from a `.pkl` file.
- Predicts the **penguin species** (Adelie, Chinstrap, Gentoo).
- Shows prediction + probabilities.

## Libraries Used

```bash
pip install streamlit pandas scikit-learn
```

## File Structure

```
app_8_classification_penguins/
├── penguins-app.py          ← Streamlit app
├── penguins_clf.pkl         ← Pre-trained model (saved separately)
├── penguins_training.py     ← Script to train & save the model
└── penguins_cleaned.csv     ← Cleaned dataset
```

## Training Script (Run Once)

```python
# penguins_training.py — run this separately to generate the .pkl file
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
import pickle

penguins = pd.read_csv('penguins_cleaned.csv')

# Encode target variable
target_mapper = {'Adelie': 0, 'Chinstrap': 1, 'Gentoo': 2}
penguins['species'] = penguins['species'].apply(lambda x: target_mapper[x])

# Features and target
X = penguins.drop('species', axis=1)
Y = penguins['species']

# Train
clf = RandomForestClassifier()
clf.fit(X, Y)

# Save to pickle
with open('penguins_clf.pkl', 'wb') as f:
    pickle.dump(clf, f)

print("Model saved!")
```

## App Code

```python
import streamlit as st
import pandas as pd
import pickle
import numpy as np

st.write("""
# Penguin Prediction App
This app predicts the **Palmer Penguin** species!
""")

# --- Sidebar inputs ---
st.sidebar.header('User Input Features')

def user_input_features():
    island = st.sidebar.selectbox('Island', ('Biscoe', 'Dream', 'Torgersen'))
    sex = st.sidebar.selectbox('Sex', ('male', 'female'))
    bill_length_mm = st.sidebar.slider('Bill length (mm)', 32.1, 59.6, 43.9)
    bill_depth_mm  = st.sidebar.slider('Bill depth (mm)', 13.1, 21.5, 17.2)
    flipper_length_mm = st.sidebar.slider('Flipper length (mm)', 172.0, 231.0, 201.0)
    body_mass_g    = st.sidebar.slider('Body mass (g)', 2700.0, 6300.0, 4207.0)

    data = {
        'island': island,
        'bill_length_mm': bill_length_mm,
        'bill_depth_mm': bill_depth_mm,
        'flipper_length_mm': flipper_length_mm,
        'body_mass_g': body_mass_g,
        'sex': sex
    }
    return pd.DataFrame(data, index=[0])

input_df = user_input_features()

# --- Encode categorical input ---
# Map categories to integers (must match encoding used during training)
encode = ['sex', 'island']
for col in encode:
    dummy = pd.get_dummies(input_df[col], prefix=col)
    input_df = pd.concat([input_df, dummy], axis=1)
    del input_df[col]

# Ensure column order matches training data
input_df = input_df.reindex(columns=['bill_length_mm', 'bill_depth_mm',
                                      'flipper_length_mm', 'body_mass_g',
                                      'sex_female', 'sex_male',
                                      'island_Biscoe', 'island_Dream',
                                      'island_Torgersen'], fill_value=0)

# --- Load pre-trained model ---
load_clf = pickle.load(open('penguins_clf.pkl', 'rb'))

# --- Predict ---
prediction = load_clf.predict(input_df)
prediction_proba = load_clf.predict_proba(input_df)

species_map = {0: 'Adelie', 1: 'Chinstrap', 2: 'Gentoo'}

st.subheader('Prediction')
st.write(species_map[int(prediction[0])])

st.subheader('Prediction Probability')
st.write(pd.DataFrame(prediction_proba,
                      columns=['Adelie', 'Chinstrap', 'Gentoo']))
```

## Key Concepts

**`pickle.dump(obj, file)`** — serializes a Python object (the trained model) to a binary file.

**`pickle.load(file)`** — deserializes and loads the model back. Returns the exact same object that was saved.

**`pd.get_dummies(series, prefix)`** — one-hot encodes a categorical column. `'island'` with values `['Biscoe', 'Dream', 'Torgersen']` becomes 3 binary columns: `island_Biscoe`, `island_Dream`, `island_Torgersen`.

**`df.reindex(columns=list, fill_value=0)`** — reorders columns to exactly match the training feature order. Missing columns are filled with 0. This is critical — models fail silently or crash if feature order doesn't match training.

---

# 9. App 9 — Regression Boston Housing `(2:16:08)`

## 📖 Theory

Regression predicts a **continuous numeric value** (e.g., house price), unlike classification which predicts a discrete category. This app introduces **feature importance** — a key model interpretability concept. After training, a Random Forest tells you which features most influenced its predictions (measured by how much each feature reduced impurity across all trees). Visualizing feature importance is essential for explaining model decisions to stakeholders. This app also demonstrates `st.expander()` — a collapsible section that keeps the UI clean.

---

## What It Does
- Uses the **Boston Housing dataset** (13 features → house price).
- Sidebar: user adjusts all 13 input sliders.
- Predicts **median house value (MEDV)** in $1000s.
- Shows feature importance bar chart.
- Displays model evaluation metrics (MAE, MSE, R²).

## Libraries Used

```bash
pip install streamlit pandas scikit-learn matplotlib
```

## Full App Code

```python
import streamlit as st
import pandas as pd
import shap
import matplotlib.pyplot as plt
from sklearn import datasets
from sklearn.ensemble import RandomForestRegressor
import numpy as np

st.write("""
# Boston House Price Prediction App
This app predicts the **Boston House Price**!
""")

# --- Sidebar inputs ---
st.sidebar.header('Specify Input Parameters')

def user_input_features():
    CRIM  = st.sidebar.slider('CRIM',  0.01, 89.0,  3.61)
    ZN    = st.sidebar.slider('ZN',    0.0,  100.0, 11.36)
    INDUS = st.sidebar.slider('INDUS', 0.46, 27.74, 11.14)
    CHAS  = st.sidebar.slider('CHAS',  0.0,  1.0,   0.07)
    NOX   = st.sidebar.slider('NOX',   0.38, 0.87,  0.55)
    RM    = st.sidebar.slider('RM',    3.56, 8.78,  6.28)
    AGE   = st.sidebar.slider('AGE',   2.9,  100.0, 68.57)
    DIS   = st.sidebar.slider('DIS',   1.13, 12.13, 3.79)
    RAD   = st.sidebar.slider('RAD',   1.0,  24.0,  9.55)
    TAX   = st.sidebar.slider('TAX',   187.0,711.0, 408.24)
    PTRATIO=st.sidebar.slider('PTRATIO',12.6,22.0,  18.46)
    B     = st.sidebar.slider('B',     0.32, 396.9, 356.67)
    LSTAT = st.sidebar.slider('LSTAT', 1.73, 37.97, 12.65)
    data = {
        'CRIM':CRIM,'ZN':ZN,'INDUS':INDUS,'CHAS':CHAS,'NOX':NOX,
        'RM':RM,'AGE':AGE,'DIS':DIS,'RAD':RAD,'TAX':TAX,
        'PTRATIO':PTRATIO,'B':B,'LSTAT':LSTAT
    }
    return pd.DataFrame(data, index=[0])

df = user_input_features()

st.header('Specified Input parameters')
st.write(df)

# --- Load data & train model ---
boston = datasets.load_boston()   # (deprecated in newer sklearn, use alternative)
X = pd.DataFrame(boston.data, columns=boston.feature_names)
Y = pd.Series(boston.target, name='MEDV')

model = RandomForestRegressor()
model.fit(X, Y)

# --- Prediction ---
prediction = model.predict(df)
st.header('Prediction of MEDV')
st.write(f"${prediction[0] * 1000:,.0f}")  # convert to dollars

# --- Feature Importance ---
st.header('Feature Importance')
feat_importances = pd.Series(
    model.feature_importances_,
    index=boston.feature_names
).sort_values(ascending=False)

fig, ax = plt.subplots()
feat_importances.plot(kind='bar', ax=ax)
ax.set_title("Feature Importances")
st.pyplot(fig)
```

## Key Concepts

**`RandomForestRegressor()`** — same as classifier but predicts continuous values. Uses mean of tree outputs instead of majority vote.

**`model.feature_importances_`** — array of importance scores (0 to 1, sum = 1) for each feature. Higher = that feature matters more to predictions.

**Boston Housing Features (important to understand):**
| Feature | Meaning |
|---------|---------|
| CRIM | Crime rate per capita |
| RM | Average number of rooms |
| LSTAT | % lower status population |
| MEDV | Target: median house value ($1000s) |
| NOX | Nitric oxide concentration |

**`pd.Series(...).sort_values(ascending=False)`** — sorts importance scores from highest to lowest for readable bar chart.

---

# 10. App 10 — Regression Bioinformatics Solubility `(2:27:53)`

## 📖 Theory

This app brings together the entire pipeline in a real scientific context: loading a published research dataset, computing **molecular descriptors** (numerical representations of chemical properties), training a regression model to predict **drug solubility** (logS), and visualizing predicted vs actual values. It demonstrates that the same ML regression pattern from App 9 applies directly to a completely different domain — computational chemistry. The key new concept is **model evaluation**: plotting predicted vs actual on a scatter plot (perfect model = diagonal line) and computing R² (variance explained by the model).

---

## What It Does
- Loads the **Delaney solubility dataset** (1,144 molecules).
- Computes 4 molecular descriptors as features.
- Trains a **Linear Regression** model to predict logS (solubility).
- Plots **Predicted vs Actual** scatter chart + evaluates R².

## Libraries Used

```bash
pip install streamlit pandas scikit-learn matplotlib rdkit
```

## Core Code

```python
import streamlit as st
import pandas as pd
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score
import matplotlib.pyplot as plt
import numpy as np

st.write("# Molecular Solubility Prediction Web App")

# --- Load dataset ---
st.header('Dataset')

@st.cache_data
def load_data():
    url = "https://raw.githubusercontent.com/dataprofessor/data/master/delaney_solubility_with_descriptors.csv"
    return pd.read_csv(url)

df = load_data()
st.write(df.head())

# --- Split features and target ---
X = df.drop('logS', axis=1)   # molecular descriptors
Y = df['logS']                 # solubility (target)

# --- Train model ---
model = LinearRegression()
model.fit(X, Y)

# --- Predictions and metrics ---
Y_pred = model.predict(X)
mse = mean_squared_error(Y, Y_pred)
r2  = r2_score(Y, Y_pred)

st.header('Model Performance')
col1, col2 = st.columns(2)
col1.metric("MSE",  round(mse, 3))
col2.metric("R²",   round(r2, 3))

# --- Predicted vs Actual plot ---
st.header('Predicted vs Actual')
fig, ax = plt.subplots()
ax.scatter(Y, Y_pred, alpha=0.3)
ax.plot([Y.min(), Y.max()], [Y.min(), Y.max()],
        color='red', linewidth=2, linestyle='--')   # diagonal = perfect
ax.set_xlabel('Actual logS')
ax.set_ylabel('Predicted logS')
st.pyplot(fig)
```

## Key Concepts

**logS** — logarithm of solubility. More negative = less soluble. Range roughly -12 to +2.

**`LinearRegression()`** — fits a straight line: `Y = w1*X1 + w2*X2 + ... + b`. Minimizes sum of squared errors.

**`r2_score(actual, predicted)`** — R² (coefficient of determination). 1.0 = perfect predictions. 0 = model no better than predicting the mean. Negative = model worse than mean.

**`mean_squared_error(actual, predicted)`** — average squared difference between prediction and truth. Lower = better.

**Predicted vs Actual plot** — scatter of `(actual, predicted)`. Perfect model = all points on the red diagonal line. Spread = error. Systematic curves = model underfitting.

**`st.columns(2)`** — splits UI into 2 side-by-side columns. Use `col1.write(...)` / `col2.write(...)` to place content in each.

**`st.metric(label, value)`** — renders a big bold number with label. Clean way to display KPIs.

---

# 11. App 11 — Deploy to Heroku `(2:54:27)`

## 📖 Theory

A locally running app is only useful to you. Deployment makes it accessible to anyone with a URL. **Heroku** is a PaaS (Platform as a Service) that handles servers and infrastructure — you just push code, Heroku runs it. For Streamlit apps, Heroku needs three extra files beyond your Python code: a `Procfile` (tells Heroku how to start the app), a `setup.sh` (configures Streamlit for production), and a `requirements.txt` (lists all dependencies). The `setup.sh` trick is needed because Streamlit defaults to interactive mode; on a server with no terminal, it needs to be configured to run headlessly.

---

## Files Needed for Heroku Deployment

```
your-app/
├── app.py               ← your Streamlit app
├── requirements.txt     ← all dependencies
├── Procfile             ← tells Heroku how to start the app
└── setup.sh             ← configures Streamlit for server environment
```

## File Contents

```bash
# Procfile (no file extension)
web: sh setup.sh && streamlit run app.py
```

```bash
# setup.sh
mkdir -p ~/.streamlit/

echo "\
[general]\n\
email = \"your-email@domain.com\"\n\
" > ~/.streamlit/credentials.toml

echo "\
[server]\n\
headless = true\n\
enableCORS = false\n\
port = $PORT\n\
" > ~/.streamlit/config.toml
```

```bash
# requirements.txt — generate with:
pip freeze > requirements.txt

# Or manually specify versions:
streamlit==1.x.x
pandas==1.x.x
scikit-learn==0.x.x
# ... etc
```

## Deployment Steps

```bash
# 1. Install Heroku CLI
# heroku.com/cli

# 2. Login
heroku login

# 3. Create Heroku app
heroku create your-app-name

# 4. Initialize git (if not already)
git init
git add .
git commit -m "Initial commit"

# 5. Deploy
git push heroku main

# 6. Open the app
heroku open

# 7. View logs (for debugging)
heroku logs --tail
```

> ⚠️ Heroku's free tier was discontinued in 2022. Alternatives: **Railway**, **Render**, **Fly.io**, or use **Streamlit Community Cloud** (App 12).

---

# 12. App 12 — Deploy to Streamlit Sharing (Community Cloud) `(3:04:37)`

## 📖 Theory

Streamlit Community Cloud (formerly "Streamlit Sharing") is the easiest deployment option for Streamlit apps specifically — it's free, requires zero server configuration, and auto-deploys from a GitHub repository. Every time you push to your GitHub repo, the deployed app automatically updates. This is the recommended first deployment target for Streamlit apps because the entire process takes under 5 minutes and requires no CLI commands.

---

## Requirements
- Your app code on a **public GitHub repository**.
- A `requirements.txt` in the repo root.
- A free account at [share.streamlit.io](https://share.streamlit.io).

## Deployment Steps

```
1. Push your app to GitHub
   ├── app.py
   └── requirements.txt

2. Go to share.streamlit.io → Sign in with GitHub

3. Click "New app"
   ├── Repository: your-github-username/your-repo
   ├── Branch: main
   └── Main file path: app.py

4. Click "Deploy!"
   → Streamlit installs dependencies and starts your app
   → Your app gets a public URL: https://your-app.streamlit.app

5. Auto-deploy: every git push → app automatically updates
```

## requirements.txt Tips

```bash
# Too strict (causes conflicts over time)
pandas==1.3.5
numpy==1.21.0

# Better — allow minor version updates
pandas>=1.3.0
numpy>=1.20.0
streamlit>=1.0.0
```

---

# 13. Streamlit Widget Cheatsheet

## 📖 Theory

Streamlit's widget API is intentionally minimal — every widget is just a function call that returns the current value. When a user interacts with a widget, Streamlit re-runs the whole script and the widget function returns the new value. Understanding this reactive model is key — your logic should read widget values and react accordingly, not try to set up event listeners.

---

## Input Widgets

```python
# Text input
name = st.text_input("Your name", value="Alice")

# Multi-line text
bio = st.text_area("Bio", height=150)

# Number input
age = st.number_input("Age", min_value=0, max_value=120, value=25, step=1)

# Slider (single value)
score = st.slider("Score", min_value=0, max_value=100, value=50)

# Slider (range — returns tuple)
start, end = st.slider("Range", 0, 100, (20, 80))

# Selectbox (single select)
option = st.selectbox("Choose", ["A", "B", "C"])

# Multi-select
options = st.multiselect("Choose many", ["A", "B", "C"], default=["A"])

# Checkbox (bool)
show_data = st.checkbox("Show raw data", value=False)

# Radio buttons
choice = st.radio("Pick one", ["Option 1", "Option 2", "Option 3"])

# File upload
uploaded_file = st.file_uploader("Upload CSV", type=["csv"])
if uploaded_file is not None:
    df = pd.read_csv(uploaded_file)

# Button (bool — True only on click)
if st.button("Run Analysis"):
    st.write("Running...")

# Date picker
date = st.date_input("Select date")

# Color picker
color = st.color_picker("Pick a color", "#00f900")
```

## Display Components

```python
st.line_chart(df)           # line chart from DataFrame
st.bar_chart(df)            # bar chart
st.area_chart(df)           # area chart
st.map(df)                  # map (requires 'lat' and 'lon' columns)
st.pyplot(fig)              # matplotlib figure
st.plotly_chart(fig)        # plotly figure
st.altair_chart(chart)      # altair chart
st.image(image)             # image (path, URL, or numpy array)
st.dataframe(df)            # interactive table
st.table(df)                # static table
st.metric("Label", value, delta)   # KPI metric card
st.json(data)               # formatted JSON
st.code("code here", language="python")   # syntax-highlighted code block
```

## Layout

```python
# Sidebar
st.sidebar.title("Controls")
x = st.sidebar.slider("X")

# Columns
col1, col2, col3 = st.columns(3)
col1.write("Left")
col2.write("Middle")
col3.write("Right")

# Columns with custom ratios
col1, col2 = st.columns([1, 3])   # col2 is 3x wider

# Expander (collapsible)
with st.expander("See explanation"):
    st.write("Hidden content here")

# Tabs
tab1, tab2 = st.tabs(["Chart", "Data"])
with tab1:
    st.line_chart(df)
with tab2:
    st.dataframe(df)

# Container
with st.container():
    st.write("Grouped content")

# Empty placeholder (for updating content)
placeholder = st.empty()
placeholder.write("Loading...")
# Later:
placeholder.write("Done!")
```

## Performance

```python
# Cache data (re-runs only when arguments change)
@st.cache_data
def load_csv(path):
    return pd.read_csv(path)

# Cache resources (for DB connections, ML models — created once, shared)
@st.cache_resource
def load_model():
    return pickle.load(open("model.pkl", "rb"))

# Show spinner during long operations
with st.spinner("Training model..."):
    model.fit(X, y)
st.success("Done!")
```

---

## Key Takeaways

1. **Streamlit's execution model is top-to-bottom re-run.** Every widget interaction triggers a full script re-run. Design your code knowing this.

2. **`@st.cache_data` is not optional for data loading.** Without it, data is re-fetched on every interaction — making the app frustratingly slow.

3. **The EDA pattern is reusable.** Load → Filter (sidebar widgets) → Display → Download. This works for any tabular dataset in any domain.

4. **The ML app pattern is reusable.** Sidebar inputs → `pd.DataFrame(input, index=[0])` → `model.predict(df)` → display result. Works for any classification or regression model.

5. **Separate training from serving.** Train your model in a separate script, save it with `pickle`, load it in the Streamlit app. This is how production ML apps work.

6. **Feature importance is the bridge between model and human.** Always visualize it — it makes models explainable to non-technical stakeholders.

7. **Deploy to Streamlit Community Cloud first.** Zero config, free, auto-deploys from GitHub. Perfect for portfolio projects and demos.

8. **Streamlit is a rapid prototyping superpower for ML.** You built 10 different interactive apps — stock screeners, DNA analyzers, sports dashboards, ML classifiers, regression tools — all in pure Python, all in one course.

---

*Notes compiled from: Build 12 Data Science Apps with Python and Streamlit by Chanin Nantasenamat (Data Professor) | freeCodeCamp*
*Code: [github.com/dataprofessor/streamlit_freecodecamp](https://github.com/dataprofessor/streamlit_freecodecamp)*