POWER BI SEMANTIC MODEL CLEANUP AUDIT — ZERO-BREAKAGE PROMPT 

================================================================================ 

  

Objective: 

Analyze the Power BI semantic model and associated PBIP project to identify 

unused, redundant, and dependent objects. Generate an Excel audit report ONLY. 

  

⚠️ DO NOT DELETE ANYTHING. Output is ANALYSIS ONLY. 

⚠️ After I review the Excel, I will tell you which specific items to delete. 

  

================================================================================ 

SCOPE 

================================================================================ 

  

1. PBIP Report folder path: 

   <INSERT_PBIP_FOLDER_PATH> 

  

2. Semantic model: 

   Model: '<INSERT_MODEL_NAME>' 

   Workspace: '<INSERT_WORKSPACE_NAME>' 

  

================================================================================ 

MANDATORY DATA SOURCES TO QUERY (ALL 8 — DO NOT SKIP ANY) 

================================================================================ 

  

You MUST query and parse ALL of these before classifying ANY object: 

  

  1. PBIP Report JSON (visuals, filters, slicers, bookmarks, conditional formatting) 

  2. Report-level measures (reportExtensions.json) 

  3. Model measure DAX expressions (via measure Get operation) 

  4. ★ CALCULATED TABLE partition expressions (via partition_operations Get) 

  5. Calculated column expressions (via column Get operation) 

  6. ★ Sort-by-column relationships (via column Get → sortByColumn property) 

  7. Relationship key columns (via relationship_operations List) 

  8. RLS role definitions, Perspectives, Calculation Groups 

  

  

================================================================================ 

STEP 1: PARSE PBIP REPORT DEFINITION 

================================================================================ 

  

Walk every visual.json, page.json, report.json, bookmark JSON and extract: 

  

1.1 Visual Fields: 

    - Query projections (columns, measures, hierarchies) 

    - Visual-level filters 

    - Page-level filters 

    - Report-level filters 

  

1.2 Conditional Formatting: 

    - Conditional.Cases blocks (rules-based color banding) 

    - selector.metadata references (Table.Column format) 

    - Background color, font color, data bars, icons, web URL 

  

1.3 Report-Level Measures (reportExtensions.json): 

    - Extract ALL measures defined in the report (not in model) 

    - Parse their DAX expressions for column/measure references 

    - Follow explicit "references" blocks for cross-measure dependencies 

    - Any model column/measure referenced by a report measure = NEEDED 

  

1.4 Field Parameters — CRITICAL: 

    - Detect fieldParameters blocks in visual query definitions 

    - Extract BOTH: 

      a) The parameter table reference (parameterExpr → Entity) 

      b) ALL projections controlled by the parameter (index + length range) 

    - Mark ALL referenced fields as "Used via Field Parameter" 

  

1.5 Sort Definitions: 

    - Extract sortDefinition.sort[].field references from visuals 

  

1.6 Synced Slicers: 

    - Detect syncGroup on slicer visuals 

    - ALL fields in synced slicers are GLOBALLY USED 

  

1.7 Bookmarks: 

    - Parse bookmark JSON for targetVisualNames 

    - ALL fields in bookmark-targeted visuals must be retained 

  

1.8 Drillthrough / Tooltips: 

    - Extract drillFilterOtherVisuals fields 

    - Extract tooltip page field references 

  

================================================================================ 

STEP 2: PARSE SEMANTIC MODEL (QUERY THE LIVE MODEL) 

================================================================================ 

  

2.1 Tables, Columns, Measures: 

    - Use column_operations List (include hidden) 

    - Use measure_operations List + Get (with expressions) 

  

2.2 ★ CALCULATED TABLE EXPRESSIONS (CRITICAL — DO NOT SKIP): 

    - Use partition_operations List to find ALL sourceType=Calculated tables 

    - Use partition_operations Get to retrieve their DAX expressions 

    - Parse the DAX for ALL 'Table'[Column] and NAMEOF('Table'[Column]) refs 

    - These refs make those columns NEEDED even if not in any visual 

    - RULE: ALL columns inside a calculated table are structural and CANNOT 

      be independently deleted — mark them ALL as Needed=Yes automatically 

  

2.3 ★ Sort-By-Column (CRITICAL — DO NOT HARDCODE): 

    - Use column_operations Get on columns that might have sortByColumn set 

    - At minimum check: all columns in Parameter tables, Volume Locks tables, 

      vwDimDate, and any column used as a category axis 

    - If column A sorts by column B → column B is NEEDED 

    - Check groupByColumns property too 

  

2.4 Relationships: 

    - Use relationship_operations List 

    - Both fromColumn and toColumn are NEEDED (never mark as unused) 

  

2.5 Calculated Columns: 

    - Identify via isCalculated flag in column list 

    - Get their expressions to find upstream column dependencies 

  

2.6 Roles (RLS), Perspectives, Calculation Groups: 

    - Query each; mark all referenced objects as NEEDED 

  

================================================================================ 

STEP 3: BUILD COMPLETE DEPENDENCY GRAPH 

================================================================================ 

  

For EVERY measure (model + report-level): 

- Parse DAX for 'Table'[Column] references (quoted) 

- Parse DAX for Table[Column] references (unquoted) 

- Parse DAX for [MeasureName] references 

- Detect USERELATIONSHIP/CROSSFILTER/TREATAS → mark referenced tables/columns 

  

For EVERY calculated table: 

- Parse partition expression DAX for all column references 

  

For EVERY calculated column: 

- Parse expression for upstream column dependencies 

  

Build: 

- Upstream map: object → what it depends on 

- Downstream map: object → what depends on it 

  

Propagate indirect usage: 

- If measure A is used in report, and measure A depends on measure B, 

  then measure B is NEEDED (indirect dependency) 

- If measure B references column X, then column X is NEEDED 

  

================================================================================ 

STEP 4: CLASSIFICATION RULES 

================================================================================ 

  

Mark object as Needed = YES if ANY of these is true: 

  

✓ Used in report visual (query projection) 

✓ Used in visual/page/report filter 

✓ Used in conditional formatting 

✓ Used in drillthrough 

✓ Used in tooltip 

✓ Used in field parameter projection 

✓ Used in synced slicer 

✓ Used in bookmark-targeted visual 

✓ Used in sort definition 

✓ Referenced by model measure DAX (direct or indirect) 

✓ Referenced by report-level measure DAX 

✓ Referenced by calculated table DAX expression 

✓ Referenced by calculated column expression 

✓ Is a sort-by-column target (sortByColumn property) 

✓ Is a relationship key column (from/to in any relationship) 

✓ Is inside a calculated table (column cannot be independently deleted) 

✓ Is inside a field parameter table 

✓ Used in RLS role definition 

✓ Used in calculation group 

✓ Used via USERELATIONSHIP/CROSSFILTER/TREATAS in DAX 

✓ Is part of a system table (LocalDateTable, DateTableTemplate) 

  

Mark as NOT NEEDED only if NONE of the above apply. 

  

Safe to Delete: 

- "Yes" only if Needed=No AND Confidence=High 

- "No" if Needed=Yes 

- "Review Required" if any uncertainty exists 

  

WHEN IN DOUBT → Mark as "Review Required", NOT as "Safe to Delete" 

  

================================================================================ 

STEP 5: OUTPUT (EXCEL ONLY — NO DELETIONS) 

================================================================================ 

  

Sheet 1: Object Inventory 

Columns: 

- Table Name 

- Object Name 

- Object Type (Table / Column / Measure / Calc Column / Calc Table /  

  Hierarchy / Report Measure) 

- Source (Model / Report) 

- Needed (Yes / No) 

- Safe to Delete (Yes / No / Review Required) 

- Confidence Level (High / Medium / Low) 

- Usage Type (full list of all usage categories found) 

- Where Used (page names, visual IDs, measure names) 

- Dependent On (upstream) 

- Used By (downstream) 

- Is Key Column (Yes / No) 

- Is Hidden (Yes / No) 

- Is Calc Table Column (Yes / No) 

- In Perspective (Yes / No) 

- In Role (Yes / No) 

- Sort By Column (target column name if applicable) 

- Recommendation (Keep / Review / Remove / Optimize) 

  

Sheet 2: Relationships 

- From Table.Column 

- To Table.Column 

- Cardinality 

- Cross Filter Direction 

- Is Active 

- Used in Report (Yes / No) 

- Used via DAX (Yes / No) 

- Recommendation 

  

Sheet 3: Dependency Tree 

- Object Name 

- Table 

- Source (Model / Report) 

- Depends On (Measures) 

- Depends On (Columns) 

- Used By 

- Used in Report (Yes / No) 

  

Sheet 4: Unused Objects Summary 

- Object Type 

- Total Count 

- Needed Count 

- Not Needed Count 

- Safe to Delete Count 

- Suggested Action 

  

Sheet 5: Model Optimization Suggestions 

- Redundant columns 

- Duplicate measures 

- BiDi / M:M relationship reviews 

- Auto date/time cleanup 

- Report measures to promote to model 

  

================================================================================ 

SAFETY RULES (NON-NEGOTIABLE) 

================================================================================ 

  

1. NEVER mark a column as unused without checking ALL 8 data sources listed above 

2. NEVER suggest deleting columns from calculated tables (they are structural) 

3. NEVER suggest deleting sort-by-column targets 

4. NEVER suggest deleting relationship key columns 

5. NEVER use cached/stale data — always query the live model 

6. NEVER assume a column is unused just because it's not in a visual — 

   check DAX, calculated tables, report measures, field parameters 

7. If a column appears in ANY calculated table expression (including 

   SELECTEDVALUE, NAMEOF, or any other function) → it is NEEDED 

8. ALL columns inside field parameter tables (Parameter, Volume Locks, 

   Parameter_P, etc.) are NEEDED — they are part of the table definition 

9. PRIORITIZE SAFETY OVER CLEANUP — a false "Keep" is acceptable, 

   a false "Delete" is not 

  

================================================================================ 

FINAL OUTPUT 

================================================================================ 

  

- Excel file (.xlsx) — ANALYSIS ONLY 

- Summary: 

  - Total objects in model 

  - Used vs unused percentage 

  - Safe cleanup candidates (High confidence only) 

  - Items requiring manual review 

- DO NOT execute any deletions 

- DO NOT modify the model in any way 

- Wait for my explicit approval before making any changes 

  

================================================================================ 

 

 

Output: 
semantic_model_audit.xlsx 
