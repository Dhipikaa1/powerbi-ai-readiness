# 📝 Step 4 — Describe

> **Auto-generate descriptions** for every measure, column, table, and relationship using an LLM.

## 🎯 What this step does

Most semantic models have zero descriptions. Manual writing takes weeks.

This step:

- 🤖 Sends each measure's **DAX expression**, name, and table context to an LLM
- 📝 Generates a **business-friendly description** (1–2 sentences, plain English)
- 🏷️ Suggests **synonyms** for natural-language Q&A
- 📂 Recommends a **display folder** path
- 💾 Writes back into TMDL `description` and `annotation` properties

## 📥 Inputs

- TMDL semantic model (cleaned & optimized — Steps 1 & 2)
- LLM credentials (OpenAI / Anthropic / Azure OpenAI)

## 📤 Outputs

- Modified TMDL with description + synonym annotations
- `descriptions-changelog.md` — every change with before/after

## 🚀 Usage

```bash
# Set up credentials
echo "OPENAI_API_KEY=sk-..." > ../.env

# Run
python scripts/generate_descriptions.py \
    --model ../sample-model/AdventureWorks.SemanticModel \
    --target measures columns tables \
    --batch-size 20
```

## 🧠 The Prompt

See [prompts/describe_prompt.md](prompts/describe_prompt.md)

## 📊 Sample Results

**Before:**
```tmdl
measure 'Sales Amount' = SUM(Sales[SalesAmount])
```

**After:**
```tmdl
measure 'Sales Amount' = SUM(Sales[SalesAmount])
    description: "Total gross sales revenue in USD across all transactions. Used for top-line revenue reporting."
    displayFolder: "Revenue Metrics"
    formatString: "$#,##0"
    annotation Synonyms = "[\"Total Sales\", \"Revenue\", \"Gross Revenue\", \"Top Line\"]"
```

## ⚙️ Configuration

| Setting | Default | Description |
|---------|---------|-------------|
| `--model-provider` | `openai` | `openai`, `anthropic`, `azure` |
| `--model-name` | `gpt-4o-mini` | Cheaper/faster model |
| `--temperature` | `0.2` | Lower = more consistent |
| `--max-synonyms` | `5` | Max synonyms per artifact |

## 🔗 Next Step

→ [05-rename](../05-rename/)
