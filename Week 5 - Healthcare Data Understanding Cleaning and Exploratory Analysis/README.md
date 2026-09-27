# Task 4 — Healthcare Dataset: Exploratory Data Analysis (EDA)

## Overview
This notebook performs exploratory data analysis (EDA) on a synthetic **healthcare dataset** containing 500 patient records. The workflow covers loading the raw data, cleaning and type-casting it, engineering a new feature (length of hospital stay), and summarizing key statistics and categorical distributions.

## Dataset
- **File:** `healthcare_dataset.csv` (loaded from Google Drive via Colab: `/content/drive/MyDrive/Colab Notebooks/healthcare_dataset.csv`)
- **Shape:** 500 rows × 9 columns
- **Columns:**
  | Column | Description |
  |---|---|
  | `Patient_ID` | Unique patient identifier (e.g., `P1001`) |
  | `Gender` | Male / Female |
  | `Age` | Patient age in years |
  | `Medical_Condition` | Diagnosed condition (e.g., Hypertension, Pneumonia, Diabetes Type 2) |
  | `Admission_Date` | Date the patient was admitted (`DD-MM-YYYY`) |
  | `Admission_Type` | Urgent / Emergency / Routine |
  | `Medical_Code` | ICD-10 diagnosis code |
  | `Billing_Amount` | Billed amount for the visit |
  | `Discharge_Date` | Date the patient was discharged (`DD-MM-YYYY`) |

## Environment & Dependencies
Built and run in **Google Colab**. Requires:
```
pandas
numpy
matplotlib
```
Install locally with:
```bash
pip install pandas numpy matplotlib
```

## Workflow / Steps Performed

1. **Import libraries** — `pandas`, `numpy`, `matplotlib.pyplot`.
2. **Load the dataset** from CSV into a DataFrame.
3. **Initial inspection**
   - `df.head()` — preview the first rows.
   - `df.shape` — confirmed dataset size: **500 rows, 9 columns**.
   - `df.describe()` — summary statistics for numeric columns (`Age`, `Billing_Amount`).
4. **Missing value check** — `df.isnull().sum()` confirmed **no missing values** in any column.
5. **Data cleaning**
   - Dropped any null rows (`df.dropna()`) as a safeguard, though none were found.
   - Converted `Admission_Date` and `Discharge_Date` from string to proper `datetime` objects (`dayfirst=True`, UTC-aware) using `pd.to_datetime`.
   - Standardized `Admission_Type` text by stripping whitespace and lowercasing (`urgent`, `emergency`, `routine`).
6. **Feature engineering**
   - Created a new column `Hospital_Stays` = `Discharge_Date − Admission_Date`, then converted the resulting timedelta to an integer number of days.
7. **Statistical summaries**
   - **Hospital stay length:** mean ≈ 2.74 days, ranging from 1 to 10 days.
   - **Billing amount:** mean ≈ ₹/$7,249, ranging from 2,300 to 14,200.
8. **Categorical analysis**
   - `value_counts()` on `Medical_Condition` to see the frequency of each diagnosis (most common: COPD, Appendicitis, Stroke; least common: Hypothyroidism).
   - `pd.crosstab()` between `Medical_Condition` and `Gender` to compare condition frequency across genders.

## Key Findings
- The dataset has **no missing values**, so minimal cleaning was required beyond type conversions.
- Average hospital stay is short (**~2.7 days**), with most patients discharged within 2–4 days.
- Billing amounts vary widely (**$2,300–$14,200**), reflecting different treatment complexities.
- **COPD, Appendicitis, and Stroke** are the most frequently occurring conditions in the dataset; **Hypothyroidism** is the rarest.
- The `Medical_Condition` × `Gender` crosstab highlights conditions with notable gender skew (e.g., Pregnancy and Allergic Reaction skew female; Appendicitis and Gallstones skew male).

## How to Run
1. Open `Task_4.ipynb` in Google Colab (or Jupyter).
2. Update the CSV file path in the data-loading cell to match your local/Drive location.
3. Run all cells sequentially — later cells depend on the cleaned `df` and the `Hospital_Stays` column created earlier.

## File Structure
```
.
├── Task_4.ipynb          # This analysis notebook
├── healthcare_dataset.csv # Source data (not included; place in the referenced path)
└── README.md              # This file
```
