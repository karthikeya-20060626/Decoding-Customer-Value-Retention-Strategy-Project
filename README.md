# Customer Retention Strategy — D2C Fashion Brand
**Consulting & Analytics Club, IIT Guwahati**

An end-to-end customer intelligence project on a 3,900-customer D2C fashion brand, designed to answer one question: *is the brand building genuine loyalty, or renting attention through discounts?*

The project runs from raw transactional data through Python feature engineering, PostgreSQL segmentation, lift analysis, and a Power BI dashboard — and closes with a phased playbook to recover margin without losing the customers that actually matter.

---

## The answer
The brand is currently dependent on promotional activity, not loyalty. 43% of customers buy exclusively on discounts. Only 15.4% (600 customers) qualify as genuinely loyal. The subscription program — 1,053 members — has a **0.00 loyalty lift**, a structural product failure. Two demographic signals break through the noise: female customers show 1.65x loyalty lift and non-subscribers show 1.37x, the only statistically meaningful predictors in the dataset.

---

## Key findings

| Finding | Number |
|---|---|
| Customers analysed | 3,900 |
| Genuine loyalists (Loyal_A) | 600 (15.4%) |
| Annual margin lost to discounts | $19,882 |
| Discount spend on lowest-value segments | $19,441 |
| Subscription program loyalty lift | 0.00 (1,053 members) |
| Female loyalty lift | 1.65x |
| Non-subscriber loyalty lift | 1.37x |
| Projected net recovery, first 6 months | $9,338 |

---

## Project structure

```
Customer_Retention_Project/
│
├── Deliverable_1_Python_Notebook.ipynb      # Feature engineering (9 metrics)
├── Deliverable_1_engineered_dataset.csv     # Output of notebook — 31 columns
├── Deliverable_2_SQL_Queries.sql            # 8 PostgreSQL segmentation queries
├── Deliverable_3_Power_BI_Template.pbix     # 4-panel dashboard
├── Deliverable_4_Retention_Playbook.docx    # 4-phase Promo Sunset Playbook
└── Deliverable_5_Executive_Summary.pdf      # Board-ready summary
```

---

## How to run

### Step 1 — Python (feature engineering)
Open `Deliverable_1_Python_Notebook.ipynb` in Google Colab or Jupyter.
Upload the raw dataset when prompted, or replace the Colab upload cell with a local `pd.read_csv()` path.

The notebook engineers **9 customer-level metrics** on top of the raw data:
- `is_promo_buyer` — promo dependency flag
- `freq_score` — purchase frequency mapped to annual visits
- `spend_norm`, `freq_norm`, `prev_norm` — min-max normalised dimensions
- `value_score` — composite score (spend + frequency + tenure)
- `value_tier` — Low / Mid / High / Premium
- `satisfied` — satisfaction flag based on review rating
- `Loyal_A` — behavioural loyalty (no discounts + long tenure + satisfied)
- `Loyal_B` — revenue-based loyalty (high spend + high frequency)
- `segment` — 5-group label: Organic Loyalist / High-Value Retainer / At-Risk / Promo Hunter / Dormant
- `age_band` — life-cycle classification (Gourinchas & Parker 2002 framework)
- `discount_cost` — estimated annual margin lost per customer

Output: `engineered_dataset.csv` (31 columns, 3,900 rows)

### Step 2 — PostgreSQL (segmentation queries)
Run `Deliverable_2_SQL_Queries.sql` against a PostgreSQL instance.

The file creates and loads the `customers` table, then runs **8 analytical queries**:

| Query | Business question |
|---|---|
| Q1 | What separates high-value from low-value customers? |
| Q2 | Which segments drive discount costs? |
| Q3 | How does loyalty vary by gender and subscription status? |
| Q4 | Lift analysis — which attributes predict loyalty? |
| Q5 | Subscription program — does it drive loyalty? |
| Q6 | Geographic concentration — where are loyalists? |
| Q7 | Segment-level margin leakage breakdown |
| Q8 | Value tier × promo dependency cross-tab |

> **Note:** update the `COPY ... FROM` path on line ~35 to your local path for `engineered_dataset.csv` before running.

### Step 3 — Power BI dashboard
Open `Deliverable_3_Power_BI_Template.pbix` in Power BI Desktop.

The dashboard has 4 panels:
- **Overview** — segment distribution, loyalty rates, discount cost summary
- **Loyalty Drivers** — lift analysis by gender, subscription, age band, location
- **Segment Deep-Dive** — per-segment spend, frequency, promo rate, review rating
- **Recovery Roadmap** — projected net gain from each phase of the playbook

> If visuals show no data, go to **Transform Data → Data Source Settings** and re-point to your local `engineered_dataset.csv`.

### Step 4 — Playbook & summary
`Deliverable_4_Retention_Playbook.docx` — the 4-phase Promo Sunset Playbook:
- **Phase 1:** Stop discounting Promo Hunters → $3,738 net gain
- **Phase 2:** Gradually reduce discounts for At-Risk → $5,600 net gain
- **Phase 3:** Protect Organic Loyalists and High-Value Retainers → safeguards $59,376 clean revenue
- **Phase 4:** Fix the subscription program before further investment

`Deliverable_5_Executive_Summary.pdf` — board-ready one-pager with all findings and recommendations.

---

## Tech stack

| Tool | Purpose |
|---|---|
| Python (Pandas, NumPy) | Feature engineering, metric construction |
| PostgreSQL | Segmentation queries, lift analysis |
| Power BI | 4-panel management dashboard |
| Google Colab | Notebook execution environment |

---

## Loyalty definitions

Two competing definitions were constructed and tested:

**Loyal_A (behavioural)** — customer must satisfy all three:
1. No discount or promo code used (organic buyer)
2. Previous purchases ≥ 25 (long tenure)
3. Review rating above dataset mean (satisfied)

**Loyal_B (revenue-based)** — customer must satisfy all three:
1. Purchase amount in top 40% of dataset
2. Frequency score in top 40%
3. No discount applied

Loyal_A is the primary metric throughout the analysis. Loyal_B was constructed as a stress test to check whether revenue-heavy customers are also behaviourally loyal (finding: they often are not, if they are also promo-dependent).

---

## Segment definitions

| Segment | Criteria | Size |
|---|---|---|
| Organic Loyalist | Loyal_A = 1, no promo use | High-value core |
| High-Value Retainer | High value score, some promo use | Retention target |
| At-Risk | Mid value, high promo dependency | Gradual discount reduction |
| Promo Hunter | Low value, exclusively promo-driven | Stop discounting immediately |
| Dormant | Low frequency, low tenure | Re-engagement or deprioritise |

---

> Data: synthetic D2C fashion brand transactional dataset (3,900 customers).
> This is an independent analytics project completed as part of the Consulting & Analytics Club, IIT Guwahati.
