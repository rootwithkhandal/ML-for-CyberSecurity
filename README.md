# ML for CyberSecurity

A hands-on 4-day course applying machine learning techniques to real-world cybersecurity problems. Each day builds on the previous, progressing from Python fundamentals to training and evaluating classifiers on network intrusion and malware datasets.

## Project Structure

```
ML-for-CyberSec/
├── Day1-ML-Essentials/
│   ├── Day-1-Assignment.ipynb
│   └── Day-1-Assignment.pdf
├── Day2-Malware-Detection/
│   ├── Malware detection assignment.ipynb
│   └── API_Functions.csv
├── Day3-Web-Attack-Detection/
│   ├── Assignment-Day3.ipynb
│   └── Thursday-WorkingHours-Morning-WebAttacks.csv
├── Day4-DDoS-Detection/
│   ├── DDOS Attack Assignment.ipynb
│   └── Friday-WorkingHours-Afternoon-DDos_ISCX.csv
└── README.md
```

## Course Structure

### Day 1 — ML Essentials (NumPy, Pandas, Matplotlib)
Foundational Python data science skills needed for the rest of the course.

- NumPy array creation and statistical operations
- Pandas DataFrame construction and filtering
- Matplotlib bar chart visualization
- 2D array slicing and reshaping
- Boolean indexing with multiple conditions

**Folder:** `Day1-ML-Essentials/` &nbsp;|&nbsp; **Files:** `Day-1-Assignment.ipynb`, `Day-1-Assignment.pdf`

---

### Day 2 — Malware Detection via API Call Analysis
Build and compare classification models to distinguish malware from benign software based on API call patterns.

- **Dataset:** `API_Functions.csv` — rows represent software samples; columns represent API function calls; target column is `Type`
- **Algorithms:** K-Nearest Neighbors (KNN), Random Forest, Decision Tree, Naive Bayes
- **Techniques:** Train/test split, model evaluation (accuracy, precision, recall, F1-score), confusion matrix visualization, hyperparameter tuning with `GridSearchCV`

**Folder:** `Day2-Malware-Detection/` &nbsp;|&nbsp; **Files:** `Malware detection assignment.ipynb`, `API_Functions.csv`

---

### Day 3 — Web Attack Detection with Decision Trees
Classify network traffic as benign or a web attack type using the CICIDS dataset.

- **Dataset:** `Thursday-WorkingHours-Morning-WebAttacks.pcap_ISCX.csv`
- **Steps:** Data loading, cleaning (strip whitespace, handle inf/NaN, drop duplicates), target distribution analysis, label encoding, 70/30 train-test split, Decision Tree training, evaluation via `classification_report`

**Folder:** `Day3-Web-Attack-Detection/` &nbsp;|&nbsp; **Files:** `Assignment-Day3.ipynb`, `Thursday-WorkingHours-Morning-WebAttacks.csv`

---

### Day 4 — DDoS Attack Detection
Detect Distributed Denial of Service (DDoS) attacks from network flow data.

- **Dataset:** `Friday-WorkingHours-Afternoon-DDos.pcap_ISCX.csv` — 225,745 rows × 79 features; binary target: `BENIGN` vs `DDoS`
- **Steps:** Data loading and exploration, feature engineering, model training and evaluation

**Folder:** `Day4-DDoS-Detection/` &nbsp;|&nbsp; **Files:** `DDOS Attack Assignment.ipynb`, `Friday-WorkingHours-Afternoon-DDos_ISCX.csv`

---

## Datasets

All datasets are from the **[CICIDS (Canadian Institute for Cybersecurity Intrusion Detection System)](https://www.unb.ca/cic/datasets/ids-2017.html)** benchmark — a widely used standard in ML-based network intrusion detection research.

## Requirements

```
numpy
pandas
matplotlib
seaborn
scikit-learn
```

Install dependencies with:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn
```

## Usage

Each day's assignment is a self-contained Jupyter notebook. Open the notebook for the relevant day and run the cells in order. Datasets should be placed in the same directory as the notebook (or update the file path in the `pd.read_csv()` call accordingly).
