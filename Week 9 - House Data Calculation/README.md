# Week 9 – King County House Sales: Data Cleaning, Feature Engineering & Outlier Detection

A Jupyter/Google Colab notebook (`Week_9.ipynb`) that performs exploratory data analysis (EDA) on the **King County (Seattle area) house sales dataset**. It inspects the raw data, cleans invalid records, engineers new features (sale date parts, property age, renovation status, price per square foot), and uses box plots and the IQR rule to identify valuation outliers.

---

## Table of Contents

1. [Objectives](#objectives)
2. [Dataset](#dataset)
3. [Requirements](#requirements)
4. [How to Run](#how-to-run)
5. [Notebook Walkthrough](#notebook-walkthrough)
6. [Engineered Features](#engineered-features)
7. [Key Results](#key-results)
8. [Methodology Notes](#methodology-notes)
9. [Known Issues and Possible Improvements](#known-issues-and-possible-improvements)
10. [Project Structure](#project-structure)

---

## Objectives

- Understand the structure, quality, and statistical profile of the dataset.
- Convert the raw `date` column into a usable datetime type and extract time-based features.
- Remove records with impossible room counts (zero bedrooms or bathrooms).
- Create features that describe a property's age and renovation history.
- Create unit valuation metrics (price per square foot of living area and of lot).
- Detect unusually priced properties using box plots and the **IQR (Interquartile Range)** method.

---

## Dataset

| Property | Value |
|---|---|
| File | `kc_house_data.csv` |
| Source | King County, WA house sales (commonly distributed on Kaggle) |
| Raw size | 21,613 rows × 21 columns |
| Missing values | None |
| Duplicate rows | None |
| Sale period | May 2014 – May 2015 |

**Original columns:** `id`, `date`, `price`, `bedrooms`, `bathrooms`, `sqft_living`, `sqft_lot`, `floors`, `waterfront`, `view`, `condition`, `grade`, `sqft_above`, `sqft_basement`, `yr_built`, `yr_renovated`, `zipcode`, `lat`, `long`, `sqft_living15`, `sqft_lot15`.

> The dataset is not included in this repository. Download `kc_house_data.csv` and place it where the notebook expects it (see [How to Run](#how-to-run)).

---

## Requirements

- Python 3.8+
- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`

Install locally with:

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

All of these come pre-installed in Google Colab.

---

## How to Run

### Option A: Google Colab (as originally written)

1. Upload `kc_house_data.csv` to Google Drive at `MyDrive/Colab Notebooks/`.
2. Open `Week_9.ipynb` in Colab.
3. **Run the Drive-mounting cell first** (`drive.mount('/content/drive')`), then run the remaining cells in order.

### Option B: Local Jupyter

1. Place `kc_house_data.csv` in the same folder as the notebook.
2. In the notebook, remove the `google.colab` import and `drive.mount(...)` cell.
3. Change the loading line to:
   ```python
   df = pd.read_csv("kc_house_data.csv")
   ```
4. Launch with `jupyter notebook` and run all cells in order.

---

## Notebook Walkthrough

### 1. Basic Data Inspection
Loads the data and runs standard checks: `head()`, `tail()`, `info()`, `describe()`, `shape`, `columns`, `isnull().sum()`, and `duplicated().sum()`. This confirms the data types, value ranges, and that there are no nulls or duplicates.

### 2. Parse Sale Date
The raw `date` column is stored as text like `20141013T000000`. The notebook keeps the first 8 characters (`YYYYMMDD`) and converts them to a real datetime with `pd.to_datetime(..., format="%Y%m%d", errors="coerce")`. It then extracts `sale_year`, `sale_month`, and `sale_quarter`.

### 3. Handle Missing Values and Invalid Room Counts
There are no missing values, but `describe()` shows minimum values of 0 for bedrooms and bathrooms, which are not realistic for a sold house. Rows where either value is 0 are removed.

### 4. Property Age and Renovation Features
Creates `property_age`, `is_renovated`, and `years_since_renovation` from the sale year, build year, and renovation year.

### 5. Unit Valuation Metrics: Price per Square Foot
Creates `price_per_sqft_living` and `price_per_sqft_lot`, which allow fair comparison between houses of different sizes.

### 6. Box Plots for Valuation Outliers
Draws two horizontal box plots (one per metric) to visualize the spread and extreme values.

### 7. Statistical Outlier Detection Using IQR
Computes Q1, Q3, IQR, and the lower/upper fences for each metric, then counts the rows that fall outside them.

### 8. Final Dataset Preview
Displays a summary of the key original and engineered columns.

---

## Engineered Features

| Feature | Formula | Meaning |
|---|---|---|
| `sale_year` | year of `date` | Year the house was sold |
| `sale_month` | month of `date` | Month of sale (1–12) |
| `sale_quarter` | quarter of `date` | Quarter of sale (1–4) |
| `property_age` | `sale_year − yr_built` | Age of the house at time of sale |
| `is_renovated` | `1` if `yr_renovated > 0`, else `0` | Whether the house was ever renovated |
| `years_since_renovation` | `sale_year − yr_renovated` if renovated, else `0` | Time between renovation and sale |
| `price_per_sqft_living` | `price / sqft_living` | Price per square foot of interior living space |
| `price_per_sqft_lot` | `price / sqft_lot` | Price per square foot of land |

---

## Key Results

**Data quality**

| Check | Result |
|---|---|
| Rows before cleaning | 21,613 |
| Houses with 0 bedrooms | 13 |
| Houses with 0 bathrooms | 10 |
| Rows after cleaning | 21,597 |

**Price per square foot (after cleaning)**

| Metric | Mean | Median | Min | Max |
|---|---|---|---|---|
| Living area | ≈ $264.12 | ≈ $244.64 | ≈ $87.59 | ≈ $810.14 |
| Lot size | ≈ $88.98 | ≈ $59.26 | ≈ $0.16 | ≈ $2,027.21 |

**IQR outlier detection**

| Metric | Q1 | Q3 | IQR | Lower bound | Upper bound | Outliers |
|---|---|---|---|---|---|---|
| Price/sqft (living) | 182.29 | 318.31 | 136.02 | −21.74 | 522.35 | **705** |
| Price/sqft (lot) | 33.33 | 107.61 | 74.28 | −78.08 | 219.02 | **1,718** |

**Interpretation:** Both distributions are right-skewed (the mean is above the median). Lot-based pricing has far more outliers because lot sizes vary enormously, so a small lot can produce a very high price per square foot. Living-area pricing is more stable and is generally the better valuation metric.

---

## Methodology Notes

- **IQR rule:** a value is flagged as an outlier if it is below `Q1 − 1.5 × IQR` or above `Q3 + 1.5 × IQR`. This method does not assume a normal distribution, which suits skewed data like prices.
- **Negative lower bounds:** because price per square foot cannot be negative, the lower fence never flags anything. All detected outliers here are on the high end.
- **Outliers are detected, not removed.** The notebook only counts and reports them. Whether to remove, cap, or keep them depends on the downstream task.
- **Why drop zero-room rows:** a house with no bedrooms or bathrooms is most likely a data-entry error and would distort room-based analysis.
- **Renovation encoding:** in this dataset `yr_renovated = 0` means "never renovated", which is why it is converted into a binary flag rather than treated as a year.

---

## Known Issues and Possible Improvements

- **Cell order:** the notebook reads the CSV (cell 2) before mounting Google Drive (cell 5). In a fresh Colab session, run the `drive.mount` cell first or the load will fail.
- **Repeated cells:** `df.head()` and `df.isnull().sum()` appear more than once. They are harmless but can be consolidated.
- **Hard-coded path:** the Drive path is specific to one account; use a relative path or a config variable for portability.
- **Possible extensions:**
  - Remove or winsorize outliers and compare model performance.
  - Analyze price per square foot by `zipcode` or `grade`.
  - Add a correlation heatmap and distribution plots.
  - Train a regression model (e.g., linear regression, random forest) to predict `price`.

---

## Project Structure

```
.
├── Week_9.ipynb        # Main analysis notebook
├── kc_house_data.csv   # Dataset (download separately)
└── README.md           # This file
```
