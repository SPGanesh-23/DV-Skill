# Week 8 – Student Performance: Exploratory Data Analysis (EDA)

A Jupyter/Google Colab notebook that performs an initial exploratory analysis of a preprocessed **student performance** dataset. It inspects the structure of the data, separates the score and demographic features, checks the categories in key columns, and visualizes how **parental level of education** relates to students' average exam scores.

---

## Table of Contents

1. [Objective](#objective)
2. [Dataset](#dataset)
3. [Tech Stack](#tech-stack)
4. [Project Structure](#project-structure)
5. [Setup and Usage](#setup-and-usage)
6. [Notebook Walkthrough](#notebook-walkthrough)
7. [Output](#output)
8. [Possible Extensions](#possible-extensions)

---

## Objective

- Load and understand a preprocessed student performance dataset.
- Inspect its size, columns, data types, and summary statistics.
- Group the columns into **score** and **demographic** features.
- Explore the categories in selected demographic columns.
- Compare the average exam score across different levels of parental education.

---

## Dataset

**File:** `studentperformance_preprocessed.csv`

The notebook reads the file from Google Drive (`/content/drive/MyDrive/Colab Notebooks/`). The dataset is not included in this repository, so place your own copy at the path above or update the path in the notebook.

The notebook works with the following columns:

| Group | Column | Description |
|---|---|---|
| Score | `math score` | Exam score in math |
| Score | `reading score` | Exam score in reading |
| Score | `writing score` | Exam score in writing |
| Demographic | `gender` | Student's gender |
| Demographic | `race/ethnicity` | Group the student belongs to |
| Demographic | `parental level of education` | Highest education level of the student's parents |
| Demographic | `lunch` | Lunch type the student receives |
| Demographic | `test preparation course` | Whether the student completed a test prep course |

---

## Tech Stack

- **Python 3**
- [pandas](https://pandas.pydata.org/) – loading and manipulating tabular data
- [NumPy](https://numpy.org/) – numerical operations
- [Matplotlib](https://matplotlib.org/) – plotting
- [seaborn](https://seaborn.pydata.org/) – statistical visualization (imported for further analysis)
- Google Colab (notebook environment, with Google Drive for file access)

---

## Project Structure

```
.
├── Week_8.ipynb                           # Main analysis notebook
├── studentperformance_preprocessed.csv    # Input dataset (not included, add your own)
└── README.md                              # Project documentation
```

---

## Setup and Usage

### Option 1: Google Colab (recommended)

1. Upload `Week_8.ipynb` to Google Colab.
2. Upload `studentperformance_preprocessed.csv` to `MyDrive/Colab Notebooks/` in your Google Drive.
3. Mount Google Drive in the first cell if it is not already mounted:
   ```python
   from google.colab import drive
   drive.mount('/content/drive')
   ```
4. Run all cells (**Runtime → Run all**).

### Option 2: Run locally

1. Install the dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```
2. Change the file path in the data-loading cell to your local path:
   ```python
   df = pd.read_csv("studentperformance_preprocessed.csv")
   ```
3. Launch Jupyter and open the notebook:
   ```bash
   jupyter notebook Week_8.ipynb
   ```

---

## Notebook Walkthrough

### 1. Import libraries
Imports `pandas`, `numpy`, `matplotlib.pyplot`, and `seaborn`.

### 2. Load the data
Reads the CSV file into a DataFrame called `df`.

### 3. Inspect the data
| Command | Purpose |
|---|---|
| `df.head()` / `df.tail()` | Preview the first and last rows |
| `df.info()` | Column names, data types, and non-null counts |
| `df.describe()` | Summary statistics for numeric columns (count, mean, std, min, quartiles, max) |
| `df.columns` | List of column names |
| `df.shape` | Number of rows and columns |

### 4. Split columns into feature groups
- `score_columns` = `math score`, `reading score`, `writing score`
- `demographic_columns` = `gender`, `race/ethnicity`, `parental level of education`, `lunch`, `test preparation course`

Keeping these in lists makes it easy to apply the same operation to a whole group of columns.

### 5. Explore categories
`.unique()` is called on `gender`, `parental level of education`, `lunch`, and `test preparation course` to see which distinct values each column contains. This helps confirm the data is clean and shows how many groups a later comparison will have.

### 6. Average score by parental education
The notebook computes the average score for each parental education level:

1. **Group** the rows by `parental level of education`.
2. Take the **mean** of each score column within every group.
3. **Sum** the three subject means and divide by 3 to get one overall average per group.
4. Plot the result as a **bar chart** titled *Average Exam Scores by Parental Level of Education*, with rotated x-axis labels so the category names stay readable.

This answers a simple question: *do students whose parents have higher education levels tend to score higher?*

---

## Output

- Tabular previews and summary statistics for the dataset.
- A bar chart showing the overall average exam score for each parental education level.
<img width="1112" height="710" alt="image" src="https://github.com/user-attachments/assets/505434b2-60a6-422b-9253-ebb7d030b213" />

---

## Possible Extensions

- Compare average scores by `gender`, `lunch`, and `test preparation course` using the same grouping approach.
- Use seaborn for box plots or violin plots to show score distributions rather than only averages.
- Add a correlation heatmap between the three subject scores.
- Sort the bars by value so the trend across education levels is easier to see.
- Check for missing values and outliers explicitly (`df.isnull().sum()`).
