# Mobile Money Transaction Fraud Analysis (PaySim Dataset)

Statistical analysis of 10.48M real-world-structured mobile money transactions to identify patterns that distinguish fraudulent from legitimate activity, using descriptive statistics, outlier detection, hypothesis testing, and logistic regression.

## 📊 Dataset
- **Source:** [PaySim - Synthetic Financial Datasets For Fraud Detection](https://www.kaggle.com/datasets/ealaxi/paysim1) (Kaggle)
- **Size:** 10,48,576 transactions
- **Columns:** `type`, `amount`, `oldbalanceOrg`, `newbalanceOrig`, `oldbalanceDest`, `newbalanceDest`, `isFraud`

## 🎯 Business Questions & Methods

| Question | Method |
|---|---|
| What does typical transaction behavior look like? | Descriptive Statistics |
| Which transactions are abnormal? | Z-score / IQR Outlier Detection |
| Do fraud transactions really differ in size? | T-test |
| Does amount vary by transaction type? | ANOVA |
| What predicts fraud? | Correlation + Logistic Regression |

## 📈 Key Results

**Descriptive Statistics (amount):**
- Mean: 179,861.90 | Median: 74,871.94 | Std Dev: 603,858.23
- Skewness: 30.99 | Kurtosis: 1797.96 → extremely right-skewed with heavy-tailed outliers

**Outlier Detection:**
- IQR method flagged 338,078 transactions (5.31%) as outliers
- Fraud rate among Z-score outliers: **1.14%** vs overall fraud rate of **0.13%** → outliers are ~8.8x more likely to be fraud

**T-test — Fraud vs Non-Fraud Amount:**
- Fraud avg: 1,467,967.30 | Non-fraud avg: 178,197.04
- t = 48.615, **p < 0.001** → statistically significant; fraud transactions are ~8.2x larger on average
- 95% CI does not overlap between groups (Fraud: 1.42M–1.52M | Non-fraud: 177.7K–178.7K)

**ANOVA — Amount by Transaction Type:**
- F = 278,715.54, **p < 0.001**
- TRANSFER (910,647) and CASH_OUT (176,274) carry by far the highest average amounts — the two types most exploited for fraud

**Correlation with Fraud:**
- `amount`: 0.077 (strongest, though weak in raw correlation terms)
- Other balance fields show negligible correlation

**Logistic Regression (Pseudo R² = 0.518):**
- All three predictors (`amount`, `oldbalanceOrg`, `balance_diff_org`) statistically significant (p<0.001)
- Note: model shows signs of quasi-separation (~40% of cases perfectly predicted), suggesting a near-deterministic threshold pattern in how fraud transactions are structured — a useful lead for rule-based flagging in addition to the model itself

## 💡 Business Recommendation
Prioritize real-time monitoring on **TRANSFER** and **CASH_OUT** transaction types, using a combined rule of (a) statistical outlier flagging on `amount` and (b) balance-discrepancy checks — the two signals shown here to carry the strongest fraud relationship.

## 🛠️ Tools Used
Python · Pandas · SciPy · Statsmodels · Matplotlib · Seaborn

## 📁 Repository Structure
```
├── 01_descriptive_stats.ipynb
├── 02_outlier_detection.ipynb
├── 03_hypothesis_testing.ipynb
├── 04_anova.ipynb
├── 05_correlation_regression.ipynb
├── data/ (link to Kaggle dataset)
└── visuals/
```

## ⚠️ Limitations
- Regression shows quasi-separation, meaning some coefficients may be less stable — a regularized logistic regression (e.g., L2 penalty) or tree-based model (Random Forest) would be a natural next step for production use
- Analysis is exploratory/statistical, not a deployed fraud-detection system
