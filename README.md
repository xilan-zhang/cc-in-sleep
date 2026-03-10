# CC-in-Sleep: Autonomous Overnight Research with Claude Code

Claude Code skills for autonomous overnight research workflows — review, literature search, analysis — adapted for **organization theory / strategy** (ASQ, AMR, SMJ, Organization Science, AMJ).

Forked from [wanshuiyin/Auto-claude-code-research-in-sleep](https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep).

## What's Different

Original targets ML/CS (NeurIPS/ICML, GPU experiments). This fork is for social science:

- **Reviewer**: ASQ/AMR/SMJ/OrgSci senior reviewer persona
- **Fixes**: Theory refinement, Python robustness checks, literature integration (not GPU experiments)
- **Tables**: AER/ASQ/SMJ standard format
- **Lit search**: Author neighborhoods + forward citations + Semantic Scholar API (not just keywords)
- **Theory**: Tension types taxonomy, contribution threat checks, outcome scenario analysis
- **Style**: Anti-AI-writing constraints

## Setup

```bash
# Install
git clone https://github.com/xilan-zhang/cc-in-sleep.git
cp -r cc-in-sleep/* ~/.claude/skills/cc-in-sleep/

# Codex MCP (needed for review skills)
npm install -g @openai/codex
claude mcp add codex -s user -- codex mcp-server
```

For overnight runs without permission prompts, add to `~/.claude/settings.json`:
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

## Skills

| Skill | What it does | Overnight? |
|-------|-------------|------------|
| `/auto-review-loop` | Review → fix → re-review, up to 4 rounds | Yes |
| `/research-review` | Interactive deep review via Codex MCP | No (you guide it) |
| `/research-lit` | Literature search with author neighborhoods + forward citations | Yes (after pre-sleep briefing) |
| `/analyze-results` | Run analyses, output AER/ASQ/SMJ tables | Yes |
| `/monitor-experiment` | Check running Python scripts | Yes |

### `/auto-review-loop` — the main overnight skill

```
/auto-review-loop SMJ R&R revision
```

Sends your paper to an external LLM reviewer (ASQ/SMJ level), gets a score + weaknesses, then autonomously implements fixes: theory reframing, contribution threat checks, Python robustness analyses, literature integration. Loops until score >= 6 or 4 rounds. Produces `AUTO_REVIEW.md` with round-by-round log and outcome scenario analysis.

### `/research-lit` — overnight literature mapping

```
/research-lit institutional environments and entrepreneurial entry
```

Asks for seed authors/papers before you sleep, then runs keyword search, author neighborhood search (faculty pages, Google Scholar, SSRN), and forward citations. Groups papers into theoretical conversations. Flags contribution threats.

### `/analyze-results` — field experiment analysis

```
/analyze-results treatment effects in cohort 2 data
```

Outputs AER/ASQ/SMJ standard regression tables (coefficients, SEs in parens, significance stars, controls/FE/N/R-squared rows).

## Journal Heuristics

Reviews are tailored to target journal:
- **ASQ**: Most theory-forward. Loves boundary conditions on established mechanisms.
- **AMR**: Pure theory. Mechanism must be novel and well-specified.
- **SMJ**: Strategic implications must be clear. What should firms do differently?
- **Organization Science**: Theory-forward. Boundary conditions and mechanism precision.
- **AMJ**: Balances theory and evidence. "What we didn't know before."
- **Management Science**: Clean identification, precise mechanism.
- **SEJ**: Must advance entrepreneurship theory specifically.

## License

MIT
