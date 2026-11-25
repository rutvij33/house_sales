# Exploratory Data Analysis of House Prices

An exploratory data analysis (EDA) of the **Ames Housing dataset** to understand which factors drive house sale prices. This project uses statistics and visualisation only. **No machine learning model is built.**

**Author:** [Your Name]
**Tools:** Python, Pandas, NumPy, Matplotlib, Seaborn, SciPy, Jupyter Notebook

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Guiding Questions](#guiding-questions)
3. [Dataset](#dataset)
4. [Repository Structure](#repository-structure)
5. [How to Run](#how-to-run)
6. [Methodology](#methodology)
7. [Key Findings](#key-findings)
8. [Data Cleaning Decisions](#data-cleaning-decisions)
9. [Limitations](#limitations)
10. [Possible Future Work](#possible-future-work)

---

## Project Overview

The goal is to practise the full EDA workflow on a real dataset: loading and inspecting data, classifying variables, summarising them, visualising distributions and relationships, detecting outliers, cleaning the data, and communicating the results in a dashboard.

The notebook follows a "questions first" approach: six guiding questions were written before touching the data, and every section helps answer one or more of them.

## Guiding Questions

1. How is `SalePrice` distributed? Is it skewed?
2. Which numerical features correlate most strongly with `SalePrice`?
3. Does overall quality (`OverallQual`) change price in a clear, ordered way?
4. Do some neighbourhoods consistently cost more than others?
5. Are there outliers, and are they errors or real but unusual sales?
6. Which columns have missing values, and which of those are not truly missing?

## Dataset

- **Name:** House Prices: Advanced Regression Techniques (Kaggle)
- **File:** `train.csv`
- **Size:** 1,460 rows and 81 columns
- **Target variable:** `SalePrice`
- **Download:** <https://www.kaggle.com/c/house-prices-advanced-regression-techniques/data>

> The dataset is not included in this repository. Download `train.csv` from Kaggle and place it in the same folder as the notebook.

## Repository Structure

```
.
├── House_Price_EDA.ipynb    # Main analysis notebook
├── train.csv                # Dataset (download from Kaggle)
├── eda_dashboard.png        # Final dashboard (created when the notebook runs)
└── README.md
```

## How to Run

**1. Install the requirements**

```bash
pip install pandas numpy matplotlib seaborn scipy jupyter
```

**2. Add the dataset**

Place `train.csv` in the same folder as `House_Price_EDA.ipynb`.

**3. Open and run the notebook**

```bash
jupyter notebook House_Price_EDA.ipynb
```

Choose **Kernel > Restart & Run All**. The notebook creates `eda_dashboard.png` in the same folder.

The notebook was tested with Python 3.12 and pandas 3.0.6.

## Methodology

| Step | Topic | What is done |
|---|---|---|
| 1 | Loading and first look | Shape, data types, missing-value counts and percentages |
| 2 | Classifying variables | Numerical (continuous / discrete), nominal, and ordinal; variable summary table |
| 3 | Descriptive statistics | Mean, median, mode, standard deviation, variance, range, IQR, skewness, kurtosis |
| 4 | Distribution analysis | Histogram with KDE, boxplots, log transform, Q-Q plots, countplots |
| 5 | Correlation and covariance | Covariance, Pearson and Spearman correlation, top 10 features |
| 6 | Scatter and pair plots | Size, basement and year-built relationships; pair plot by `CentralAir` |
| 7 | Correlation heatmap | Top 10 features, upper triangle masked |
| 8 | Outlier detection | IQR vs z-score comparison, then a documented keep/remove decision |
| 9 | Data quality | Duplicates, inconsistent labels, missing values, impossible values |
| 10 | Dashboard | Four-panel summary figure saved as `eda_dashboard.png` |
| 11 | Verification | Recomputes the six expected insights with numbers |
| 12 | Conclusion | Summary of findings and limitations |

Steps 1 to 8 explore the **raw** data (`df`). Step 9 builds a separate cleaned copy (`df_clean`), and the dashboard uses `df_clean`. The raw data is never overwritten.

## Key Findings

1. **`SalePrice` is right skewed.** The mean (about 180,921) is above the median (163,000). Skewness is 1.88 and falls to 0.12 after a log transform, which is close to normal.
2. **Quality and size are the strongest correlates.** `OverallQual` (0.79) and `GrLivArea` (0.71) lead, followed by `GarageCars` (0.64), `GarageArea` (0.62) and `TotalBsmtSF` (0.61).
3. **Price rises with quality in a clear order.** The median price increases at every quality grade, from 50,150 at grade 1 to 432,390 at grade 10. The spread is much larger at the top grades.
4. **Neighbourhood matters.** `NridgHt`, `NoRidge` and `StoneBr` have the highest median prices. The most expensive neighbourhood's median (315,000) is about 3.6 times the cheapest (88,000).
5. **Outliers need judgement.** For `SalePrice`, the IQR method flags 61 points and the z-score method flags 22, because extreme values inflate the mean and standard deviation. Two very large, low-priced houses (`Id` 524 and 1299) are Partial sales and were removed as unusual sales. Removing them raises the `GrLivArea`-`SalePrice` correlation from 0.709 to 0.735.
6. **Most "missing" values are not missing.** 19 columns contain missing values, and 15 of them mean the feature is absent (no pool, no garage, no basement and so on). Only `LotFrontage` and `Electrical` are genuinely missing.

## Data Cleaning Decisions

Every cleaning decision is listed in a decision table inside the notebook (Step 9). A summary:

| Issue | Decision |
|---|---|
| Two very large, low-priced Partial sales | Removed as unusual sales |
| NA in pool, alley, fence, fireplace, garage and basement columns | Meaning "feature absent", so filled with `"None"` |
| `MasVnrType` has 872 "missing" values | Pandas reads the real label `"None"` as missing. Filled with `"None"`; `MasVnrArea` filled with 0 |
| `Electrical` (1 missing) | Filled with the most common value |
| `GarageYrBlt` (houses without a garage) | Filled with `YearBuilt` (an assumption) |
| `LotFrontage` (259 missing) | Filled with the median of the same neighbourhood |
| Inconsistent spellings in `Exterior2nd` | Standardised to match `Exterior1st` (`CmentBd`, `Wd Shng`, `Brk Cmn`) |
| Duplicate rows | None found (checked ignoring `Id`) |
| Impossible values | None found (checked build year vs sale year, and negative areas) |

## Limitations

- The data covers **one city (Ames, Iowa) in a short time window**, so the conclusions may not apply elsewhere.
- **Correlation is not causation.** Size, quality and neighbourhood are linked to each other, so the effect of any single factor cannot be isolated.
- Pearson correlation only captures **linear** relationships.
- Some cleaning choices are **assumptions** (such as filling `GarageYrBlt` with `YearBuilt`, and removing the two Partial sales) and different choices could change the numbers.
- Averages for **neighbourhoods with few houses** are less reliable.
- This is exploratory work only. No predictions are made.

## Possible Future Work

- Build a regression model on the log of `SalePrice`, after checking multicollinearity (for example `GarageArea` and `GarageCars`, correlation 0.88).
- Engineer features such as total square footage or house age.
- Compare results with and without the removed outliers.
- Add interactive charts (for example with Plotly).

## Acknowledgements

Dataset: Ames Housing data compiled by Dean De Cock, distributed through the Kaggle competition *House Prices: Advanced Regression Techniques*.
