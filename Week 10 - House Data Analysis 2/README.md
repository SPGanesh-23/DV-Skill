# Week 10 – King County House Price Analysis

An exploratory data analysis (EDA) of residential property sales, written as a real-estate investment study. The notebook examines which property characteristics are associated with sale price, quantifies the waterfront premium, and locates geographic clusters of high-value sales.

> **Note:** all findings are *observed associations*, not causal effects. See [Limitations](#limitations).

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Dataset](#dataset)
3. [Notebook Structure](#notebook-structure)
4. [Key Findings](#key-findings)
5. [Setup and Usage](#setup-and-usage)
6. [Methodology Notes](#methodology-notes)
7. [Limitations](#limitations)
8. [Possible Next Steps](#possible-next-steps)

---

## Project Overview

**Goal:** identify the factors that influence house prices and summarize them in a way useful for property valuation and investment decisions.

**Questions the notebook answers:**

- How strongly does living area relate to price, and is the relationship linear?
- How do property grade and condition affect price and price variability?
- How large is the waterfront price premium?
- Where are high-value properties concentrated geographically?
- Which combination of variables best describes market valuation?

---

## Dataset

| Item | Detail |
|---|---|
| File | `cleaned_kc_house_data.csv` (not included in this repository) |
| Records | 21,597 sales |
| Columns | 29 |
| Location | King County, Washington (latitude ≈ 47.x, longitude ≈ −122.x) |
| Sale period | 2014–2015 |

**Original columns** include `price`, `bedrooms`, `bathrooms`, `sqft_living`, `sqft_lot`, `floors`, `waterfront`, `view`, `condition`, `grade`, `yr_built`, `yr_renovated`, `zipcode`, `lat`, and `long`.

**Pre-engineered columns** already present in the cleaned file: `sale_year`, `sale_month`, `sale_quarter`, `property_age`, `is_renovated`, `years_since_renovation`, `price_per_sqft_living`, `price_per_sqft_lot`.

**Columns created inside the notebook:** `high_value` (top 10% of prices), `lat_cluster`, and `long_cluster` (grid cells for geographic grouping).

The dataset has no missing values (all 21,597 rows are non-null in every column inspected).

---

## Notebook Structure

| Section | Topic | What it does |
|---|---|---|
| 1 | Basic Data Inspection | `head`, `tail`, `info`, `describe`, `shape`, `columns` |
| 2 | Living Area vs. Price | Correlation, regression plots, IQR-based detection of unusual price-per-sq-ft properties |
| 3 | Grade and Condition | Box plots, median and variation tables, per-grade outliers, grade × condition heatmap |
| 4 | Waterfront Premium | Group summary, box plot, mean and median premium calculation |
| 5 | Geographic Analysis | Price-colored lat/long scatter, top-10% high-value flag and profile |
| 6 | Multivariable Valuation | Age, renovation, bedrooms, bathrooms, price per sq ft |
| 7 | Additional Visualizations | Price per sq ft by grade, area vs. price per sq ft, correlation matrix |
| 8 | Geographic Clusters | 0.02° grid clusters ranked by median price |
| 9 | Investment Summary | Printed report that pulls the main metrics together |
| 10 | Insights and Conclusion | Ranked correlations with price and final conclusions |

---

## Key Findings

### 1. Living area is the strongest numeric predictor
- Correlation between `sqft_living` and `price`: **0.70**.
- Ranked correlations with price: `sqft_living` (0.70) > `grade` (0.67) > `price_per_sqft_living` (0.56) > `bathrooms` (0.53) > `bedrooms` (0.31) > `lat` (0.31) > `waterfront` (0.27).

### 2. Grade has a strong, consistent effect; condition does not
- Median price rises steadily with grade, from **$200,000 (grade 4)** to **$2,983,000 (grade 13)**.
- Price *spread* also widens sharply at higher grades, so top-grade homes are both more expensive and less predictable.
- Condition is weaker and non-monotonic: the median for condition 3 ($450,250) is slightly above condition 4 ($440,000), and condition 5 is highest ($526,000). Condition matters most when combined with grade (see the heatmap in Section 3).

### 3. Waterfront carries a large premium

| | Count | Mean price | Median price |
|---|---|---|---|
| Non-waterfront | 21,434 | $531,673 | $450,000 |
| Waterfront | 163 | $1,661,876 | $1,400,000 |

Observed premium: **≈ 212% (mean)** and **≈ 211% (median)**. Waterfront homes are also larger (avg. 3,174 vs. 2,072 sq ft) and higher grade (8.8 vs. 7.6), so part of the premium reflects size and quality, not the waterfront alone.

### 4. Property age matters little; renovation matters more
- Age vs. price correlation is only **−0.054**.
- Renovated homes average **$760,379** vs. **$530,473** for non-renovated homes.

### 5. High-value properties cluster geographically
- The top-10% threshold is **$887,000** (2,161 properties).
- High-value homes have a median price of $1,156,000, average 3,566 sq ft, and average grade 9.5. Their waterfront share is about 5.3% vs. 0.25% for other homes.
- The highest-ranked grid cell is centered near **lat 47.62, long −122.24** (median price ≈ $2.1M).

### 6. Outliers
- **705** properties are unusual by price per sq ft (IQR rule, whole dataset).
- **678** are price outliers *within their grade* (IQR rule per grade).

---

## Setup and Usage

### Requirements

- Python 3.8+
- `pandas`, `numpy`, `matplotlib`, `seaborn`
- Jupyter Notebook, JupyterLab, or Google Colab

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

### Running in Google Colab (as written)

1. Upload `cleaned_kc_house_data.csv` to `MyDrive/Colab Notebooks/`.
2. Run the Drive-mount cell (`drive.mount('/content/drive')`) **before** the cell that loads the CSV. In the notebook file the load cell appears first, so run them in the order mount → load.
3. Run the remaining cells top to bottom.

### Running locally

1. Remove the `google.colab` import and the `drive.mount` cell.
2. Change the data path to your local file:

   ```python
   df = pd.read_csv("cleaned_kc_house_data.csv")
   ```
3. Run all cells in order. Later sections depend on variables from earlier ones (for example, `mean_premium` from Section 4 and `high_value` from Section 5 are used in Sections 8–9).

---

## Methodology Notes

- **Outlier detection:** the interquartile range (IQR) rule flags values below Q1 − 1.5 × IQR or above Q3 + 1.5 × IQR.
- **Median vs. mean:** medians are used alongside means because house prices are right-skewed, and a few very expensive homes inflate averages.
- **High-value definition:** a property is "high value" if its price is at or above the 90th percentile.
- **Geographic clusters:** coordinates are snapped to a 0.02° grid (`GRID_SIZE`). Only cells with at least 10 sales are ranked, to avoid conclusions from tiny samples. This is an approximation, not a statistical clustering algorithm such as K-Means or DBSCAN.
- **Map color cap:** the price color scale is clipped at the 99th percentile so a handful of extreme sales do not wash out the map.

---

## Limitations

- **Correlation is not causation.** For example, waterfront homes are also larger and higher grade, so the raw premium overstates the effect of the waterfront alone.
- **Grade and condition categories are uneven.** Some grades have very few sales (grade 3 has one record), so their statistics are unreliable.
- **Grid clusters are approximate** and depend on the chosen grid size.
- **Single time window.** Results reflect 2014–2015 market conditions.
- **Univariate and pairwise views.** Most analyses look at one or two variables at a time, so interactions between variables are not modeled.

---

## Possible Next Steps

- Fit a multivariable regression (for example, on log-transformed price) to isolate each factor's effect.
- Try tree-based models such as Random Forest or Gradient Boosting for price prediction.
- Replace the grid method with real clustering (K-Means, DBSCAN) or a folium/geopandas map.
- Add `zipcode`-level analysis and use `view`, `floors`, and lot size, which are currently unused.
- Remove the duplicate regression plot cell (Section 2 draws the same plot twice; the second is titled "Nonlinear Relationship" and could instead use a log scale or a polynomial fit).
