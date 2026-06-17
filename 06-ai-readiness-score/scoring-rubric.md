# AI Readiness Scoring Rubric

Total: **100 points** across 7 categories.

---

## 1. Descriptions (30 points)

| Check | Points | Criteria |
|-------|--------|----------|
| Measures with description (>15 chars) | 12 | `% measures with description × 12` |
| Visible columns with description | 10 | `% columns with description × 10` |
| Visible tables with description | 5  | `% tables with description × 5` |
| Relationships with description | 3  | `% relationships with description × 3` |

---

## 2. Synonyms (20 points)

| Check | Points | Criteria |
|-------|--------|----------|
| Measures with ≥1 synonym | 10 | `% measures with synonym × 10` |
| Columns used in filters with synonym | 7 | Slicer/filter columns |
| Tables with synonym | 3 | `% tables with synonym × 3` |

---

## 3. Naming Conventions (15 points)

| Check | Points | Penalty |
|-------|--------|---------|
| No `_` prefix in measures | 4 | −1 per violation (capped) |
| No `tmp`/`test`/`old` in names | 4 | −1 per violation |
| No `fct_*` / `dim_*` in visible names | 4 | −1 per violation |
| Title Case for measures | 3 | −0.5 per violation |

---

## 4. Display Folders & Format Strings (10 points)

| Check | Points |
|-------|--------|
| Measures in a display folder | 5 |
| Numeric measures with format string | 5 |

---

## 5. Relationship Hygiene (10 points)

| Check | Points |
|-------|--------|
| No inactive relationships | 3 |
| No bidirectional ambiguity | 4 |
| All relationships have referential integrity | 3 |

---

## 6. DAX Quality (10 points)

| Check | Points | Criteria |
|-------|--------|----------|
| No measures > 30 lines | 4 | Penalty per long measure |
| No nested IF (>3 levels) | 3 | Should be SWITCH |
| No FILTER(ALL(...)) pattern | 3 | Should be KEEPFILTERS / CALCULATE |

---

## 7. Performance Signals (5 points)

| Check | Points |
|-------|--------|
| Fact tables >10M rows have partitions | 2 |
| No calculated columns on >1M-row tables | 2 |
| Model size < 1 GB (.pbix) | 1 |

---

## Score Bands

| Score | Status | Action |
|-------|--------|--------|
| **0–40**   | 🔴 Not AI-ready | Run full pipeline (Steps 1–5) |
| **41–70**  | 🟠 Partial | Focus on descriptions + synonyms |
| **71–85**  | 🟡 Mostly ready | Polish & launch Copilot |
| **86–100** | 🟢 Production-ready | Enable Q&A + Data Agents |
