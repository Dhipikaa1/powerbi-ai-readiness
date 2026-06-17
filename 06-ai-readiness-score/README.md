# 📊 Step 6 — AI Readiness Score

> A **0–100 score** that tells you how AI-ready your Power BI / Fabric semantic model is — and exactly what to fix.

## 🎯 What this step does

After running Steps 1–5, you want one number to track progress:

> **"How AI-ready is this model right now?"**

This step runs a **Fabric notebook** that:

- 🔍 Scans the semantic model via XMLA / TMSL
- ✅ Checks 25+ AI-readiness criteria
- 🎯 Calculates a weighted score (0–100)
- 📊 Generates a color-coded HTML report
- 📈 Tracks scores over time (writes to a Fabric Lakehouse table)

## 📥 Inputs

- A published Power BI / Fabric semantic model (XMLA endpoint)
- Connection string

## 📤 Outputs

- `score-report.html` — visual report with sections per criterion
- `score-history.csv` — historical scores for trend analysis
- Slack / Teams notification (optional)

## 🚀 Usage

### Run in Fabric
1. Import [`notebooks/AI_Readiness_Score.ipynb`](notebooks/AI_Readiness_Score.ipynb) into your Fabric workspace
2. Update the connection string in cell 1
3. Run all cells
4. View the HTML report in cell 6

### Run locally
```bash
pip install -r ../requirements.txt
jupyter notebook notebooks/AI_Readiness_Score.ipynb
```

## 📐 Scoring Rubric

See [scoring-rubric.md](scoring-rubric.md) for the full breakdown.

| Category | Weight |
|----------|--------|
| **Descriptions** (measures, columns, tables) | 30% |
| **Synonyms** coverage | 20% |
| **Naming conventions** | 15% |
| **Display folders** & format strings | 10% |
| **Relationship hygiene** | 10% |
| **DAX quality** (no long/nested expressions) | 10% |
| **Performance signals** (size, partitions) | 5% |

## 📊 Score Bands

| Score | Status | Action |
|-------|--------|--------|
| 🔴 **0–40** | Not AI-ready | Run full pipeline (Steps 1–5) |
| 🟠 **41–70** | Partial | Focus on description + synonyms |
| 🟡 **71–85** | Mostly ready | Polish & launch Copilot |
| 🟢 **86–100** | Production-ready | Enable Q&A + Data Agents |

## 📷 Sample Report

![Sample AI Readiness Score](../docs/images/readiness-score.png)

## 🔗 Back to start

→ [Main README](../README.md)
