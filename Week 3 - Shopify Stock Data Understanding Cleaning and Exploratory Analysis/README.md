# AAPL Stock Data — Exploratory Data Analysis
## 1. Overview

This project performs an **Exploratory Data Analysis (EDA)** on historical stock price data for **Apple Inc. (AAPL)**. The dataset spans daily trading records from **December 12, 1980** to **March 24, 2022** (10,409 rows), and the notebook walks through data inspection, cleaning, derived-metric calculation, and statistical summarization using **pandas** and **NumPy**.

The notebook was built and run in **Google Colab**, with the source CSV loaded from Google Drive.

---

## 2. Dataset

| Property | Details |
|---|---|
| **File name** | `AAPL.csv` |
| **Source path used in notebook** | `/content/drive/MyDrive/Colab Notebooks/AAPL.csv` |
| **Rows** | 10,409 |
| **Columns** | 7 (raw) → 9 (after feature engineering) |

**Raw columns:**

| Column | Description |
|---|---|
| `Date` | Trading date (string, later converted to `datetime`) |
| `Open` | Opening price for the day |
| `High` | Highest price reached during the day |
| `Low` | Lowest price reached during the day |
| `Close` | Closing price for the day |
| `Adj Close` | Closing price adjusted for dividends/splits |
| `Volume` | Number of shares traded that day |

**Derived columns (added during analysis):**

| Column | Formula | Meaning |
|---|---|---|
| `Daily_Delta` | `Close − Open` | Absolute price change within the trading day |
| `Daily_Return` | `((Close − Open) / Open) × 100` | Percentage price change within the trading day |

> **Note:** To reproduce this notebook, place `AAPL.csv` in the path referenced in Cell 2, or update that path to point to your own copy of the dataset (e.g., a local file path if not using Colab).

---

## 3. Requirements

The notebook depends on the following Python libraries:

```
pandas
numpy
matplotlib
```

Install them with:

```bash
pip install pandas numpy matplotlib
```

If running outside Google Colab, replace the `pd.read_csv(...)` path in Cell 2 with a local path, and remove/replace any Colab-specific `display()` calls with `print()`.

---

## 4. Notebook Workflow

The analysis proceeds through the following stages, in order:

### Step 1 — Setup
Import `pandas`, `numpy`, and `matplotlib.pyplot`.

### Step 2 — Load the Data
Read `AAPL.csv` into a DataFrame `df`.

### Step 3 — Initial Inspection
- `df.head()` — preview the first few rows
- `df.shape` — confirm row/column counts
- `df.info()` — check data types and non-null counts
- `df.describe()` — generate summary statistics (mean, std, min, max, quartiles) for all numeric columns

### Step 4 — Data Cleaning
- Convert the `Date` column from string to a proper `datetime` type (UTC-aware) using `pd.to_datetime()`.
- Check for missing values across all columns with `df.isnull().sum()`.

### Step 5 — Price Extremes and Averages
- Identify the **highest** and **lowest** adjusted closing prices ever recorded.
- Compute the **average opening price** and **average closing price** across the full history.

### Step 6 — Daily Price Movement
- Sort the data chronologically by `Date`.
- Compute `Daily_Delta` (absolute change) for every trading day.
- Extract the **top 10 single-day gains** by sorting `Daily_Delta` in descending order.

### Step 7 — Daily Return (%) and Losses
- Compute `Daily_Return` (percentage change) for every trading day.
- Extract the **top 10 single-day losses** by sorting `Daily_Return` in ascending order.

### Step 8 — Volume Analysis
- Calculate the **average**, **maximum**, and **minimum** trading volume across the dataset.

### Step 9 — Anomaly Detection
- Flag "anomalous trading days" as any day where `Volume` exceeds **2× the average volume**.
- Display the date, price, and return details for each anomalous day (1,265 days met this criterion).

### Step 10 — Statistical Summary of Returns
- Compute the **mean**, **variance**, and **standard deviation** of `Daily_Return` across the entire dataset, giving a measure of AAPL's typical daily volatility.

---

## 5. Key Results

| Metric | Value |
|---|---|
| Average daily trading volume | ≈ 332.2 million shares |
| Maximum daily trading volume | 7,421,640,800 shares |
| Minimum daily trading volume | 0 |
| Mean daily return | ≈ −0.0079% |
| Variance of daily return | ≈ 5.39 |
| Standard deviation of daily return | ≈ 2.32% |
| Anomalous trading days (Volume > 2× average) | 1,265 out of 10,409 |

These figures indicate that while AAPL's average daily return over its full history is close to zero, individual days can swing significantly (a standard deviation of ~2.32% is high volatility), and a notable fraction of trading days (~12%) saw unusually heavy trading volume.

---

## 6. How to Run

1. Open `DBMS_Test_1.ipynb` in Google Colab (or Jupyter Notebook/JupyterLab).
2. If using Colab, mount Google Drive and ensure `AAPL.csv` exists at the referenced path. If running locally, update the file path in the second cell.
3. Run all cells in order (**Runtime → Run all** in Colab).
4. Review the printed outputs and DataFrames at each stage — no plots are currently rendered (`matplotlib` is imported but not yet used for visualization).

---

## 7. Possible Extensions

- Use the imported `matplotlib` (or `plotly`) to visualize price trends, daily returns, and volume over time.
- Add rolling averages (e.g., 50-day/200-day moving averages) for trend analysis.
- Compare AAPL's volatility against a benchmark index (e.g., S&P 500).
- Export cleaned data and summary statistics to a separate CSV/report for downstream use.

---

## 8. File Structure

```
.
├── DBMS_Test_1.ipynb   # Main analysis notebook
├── AAPL.csv             # Source dataset (not included — see Section 2)
└── README.md            # This file
```
