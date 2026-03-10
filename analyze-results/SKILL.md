---
name: analyze-results
description: Analyze field experiment or empirical results, compute statistics, generate comparison tables and insights. Use when user says "analyze results", "compare", or needs to interpret experimental data.
argument-hint: "[results-path-or-description]"
---

# Analyze Experiment Results

Analyze: $ARGUMENTS

## Workflow

### Step 1: Locate Results
Find all relevant data files:
- Check project directories for CSV, .dta, or output files
- Parse data into structured format using Python (pandas, statsmodels, linearmodels)

### Step 2: Build Comparison Table
Organize results by:
- **Independent variables**: treatment conditions, covariates, subgroups
- **Dependent variables**: primary outcomes, secondary outcomes, mechanism variables
- **Treatment effects**: ATE, heterogeneous effects, with standard errors and p-values

### Step 3: Statistical Analysis
- Report coefficients with robust/clustered standard errors
- Check balance tables across treatment arms
- Run robustness checks: alternative specifications, subgroup analyses, placebo tests
- Flag attrition, non-compliance, or multiple testing concerns

### Step 4: Generate Insights
For each finding, structure as:
1. **Observation**: what the data shows (with numbers)
2. **Interpretation**: why this might be happening
3. **Implication**: what this means for the research question
4. **Next step**: what experiment would test the interpretation

### Step 5: Update Documentation
If findings are significant:
- Propose updates to project notes or experiment reports
- Draft a concise finding statement (1-2 sentences)

## Output Format

### Table Format (AER/SMJ standard)
Follow the standard regression table format used in AER and SMJ:
- Dependent variable name in column header
- Each column = one specification
- Coefficients with standard errors in parentheses below
- Significance stars: * p<0.10, ** p<0.05, *** p<0.01
- Bottom rows: Controls (Yes/No), Fixed Effects, N, R²
- Use Python (`stargazer` or manual formatting) to generate LaTeX tables

Example:
```
                    (1)         (2)         (3)
                  Revenue    Revenue    Revenue
Treatment          0.234**     0.198*     0.215**
                  (0.098)    (0.102)    (0.099)
Controls             No        Yes        Yes
District FE          No         No        Yes
N                   1,200      1,180      1,180
R²                 0.045      0.112      0.134
```

### Summary
Always include:
1. Formatted regression table (LaTeX-ready)
2. Key findings (numbered, concise)
3. Suggested next analyses (if any)
