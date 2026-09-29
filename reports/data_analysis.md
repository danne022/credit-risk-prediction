# Credit Risk Analysis – Data Exploration

## 1. Project Overview

This project analyzes the Home Credit Default Risk dataset to investigate
factors associated with loan default and eventually develop a machine
learning model for credit risk prediction.

The analysis uses Snowflake and SQL for data exploration and data
preparation, followed by Python for statistical analysis and machine learning.

> **SQL source:** The queries and data exploration for this report are
> available in [`02_data_exploration.sql`](../sql/02_data_exploration.sql).
> The results and interpretations presented below are based on these queries.

---

## 2. Dataset Overview

The training dataset contains:

- **307,511 loan applications**
- **122 variables**

The data has been loaded into Snowflake as:

`HOME_CREDIT.RAW.APPLICATION_TRAIN`

The dataset contains information about applicants, including income,
credit amount, loan annuity, demographic characteristics, employment,
and other financial variables.

---

## 3. Target Variable

The target variable is `TARGET`, which represents the outcome of the
loan application.

- `TARGET = 0`: no default
- `TARGET = 1`: default

The distribution of the target variable is:

| TARGET | Applications | Share |
|-------:|-------------:|------:|
| 0 | 282,686 | 91.93% |
| 1 | 24,825 | 8.07% |
| **Total** | **307,511** | **100%** |

### Observation

The target variable is highly imbalanced. Only approximately 8% of
applications belong to the default class, while approximately 92% belong
to the non-default class.

This is important for the later machine learning stage because model
performance should not be evaluated using accuracy alone. Metrics such
as precision, recall, ROC-AUC and PR-AUC will also be considered.

---

## 4. Initial Financial Characteristics

The first financial variables investigated were:

- `AMT_INCOME_TOTAL` – applicant's total income
- `AMT_CREDIT` – amount of credit
- `AMT_ANNUITY` – loan annuity/payment amount

The overall averages are:

| Variable | Mean |
|---|---:|
| Income | 168,797.92 |
| Credit | 599,026.00 |
| Annuity | 27,108.57 |

These values provide an initial overview of the financial
characteristics of applicants in the dataset.

However, overall averages do not tell us whether these characteristics
are associated with default. The next stage of the analysis will
therefore compare financial characteristics between the two `TARGET`
groups.

---

## 5. Financial Characteristics by Default Status

### Question

How do average income, credit amount and annuity differ between
default and non-default applications?

### Results

| Default Status | Applications | Average Income | Average Credit | Average Annuity |
|---|---:|---:|---:|---:|
| No default (`0`) | 282,686 | 169,077.72 | 602,648.28 | 27,163.62 |
| Default (`1`) | 24,825 | 165,611.76 | 557,778.53 | 26,481.74 |

### Interpretation

The two groups show differences across all three financial variables.
Applicants with `TARGET = 0` have higher average income, credit amount
and annuity than applicants with `TARGET = 1`.

The largest absolute difference is observed for average credit amount,
which differs by approximately 44,870.

These differences describe the characteristics of the two groups but do
not establish that any of the variables independently cause or predict
default. Further analysis is required to examine the distributions and
relationships between these variables and default status.
