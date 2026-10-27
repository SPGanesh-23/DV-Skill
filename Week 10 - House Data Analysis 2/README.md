# Week 10 – King County House Prices: Exploratory Data Analysis (EDA)

A Jupyter/Google Colab notebook that performs an exploratory analysis of a cleaned **King County (Washington) house sales** dataset. It examines how living area, grade, condition, waterfront access, renovation, age, and location relate to **sale price**, and summarizes the results as a real-estate investment report.

---

## Table of Contents

1. [Objective](#objective)
2. [Dataset](#dataset)
3. [Tech Stack](#tech-stack)
4. [Project Structure](#project-structure)
5. [Setup and Usage](#setup-and-usage)
6. [Notebook Walkthrough](#notebook-walkthrough)
7. [Output](#output)
8. [Key Findings](#key-findings)
9. [Limitations](#limitations)
10. [Possible Extensions](#possible-extensions)

---

## Objective

- Load and understand a cleaned house sales dataset.
- Measure how strongly living area relates to price and flag unusual properties.
- Compare prices across property **grade** and **condition**.
- Quantify the **waterfront price premium**.
- Identify **geographic clusters** of high-value properties.
- Combine several variables (age, renovation, bedrooms, bathrooms, price per sq ft) into an overall valuation view.

---

## Dataset

**File:** `cleaned_kc_house_data.csv`

The notebook reads the file from Google Drive (`/content/drive/MyDrive/Colab Notebooks/`). The dataset is not included in this repository, so place your own copy at the path above or update the path in the notebook.

- **Records:** 21,597 sales
- **Columns:** 29
- **Missing values:** none in the inspected columns

| Group | Columns | Description |
|---|---|---|
| Price | `price`, `price_per_sqft_living`, `price_per_sqft_lot` | Sale price and unit prices |
| Size | `sqft_living`, `sqft_lot`, `sqft_above`, `sqft_basement`, `bedrooms`, `bathrooms`, `floors` | Property size and layout |
| Quality | `grade`, `condition`, `view`, `waterfront` | Quality and desirability ratings |
| Age | `yr_built`, `yr_renovated`, `property_age`, `is_renovated`, `years_since_renovation` | Age and renovation history |
| Location | `zipcode`, `lat`, `long` | Geographic position |
| Time | `date`, `sale_year`, `sale_month`, `sale_quarter` | Sale timing |

**Columns created in the notebook:** `high_value` (top 10% of prices), `lat_cluster`, `long_cluster` (0.02° grid cells).

---

## Tech Stack

- **Python 3**
- [pandas](https://pandas.pydata.org/) – loading and aggregating tabular data
- [NumPy](https://numpy.org/) – numerical operations
- [Matplotlib](https://matplotlib.org/) – plotting
- [seaborn](https://seaborn.pydata.org/) – box plots, regression plots, and heatmaps
- Google Colab (notebook environment, with Google Drive for file access)

---

## Project Structure

```
.
├── Week_10.ipynb                 # Main analysis notebook
├── cleaned_kc_house_data.csv     # Input dataset (not included, add your own)
└── README.md                     # Project documentation
```

---

## Setup and Usage

### Option 1: Google Colab (recommended)

1. Upload `Week_10.ipynb` to Google Colab.
2. Upload `cleaned_kc_house_data.csv` to `MyDrive/Colab Notebooks/` in your Google Drive.
3. Mount Google Drive **before** loading the CSV:
   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```
   In the notebook file the load cell comes before the mount cell, so run the mount cell first.
4. Run all cells (**Runtime → Run all**).

### Option 2: Run locally

1. Install the dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```
2. Remove the `google.colab` import and the `drive.mount` cell, then change the file path:
   ```python
   df = pd.read_csv("cleaned_kc_house_data.csv")
   ```
3. Launch Jupyter and open the notebook:
   ```bash
   jupyter notebook Week_10.ipynb
   ```

Later sections reuse variables from earlier ones (for example `mean_premium` and `high_value`), so run the cells in order.

---

## Notebook Walkthrough

### 1. Basic data inspection
`head()`, `tail()`, `info()`, `describe()`, `shape`, and `columns` to understand the size and structure of the data.

### 2. Living area and house price
- Computes the correlation between `sqft_living` and `price`.
- Draws regression plots to check whether the relationship is linear.
- Flags **unusual properties** using the IQR rule on `price_per_sqft_living` (values outside Q1 − 1.5 × IQR and Q3 + 1.5 × IQR) and highlights them on a scatter plot.

### 3. Property grade and condition
- Box plots of price by grade and by condition.
- Median and variation tables (count, mean, median, std, min, max).
- Finds outliers **within each grade**.
- Builds a grade × condition pivot table of median prices and shows it as a heatmap.

### 4. Waterfront price premium
- Compares waterfront and non-waterfront prices (count, mean, median, std).
- Calculates the mean and median premium as a percentage.
- Compares average size, grade, and condition between the two groups to show what else differs.

### 5. Geographic house price analysis
- Scatter plot of latitude vs. longitude colored by price (clipped at the 99th percentile).
- Flags the **top 10%** of prices as `high_value` and plots them against the rest.
- Profiles high-value vs. other properties.

### 6. Multivariable property valuation
- Ensures `property_age`, `is_renovated`, and the price-per-sq-ft columns exist.
- Plots price per sq ft by grade and condition, property age vs. price, renovated vs. non-renovated prices, and median price by bedrooms and bathrooms.

### 7. Additional visualizations
Average price per sq ft by grade, living area vs. price per sq ft, and a correlation matrix of the key variables.

### 8. Geographic cluster identification
Snaps coordinates to a 0.02° grid, keeps cells with at least 10 sales, ranks them by median price, and plots the top clusters.

### 9. Real-estate investment summary
Prints a report that collects the main metrics: correlations, best grade and condition, waterfront premium, age effect, top cluster, and limitations.

### 10. Insights and conclusion
Ranks all numeric variables by correlation with price and prints the final conclusions.

---

## Output

- Tabular previews, summary statistics, and grouped tables.
- A printed investment summary report.
- The key charts below.

**1. Living Area vs. House Price**

<img width="942" height="607" alt="image" src="https://github.com/user-attachments/assets/0938e9c4-91d8-4113-b518-881862f1a064" />

**2. Median Price by Property Grade**

<img width="940" height="611" alt="image" src="https://github.com/user-attachments/assets/a034e4ac-68f1-4939-b12c-4f269a80f827" />

**3. Waterfront vs. Non-Waterfront Prices**

<img width="752" height="605" alt="image" src="https://github.com/user-attachments/assets/e1033b7c-12dc-42d7-a946-f1e3852e1b2c" />

**4. Clusters of High-Value Properties**

<img width="926" height="693" alt="image" src="https://github.com/user-attachments/assets/58400135-c87c-49a2-a1a7-ade09588c2c1" />

**5. Correlation Matrix**

<img width="1040" height="887" alt="image" src="https://github.com/user-attachments/assets/3f3b51f6-dd66-4adc-8365-0ac6a1197f25" />

---

## Key Findings

- **Living area** has the strongest numeric link to price (correlation **0.70**), followed by grade (0.67), price per sq ft (0.56), and bathrooms (0.53).
- **Grade** has a strong effect: median price rises from **$200,000 (grade 4)** to **$2,983,000 (grade 13)**, and the spread widens at higher grades.
- **Condition** is weaker and not strictly increasing (condition 3 has a slightly higher median than condition 4).
- **Waterfront** homes sell at a premium of about **212%** (mean) and **211%** (median): $1.66M vs. $0.53M on average. They are also larger and higher grade, so part of the premium is not the waterfront alone.
- **Property age** has almost no relationship with price (−0.054), while renovated homes average **$760,379** vs. **$530,473**.
- **High-value homes** (top 10%, at or above **$887,000**) cluster geographically; the top grid cell is near latitude 47.62, longitude −122.24 with a median price of about $2.1M.
- **Outliers:** 705 properties are unusual by price per sq ft, and 678 are price outliers within their grade.

---

## Limitations

- Results are **observed associations**, not causal effects.
- Some grade and condition groups are very small (grade 3 has one sale), so their statistics are unreliable.
- Grid clusters are approximate and depend on the chosen grid size.
- Most analyses look at one or two variables at a time and do not model interactions.

---

## Possible Extensions

- Fit a multivariable regression (for example on log-transformed price) to isolate each factor's effect.
- Try Random Forest or Gradient Boosting models for price prediction.
- Replace the grid method with K-Means or DBSCAN, or plot the results on an interactive map.
- Add `zipcode`-level analysis and use `view`, `floors`, and lot size, which are currently unused.
- Remove the duplicate regression plot in Section 2, or use a log scale or polynomial fit for the nonlinear check.
