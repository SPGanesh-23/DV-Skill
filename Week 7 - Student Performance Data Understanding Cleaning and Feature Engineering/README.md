# Week 7 — Exploratory Data Analysis: Students Performance Dataset

## Overview
This notebook performs exploratory data analysis (EDA) on the **Students Performance** dataset (`StudentsPerformance.csv`). The goal is to understand the structure of the data, check its quality, compute descriptive statistics for the three core score variables, and derive summary metrics (total score and percentage) for each student.

## Dataset
- **File:** `StudentsPerformance.csv`
- **Source path (Colab):** `/content/drive/MyDrive/Colab Notebooks/StudentsPerformance.csv`
- **Key columns used:**
  - `math score`
  - `reading score`
  - `writing score`

## Workflow

### 1. Data Loading
The dataset is read into a pandas DataFrame using `pd.read_csv()`.

### 2. Initial Inspection
The following methods are used to get a first look at the data:
- `df.head()` — first 5 rows
- `df.tail()` — last 5 rows
- `df.shape` — number of rows and columns
- `df.info()` — column data types and non-null counts
- `df.describe()` — summary statistics (count, mean, std, min, max, quartiles) for all numeric columns

### 3. Data Cleaning
- `df.isnull().sum()` checks for missing values in each column.
- `df.dropna()` removes any rows containing missing values, ensuring the statistics computed afterward are not skewed by incomplete records.

### 4. Descriptive Statistics on Score Columns
A subset of columns — `math score`, `reading score`, `writing score` — is analyzed in detail using:
- `.describe()` — full summary statistics
- `.mean()` — average score
- `.median()` — middle value (robust to outliers)
- `.mode()` — most frequently occurring score(s)
- `.std()` — standard deviation (spread of scores around the mean)
- `.quantile(0.25)` — first quartile (25th percentile)
- `.quantile(0.50)` — second quartile / median (50th percentile)
- `.quantile(0.75)` — third quartile (75th percentile)

Together, the mean, median, and mode describe the **central tendency** of scores, while the standard deviation and quartiles describe the **spread and distribution**.

### 5. Feature Engineering
Two new columns are derived from the existing score columns:

| Column | Formula | Purpose |
|---|---|---|
| `Total Score` | `math score + reading score + writing score` | Combined score across all three subjects |
| `Percentage` | `(Total Score / 300) * 100` | Total score expressed as a percentage (300 = maximum possible total) |

## Libraries Used
| Library | Purpose |
|---|---|
| `pandas` | Data loading, cleaning, and manipulation |
| `numpy` | Numerical operations |
| `matplotlib.pyplot` | Visualization (imported, available for plotting) |
| `seaborn` | Statistical data visualization (imported, available for plotting) |

## How to Run
1. Place `StudentsPerformance.csv` in the working directory (or update the file path if not using Google Colab / Google Drive).
2. Run the notebook cells in order, from data loading through feature engineering.
3. Review the printed outputs (`.head()`, `.describe()`, etc.) to verify each step before moving to the next.

## Key Takeaways
- The dataset was checked for missing values and cleaned before analysis.
- Central tendency (mean, median, mode) and spread (std, quartiles) were computed for `math score`, `reading score`, and `writing score`.
- Two derived features — `Total Score` and `Percentage` — summarize each student's overall performance across subjects.
