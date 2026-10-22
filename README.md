# Customer Churn Analysis (Python + Power BI)

An end-to-end analysis of customer churn: cleaning and exploring the data in **Python**, then presenting the results in a four-page **Power BI** report. The goal is to find out **who churns, why, and how much revenue is at risk**.

> **Note on the data:** the dataset appears to be **synthetic** (88.3% churn and several groups with exactly 100% churn). Findings are treated as directional practice insights, not real-world customer behaviour.

---

## Table of Contents
1. [Business Questions](#business-questions)
2. [Dataset](#dataset)
3. [Tools](#tools)
4. [Project Structure](#project-structure)
5. [Workflow](#workflow)
6. [Key Findings](#key-findings)
7. [Power BI Report](#power-bi-report)
8. [How to Run](#how-to-run)
9. [Limitations](#limitations)
10. [Skills Demonstrated](#skills-demonstrated)
11. [Next Steps](#next-steps)

---

## Business Questions
1. What is the overall churn rate, and is the data balanced?
2. Which customer groups (contract, internet service, tech support, gender) churn the most?
3. Do new customers churn faster than long-term ones?
4. Does the monthly price relate to churn?
5. Which segments are the highest risk, and how much revenue is at risk?

## Dataset
- **Source file:** `customer_churn_data.csv` (raw), `customer_churn.csv` (cleaned)
- **Size:** 1,000 customers, 10 raw columns (11 after cleaning)

| Column | Description |
|---|---|
| `CustomerID` | Unique customer identifier |
| `Age` | Customer age |
| `Gender` | Male / Female |
| `Tenure` | Months as a customer |
| `MonthlyCharges` | Monthly bill |
| `ContractType` | Month-to-Month, One-Year, Two-Year |
| `InternetService` | DSL, Fiber Optic, No Internet |
| `TotalCharges` | Total amount billed |
| `TechSupport` | Yes / No |
| `Churn` | Yes / No (target) |
| `Churn_Flag` | 1 / 0 version of `Churn` (added during cleaning) |

## Tools
- **Python:** pandas, NumPy, seaborn, matplotlib, Jupyter Notebook
- **Power BI Desktop:** Power Query, DAX, custom theme, drill-through

## Project Structure
```
customer-churn-analysis/
├── Dataset/
│   └── customer_churn_data.csv      # raw data
├── customer_churn.csv               # cleaned data (output of cleaning notebook)
├── data_cleaning.ipynb              # Python: cleaning
├── EDA.ipynb                        # Python: exploratory data analysis
├── churn_powerbi_dax.txt            # DAX: calculated columns and measures
├── customer_churn_dashboard.pbix    # Power BI report
├── images/                          # dashboard screenshots
└── README.md
```

## Workflow
**1. Data cleaning (`data_cleaning.ipynb`)**
- Inspected shape, data types, missing values, and duplicates.
- Found 297 "missing" `InternetService` values. They were the text **"None"**, which pandas reads as blank. The other add-on column checked (`TechSupport`) was "No" for all 297 rows, so they were filled with a real category: **"No Internet"**.
- Confirmed no duplicate rows or duplicate customer IDs.
- Created a numeric `Churn_Flag` (Yes = 1, No = 0) for calculations.
- Exported the cleaned file for analysis and Power BI.

**2. Exploratory data analysis (`EDA.ipynb`)**
- Baseline churn rate and class balance.
- Distributions and outlier checks for numerical columns (histograms and boxplots).
- Churn **rates** by category and numerical comparisons by churn status.
- Correlation, segment analysis (contract × internet, tenure × price), and revenue at risk.
- Each chart is followed by a written *Observation → So what* note.

**3. Dashboard (Power BI)**
- Loaded the cleaned data, built calculated columns (`TenureGroup`, `PriceTier`) and DAX measures, then designed a four-page report.

## Key Findings
1. **Overall churn is 88.3%** (883 of 1,000 customers), far above what a real business would see.
2. **Month-to-Month, no tech support, and no internet each show 100% churn.** One-Year (75.4%) and Two-Year (77.0%) contracts are almost the same.
3. **The first year is the danger zone:** all 468 customers with under 12 months of tenure churned.
4. **Gender and age do not drive churn** (female 87.4% vs male 89.4%; average age 44.8 for churned vs 43.5 for retained).
5. **Higher price goes with higher churn:** 83.6% (low tier), 86.7% (mid), 94.6% (high).
6. **Churned customers hold about 90% of monthly revenue** (about 67,073 of 74,391).
7. **Outside the always-churn groups** (not Month-to-Month, has tech support, has internet, 12+ months tenure), there are 143 customers and the churn rate is **18.2%**. All of that churn is in the high-price tier.
8. **Data quality notes:** `TotalCharges` equals `Tenure × MonthlyCharges` in every row (derived column), and one customer is 12 years old (kept and documented).

## Power BI Report

| Page | Title | Purpose |
|---|---|---|
| 1 | Executive Summary | KPIs (customers, churn rate, revenue at risk) and churn by tenure, contract, and price |
| 2 | Churn Drivers | Decomposition tree, churn by tech support / internet / gender, tenure vs charges scatter |
| 3 | Risk Segments | Heat maps (contract × internet, tenure × price), revenue at risk treemap, key takeaways |
| 4 | Customer Details | Drill-through table of individual customers |

**Main DAX measures:** `Total Customers`, `Churned Customers`, `Retained Customers`, `Churn Rate %`, `Retention Rate %`, `Monthly Revenue`, `Revenue at Risk`, `Revenue at Risk %`, `Avg Tenure (Months)`, `Avg Monthly Charges`, `Churn Rate vs Overall`, `Customers Outside Always-Churn Groups`, `Churn Rate Outside Always-Churn Groups`.
**Calculated columns:** `TenureGroup`, `PriceTier` (plus sort-order columns).
Full formulas are in `churn_powerbi_dax.txt`.

*Screenshots:* add images to `images/` and link them here, for example `![Page 1](images/page1_executive_summary.png)`.

## How to Run
1. Clone or download this repository.
2. Install the requirements:
   ```bash
   pip install pandas numpy seaborn matplotlib jupyter
   ```
3. Open `data_cleaning.ipynb` and run **Restart & Run All** (it reads `Dataset/customer_churn_data.csv` and writes `customer_churn.csv`).
4. Open `EDA.ipynb` and run **Restart & Run All**.
5. Open `customer_churn_dashboard.pbix` in Power BI Desktop. If the data source path breaks, use **Transform data → Data source settings** to point to your local `customer_churn.csv`.

## Limitations
- **Synthetic-looking data:** the 100% churn groups and 88% overall churn are unrealistic, so results should not be presented as real-world behaviour.
- **Overlapping factors:** all customers without internet also have no tech support, so their effects cannot be separated.
- **Small comparison group:** only 117 customers stayed, so differences between small subgroups are uncertain.
- **Correlation is not causation:** a link between price and churn does not prove price causes churn.
- **No predictive model yet:** this project describes the data; it does not forecast churn.

## Skills Demonstrated
- Data cleaning and validation in pandas (types, missing values, duplicates, outliers)
- Exploratory data analysis with seaborn, including group-based rates and imbalance handling
- Critical thinking about data quality (spotting synthetic patterns and derived columns)
- Power BI modelling, DAX measures and calculated columns, and dashboard design
- Communicating findings with clear takeaways and honest limitations

## Next Steps
- Build a baseline classification model (logistic regression, then a tree-based model) and evaluate it with precision, recall, and ROC-AUC instead of accuracy.
- Repeat the analysis on a real, less imbalanced churn dataset.
- Publish the report to the Power BI Service and add dashboard screenshots.

---

**Author:** Ahmad
