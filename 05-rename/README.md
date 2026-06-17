# 🏷️ Step 5 — Rename

> Convert cryptic technical names (`fct_sls_amt_usd_ytd`) into **business-friendly names** (`Sales Amount YTD`).

## 🎯 What this step does

Legacy semantic models often have names that came from:
- Source-system column names (`CUST_ID_PK`)
- ETL conventions (`dim_customer_scd2`)
- Developer shorthand (`m_sls_amt_q1_24`)

These break Copilot, Q&A, and any AI agent — none of them understand `fct_*` or `_ytd_lcy`.

This step uses an LLM to:

- 🏷️ Suggest **business-friendly names** following Power BI conventions
- 🔗 Detect **renames that will break references** (and rewrite dependent DAX automatically)
- 📋 Generate a **rename mapping table** for stakeholder review
- 💾 Apply renames safely (with rollback)

## 📥 Inputs

- TMDL model (post Step 4 — with descriptions)
- (Optional) naming-convention guide (`naming-rules.md`)

## 📤 Outputs

- `rename-mapping.csv` — old name → new name + impact
- Patched TMDL with renames applied
- All dependent DAX expressions updated automatically

## 🚀 Usage

```bash
python scripts/rename_measures.py \
    --model ../sample-model/AdventureWorks.SemanticModel \
    --conventions prompts/naming-conventions.md \
    --dry-run
```

## 🧠 The Prompt

See [prompts/rename_prompt.md](prompts/rename_prompt.md)

## 📊 Sample Results

| Old Name | New Name | References Updated |
|----------|----------|-------------------|
| `fct_sls_amt` | `Sales Amount` | 23 measures, 8 visuals |
| `m_cnt_orders_ytd` | `Orders YTD` | 5 measures, 12 visuals |
| `dim_cust_scd2_active` | `Active Customers` | 17 measures |
| `_pct_growth_v2` | `Growth %` | 9 measures |

## 📐 Naming Conventions Applied

- **Tables:** Singular nouns, Title Case (`Customer`, not `customers_dim`)
- **Measures:** Verb + noun in Title Case (`Total Sales`, `Average Order Value`)
- **Columns:** Title Case, no underscores (`Customer ID`, not `customer_id`)
- **Folders:** Plural categories (`Revenue Metrics`, `Customer KPIs`)
- **No abbreviations** unless industry-standard (USD, KPI, YTD, MoM)

## 🔗 Next Step

→ [06-ai-readiness-score](../06-ai-readiness-score/)
