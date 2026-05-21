# Day 4 — DDoS Attack Detection

## Overview
A **Distributed Denial of Service (DDoS)** attack floods a target with traffic from many sources, making it unavailable to legitimate users. This session trains a classifier on the **CICIDS Friday Afternoon** dataset to distinguish DDoS traffic from normal (BENIGN) traffic.

---

## Dataset: `Friday-WorkingHours-Afternoon-DDos.pcap_ISCX.csv`
- **225,745 rows × 79 columns**
- Each row = one network flow
- Target column: `Label` — values: `BENIGN`, `DDoS`
- Features: packet lengths, flow durations, inter-arrival times, flag counts, etc.

### Label Distribution (from exploration)
```
BENIGN    ~97,718
DDoS      ~128,027
```
Relatively balanced — both classes are well represented.

---

## Workflow

### 1. Load Data
```python
import pandas as pd

df = pd.read_csv('Friday-WorkingHours-Afternoon-DDos.pcap_ISCX.csv')
display(df.head())
display(df.shape)  # (225745, 79)
```

### 2. Explore Data
```python
# Strip whitespace from column names (important — many columns have leading spaces)
df.columns = df.columns.str.strip()

df.info()           # column types, non-null counts
df.describe()       # summary statistics
df.isnull().sum()   # check for missing values
df['Label'].value_counts()  # class distribution
```

**Notable findings from exploration:**
- `Flow Bytes/s` has 4 null values — handle before training
- `Flow Duration` has a minimum of `-1` — likely a data artifact
- Most features are `int64` or `float64`; `Label` is `object` (string)

### 3. Data Cleaning
```python
import numpy as np

df.replace([np.inf, -np.inf], np.nan, inplace=True)
df.dropna(inplace=True)
df.drop_duplicates(inplace=True)
```

### 4. Feature & Target Separation
```python
X = df.drop(columns=['Label'])
y = df['Label']
```

### 5. Encode Labels
```python
from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()
y_encoded = le.fit_transform(y)
# BENIGN → 0, DDoS → 1
```

### 6. Train/Test Split
```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y_encoded, test_size=0.2, random_state=42
)
```

### 7. Train & Evaluate
```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, confusion_matrix

model = RandomForestClassifier(n_estimators=100, random_state=42, n_jobs=-1)
model.fit(X_train, y_train)

y_pred = model.predict(X_test)
print(classification_report(y_test, y_pred, target_names=le.classes_))
```

---

## DDoS Attack Characteristics

DDoS traffic tends to show distinct patterns in network flow features:

| Feature | DDoS Pattern | Benign Pattern |
|---|---|---|
| `Flow Duration` | Very short (flood packets) | Longer, varied |
| `Total Fwd Packets` | Very high | Moderate |
| `Flow Packets/s` | Extremely high | Normal range |
| `SYN Flag Count` | High (SYN flood) | Low |
| `Fwd Packet Length` | Small, uniform | Varied |
| `Idle Mean` | Near zero | Higher |

---

## Feature Importance
After training a tree-based model, you can inspect which features matter most:

```python
import pandas as pd
import matplotlib.pyplot as plt

importances = pd.Series(model.feature_importances_, index=X.columns)
importances.nlargest(15).plot(kind='barh')
plt.title('Top 15 Feature Importances')
plt.tight_layout()
plt.show()
```

---

## Key Concepts

| Concept | Notes |
|---|---|
| Binary classification | Two classes: BENIGN vs DDoS |
| Random Forest | Strong baseline; handles 79 features well |
| `n_jobs=-1` | Uses all CPU cores for faster training |
| Feature importance | Tree-based models provide built-in feature ranking |
| Flow-based detection | Works on aggregated flow stats, not raw packets |

---

## Comparison: All 4 Days

| Day | Problem | Algorithm(s) | Dataset |
|---|---|---|---|
| 1 | Foundations | NumPy / Pandas / Matplotlib | — |
| 2 | Malware Detection | KNN, RF, DT, Naive Bayes | API_Functions.csv |
| 3 | Web Attack Detection | Decision Tree | CICIDS Thursday Morning |
| 4 | DDoS Detection | Random Forest | CICIDS Friday Afternoon |

---

## Tips
- With 225K rows, training can be slow. Use `n_jobs=-1` to parallelize.
- Try `max_features='sqrt'` in Random Forest to speed up training on 79 features.
- DDoS detection in production uses streaming data — consider online learning algorithms for real-time scenarios.
- Always validate that your model generalizes across different time windows, not just the same capture file.
