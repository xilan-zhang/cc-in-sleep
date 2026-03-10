# CC-in-Sleep: Autonomous Overnight Research with Claude Code

Claude Code skills for running autonomous research workflows overnight — review papers, search literature, run analyses — while you sleep.

Forked from [wanshuiyin/Auto-claude-code-research-in-sleep](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep) and adapted for **organization theory / strategy scholarship** (ASQ, AMR, SMJ, Organization Science).

## What's Different from the Original

The original repo targets ML/CS researchers running GPU experiments for NeurIPS/ICML. This fork is rebuilt for social science:

| | Original | This Fork |
|---|---|---|
| Reviewer persona | NeurIPS/ICML ML reviewer | ASQ/AMR/SMJ/OrgSci reviewer |
| Fix actions | GPU experiments, model code | Theory refinement, Python robustness checks, literature integration |
| Analysis tools | PyTorch, perplexity/accuracy | pandas, statsmodels, linearmodels |
| Table format | Ad hoc | AER/SMJ standard (coefficients, SEs in parens, significance stars) |
| Literature search | Keyword only, last 2 years | Author neighborhoods, forward citations, Semantic Scholar API, foundational works included |
| Theory framework | None | Tension types taxonomy, contribution threat checks, outcome scenario analysis |
| Writing style | None | Anti-AI-writing constraints for academic tone |

## Prerequisites

1. [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
2. [Codex CLI](https://github.com/openai/codex) with MCP server (needed for `research-review` and `auto-review-loop`):
   ```bash
   npm install -g @openai/codex
   claude mcp add codex -s user -- codex mcp-server
   ```

## Installation

```bash
git clone https://github.com/xilan-zhang/cc-in-sleep.git
cp -r cc-in-sleep/* ~/.claude/skills/cc-in-sleep/
```

## Skills

### `/research-review` — Interactive Deep Review
Get a multi-round critical review from an external LLM via Codex MCP. You stay awake and guide the conversation.

```
/research-review my paper on network formation in refugee entrepreneurship
```

**What it does:**
- Compiles your paper's context (claims, methods, results, weaknesses)
- Sends to external LLM with `xhigh` reasoning as a senior OT/Strategy reviewer
- You iterate with follow-ups: reframing, mock reviews, claims matrices

### `/auto-review-loop` — Autonomous Review + Fix (Overnight)
The main overnight skill. Autonomously loops up to 4 rounds: review → implement fixes → re-review.

```
/auto-review-loop SMJ R&R revision
```

**What it does:**
- Round 1: External LLM reviews the paper, scores 1-10, lists weaknesses
- If score < 6: implements fixes (theory reframing, robustness checks in Python, literature additions)
- Checks for contribution threats before rewriting
- Re-submits to reviewer, repeats until score >= 6 or 4 rounds done
- Produces `AUTO_REVIEW.md` with full round-by-round log
- Ends with outcome scenario analysis (what if results don't pan out?)

**Stop condition:** Score >= 6 AND verdict contains "ready" or "almost"

### `/research-lit` — Literature Search + Gap Analysis
Autonomous literature mapping with a pre-sleep briefing step.

```
/research-lit institutional environments and entrepreneurial entry
```

**What it does:**
1. **Asks you first** (before you sleep): seed authors? local paper folders? target journal? scope?
2. Keyword search via Semantic Scholar API + Google Scholar
3. Author neighborhood search (faculty pages, Google Scholar profiles, SSRN)
4. Forward citation search from anchor papers
5. Groups papers into theoretical conversations, flags contribution threats
6. Outputs structured literature table + narrative summary

### `/analyze-results` — Field Experiment Analysis
Run statistical analysis on existing data with AER/SMJ formatted output.

```
/analyze-results treatment effects in cohort 2 data
```

**Output format:**
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

### `/monitor-experiment` — Check Running Analyses
Check on Python scripts running in the background.

```
/monitor-experiment
```

## Overnight Setup

For fully autonomous runs without permission prompts, add to `~/.claude/settings.json`:

```json
{
  "permissions": {
    "allow": [
      "mcp__codex__codex",
      "mcp__codex__codex-reply",
      "Skill(auto-review-loop)",
      "Skill(research-review)",
      "Skill(research-lit)",
      "Skill(analyze-results)",
      "Skill(monitor-experiment)"
    ]
  }
}
```

## Customization

### Target Journal
The skills include journal-specific heuristics for ManSci, SMJ, OrgSci, AMJ, and SEJ. The reviewer persona and feedback are tailored accordingly.

### Theoretical Framework
Reviews use a tension types taxonomy to diagnose contribution:
- Scope condition violation
- Mechanism substitution
- Overlooked heterogeneity
- Level-of-analysis gap
- Boundary condition on established mechanism

### Writing Style
All outputs follow anti-AI-writing constraints (see `references/style.md`): no em dashes, no "Crucially/Importantly," analytical working-document tone.

## Typical Overnight Workflow

1. Navigate to your project directory
2. Run the skill:
   ```
   /auto-review-loop SMJ R&R paper
   ```
3. Answer any pre-sleep questions (seed references, scope)
4. Sleep
5. Wake up to `AUTO_REVIEW.md` with what happened overnight

## License

MIT
