# 📷 Before / After Examples

> Anonymized examples showing the impact of running the full 6-step pipeline.

## Sample Measure

### ❌ Before
```tmdl
measure 'fct_sls_amt_ytd_lcy' = 
    CALCULATE(
        SUM(fct_sales_transactions[sls_amt]),
        FILTER(ALL(dim_dt_calendar), 
            dim_dt_calendar[dt_key] <= MAX(dim_dt_calendar[dt_key])
            && YEAR(dim_dt_calendar[dt_key]) = YEAR(MAX(dim_dt_calendar[dt_key]))
        )
    )
```
- No description
- No synonyms
- No format string
- No display folder
- Technical name (`fct_*`, `_lcy`)
- Inefficient `FILTER(ALL(...))` pattern

### ✅ After
```tmdl
measure 'Sales Amount YTD' = 
    TOTALYTD(SUM(Sales[Sales Amount]), 'Date'[Date])
    formatString: "$#,##0"
    displayFolder: "Revenue Metrics"
    description: "Year-to-date gross sales revenue in USD. Resets January 1st each year."
    annotation Synonyms = "[\"YTD Sales\", \"Year-to-date Revenue\", \"Sales This Year\"]"
```

---

## Sample Model — Score Improvement

| Metric | Before | After |
|---|---|---|
| **AI Readiness Score** | 🔴 32 / 100 | 🟢 91 / 100 |
| Measures with description | 4% | 100% |
| Measures with synonyms | 0% | 96% |
| Measures in display folders | 12% | 100% |
| Cryptic / `_tmp` names | 47 | 0 |
| Dead measures | 23 | 0 |
| Long DAX (>30 lines) | 18 | 2 |

---

## Copilot in action

### ❌ Before — Copilot fails
> **User:** "What's our YTD revenue?"
>
> **Copilot:** "I couldn't find a measure for YTD revenue."

### ✅ After — Copilot succeeds
> **User:** "What's our YTD revenue?"
>
> **Copilot:** "Your **Sales Amount YTD** is **$4.2M**, up 12% vs. last year. Here's the trend chart..."

---

## Q&A in action

### ❌ Before
> **User types:** "top customers this year"
>
> **Q&A:** "I don't understand 'top customers this year'."

### ✅ After
> **User types:** "top customers this year"
>
> **Q&A:** Returns a clustered bar chart of `Customer Name` by `Sales Amount YTD`, top 10.
