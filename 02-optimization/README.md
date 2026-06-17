# ⚡ Step 2 — Optimization

> Simplify DAX, add partitions, fix bad relationships, and tune storage modes for performance.

## 🎯 What this step does

Even clean models can be slow. This step focuses on:

- 🧮 **DAX simplification** — replace nested IF chains with SWITCH, replace SUMX over large tables with optimized variants
- 🗂️ **Partitioning** — add date-based incremental refresh policies
- 🔗 **Relationship hygiene** — flag bi-directional that should be single, ambiguous joins
- 💾 **Storage modes** — recommend Direct Lake / DirectQuery / Import per table
- 🎯 **Aggregations** — suggest pre-aggregated tables for slow queries
- 🚫 **Cardinality reduction** — find high-cardinality columns degrading compression

## 📥 Inputs

- Cleaned TMDL model (from Step 1)
- (Optional) DAX Studio query plan / VertiPaq Analyzer JSON

## 📤 Outputs

- `optimization-report.md` — every recommendation with rationale
- Patched TMDL with safe automatic fixes applied
- Manual review queue for risky changes

## 🚀 Usage

```bash
python scripts/optimize.py \
    --model ../sample-model/AdventureWorks.SemanticModel \
    --vertipaq vertipaq-analyzer-export.json
```

## 🧠 The Prompt

See [prompts/optimization_prompt.md](prompts/optimization_prompt.md)

## 📊 Sample Results

| Metric | Before | After |
|--------|--------|-------|
| Average refresh time | 12 min | 3.5 min |
| Largest measure DAX length | 487 lines | 92 lines |
| Avg query response | 4.2 s | 0.8 s |
| Model size | 1.8 GB | 920 MB |

## 🔗 Next Step

→ [03-bpa](../03-bpa/)
