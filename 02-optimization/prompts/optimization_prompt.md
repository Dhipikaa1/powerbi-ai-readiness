SESSION 1 — AI READINESS RELATIONSHIP ANALYSIS PROMPT 

================================================================================ 

PURPOSE: Analyze any Power BI semantic model's relationships for AI readiness. 

         Produce a multi-sheet Excel workbook with issues, all fix approaches, 

         report impact, and prioritized action plan. 

         DO NOT MODIFY ANYTHING — ANALYSIS ONLY. 

================================================================================ 

  

You are an expert Power BI architect specializing in AI readiness optimization. 

Analyze the semantic model and its associated PBIP report to produce a 

comprehensive relationship analysis Excel workbook. 

  

⚠️ DO NOT MODIFY ANYTHING — THIS SESSION IS ANALYSIS ONLY. 

  

================================================================================ 

INPUTS (UPDATE THESE FOR YOUR MODEL) 

================================================================================ 

  

1. Semantic Model: 

   Model:     '<YOUR_MODEL_NAME>' 

   Workspace: '<YOUR_WORKSPACE_NAME>' 

  

2. PBIP Report Folder (if available): 

   <YOUR_PBIP_REPORT_FOLDER_PATH> 

  

   (If no PBIP folder, skip report impact analysis and note it in the Excel.) 

  

================================================================================ 

PHASE 1: CONNECT & INVENTORY (MCP Tools) 

================================================================================ 

  

Run ALL of the following. Do not skip any step. 

  

1.1 — Connect to Model 

    Tool: connection_operations ConnectFabric 

    Connect using the model name and workspace name above. 

  

1.2 — List ALL Relationships 

    Tool: relationship_operations List 

    Capture for EVERY relationship: 

      • name, fromTable, fromColumn, toTable, toColumn 

      • fromCardinality, toCardinality 

      • crossFilteringBehavior (OneDirection / BothDirections) 

      • isActive (true / false) 

  

1.3 — List ALL Tables 

    Tool: table_operations List 

    Classify each table as: 

      • Fact (contains measures + numeric columns) 

      • Fact (Aggregate) (pre-aggregated fact table) 

      • Dimension (lookup, descriptive attributes) 

      • Bridge (M:M resolver between fact and dim) 

      • Field Parameter (DAX calc table with NAMEOF/SELECTEDVALUE pattern) 

      • Measures Only (no columns, only measures) 

      • Utility (DataRefresh, Volume Locks, config tables) 

      • Reference/Mapping (lookup/mapping tables like SAP mappings) 

      • Date Dimension (datekey, FiscalYear, etc.) 

      • Disconnected (no relationships at all) 

      • Auto-generated (LocalDateTable_*) 

  

1.4 — Get Model Properties 

    Tool: model_operations Get 

    Capture: 

      • discourageImplicitMeasures (must be TRUE for AI readiness) 

      • discourageReportMeasures 

      • culture, defaultMode 

  

1.5 — List ALL Measures with Expressions 

    Tool: measure_operations List, then Get (with References) for all measures 

    Check DAX for: USERELATIONSHIP, CROSSFILTER, TREATAS 

    Note which table each measure belongs to. 

  

1.6 — List ALL Security Roles 

    Tool: security_role_operations List 

    If any roles exist, capture role names and table filter expressions. 

    Flag relationships where either table is used in RLS. 

  

1.7 — List ALL Calculation Groups 

    Tool: calculation_group_operations LISTGROUPS 

    Capture group names and calculation items. 

  

1.8 — Get Column Details for Key Tables 

    Tool: column_operations List 

    At minimum, get columns for: 

      • All tables involved in M:M relationships 

      • All bridge tables 

      • All tables involved in BiDi relationships 

    Capture: name, dataType, isHidden, sortByColumn 

  

1.9 — Check for Field Parameter Tables 

    Tool: partition_operations Get 

    For any table that looks like a parameter table (name contains "Parameter"), 

    get the partition DAX to confirm NAMEOF pattern. 

  

1.10 — Check Report-Level Measures (if PBIP folder provided) 

    Read: <PBIP_FOLDER>/definition/reportExtensions.json 

    Capture ALL measures defined in the report (NOT in the model). 

    These are INVISIBLE to Copilot and must be migrated. 

  

1.11 — Parse PBIP Report Visuals (if PBIP folder provided) 

    Read: <PBIP_FOLDER>/definition/pages/pages.json for page order 

    For each page folder: 

      • Read page.json for display name 

      • List visuals folder 

      • For each visual, read visual.json to capture: 

        - Visual type 

        - Tables and columns in projections 

        - Filters (visual/page/report level) 

        - Measures referenced 

  

================================================================================ 

PHASE 2: CLASSIFY EVERY RELATIONSHIP 

================================================================================ 

  

For EACH relationship, assign one of these categories: 

  

• KEEP — Standard 1:M OneDirection between Dim→Fact. No issues. 

• KEEP-AUTO — Auto-generated LocalDateTable relationships. Leave as-is. 

• KEEP-INACTIVE — Purposefully inactive, activated by USERELATIONSHIP in DAX. 

• FIX-MM — Many-to-Many relationship. MUST be resolved for AI readiness. 

• FIX-BIDI — BothDirections relationship. MUST be changed or justified. 

• FIX-MM-BIDI — Both M:M AND BiDi. WORST CASE. Highest priority fix. 

• FIX-BIDI-PARAM — BiDi on field parameter tables. May be acceptable exception. 

• REMOVE — Inactive and unused (no USERELATIONSHIP in any DAX). 

  

================================================================================ 

PHASE 3: GENERATE EXCEL WORKBOOK 

================================================================================ 

  

Create a Python script that generates an Excel workbook (.xlsx) using openpyxl 

with EXACTLY these 10 sheets. Use professional formatting throughout. 

  

Color coding rules: 

  • Headers: Dark blue background (#2F5496), white bold text 

  • CRITICAL/FAIL rows: Red fill (#FFC7CE) 

  • WARNING/WARN rows: Yellow fill (#FFEB9C) 

  • OK/PASS rows: Green fill (#C6EFCE) 

  • Recommended approaches: Green fill 

  • Not recommended: Light red fill 

  • Partial/Maybe: Yellow fill 

  • All cells: Thin borders, wrap text, auto-width columns 

  

─────────────────────────────────────────────────────────────── 

SHEET 1: "Executive Summary" 

Tab color: #2F5496 

─────────────────────────────────────────────────────────────── 

Title row: "AI READINESS — RELATIONSHIP ANALYSIS" 

Subtitle: "Model: [name] | Workspace: [name]" 

  

Table with columns: Parameter | Value | Notes 

  

Include these rows: 

  • Total Tables 

  • Total Relationships 

  • Total Measures (Model) 

  • Total Measures (Report-Level) — count from reportExtensions.json 

  • RLS Roles — count 

  • Calculation Groups — count 

  • discourageImplicitMeasures — current value (flag if FALSE) 

  • [blank separator] 

  • ISSUES FOUND header 

  • M:M Relationships — count + brief description 

  • BiDi Relationships — count + breakdown by type 

  • M:M + BiDi Combined — count (worst cases) 

  • 1:1 BiDi — count + explanation 

  • Inactive Unused — count 

  • Inactive Used (USERELATIONSHIP) — count 

  • Report-Level Measures with USERELATIONSHIP — count 

  • [blank separator] 

  • RISK LEVEL — LOW / MEDIUM / HIGH based on issue count 

  

─────────────────────────────────────────────────────────────── 

SHEET 2: "Model Inventory" 

Tab color: #548235 

─────────────────────────────────────────────────────────────── 

Section title: "MODEL INVENTORY" 

  

Table with columns: 

  Table Name | Type | Columns | Measures | Relationships | Notes 

  

One row per table. Include ALL tables from the model. 

Classify each table type as described in Phase 1.3. 

  

─────────────────────────────────────────────────────────────── 

SHEET 3: "All Relationships" 

Tab color: #BF8F00 

─────────────────────────────────────────────────────────────── 

Section title: "ALL RELATIONSHIPS" 

  

Table with columns: 

  # | From Table | From Column | To Table | To Column | 

  From Card | To Card | Direction | Active | Category | Issue 

  

One row per relationship. Include ALL relationships. 

Category = the classification from Phase 2. 

Issue = brief description of why it's flagged (empty if KEEP). 

  

Color-code entire rows: 

  • FIX-MM-BIDI rows: Red fill 

  • FIX-MM rows: Orange fill (#F4B084) 

  • FIX-BIDI rows: Yellow fill 

  • KEEP rows: No fill 

  

─────────────────────────────────────────────────────────────── 

SHEET 4: "AI Readiness Issues" 

Tab color: #C00000 

─────────────────────────────────────────────────────────────── 

Section title: "AI READINESS ISSUES — WHY COPILOT STRUGGLES WITH THIS MODEL" 

  

First: Add 5 explanation lines about why M:M and BiDi are problems: 

  1. M:M causes DUPLICATE ROWS — SUM() over-counts 

  2. BiDi creates AMBIGUOUS FILTER PATHS 

  3. Copilot assumes standard star schema (1:M single direction) 

  4. Microsoft AI readiness guidance: NO M:M, NO BiDi, discourageImplicitMeasures=TRUE 

  5. [count] M:M + [count] BiDi = wrong answers from Copilot 

  

Then: "ISSUE INVENTORY" table with columns: 

  # | Issue Type | Tables Involved | Current State | 

  Why It's a Problem | Severity | Report Pages Affected | Measures Affected 

  

One row per unique issue (group similar relationships). 

Severity: CRITICAL / HIGH / MEDIUM / LOW 

Explain in plain language WHY each issue causes Copilot to fail. 

  

Always include these issue types if they exist: 

  • M:M Active relationships 

  • M:M Inactive (used by USERELATIONSHIP) 

  • M:M + BiDi Active (worst case) 

  • 1:1 BiDi chains 

  • Parameter table BiDi 

  • discourageImplicitMeasures = FALSE 

  • Report-Level Measures invisible to Copilot 

  • No Descriptions on objects 

  • Disconnected tables 

  

─────────────────────────────────────────────────────────────── 

SHEET 5: "Fix Suggestions"  ← MOST IMPORTANT SHEET 

Tab color: #00B050 

─────────────────────────────────────────────────────────────── 

Section title: "RELATIONSHIP FIX SUGGESTIONS — ALL POSSIBLE APPROACHES" 

  

Table with columns: 

  Issue # | Relationship / Issue | Approach | Approach Name | 

  What To Do | DAX / Model Changes Required | 

  Report Impact | Pros | Cons | Recommended? 

  

CRITICAL RULES FOR THIS SHEET: 

  • For EVERY issue, provide AT LEAST 3-4 different approaches (A, B, C, D) 

  • Approaches must include: 

    A. Fix source data / create proper keys (best for star schema) 

    B. Denormalize (merge tables to eliminate relationship) 

    C. Use TREATAS / virtual relationship (DAX-based fix) 

    D. Use CROSSFILTER in measures (band-aid) 

    E. Deactivate and use USERELATIONSHIP (if applicable) 

    F. Keep as exception with documentation (if justified) 

  • For each approach, clearly state: 

    - Exact model/source changes needed 

    - Exact DAX code if applicable 

    - Which report visuals break and how to fix them 

    - Whether report needs recreation 

  • Mark ONE approach as "YES — BEST" (the recommended one) 

  • Mark approaches as "YES — GOOD", "PARTIAL", "MAYBE", "NO" 

  

Color-code rows: 

  • "YES — BEST": Bright green fill 

  • "YES — GOOD": Light green fill 

  • "NO": Light red fill 

  • "PARTIAL" / "MAYBE": Yellow fill 

  

─────────────────────────────────────────────────────────────── 

SHEET 6: "Report-Level Measures" 

Tab color: #7030A0 

─────────────────────────────────────────────────────────────── 

Section title: "REPORT-LEVEL MEASURES" 

Subtitle (red italic): "These measures are INVISIBLE to Copilot." 

  

Table with columns: 

  # | Measure Name | Defined In Entity | DAX Expression | Data Type | 

  Uses USERELATIONSHIP? | Tables Referenced | Used On Pages | Migration Priority 

  

One row per report-level measure found in reportExtensions.json. 

Migration Priority: CRITICAL (if uses USERELATIONSHIP) / HIGH / MEDIUM / LOW 

  

If no PBIP folder was provided, state: "No PBIP folder provided. Cannot analyze 

report-level measures. Please provide the PBIP report folder path." 

  

─────────────────────────────────────────────────────────────── 

SHEET 7: "15-Point Checklist" 

Tab color: #4472C4 

─────────────────────────────────────────────────────────────── 

Section title: "15-PARAMETER AI READINESS CHECKLIST" 

  

Table with columns: 

  # | Parameter | Status | Current Value | Required Value | Details | Action Needed 

  

ALWAYS evaluate these 15 parameters: 

   1. M:M Relationships — count. Required: 0 

   2. BothDirections (BiDi) — count. Required: 0 (or justified only) 

   3. Hub Tables (5+ inbound rels) — list. Required: documented 

   4. Visual Dependencies — page/visual count. Required: documented 

   5. Column Uniqueness — check "to-side" key columns. Required: all unique 

   6. Blank Join Keys — COUNTBLANK check. Required: 0 

   7. Description Coverage — %. Required: 100% on measures/key columns 

   8. Naming Quality — technical prefixes? Required: business-friendly 

   9. Hidden ID Columns — are keys hidden? Required: yes 

  10. Format Strings — all measures formatted? Required: yes 

  11. discourageImplicitMeasures — value. Required: TRUE 

  12. Inactive Relationships — count used vs unused. Required: only purposeful 

  13. Circular Paths — graph analysis. Required: 0 

  14. Sort-by-Column — text columns sorted? Required: set where needed 

  15. Synonyms — alternative names for Q&A? Required: yes on key columns 

  

Color-code rows: FAIL=red, WARN=yellow, PASS=green, INFO=no fill 

  

─────────────────────────────────────────────────────────────── 

SHEET 8: "Report Impact" 

Tab color: #FF6600 

─────────────────────────────────────────────────────────────── 

Section title: "REPORT IMPACT ANALYSIS" 

Subtitle: "Impact of recommended relationship changes on each report page" 

  

Table with columns: 

  Page | Visual Count | Change Applied | Impact Level | 

  What Changes | Visuals Affected | Fix Required | Report Needs Recreation? 

  

One row per report page. 

Impact Level: NONE / LOW / LOW-MEDIUM / MEDIUM / MEDIUM-HIGH / HIGH 

Color-code: HIGH=red, MEDIUM=yellow, LOW=light yellow, NONE=green 

  

If no PBIP folder provided, state: "No PBIP folder. Impact analysis requires 

the PBIP report project. Provide the path and re-run." 

  

─────────────────────────────────────────────────────────────── 

SHEET 9: "Action Plan" 

Tab color: #00B0F0 

─────────────────────────────────────────────────────────────── 

Section title: "PRIORITIZED ACTION PLAN — AI READINESS ROADMAP" 

  

Table with columns: 

  Priority | Step | Action | Details | Dependencies | 

  Report Change Needed? | Risk | Status 

  

Priority levels: 

  P0 - Immediate: Model settings, measure migration, remove unused rels 

  P1 - High: Fix M:M and BiDi on primary star schema 

  P2 - Medium: Fix secondary relationship issues 

  P3 - Low: Descriptions, synonyms, format strings, cleanup 

  

All steps should be in dependency order (earlier steps first). 

Color-code: P0=light green, P1=light blue, P2=yellow, P3=light gray 

Status: All "Not Started" 

  

─────────────────────────────────────────────────────────────── 

SHEET 10: "Model Diagram" 

Tab color: #A9A9A9 

─────────────────────────────────────────────────────────────── 

Section title: "MODEL RELATIONSHIP DIAGRAM — TEXT REPRESENTATION" 

  

Create ASCII-art diagrams using Consolas font showing: 

  1. Primary star schema (fact tables in center, dims around) 

  2. M:M problem areas with arrows and labels 

  3. BiDi chains with ◄──BiDi──► notation 

  4. Recommendations inline 

  

Use box-drawing characters: ┌ ┐ └ ┘ ─ │ ▼ ▲ ► ◄ ┬ ┴ ├ ┤ 

  

Column A width: 100 

  

================================================================================ 

EXECUTION INSTRUCTIONS 

================================================================================ 

  

1. Connect to the model using MCP connection_operations 

2. Run ALL inventory steps (Phase 1) — do not skip any 

3. Classify every relationship (Phase 2) 

4. Create the Python script with ALL 10 sheets 

5. Run the script to generate the Excel file 

6. Save to: output_session1/AI_Readiness_Relationship_Analysis.xlsx 

7. Present summary to user 

  

SAFETY: Do NOT modify the model or report. Analysis only. 

The Excel output will be reviewed and used as input for Session 2. 

  

================================================================================ 

 

 

Output: 
AI_Readiness_Relationship_Analysis 1.xlsx 

 
