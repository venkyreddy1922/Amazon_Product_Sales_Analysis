# 🛒 Amazon Electronics Sales Data Analysis

An end-to-end Exploratory Data Analysis (EDA) project on Amazon electronics product listings, covering data cleaning, feature engineering, and multi-level visual analysis using Python.

---

## 📌 Project Overview

This project analyzes **42,675 Amazon electronics product records** to extract actionable business insights. The pipeline covers the complete data analytics workflow — from raw data ingestion and cleaning to feature engineering and visual storytelling.

| Attribute        | Detail                                      |
|------------------|---------------------------------------------|
| Dataset          | Amazon Electronics Products — 42,675 rows × 17 columns |
| Tools Used       | Python, Pandas, Matplotlib, Seaborn, Jupyter Notebook |
| Domain           | E-Commerce / Retail Analytics               |
| Analysis Levels  | Univariate · Bivariate · Multivariate       |
| Output           | Cleaned dataset + engineered features + visual insights |

---

## 📁 Project Structure

```
amazon-product-sales-analysis/
│
├── amazon_products_sales_data_cleaned.csv   ← Cleaned dataset
├── amazon_sales_analysis.ipynb              ← Main Jupyter Notebook
├── README.md                                ← Project documentation

```

---

## 🛠️ Tools & Technologies

- **Python 3** — Core scripting and analysis
- **Pandas** — Data manipulation, groupby, pivot tables, crosstab
- **Matplotlib** — Bar charts, histograms, correlation plots
- **Seaborn** — Heatmaps, statistical visualizations
- **Jupyter Notebook** — Interactive notebook-based analysis

---

## 🧹 Data Cleaning & Preprocessing

### Step 1 — Missing Value Analysis
- Used `df.isnull().sum() * 100 / len(df)` to compute null percentage per column
- `sustainability_tags` had **92% nulls** — dropped entirely
- Other columns imputed using median (numerical) or mode (categorical)

| Column | Null % | Action |
|--------|--------|--------|
| sustainability_tags | 92.0% | Dropped |
| purchased_last_month | 24.6% | Median imputation |
| delivery_date | 28.1% | Parsed to datetime, NaT retained |
| original_price | 4.8% | Median imputation |
| discount_percentage | 4.8% | Median imputation |

### Step 2 — Data Type Corrections
- Cast `total_reviews` and `purchased_last_month` from `float` → `int`
- Parsed `delivery_date` and `data_collected_at` to `datetime` using `pd.to_datetime(..., errors='coerce')`
- Used `select_dtypes()` to split and process numerical vs categorical columns independently before `pd.concat()`

---

## ⚙️ Feature Engineering

Three new business-relevant features were created:

| Feature | Formula | Business Purpose |
|---------|---------|-----------------|
| `discounted_amount` | `original_price - discounted_price` | Measures absolute savings per product |
| `estimated_revenue` | `discounted_price × purchased_last_month` | Proxy for monthly seller revenue |
| `percentage_band` | `pd.cut()` with bins `[0, 10, 40, 70, 99]` | Segments products into 4 discount tiers |

> **Why `pd.cut()` over `pd.qcut()`?**  
> `pd.cut()` with manual bins reflects real retail discount psychology: sub-10% = negligible, 10–40% = promotional, 40–70% = clearance, 70%+ = distress pricing.

---

## 📊 Exploratory Data Analysis (EDA)

### Univariate Analysis
- **Discount band distribution:** 71% of products had no discount data. Among discounted — 18.7% Medium, 2.7% High, 0.33% Very High.
- **Total reviews distribution:** Highly right-skewed — power-law distribution typical of e-commerce.

### Bivariate Analysis
- **Category vs Purchases (groupby):** Power & Batteries dominated with 26M+ monthly purchases — 7x more than any other category.
- **Sponsorship vs Category (crosstab):** Used `pd.crosstab()` to map sponsorship penetration across all 15 categories.
- **Correlation Matrix:**

| Variable Pair | Correlation | Interpretation |
|---------------|------------|----------------|
| `product_rating` vs `purchased_last_month` | 0.152 | Weak — ratings alone don't drive purchases |
| `discount_percentage` vs `estimated_revenue` | -0.001 | Near zero — discounts don't guarantee revenue |
| `purchased_last_month` vs `estimated_revenue` | 0.687 | Strong — volume is the primary revenue driver |

### Multivariate Analysis
- Used `pd.pivot_table()` combining `product_category`, `is_sponsored`, `estimated_revenue`, and `total_reviews` simultaneously to reveal how sponsorship effect on revenue varies by category.

---

## 🔍 Key Findings & Insights

1. **Sponsored listings generate 4x higher revenue** — Median: Sponsored = ₹49,800 vs Organic = ₹12,494. In Wearables, the gap is 56x.

2. **Optimal discount band is 10–40%, not the deepest** — Medium discounts drive 2,783 avg monthly units. Very High (70%+) drives only 68 units.

3. **Rating 4.5+ creates a non-linear purchase spike** — 4.5–5 star products average 2,487 purchases/month vs only 431 for 4.0–4.5 rated — a 5.8x jump.

4. **Top products had 0% discount** — Amazon Basics batteries and TI-84 calculators dominate without any discounting — brand trust wins over price incentives.

5. **Discount % and revenue have near-zero correlation** — Pearson r = -0.001. Revenue is driven by purchase volume (r = 0.687), not discounting.

6. **Power & Batteries dominates with 26M+ monthly purchases** — driven by repeat consumable purchases, not one-time tech buys.

---

## ▶️ How to Run This Project

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/amazon-sales-analysis.git
   cd amazon-sales-analysis
   ```

2. **Install required libraries**
   ```bash
   pip install pandas matplotlib seaborn jupyter
   ```

3. **Launch Jupyter Notebook**
   ```bash
   jupyter notebook
   ```

4. **Open** `amazon_sales_analysis.ipynb` and run all cells.

---

## 📦 Requirements

```
pandas
matplotlib
seaborn
jupyter
```

---

## 👤 Author

Vanga Venkateshwar Reddy 
Aspiring Data Analyst | Python · SQL · Power BI  




