---
name: monitor-experiment
description: Monitor running analyses or field experiment data collection status. Use when user says "check results", "is it done", "monitor", or wants to check on running scripts or data status.
argument-hint: "[analysis-script or data-source]"
---

# Monitor Experiment / Analysis Progress

Monitor: $ARGUMENTS

## Workflow

### Step 1: Check What's Running
Check for any running Python processes:
```bash
ps aux | grep python | grep -v grep
```

### Step 2: Check Output Files
Look for recent output in the project directory:
- Python output files (`.csv`, `.txt`, `.log`)
- LaTeX table files (`.tex`)
- Recently modified CSV/Excel files with results

### Step 3: Parse Latest Results
If results files exist:
- Read the most recent output
- Extract key statistics (coefficients, standard errors, p-values, N)
- Compare against prior results if available

### Step 4: Summarize Status
Present results in a comparison table:
```
| Specification | Coeff  | SE    | p-value | N    | Status |
|--------------|--------|-------|---------|------|--------|
| Main effect  | X.XX   | X.XX  | X.XXX   | XXXX | done   |
| Robustness 1 | X.XX   | X.XX  | X.XXX   | XXXX | done   |
```

### Step 5: Interpret
- Compare against hypothesized effects
- Flag unexpected results (sign flips, loss of significance)
- Note if any scripts are still running
- Suggest next steps based on findings

## Key Rules
- Always show raw numbers before interpretation
- Report standard errors and significance levels
- Note sample sizes and any data quality issues
- If results look wrong, check logs for errors before concluding
