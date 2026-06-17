# ✅ Step 3 — BPA (Best Practice Analyzer)

> Custom **Best Practice Analyzer rules** built specifically for AI-readiness — run inside Tabular Editor.

## 🎯 What this step does

Microsoft's [official BPA ruleset](https://github.com/microsoft/Analysis-Services/tree/master/BestPracticeRules) catches general modeling issues. We add a **second ruleset** focused on AI-readiness:

- 📝 Every measure/column/table must have a description (>15 chars)
- 🏷️ Every measure must have at least 1 synonym
- 🚫 No measure names starting with `_` or containing `tmp`, `test`, `old`
- 🏷️ Every fact/dim table must have a friendly `Label` annotation
- 📂 Every measure must belong to a `Display Folder`
- 🔢 Every numeric measure must have a `Format String`
- 📊 Every column used in slicers/filters must have synonyms
- 🔗 Every relationship must have a description
- 🤖 No reference to dropped Copilot/Q&A-incompatible features

## 📥 Inputs

- Power BI semantic model (loaded in Tabular Editor 2 or 3)
- The custom rules file: [`rules/ai-readiness-rules.json`](rules/ai-readiness-rules.json)

## 📤 Outputs

- BPA report — list of violations with severity
- Fix-suggestions auto-applied (where safe)

## 🚀 Usage

### Option A — Tabular Editor 3 (GUI)
1. Open your model
2. Tools → Best Practice Analyzer
3. Settings → Add new rule file → point to `rules/ai-readiness-rules.json`
4. Run analyzer

### Option B — Command line (CI/CD)
```bash
TabularEditor.exe "Model.bim" \
  -A "rules/ai-readiness-rules.json" \
  -V
```

## 📊 Sample Results

| Severity | Rule | Violations |
|----------|------|-----------|
| 🔴 Error | Missing description on measure | 87 |
| 🟡 Warning | Missing synonym | 142 |
| 🟡 Warning | Measure not in display folder | 56 |
| 🟢 Info | Missing format string | 23 |

## 🔗 Next Step

→ [04-describe](../04-describe/) (auto-fix the description violations)
