# Day 1 — ML Essentials: NumPy, Pandas, Matplotlib

## Overview
This session covers the three core Python libraries used throughout the course. These tools form the foundation of any data science or machine learning workflow.

---

## Libraries

### NumPy
- Provides fast, efficient numerical computation using **arrays** (instead of Python lists)
- Arrays support vectorized operations — no need for explicit loops
- Key functions:
  - `np.array()` — create an array
  - `.mean()`, `.max()`, `.min()`, `.sum()` — basic statistics
  - `.size`, `.shape` — array dimensions
  - Slicing: `array[row_start:row_end, col_start:col_end]`
  - `.reshape(rows, cols)` — change array dimensions without changing data

```python
import numpy as np

sales = np.array([200, 450, 300, 150, 500])
print(sales.mean())   # 320.0
print(sales.max())    # 500
print(sales.min())    # 150
```

---

### Pandas
- Built on top of NumPy; provides **DataFrames** — labeled 2D tables (like Excel in Python)
- Key operations:
  - `pd.DataFrame(dict)` — create a DataFrame from a dictionary
  - `.head(n)` — view first n rows (default 5)
  - `.shape` — (rows, columns)
  - `.info()` — column types and null counts
  - `.describe()` — summary statistics
  - Boolean indexing: `df[df['col'] > value]`
  - Multiple conditions: `df[(df['col1'] < 10) & (df['col2'] == 'X')]`

```python
import pandas as pd

data = {'Name': ['Alice', 'Bob', 'Charlie'],
        'Score': [85, 92, 78],
        'Grade': ['B', 'A', 'C']}
df = pd.DataFrame(data)
print(df.shape)   # (3, 3)
print(df.head())
```

---

### Matplotlib
- Standard Python library for creating static visualizations
- Key functions:
  - `plt.bar(x, y)` — bar chart
  - `plt.plot(x, y)` — line chart
  - `plt.title()`, `plt.xlabel()`, `plt.ylabel()` — labels
  - `plt.show()` — render the plot

```python
import matplotlib.pyplot as plt

months = ['Jan', 'Feb', 'Mar']
profits = [5000, 7000, 6500]

plt.bar(months, profits)
plt.title('Monthly Profits')
plt.xlabel('Month')
plt.ylabel('Profit ($)')
plt.show()
```

---

## Key Concepts

| Concept | Library | Notes |
|---|---|---|
| 1D/2D arrays | NumPy | Core data structure for numerical data |
| Slicing | NumPy | `arr[0:2, 1:3]` extracts a subgrid |
| Reshape | NumPy | `arr.reshape(2, 6)` — total elements must stay the same |
| DataFrame | Pandas | Rows = samples, Columns = features |
| Boolean filter | Pandas | Use `&` for AND, `\|` for OR; wrap each condition in `()` |
| Bar chart | Matplotlib | Good for comparing categories |

---

## Tips
- NumPy arrays are **homogeneous** (all same type); Pandas DataFrames can have mixed types per column.
- Always check `.shape` after loading or transforming data to confirm dimensions.
- Use `.head()` and `.info()` as your first steps when exploring any new dataset.
