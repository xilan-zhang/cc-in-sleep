---
name: research-lit
description: Search and analyze research papers, find related work, summarize key ideas. Use when user says "find papers", "related work", "literature review", "what does this paper say", or needs to understand academic papers.
argument-hint: "[paper-topic-or-url]"
allowed-tools: Bash(*), Read, WebSearch, WebFetch, Write, Agent
---

# Research Literature Review

Research topic: $ARGUMENTS

## Step 0: Pre-Sleep User Briefing (REQUIRED)

Before starting the search, ASK the user:

1. **Seed references**: "Are there specific authors, papers, or key citations I should start from?"
2. **Local papers**: "Do you have a folder with potentially relevant papers I should read first?" (e.g., a literature folder, Zotero collection, or papers on your Drive/Dropbox)
3. **Scope guidance**: "Any particular theoretical lens, debate, or conversation in the field I should focus on?"
4. **Target journal**: "Which journal are you aiming for? This affects what counts as the relevant conversation." (See Journal Heuristics below)

Wait for the user's response before proceeding. Use their pointers as starting anchors for the search — this dramatically improves relevance for OT/Strategy literature where citation networks matter more than keyword matching.

## Workflow

### Step 1: Keyword Search
- First check Zotero library (`/Users/xilan/Zotero/`) for papers already collected
- Generate 5-8 queries from the topic, mixing setting terms with generic outcomes (entry, exit, firm formation, performance, innovation, growth)
- Use Semantic Scholar API for structured search:
  ```bash
  curl -s "https://api.semanticscholar.org/graph/v1/paper/search?query=QUERY&limit=20&fields=title,authors,year,venue,abstract,citationCount" | python3 -m json.tool
  ```
- Use WebSearch for Google Scholar, SSRN
- Include both recent work AND foundational/classic papers (OT/Strategy values seminal contributions heavily)
- Check management journals: AMR, ASQ, SMJ, Organization Science, AMJ, MS

### Step 2: Author Neighborhood Search

**This is the most valuable step.** Papers found through author networks are often more relevant than keyword results.

Compile an author list from:
- Seed authors the user provided
- First authors of the most-cited Step 1 results
- Authors appearing in multiple Step 1 results

For each author, use WebSearch + WebFetch to browse:
- Faculty page — publications, research themes, co-author names
- Google Scholar profile — recent publications (last 5 years), highly cited work
- SSRN author page — working papers not yet indexed elsewhere

If the user named an advisor, search that advisor's publication list exhaustively. Reviewers will notice if the advisor's work is absent.

### Step 3: Forward Citation Search

For the most important seed/anchor papers, get forward citations via Semantic Scholar:
```bash
curl -s "https://api.semanticscholar.org/graph/v1/paper/PAPER_ID/citations?fields=title,authors,year,venue,abstract,citationCount&limit=100" | python3 -m json.tool
```
Scan for papers that extend, challenge, or build on the anchor work.

**Rate limits:** Run sequentially, one at a time. Wait 2 seconds between API calls.

### Step 4: Analyze Each Paper
For each relevant paper, extract:
- **Problem**: What gap does it address?
- **Method**: Core approach (1-2 sentences)
- **Results**: Key findings/claims
- **Relevance**: How does it relate to our work?

### Step 5: Synthesize
- Group papers into **candidate theoretical conversations** (name each in 3-5 words)
- Identify consensus vs disagreements in the field
- Find gaps that our work could fill
- Flag any **contribution threats** — papers that already make a similar claim

### Step 6: Output
Present as a structured literature table:

```
| Paper | Venue | Year | Conversation | Key Result | Relevance to Us |
|-------|-------|------|-------------|------------|-----------------|
```

Plus a narrative summary of the landscape (3-5 paragraphs), organized by conversation, not chronologically.

### Step 7: Save
- Save results to a literature report in the project directory
- Update related work notes in project memory

## Journal Heuristics

When assessing relevance and positioning, consider target journal preferences:
- **Management Science:** Clean identification, precise mechanism. Less concerned with broad theoretical contribution.
- **Strategic Management Journal:** Strategic implications must be clear. What should firms do differently?
- **Organization Science:** Most theory-forward. Loves boundary conditions on established mechanisms.
- **Academy of Management Journal:** Balances theory and evidence. Framed as "what we didn't know before."
- **Strategic Entrepreneurship Journal:** Must advance entrepreneurship theory specifically.

## Key Rules
- Always include paper citations (authors, year, venue)
- Distinguish between peer-reviewed and preprints
- Be honest about limitations of each paper
- Note if a paper directly competes with or supports our approach
- **Vocabulary matters**: Use vocabulary from found papers, not just your own terms. Adjacent fields may study the same mechanism with completely different words.
