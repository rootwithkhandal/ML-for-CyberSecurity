# Day 3 — Web Attack Detection with Decision Trees

## Overview
This session uses the **CICIDS Thursday Morning** dataset to classify network traffic as either benign or one of several web attack types (SQL Injection, XSS, Brute Force). The focus is on data cleaning, preprocessing, and building a Decision Tree classifier.

---

## Dataset: `Thursday-WorkingHours-Morning-WebAttacks.pcap_ISCX.csv`
- Network flow data captured on a Thursday morning
- Each row = one network flow
- 80+ feature columns (packet lengths, flow durations, flag counts, etc.)
- Target column: `Label` — e.g., `BENIGN`, `Web Attack – Brute Force`, `Web Attack – XSS`, `Web Attack – Sql Injection`

---

## Workflow

### 1. Load Data
```python
import pandas as pd
df = pd.read_csv('Thursday-WorkingHours-Morning-WebAttacks.pcap_ISCX.csv')
print(df.shape)
df.head()
```

### 2. Data Cleaning
Real-world network datasets are messy. Clean in this order:

```python
import numpy as np

# Strip leading/trailing whitespace from column names
df.columns = df.columns.str.strip()

# Replace infinite values with NaN
df.replace([np.inf, -np.inf], np.nan, inplace=True)

# Drop rows with any missing values
df.dropna(inplace=True)

# Remove duplicate rows
df.drop_duplicates(inplace=True)

print(df.shape)  # Check how many rows remain
```

**Why each step matters:**
- Whitespace in column names causes `KeyError` when accessing columns by name
- `inf` values break most ML algorithms
- NaN values cause errors during model training
- Duplicates can bias the model toward overrepresented patterns

### 3. Analyze the Target Variable
```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.countplot(y='Label', data=df)
plt.tight_layout()
plt.show()

print(df['Label'].value_counts())
```
This reveals **class imbalance** — BENIGN traffic vastly outnumbers attack traffic, which is typical in real network data.

### 4. Data Preparation
```python
from sklearn.preprocessing import LabelEncoder
from sklearn.model_selection import train_test_split

# Separate features and target
X = df.drop(columns=['Label'])
y = df['Label']

# Encode string labels to integers
le = LabelEncoder()
y_encoded = le.fit_transform(y)

# 70/30 train-test split
X_train, X_test, y_train, y_test = train_test_split(
    X, y_encoded, test_size=0.3, random_state=42
)
```

### 5. Train Decision Tree
```python
from sklearn.tree import DecisionTreeClassifier

dt = DecisionTreeClassifier(random_state=42)
dt.fit(X_train, y_train)
```

### 6. Evaluate
```python
from sklearn.metrics import classification_report

y_pred = dt.predict(X_test)
print(classification_report(y_test, y_pred, target_names=le.classes_))
```

---

## Decision Tree — How It Works

A Decision Tree recursively splits the data by asking yes/no questions about features:

```
Is Flow Duration < 500?
├── Yes → Is SYN Flag Count > 0?
│         ├── Yes → BENIGN
│         └── No  → Web Attack – Brute Force
└── No  → BENIGN
```

- **Splitting criterion:** Gini impurity or entropy (information gain)
- **Leaf nodes:** Final class predictions
- **Depth:** Deeper trees fit training data better but may overfit

---

## Classification Report

The `classification_report` gives per-class metrics:

```
                          precision  recall  f1-score  support
BENIGN                       0.99    1.00      0.99    ...
Web Attack – Brute Force     0.95    0.88      0.91    ...
Web Attack – Sql Injection   0.87    0.79      0.83    ...
Web Attack – XSS             0.91    0.85      0.88    ...
```

| Metric | Meaning |
|---|---|
| **Precision** | How many predicted attacks were real attacks |
| **Recall** | How many real attacks were caught |
| **F1-Score** | Balance between precision and recall |
| **Support** | Number of actual samples in that class |

---

## Key Concepts

| Concept | Notes |
|---|---|
| Label encoding | Converts string labels to integers (0, 1, 2...) for sklearn |
| `le.classes_` | Maps encoded integers back to original label names |
| Class imbalance | BENIGN >> attack traffic; affects recall on minority classes |
| Overfitting | Decision Trees can memorize training data; use `max_depth` to limit |

---

## Tips
- Always check `df['Label'].value_counts()` before training — class imbalance affects which metrics matter.
- Use `target_names=le.classes_` in `classification_report` to see readable label names instead of integers.
- To reduce overfitting, try `DecisionTreeClassifier(max_depth=10, min_samples_leaf=5)`.
- For imbalanced data, consider `class_weight='balanced'` in the classifier.
