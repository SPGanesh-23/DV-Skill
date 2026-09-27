# Superstore Sales — Data Understanding, Cleaning & Exploratory Analysis

This project performs an initial data understanding, cleaning, and exploratory
data analysis (EDA) pass on the **Sample Superstore** sales dataset. It is the
first task (`Task_1`) in a Big Data Analytics workflow, and focuses on getting
the raw data into a clean, well-understood state, then using targeted
visualizations to answer practical business questions about sales, profit,
and discounting.

## Overview

The notebook loads the Superstore sales dataset, inspects its structure and
quality, converts date fields into proper datetime objects, engineers a new
feature (`Delivery Days`), checks for missing values, and produces a series
of visualizations — bar plots, box plots, a scatter plot, and a correlation
heatmap — to explore sales performance, profit variation across categories,
the relationship between discounts and profit, and correlations between
numeric variables.

## Files

| File | Description |
|---|---|
| `Task_1_BDA.ipynb` | Original Google Colab notebook (source of truth, includes cell outputs). |
| `Task_1_BDA.py` | Auto-generated plain-Python export of the notebook (same logic, no outputs). |

## Dataset

- **Name:** Sample Superstore
- **Expected location (as written):** `/content/samplesuperstore.csv`
- **Access method:** Local upload to the Colab session's `/content/` directory
  (no Google Drive mounting is used in this notebook).

> **Note:** Because the notebook was built in Google Colab, the file path is
> Colab-specific. To run this outside of Colab, update the `pd.read_csv(...)`
> path to point to wherever `samplesuperstore.csv` lives on your machine.

Typical columns in this dataset include order/ship details (`Order Date`,
`Ship Date`), product details (`Category`), and sales metrics (`Sales`,
`Discount`, `Profit`).

## Requirements

The project depends on the following Python libraries:

```
pandas
numpy
matplotlib
seaborn
```

Install them with:

```bash
pip install pandas numpy matplotlib seaborn
```

## Workflow / What the Code Does

The analysis proceeds through the following steps, in order:

### 1. Import Libraries
Loads `pandas` and `numpy` for data handling, and `matplotlib.pyplot` /
`seaborn` for visualization.

### 2. Load the Dataset
Reads `samplesuperstore.csv` into a pandas DataFrame (`df`).

### 3. Initial Data Understanding
- **`df.head()`** — previews the first 5 rows to get a feel for the data's
  shape and content.
- **`df.info()`** — reports column names, non-null counts, and data types,
  which is used to confirm which columns need type conversion.
- **`df.describe()`** — generates summary statistics (mean, std, min, max,
  quartiles) for all numeric columns.

### 4. Data Cleaning: Date Conversion
`Order Date` and `Ship Date` are converted from raw string/object columns
into proper `datetime64` objects using `pd.to_datetime()`. This is a
necessary step because date arithmetic (used in the next step) only works
correctly once pandas recognizes these columns as datetimes rather than
plain text.

`df.info()` is called again afterward to confirm the dtype change took
effect.

### 5. Feature Engineering: Delivery Days
A new column, **`Delivery Days`**, is created by subtracting `Order Date`
from `Ship Date` and extracting the number of days:

```python
df['Delivery Days'] = (df['Ship Date'] - df['Order Date']).dt.days
```

This gives a simple, directly usable numeric measure of how long each order
took to ship — useful for later analysis of shipping performance.

### 6. Data Quality Checks
- **`df['Category'].unique()`** — lists the distinct product categories
  present, as a sanity check on the categorical data.
- **`df.isnull().sum()`** — counts missing values per column, to confirm
  whether any cleaning/imputation is needed before further analysis.

### 7. Exploratory Analysis: Sales by Category
Sales are aggregated by `Category` using `groupby().sum()`, then visualized
as a bar chart to compare total revenue contribution across product
categories.

### 8. Exploratory Analysis: Sales Distribution
A histogram (via `seaborn.histplot`, 30 bins) is plotted for the `Sales`
column to visualize how individual order sales values are distributed
(e.g., skew, typical order size, presence of outliers/high-value orders).

### 9. Part 1 — Bar Plots (Category Comparison)
<img width="632" height="506" alt="image" src="https://github.com/user-attachments/assets/e355387d-02d2-42a6-8974-8376972c5b1a" />
<img width="632" height="503" alt="image" src="https://github.com/user-attachments/assets/f16eb0d8-9058-4fb1-9557-cce031e7f2f1" />

**Purpose:** Compare sales and profit across categories to answer questions
like *"Which category generates maximum profit?"*
- `sns.barplot()` of `Profit` by `Category` — shows average profit per
  category (with confidence interval).
- `sns.barplot()` of `Sales` by `Category` — shows average sales per
  category.

### 10. Part 2 — Box Plots (Distribution & Outliers)
<img width="787" height="525" alt="image" src="https://github.com/user-attachments/assets/669ee437-cf79-4b32-86cd-3221c890a2ea" />

**Purpose:** Box plots reveal data distribution, median value, outliers, and
variation — useful for spotting inconsistent or extreme profit values.
- Overall `Profit` distribution across all orders.
- `Profit` distribution broken down by `Category`, to see which categories
  have more volatile or negative profit outcomes.

### 11. Part 3 — Discount vs. Profit Analysis
<img width="662" height="506" alt="image" src="https://github.com/user-attachments/assets/ad486990-a3c2-409f-8e74-603a5a75c4c0" />

**Purpose:** Find whether discounts improve sales or reduce profitability,
and answer *"At what discount level does profit start decreasing?"*
- Inspected the unique discount levels applied (`df["Discount"].unique()`).
- Used a scatter plot (`Discount` vs. `Profit`) to visually identify the
  discount threshold beyond which profit tends to decline.

### 12. Part 4 — Correlation Heatmap
<img width="678" height="577" alt="image" src="https://github.com/user-attachments/assets/382b18c8-7b28-4a7a-87cc-04004b456676" />

**Purpose:** Quantify linear relationships between numerical variables.
- Selected only numeric columns with `select_dtypes(include="number")`.
- Computed the correlation matrix with `.corr()`.
- Visualized it using `sns.heatmap()` with annotated values, making it easy
  to spot strong positive/negative relationships (e.g., Discount vs. Profit).

## How to Run

**In Google Colab (as originally built):**
1. Upload `samplesuperstore.csv` to the Colab session (`/content/`), or
   update the path to match wherever you've placed it.
2. Open `Task_1_BDA.ipynb` in Colab and run all cells in order.

**Locally / outside Colab:**
1. Place `samplesuperstore.csv` in your working directory.
2. In `Task_1_BDA.py`, update the `pd.read_csv(...)` call to point to your
   local file, e.g.:
   ```python
   df = pd.read_csv("samplesuperstore.csv")
   ```
3. Run the script with:
   ```bash
   python Task_1_BDA.py
   ```

## Outputs

Running the notebook/script produces:

- Console/output tables from `head()`, `info()`, `describe()`, the category
  sales aggregation, the null-value count, and the correlation matrix.
- A **bar chart** of total Sales by Category.
- A **histogram** of the Sales distribution.
- **Bar plots** comparing Profit and Sales across categories.
- **Box plots** showing overall Profit distribution and Profit variation by
  Category.
- A **scatter plot** of Discount vs. Profit.
- A **correlation heatmap** of all numeric variables.

## Possible Next Steps

This notebook covers understanding, cleaning, and exploratory visualization.
Natural follow-ups for later tasks include:

- Handling outliers in `Sales`/`Profit`.
- Analyzing `Delivery Days` by `Region` or other categorical fields.
- Deeper profitability analysis at the `Sub-Category` / `Product Name` level.
- Statistical testing (not just visual inspection) of the discount–profit
  relationship.
- Predictive modeling of `Profit` using `Discount`, `Sales`, and other
  engineered features.
