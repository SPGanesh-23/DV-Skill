# Superstore Sales — Data Understanding, Cleaning & Exploratory Analysis

This project performs an initial data understanding, cleaning, and exploratory
data analysis (EDA) pass on the **Sample Superstore** sales dataset. It is the
first task (`Task_1`) in a Big Data Analytics workflow, and focuses on getting
the raw data into a clean, well-understood state before any deeper modeling
or aggregation is attempted.

## Overview

The notebook loads the Superstore sales dataset, inspects its structure and
quality, converts date fields into proper datetime objects, engineers a new
feature (`Delivery Days`), checks for missing values, and produces two initial
visualizations: total sales by product category and the overall distribution
of sales values.

## Files

| File | Description |
|---|---|
| `Task_1.ipynb` | Original Google Colab notebook (source of truth, includes cell outputs). |
| `Task_1.py` | Auto-generated plain-Python export of the notebook (same logic, no outputs). |

## Dataset

- **Name:** Sample Superstore
- **Expected location (as written):** `/content/drive/MyDrive/Colab Notebooks/samplesuperstore.csv`
- **Access method:** Google Drive, mounted via `google.colab.drive` inside
  Google Colab.

> **Note:** Because the notebook was built in Google Colab, the file path is
> Colab-specific. To run this outside of Colab, replace the `pd.read_csv(...)`
> path with a local path to `samplesuperstore.csv`, and remove/skip the
> `drive.mount()` call.

Typical columns in this dataset include order/ship details (`Order Date`,
`Ship Date`, `Ship Mode`), customer information, geography (`Region`,
`State`, `City`), product details (`Category`, `Sub-Category`, `Product Name`),
and sales metrics (`Sales`, `Quantity`, `Discount`, `Profit`).

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

If running in Google Colab, `google.colab` is already available by default.

## Workflow / What the Code Does

The analysis proceeds through the following steps, in order:

### 1. Import Libraries
Loads `pandas` and `numpy` for data handling, and `matplotlib.pyplot` /
`seaborn` for visualization.

### 2. Load the Dataset
Reads `samplesuperstore.csv` into a pandas DataFrame (`df`) from Google Drive,
after mounting the drive.

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

## How to Run

**In Google Colab (as originally built):**
1. Upload `samplesuperstore.csv` to your Google Drive under
   `Colab Notebooks/`, or update the path to match your own Drive structure.
2. Open `Task_1.ipynb` in Colab and run all cells in order.
3. Authorize Drive access when prompted by `drive.mount()`.

**Locally / outside Colab:**
1. Place `samplesuperstore.csv` in your working directory.
2. In `Task_1.py`, remove the `from google.colab import drive` /
   `drive.mount(...)` lines.
3. Update the `pd.read_csv(...)` call to point to your local file, e.g.:
   ```python
   df = pd.read_csv("samplesuperstore.csv")
   ```
4. Run the script with:
   ```bash
   python Task_1.py
   ```

## Outputs

Running the notebook/script produces:
- Console/output tables from `head()`, `info()`, `describe()`, the category
  sales aggregation, and the null-value count.
- A **bar chart** of total Sales by Category.
<img width="802" height="628" alt="image" src="https://github.com/user-attachments/assets/0f8e2e9d-70cf-4273-8567-3d4f748ca9c1" />

- A **histogram** of the Sales distribution.
<img width="782" height="522" alt="image" src="https://github.com/user-attachments/assets/15141645-738e-44ca-bea1-ed2f63bfc1cf" />

## Possible Next Steps

This notebook covers understanding and light cleaning only. Natural
follow-ups for later tasks include:
- Handling outliers in `Sales`/`Profit`.
- Analyzing `Delivery Days` by `Ship Mode` or `Region`.
- Profitability analysis by `Category` / `Sub-Category`.
- Correlation analysis between `Discount`, `Sales`, and `Profit`.
