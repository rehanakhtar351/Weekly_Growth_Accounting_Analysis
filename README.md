# Weekly Growth Accounting Analysis

Analysis of weekly active user (WAU) data using the **Growth Accounting framework** — decomposing week-over-week user changes into New, Retained, Resurrected, and Churned users, then calculating **Quick Ratio** and **Retention Rate** to assess whether user growth reflects genuine product-market fit or a "leaky bucket" propped up by acquisition.

**Framework reference:** [Validating PMF with User Growth Accounting in Tech Startups](https://surdeepsingh.com/product-management/first-principles/validating-pmf-with-user-growth-accounting-in-tech-startups/)

## What's in this repo

```
├── growth_accounting.py                        # standalone script — run end-to-end
├── notebooks/
│   └── growth_accounting_analysis.ipynb         # step-by-step notebook version
├── outputs/
│   ├── growth_accounting_output.csv             # full weekly metrics table
│   ├── growth_accounting_chart.png              # static chart (matplotlib)
│   ├── growth_accounting_interactive.html       # interactive chart (Plotly)
│   └── growth_accounting_dashboard.html         # 3-panel interactive dashboard
├── requirements.txt
└── README.md
```

> **Note:** The raw input file (`Active_Users_-_Growth_Accounting.xls`) is not included in this repo — add your own weekly active-user file with one column per week (`w1, w2, ... wN`), each cell containing a device ID active that week.

## Methodology

For every week `t`, each active device ID is compared against the previous week and the running pool of all previously-seen IDs:

| Category | Definition |
|---|---|
| **New** | Active this week, never seen in any prior week |
| **Retained** | Active this week **and** last week |
| **Resurrected** | Active this week, not last week, but seen further back |
| **Churned** | Active last week, not active this week |

```python
new         = current - ever_seen
retained    = current & previous
resurrected = current - previous - new
churned     = previous - current
```

Two derived metrics:

- **Quick Ratio** = `(New + Resurrected) / Churned` — above 1 means the business is growing; below 1 means it's shrinking.
- **Retention Rate** = `Retained / Previous week's WAU` — the % of last week's users who came back.

**Validation check:** for every week, `Retained + Churned` must equal the *previous* week's total WAU. This identity is checked automatically in both the script and notebook (0 mismatches = classification logic is correct).

## How to run

```bash
pip install -r requirements.txt
python growth_accounting.py
```

Or open `notebooks/growth_accounting_analysis.ipynb` in Jupyter for a step-by-step walkthrough with inline explanations.

Outputs (CSV + 3 chart formats) are written to `outputs/`.

## Key Insights

- **WAU grew 110%** (1,759 → 3,696) over the 56-week period, but the growth was uneven, not steady.
- **Quick Ratio averages ~1.05** — barely above the 1.0 breakeven line. In 29 of 55 weeks, it dropped below 1, meaning the app lost more users than it gained nearly half the time.
- **Churn is the core problem** — 25-30% of active users leave every single week. This is the dominant factor, far bigger than any acquisition shortfall.
- **Weeks 28-33 show a clear crisis**: WAU fell 18% (3,075 → 2,529), and new-user acquisition hit its lowest point of the year in this stretch.
- **Retention is improving** — from ~53-68% early on to ~71-78% in the second half. This is the strongest positive signal in the data.
- **New-user growth is slowing** while resurrected users are rising, suggesting a shift toward win-back campaigns over fresh acquisition.

**Verdict:** Growth is real but fragile — it's being driven by outrunning churn rather than strong retention. Product-market fit isn't fully validated yet, but the improving retention trend is the clearest sign this could be heading in the right direction.

## Author

Rehan Akhtar
[LinkedIn](https://linkedin.com/in/rehan-akhtar17) · [GitHub](https://github.com/rehanakhtar351) · [Portfolio](https://rehanakhtar351.github.io)
