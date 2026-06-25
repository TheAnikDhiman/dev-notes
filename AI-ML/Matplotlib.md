# Tutorial 8 — Matplotlib: Simple Visualization Library
### Krish Naik Machine Learning Playlist | Study Notes

---

## 1. Why Visualization Matters in ML

Before you train a single model, you need to **see** your data. Visualization is not optional — it is a core step in every ML workflow:

- **Exploratory Data Analysis (EDA)** — understand distributions, spot outliers, check for class imbalance
- **Feature analysis** — see correlations, identify redundant features
- **Model evaluation** — plot learning curves, confusion matrices, ROC curves
- **Communication** — show results to non-technical stakeholders

> **Rule of thumb:** If you can't explain your data visually, you don't understand it well enough to model it.

---

## 2. What is Matplotlib?

Matplotlib is Python's **foundational plotting library**, created in 2003 by John Hunter. Almost everything in the Python visualization ecosystem (Seaborn, Pandas `.plot()`, even parts of TensorBoard) is built on top of Matplotlib.

**Key characteristics:**
- Low-level — gives maximum control over every element of a plot
- MATLAB-inspired API (hence the name)
- Renders to screen, PNG, PDF, SVG, and more
- Works inside Jupyter Notebook with inline rendering

```python
import matplotlib.pyplot as plt  # standard alias — always use plt
```

---

## 3. The Two Interfaces — Crucial Conceptual Distinction

Matplotlib offers two ways to make plots. Understanding both prevents confusion:

### 3.1 Pyplot Interface (Quick / Stateful)

Operates on the "current active figure." Good for quick, simple plots.

```python
plt.plot([1, 2, 3], [4, 5, 6])
plt.title("My Plot")
plt.show()
```

Matplotlib internally tracks the current figure and axes — every `plt.` call modifies whatever is currently active.

### 3.2 Object-Oriented Interface (Explicit / Recommended)

Explicitly create Figure and Axes objects. Better for complex, multi-subplot work and production code.

```python
fig, ax = plt.subplots()
ax.plot([1, 2, 3], [4, 5, 6])
ax.set_title("My Plot")
plt.show()
```

> **Best practice:** Use the **OO interface** for everything beyond quick exploration. It makes your code clearer and is what most professional notebooks use.

---

## 4. The Anatomy of a Matplotlib Figure

Understanding the object hierarchy prevents confusion:

```
Figure
└── Axes (one or more "subplots")
    ├── X Axis
    │   ├── Label
    │   └── Tick marks
    ├── Y Axis
    │   ├── Label
    │   └── Tick marks
    ├── Title
    ├── Plot elements (lines, bars, scatter points)
    └── Legend
```

| Object | What it is |
|--------|-----------|
| `Figure` | The entire canvas / window |
| `Axes` | A single plot area with its own x/y axes |
| `Axis` | One of the two number-line edges (x or y) |
| `Artist` | Every visible element (lines, text, patches) |

**Common confusion:** `Axes` ≠ `Axis`. `Axes` is the whole subplot; `Axis` is just one edge.

---

## 5. Core Plot Types

### 5.1 Line Plot — `ax.plot()`

Used for: time series, training loss/accuracy curves, any sequential data.

```python
epochs = [1, 2, 3, 4, 5]
loss   = [0.9, 0.6, 0.4, 0.25, 0.18]

fig, ax = plt.subplots()
ax.plot(epochs, loss, color='blue', linewidth=2, linestyle='--', marker='o')
ax.set_xlabel("Epoch")
ax.set_ylabel("Loss")
ax.set_title("Training Loss Curve")
plt.show()
```

**Key parameters:**
- `color` — named color (`'red'`) or hex (`'#FF5733'`)
- `linewidth` / `lw` — thickness of line
- `linestyle` / `ls` — `'-'`, `'--'`, `':'`, `'-.'`
- `marker` — `'o'`, `'s'`, `'^'`, `'*'`, etc.
- `label` — used by `ax.legend()`

### 5.2 Scatter Plot — `ax.scatter()`

Used for: showing relationship between two numerical features, visualizing clusters after dimensionality reduction (PCA, t-SNE).

```python
ax.scatter(x, y, c=labels, cmap='viridis', s=50, alpha=0.7)
```

- `c` → color by a third variable (e.g., class label)
- `cmap` → colormap (`'viridis'`, `'plasma'`, `'RdYlGn'`)
- `s` → marker size
- `alpha` → transparency (0 = invisible, 1 = fully opaque)

> **ML use case:** After applying PCA or t-SNE to reduce dimensions to 2D, scatter plot the result colored by class label to visually check if classes are separable.

### 5.3 Bar Plot — `ax.bar()` / `ax.barh()`

Used for: comparing categorical values, class frequency counts, feature importance scores.

```python
categories = ['Cat', 'Dog', 'Bird']
counts      = [400, 300, 150]

ax.bar(categories, counts, color=['steelblue', 'salmon', 'lightgreen'])
ax.set_ylabel("Count")
ax.set_title("Class Distribution")
```

`ax.barh()` creates horizontal bars — better when category names are long.

### 5.4 Histogram — `ax.hist()`

Used for: visualizing the **distribution** of a numerical feature — the single most important EDA plot.

```python
ax.hist(feature_values, bins=30, edgecolor='black', color='steelblue')
ax.set_xlabel("Value")
ax.set_ylabel("Frequency")
```

- `bins` → number of buckets. Too few = loss of detail; too many = noise.
- Look for: skewness, outliers, multimodal distributions (multiple peaks)

> **Why this matters:** If a feature is heavily skewed, linear models will struggle. A histogram tells you whether to apply log-transform or normalization before training.

### 5.5 Box Plot — `ax.boxplot()`

Used for: visualizing the **5-number summary** (min, Q1, median, Q3, max) and **outliers**.

```python
ax.boxplot([feature_a, feature_b, feature_c],
           labels=['Feature A', 'Feature B', 'Feature C'])
```

Reading a box plot:
- **Box** = interquartile range (IQR): middle 50% of data
- **Line inside box** = median
- **Whiskers** = extend to 1.5 × IQR from box edges
- **Points beyond whiskers** = outliers

> Box plots are excellent for **comparing feature scales** before deciding whether to normalize.

### 5.6 Pie Chart — `ax.pie()`

Used for: proportions. Use sparingly — bar charts are almost always clearer.

```python
ax.pie(counts, labels=categories, autopct='%1.1f%%', startangle=90)
```

---

## 6. Subplots — Multiple Plots in One Figure

One of the most useful patterns — display multiple plots side by side for comparison.

```python
fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(12, 5))

axes[0].plot(x, y1)
axes[0].set_title("Training Loss")

axes[1].plot(x, y2)
axes[1].set_title("Validation Loss")

plt.tight_layout()   # prevents overlapping labels
plt.show()
```

For a 2D grid of plots:
```python
fig, axes = plt.subplots(2, 3, figsize=(15, 8))
# Access with axes[row][col]
axes[0][0].hist(...)
axes[1][2].scatter(...)
```

---

## 7. Figure Customization — Making Publication-Quality Plots

### 7.1 Figure Size and DPI

```python
fig, ax = plt.subplots(figsize=(10, 6), dpi=150)
# figsize in inches, dpi controls resolution
```

### 7.2 Labels, Title, Legend

```python
ax.set_xlabel("X Label", fontsize=12)
ax.set_ylabel("Y Label", fontsize=12)
ax.set_title("Title", fontsize=14, fontweight='bold')
ax.legend(loc='upper right')  # or 'best' for auto-placement
```

### 7.3 Grid

```python
ax.grid(True, linestyle='--', alpha=0.5)
```

### 7.4 Axis Limits

```python
ax.set_xlim(0, 100)
ax.set_ylim(-1, 1)
```

### 7.5 Annotations — Pointing to Specific Values

```python
ax.annotate("Minimum Loss",
            xy=(best_epoch, min_loss),           # point to annotate
            xytext=(best_epoch + 1, min_loss + 0.1),  # where to put the text
            arrowprops=dict(arrowstyle='->'))
```

### 7.6 Colors and Colormaps

Named colors: `'red'`, `'blue'`, `'steelblue'`, `'salmon'`, `'#2E86AB'`

Colormaps (for multi-value mappings):
| Colormap | Good for |
|----------|---------|
| `'viridis'` | Sequential, colorblind-friendly |
| `'plasma'` | Sequential, high contrast |
| `'RdYlGn'` | Diverging (good/bad) |
| `'tab10'` | Categorical (10 distinct colors) |

---

## 8. Saving Figures

```python
plt.savefig("plot.png", dpi=300, bbox_inches='tight')
# bbox_inches='tight' prevents labels from being cut off
```

Supported formats: `.png`, `.pdf`, `.svg`, `.eps`, `.jpg`

> Always save **before** calling `plt.show()` — after `show()`, the figure is cleared.

---

## 9. Matplotlib in Jupyter Notebooks

Add this magic command at the top of your notebook to display plots inline:

```python
%matplotlib inline
```

Or for interactive, zoomable plots:

```python
%matplotlib notebook
```

---

## 10. Matplotlib vs Seaborn — When to Use Which

| | Matplotlib | Seaborn |
|--|-----------|---------|
| Level | Low-level (manual control) | High-level (built on Matplotlib) |
| Learning curve | Steeper | Easier for statistical plots |
| Customization | Maximum | Good defaults, less flexible |
| Best for | Any plot type, fine control | Statistical EDA plots |

> **Typical workflow:** Use Seaborn for quick EDA, switch to Matplotlib when you need precise control over a final figure.

Seaborn is covered separately, but know that `sns` plots return Matplotlib `Axes` objects — so everything you learn here applies to Seaborn plots too.

---

## 11. Common Mistakes and Fixes

| Mistake | Fix |
|---------|-----|
| Plot doesn't appear in script | Add `plt.show()` at the end |
| Saved figure is cut off | Use `bbox_inches='tight'` |
| Labels overlap in subplots | Call `plt.tight_layout()` |
| `Axes` vs `Axis` confusion | Remember: `Axes` = the whole subplot |
| Colors not enough for many classes | Use `plt.cm.tab20` colormap |

---

## 12. Essential Patterns for ML Notebooks

### Plotting Training Curves

```python
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))

ax1.plot(history['loss'], label='Train')
ax1.plot(history['val_loss'], label='Val')
ax1.set_title("Loss"); ax1.legend()

ax2.plot(history['accuracy'], label='Train')
ax2.plot(history['val_accuracy'], label='Val')
ax2.set_title("Accuracy"); ax2.legend()

plt.suptitle("Model Training History")
plt.tight_layout()
plt.show()
```

### Feature Distribution Grid

```python
fig, axes = plt.subplots(2, 3, figsize=(15, 8))
for i, (ax, col) in enumerate(zip(axes.flatten(), df.columns)):
    ax.hist(df[col], bins=25, edgecolor='black')
    ax.set_title(col)
plt.tight_layout()
```

---

## 13. Key Takeaways

- Matplotlib is the **foundation** of Python visualization — everything else builds on it
- Learn the **OO interface** (`fig, ax = plt.subplots()`) — it scales better than pyplot
- The **Figure → Axes → Artists** hierarchy is the mental model for everything
- **Histograms** and **scatter plots** are your most important EDA tools
- Always add labels, titles, and legends — unlabeled plots are meaningless
- Save figures with `bbox_inches='tight'` before calling `plt.show()`

---

## Quick Reference

```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(8, 5))

# Plot types
ax.plot(x, y)                    # line
ax.scatter(x, y, c=z)           # scatter
ax.bar(categories, values)       # bar
ax.hist(data, bins=30)           # histogram
ax.boxplot(data)                 # box plot

# Labels
ax.set_xlabel("X"); ax.set_ylabel("Y")
ax.set_title("Title")
ax.legend()
ax.grid(True, alpha=0.4)

# Save & show
plt.tight_layout()
plt.savefig("fig.png", dpi=300, bbox_inches='tight')
plt.show()
```

---
*Notes based on Krish Naik's ML Playlist — Tutorial 8*
*Supplemented with additional context for deeper understanding*
