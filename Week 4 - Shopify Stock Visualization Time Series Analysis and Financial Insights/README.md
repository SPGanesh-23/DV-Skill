# Shopify Stock Analysis

A Python data analysis script that cleans, explores, and visualizes historical daily stock price data for Shopify (SHOP), calculating return and volume metrics and flagging unusually high-volume trading days.

## What It Does

The script takes raw daily OHLCV (Open, High, Low, Close, Volume) stock data and runs it through a full exploratory data analysis (EDA) pipeline:

1. **Loads and inspects** the raw CSV data.
2. **Cleans the data** — parses dates, drops missing values, and removes duplicate rows.
3. **Engineers new features**:
   - `Daily_Delta` — the absolute price change within a day (`close - open`)
   - `Daily_Return` — the percentage price change within a day (`(close - open) / open * 100`)
4. **Analyzes trading volume** — computes average, maximum, and minimum volume, and flags "anomalous" days where volume exceeds 2x the average.
5. **Summarizes return statistics** — mean, variance, and standard deviation of daily returns.
6. **Visualizes the data** with three charts:
   - Closing price trend over time
   - Trading volume trend over time
   - Distribution of daily returns (histogram)

## Dataset

The script expects a file named `shopify_stock.csv` in the same directory, with the following columns:

| Column      | Type    | Description                          |
|-------------|---------|---------------------------------------|
| `date`      | string  | Trading date                          |
| `open`      | float   | Opening price                         |
| `high`      | float   | Highest price of the day              |
| `low`       | float   | Lowest price of the day               |
| `close`     | float   | Closing price                         |
| `adj_close` | float   | Adjusted closing price                |
| `volume`    | int     | Number of shares traded               |

This dataset is not included in this repository — you'll need to supply your own `shopify_stock.csv` (e.g. exported from Yahoo Finance or a similar source) in the working directory before running the script.

## Requirements

- Python 3.x
- [pandas](https://pandas.pydata.org/)
- [numpy](https://numpy.org/)
- [matplotlib](https://matplotlib.org/)
- [seaborn](https://seaborn.pydata.org/)

Install dependencies with:

```bash
pip install pandas numpy matplotlib seaborn
```

## Usage

1. Place `shopify_stock.csv` in the same folder as the script.
2. Run the script:

```bash
python Task_4.py
```

Or open `Task_4.ipynb` in Jupyter/Google Colab and run all cells.

3. The script will print summary statistics and anomalous trading days to the console, and display three plots (closing price trend, volume trend, and return distribution).

## Output

- **Console output**: dataset shape, missing-value counts, calculated columns preview, volume statistics, list of anomalous high-volume trading days, and return distribution statistics (mean, variance, standard deviation).
- **Charts**: three matplotlib/seaborn plots shown inline (closing price, volume, and return distribution).

## Known Issues

- The script uses `seaborn` (via `sns.histplot`) for the return distribution chart but does not import it. Add `import seaborn as sns` alongside the other imports at the top of the script to avoid a `NameError`.
- `plt.savefig("chart.png")` is called *after* `plt.show()` for the volume chart, which saves a blank figure since `plt.show()` clears the current figure in most backends. Call `savefig()` before `show()` if you want the volume chart saved to disk.

## Project Structure

```
.
├── Task_4.py          # Main analysis script
├── Task_4.ipynb        # Original Colab/Jupyter notebook
├── shopify_stock.csv   # Input data (not included — supply your own)
└── README.md            # This file
```
