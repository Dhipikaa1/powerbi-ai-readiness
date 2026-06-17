# 🧪 Sample Model

> A demo Power BI semantic model used by all examples in this repo.

## Download

We use the **AdventureWorks** sample model maintained by Microsoft.

### Option A — Download the .pbix
1. Go to https://learn.microsoft.com/en-us/power-bi/create-reports/sample-datasets
2. Download `AdventureWorks Sales Sample.pbix`
3. Save it to this folder as `AdventureWorks.pbix`

### Option B — Use a .pbip (recommended for this repo)
1. Open `AdventureWorks.pbix` in Power BI Desktop
2. File → Save As → choose `.pbip` format
3. Save the folder structure here as `AdventureWorks.SemanticModel/`

## What's in the sample model

- **Fact tables:** `Sales`, `Returns`
- **Dimension tables:** `Customer`, `Product`, `Date`, `Territory`, `Reseller`
- **Measures:** ~40 (with intentionally bad names + missing descriptions for demo)
- **Approximate rows:** 60K sales, 18K customers, 600 products

## Try the pipeline

Once you have the model in this folder, run:

```bash
# From the repo root
python 01-cleanup/scripts/cleanup.py --model sample-model/AdventureWorks.SemanticModel --dry-run
python 04-describe/scripts/generate_descriptions.py --model sample-model/AdventureWorks.SemanticModel
python 05-rename/scripts/rename_measures.py --model sample-model/AdventureWorks.SemanticModel --dry-run
```

Then publish to Fabric and run `06-ai-readiness-score/notebooks/AI_Readiness_Score.ipynb`.

---

> **Note:** `.pbix` and `.pbip` files are git-ignored. Never commit your own enterprise models to a public repo.
