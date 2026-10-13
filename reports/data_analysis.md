# Credit Risk Analysis

## 1. Project Overview

This project explores payment problems across applicant groups
and investigates what financial averages hide.

The tools have different responsibilities:

- **Snowflake and SQL:** explore the source data, check its quality
- **Python:** investigate financial distributions, extreme values
  and differences between TARGET groups.

### Supporting Work

- [SQL exploration](../sql/02_data_exploration.sql)
- [Python analysis](../notebooks/01_data_understanding.ipynb)

Machine learning is a possible future extension.

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

TARGET indicates whether the applicant had recorded payment difficulties
according to the dataset definition.

- TARGET = 0: no recorded payment problems.
- TARGET = 1: recorded payment problems.

The distribution of the target variable is:

| TARGET | Applications | Share |
|-------:|-------------:|------:|
| 0 | 282,686 | 91.93% |
| 1 | 24,825 | 8.07% |
| **Total** | **307,511** | **100%** |

### Observation

The target variable is highly imbalanced. Only approximately 8% of
applications belong to the default class, while approximately 92% belong to the non-default class.

If prediction is added later, the class imbalance will need to be considered during evaluation.

---

## 4. Financial Characteristics by Payment Problem Status

### Question

How do average income, credit amount and annuity differ between
default and non-default applications?

The first financial variables investigated were:

- `AMT_INCOME_TOTAL` – applicant's total income
- `AMT_CREDIT` – amount of credit
- `AMT_ANNUITY` – loan annuity/payment amount

### Results

The means and medians by TARGET group are shown below.
Values are rounded to the nearest whole number.
Means were initially checked in SQL; the Python analysis adds medians.

| Financial amount | Mean TARGET 0 | Mean TARGET 1 | Median TARGET 0 | Median TARGET 1 |
|---|---:|---:|---:|---:|
| Income | 169,078 | 165,612 | 148,500| 135,000 |
| Credit | 602,648 | 557,779 | 517,788 | 497,529 |
| Annuity | 27,164 | 26,482 | 24,876| 25,263 |


### Interpretation

Income and credit have higher means and medians for TARGET 0.

Annuity shows a different pattern: its mean is higher for TARGET 0,
but its median is slightly higher for TARGET 1. The Python distribution
analysis investigates this difference.

These comparisons describe associations. They do not establish
causation or demonstrate predictive performance.

## 5. Financial Distributions in Python

The Python notebook examines financial distributions to complement
the SQL summaries.

### Credit Amount

The mean credit amount is approximately 599,026, compared with
a median of 513,531. The higher mean suggests that larger credit
amounts pull the average upward.

### Extreme Income Value

The highest recorded income is 117 million, compared with approximately
18 million for the second highest observation.

Excluding the highest observation in a sensitivity check reduces
the standard deviation by approximately 120,000. The mean changes
only slightly, while the median remains essentially unchanged.

The original dataset remains unchanged because the observation
has not been confirmed as an error.

### Annuity Distribution

Although TARGET 1 has a slightly higher median annuity, TARGET 0
has higher values at the 75th and 95th percentiles.

| Annuity percentile | TARGET 0 | TARGET 1 |
|---|---:|---:|
| 75th | 34,749 | 32,976 |
| 95th | 53,456 | 49,866 |

Values are rounded to the nearest whole number.

This helps explain why TARGET 0 has a higher mean despite having
a lower median. The middle 50% of values overlap substantially,
so annuity does not clearly separate the groups in this analysis.

### Missing Values

AMT_ANNUITY has 12 missing values. These remain missing and are
excluded from its numerical summaries rather than replaced with zero.

## 7. Findings and Limitations

### Main Findings

- Payment problems are recorded for 24,825 of 307,511 applications,
  giving an overall rate of 8.07%.

- Group rates need to be interpreted alongside group sizes.
  Maternity leave has a 40% rate, but only five applications.

- Financial averages hide important distribution differences.
  Credit has a mean above its median, and an extreme income value
  strongly affects the income standard deviation.

- Annuity comparisons depend on the statistic used:
  TARGET 0 has a higher mean, while TARGET 1 has a slightly
  higher median.

### Limitations

- These findings describe associations, not causes.
- Very small groups provide limited evidence for broader conclusions.
- The highest income observation has not been confirmed as valid
  or erroneous and remains in the dataset.
- Annuity summaries exclude 12 missing values.
- Currency and financial units require verification.
- Predictive performance has not been evaluated.

### Conclusion

SQL provides the data checks and summaries and Python investigates
financial distributions

The analysis highlights the importance of considering group sizes
and distributions alongside averages and percentages.