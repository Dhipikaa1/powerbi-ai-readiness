# 🔍 Overview — Why AI-Readiness Matters

## The Problem

Microsoft has shipped a wave of AI-on-data features:
- **Copilot for Power BI** — chat with your data
- **Q&A** — type a question, get a chart
- **Custom Data Agents** — domain-specific AI agents grounded on your model
- **Fabric Skills** — reusable AI capabilities over Lakehouses

All of these depend on **one thing**: a well-described semantic model.

### What "well-described" actually means

A typical enterprise Power BI model has:
- ❌ 0–5% of measures with descriptions
- ❌ 0% synonyms
- ❌ Names like `fct_sls_amt_usd_ytd_lcy` or `_tmp_calc_v3`
- ❌ Calculated columns and measures with no display folders
- ❌ Dozens of unused / dead measures bloating the model

When you turn on Copilot or Q&A on such a model, the results are **embarrassing**:
- It can't find measures users ask for
- It picks the wrong measure (because synonyms are missing)
- It surfaces internal `_tmp` measures to end users
- It guesses table joins (because relationships have no descriptions)

## Why this matters now

| Without AI-readiness | With AI-readiness |
|---|---|
| Copilot returns "I couldn't find..." | Copilot answers in plain English |
| Q&A picks wrong measure | Q&A picks the right measure |
| Data Agent hallucinates KPIs | Data Agent grounds on real definitions |
| Stakeholders lose trust in AI | Stakeholders adopt AI |

## The 80/20

Most teams overthink this. They either:
1. Hire a consultant for 6 months to "rewrite" the model, or
2. Skip AI features altogether because "the model isn't ready"

This repo gives you the **80/20 solution**:
- Automate descriptions with LLMs (2 hours, not 6 months)
- Run 25+ AI-readiness checks via a custom BPA ruleset
- Track readiness via a 0–100 score in Fabric

## Who this is for

- **Power BI Admins** — need to make 100+ datasets AI-ready
- **BI Architects** — need a repeatable AI-readiness process
- **Data Engineers** — want to automate semantic-model hygiene
- **Fabric Champions** — want to roll out Copilot at scale

→ Continue to [architecture.md](architecture.md)
