# 🧹 Step 1 — Cleanup

> Remove dead measures, hidden columns, unused tables, and orphaned relationships from a Power BI / Fabric semantic model.

## 🎯 What this step does

Most enterprise semantic models accumulate **dead weight** over years:

- 🪦 Measures that nobody references
- 👻 Hidden columns left from old refactors
- 🗑️ Tables that aren't joined to anything
- ⛓️ Broken relationships pointing to deleted columns
- 📌 Calculated columns that should be measures

This step uses an LLM + TMDL parser to **identify and safely remove** them.

## 📥 Inputs

- A Power BI semantic model in TMDL format (PBIP project)
- (Optional) Report definition JSON to detect actually-used measures

## 📤 Outputs

- `cleanup-report.md` — what was removed and why
- Modified TMDL files with safe deletions applied
- `rollback.json` — undo list in case you need to revert

## 🚀 Usage

```bash
python scripts/cleanup.py \
    --model ../sample-model/AdventureWorks.SemanticModel \
    --report ../sample-model/AdventureWorks.Report \
    --dry-run
```

## 🧠 The Prompt

See [prompts/cleanup_prompt.md](prompts/cleanup_prompt.md)

## 📊 Sample Results

| Artifact | Before | After |
|----------|--------|-------|
| Measures | 247 | 198 |
| Hidden columns | 89 | 12 |
| Unused tables | 6 | 0 |
| Orphan relationships | 4 | 0 |

## ⚠️ Safety

The script runs in **dry-run mode by default**. Add `--apply` only after reviewing the report.

## 🔗 Next Step

→ [02-optimization](../02-optimization/)
