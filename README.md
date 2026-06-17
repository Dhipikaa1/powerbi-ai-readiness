# 🤖 Power BI → AI-Ready in 6 Steps

> **Turn any legacy Power BI semantic model into a Copilot-ready, agent-friendly, AI-native data product — in minutes, not weeks.**

[![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](https://powerbi.microsoft.com)
[![Microsoft Fabric](https://img.shields.io/badge/Microsoft%20Fabric-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)](https://www.microsoft.com/microsoft-fabric)
[![GitHub Copilot](https://img.shields.io/badge/GitHub%20Copilot-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/features/copilot)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

---

## 🎯 The Problem

Most Power BI semantic models aren't **AI-ready**:

- ❌ No descriptions on measures, columns, tables
- ❌ Cryptic technical names (`fct_sls_amt_usd_ytd`) instead of business names
- ❌ No synonyms for natural-language Q&A
- ❌ Dead measures, hidden columns, unused tables bloating the model
- ❌ Unoptimized DAX killing Copilot response times
- ❌ No way to **measure** how AI-ready a model is

**Result:** Copilot, Q&A, and Data Agents fail silently — or return confusing answers.

---

## 💡 The Solution — A 6-Step Pipeline

```mermaid
graph LR
    A[1. Cleanup] --> B[2. Optimization]
    B --> C[3. BPA]
    C --> D[4. Describe]
    D --> E[5. Rename]
    E --> F[6. AI Readiness Score]
    style F fill:#0F766E,color:#fff
```

| # | Step | What it does | Powered By |
|---|------|--------------|-----------|
| 1️⃣ | **[Cleanup](01-cleanup/)** | Remove dead measures, hidden columns, unused tables | LLM + TMDL parser |
| 2️⃣ | **[Optimization](02-optimization/)** | Simplify DAX, add partitions, fix relationships | LLM + DAX Studio |
| 3️⃣ | **[BPA](03-bpa/)** | Custom Best Practice Analyzer rules for AI-readiness | Tabular Editor |
| 4️⃣ | **[Describe](04-describe/)** | Auto-generate descriptions for measures, columns, tables | LLM (GPT / Claude) |
| 5️⃣ | **[Rename](05-rename/)** | Business-friendly naming following naming conventions | LLM |
| 6️⃣ | **[AI Readiness Score](06-ai-readiness-score/)** | 0–100 score with detailed breakdown | Fabric Notebook |

---

## 🎬 Demo

> 📹 *2-minute walkthrough video coming soon*

![Pipeline overview](docs/images/pipeline.png)

---

## 🚀 Quick Start

```bash
# 1. Clone
git clone https://github.com/Dhipikaa1/powerbi-ai-readiness.git
cd powerbi-ai-readiness

# 2. Install Python dependencies
pip install -r requirements.txt

# 3. Pick any step folder and run
cd 04-describe
python scripts/generate_descriptions.py --model ../sample-model
```

---

## 📊 Sample Results — Before vs After

| Metric | Before | After |
|--------|--------|-------|
| Measures with descriptions | 12% | 100% |
| Cryptic measure names | 73% | 0% |
| Synonyms coverage | 0% | 95% |
| Unused measures | 47 | 0 |
| AI Readiness Score | 23/100 | 96/100 |
| Copilot Q&A accuracy | ~40% | ~92% |

📁 See [`docs/before-after.md`](docs/before-after.md) for full screenshots.

---

## 🛠️ Tech Stack

- **Microsoft:** Power BI · Microsoft Fabric · Lakehouse · TMDL
- **AI / GenAI:** GitHub Copilot · LLMs (GPT-4, Claude) · MCP Servers
- **Tools:** Tabular Editor 2/3 · DAX Studio · VS Code
- **Languages:** Python · DAX · M (Power Query) · TMDL
- **DevOps:** Azure DevOps · Git

---

## 📂 Repository Structure

```
powerbi-ai-readiness/
├── 01-cleanup/              # Remove dead artifacts
├── 02-optimization/         # DAX & model optimization
├── 03-bpa/                  # Best Practice Analyzer rules
├── 04-describe/             # Auto-generate descriptions
├── 05-rename/               # Business-friendly renaming
├── 06-ai-readiness-score/   # Fabric notebook + scoring rubric
├── sample-model/            # Demo TMDL (AdventureWorks)
├── docs/                    # Architecture & before-after
└── README.md
```

---

## 🗺️ Roadmap

- [x] 6-step pipeline framework & documentation
- [x] Custom BPA rules for AI-readiness
- [x] LLM prompts for describe + rename
- [x] Fabric scoring notebook
- [ ] Sample TMDL model (AdventureWorks-based)
- [ ] Per-step demo videos
- [ ] End-to-end orchestration script
- [ ] Integration with Power BI REST API
- [ ] Direct push to Fabric workspaces via XMLA
- [ ] Copilot Studio agent template

---

## 🤝 Contributing

This is a personal portfolio project, but contributions and feedback are welcome!  
Open an [issue](../../issues) or start a [discussion](../../discussions).

---

## 📜 License

MIT — see [LICENSE](LICENSE)

---

## 👤 Author

**Dhipikaa Chakka**  
Data & AI Engineer · ~5 yrs in Power BI, Microsoft Fabric, GenAI  
🔗 [LinkedIn](https://www.linkedin.com/in/dhipikaachakka) · [GitHub](https://github.com/Dhipikaa1)

> *"Pushing BI into the agentic AI era — making semantic models AI-ready, building Data Agents, and automating the entire BI lifecycle with LLM-powered workflows in VS Code."*

---

⭐ **If you find this useful, please star the repo!**
