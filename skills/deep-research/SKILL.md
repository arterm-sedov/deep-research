---
name: deep-research
description: |
  Conducts enterprise-grade research with multi-source synthesis, citation tracking, and verification. Produces citation-backed reports through a structured pipeline. Uses FREE providers by default (WebSearch, SearXNG). Triggers on "deep research", "comprehensive analysis", "research report", "compare X vs Y", "analyze trends", or "state of the art". Not for simple lookups, debugging, or questions answerable with 1-2 searches.
---

# Deep Research

Agent-orchestrated research pipeline. No Python dependencies required.

## Core Purpose

Deliver citation-backed, verified research reports through a structured 8-phase pipeline using the agent's native capabilities.

**Key Principle:** The agent IS the research engine. Follow methodology rules, use LLM reasoning for evaluation, and leverage built-in tools for search and extraction.

---

## Provider Priority

**FREE-FIRST approach:**

| Priority | Provider | Cost | Availability |
|----------|----------|------|--------------|
| 1 | WebSearch | FREE | Always (built-in) |
| 2 | SearXNG | FREE | Docker (optional) |
| 3 | Exa MCP | FREE tier | MCP config (optional) |
| 4 | search-cli | PAID | API keys (fallback only) |

**Always use free options first.** See [providers.md](./reference/providers.md) for details.

**Detection:**
1. WebSearch: Always available
2. SearXNG: `curl -s http://localhost:8000/health`
3. Exa MCP: Check for `mcp__Exa__exa_search` tool

---

## Decision Tree

```
Request Analysis
+-- Simple lookup? --> STOP: Use WebSearch
+-- Debugging? --> STOP: Use standard tools
+-- Complex analysis needed? --> CONTINUE

Mode Selection
+-- Initial exploration --> quick (3 phases, 2-5 min)
+-- Standard research --> standard (6 phases, 5-10 min) [DEFAULT]
+-- Critical decision --> deep (8 phases, 10-20 min)
+-- Comprehensive review --> ultradeep (8+ phases, 20-45 min)
```

**Default assumptions:** Technical query = technical audience. Comparison = balanced perspective. Trend = recent 1-2 years.

---

## Workflow Overview

| Phase | Name | Quick | Standard | Deep | UltraDeep |
|-------|------|-------|----------|------|-----------|
| 1 | SCOPE | Y | Y | Y | Y |
| 2 | PLAN | - | Y | Y | Y |
| 3 | RETRIEVE | Y | Y | Y | Y |
| 4 | TRIANGULATE | - | Y | Y | Y |
| 4.5 | OUTLINE REFINEMENT | - | Y | Y | Y |
| 5 | SYNTHESIZE | - | Y | Y | Y |
| 6 | CRITIQUE | - | - | Y | Y |
| 7 | REFINE | - | - | Y | Y |
| 8 | PACKAGE | Y | Y | Y | Y |

---

## Execution

**On invocation, load ALL reference files:**

1. **[methodology.md](./reference/methodology.md)** - Complete 8-phase pipeline instructions
2. **[providers.md](./reference/providers.md)** - Search provider options and detection
3. **[browser-scraping.md](./reference/browser-scraping.md)** - Browser fallback extraction
4. **[quality-gates.md](./reference/quality-gates.md)** - Validation checklists
5. **[report-assembly.md](./reference/report-assembly.md)** - Report generation strategy
6. **[html-generation.md](./reference/html-generation.md)** - HTML report styling
7. **[continuation.md](./reference/continuation.md)** - Long report handling

**Templates:**
- [report_template.md](./templates/report_template.md) - Report structure
- [mckinsey_report_template.html](./templates/mckinsey_report_template.html) - HTML styling

**No Python Scripts:** All orchestration happens through agent following methodology rules.

---

## Output Contract

**Required sections:**
- Executive Summary (200-400 words)
- Introduction (scope, methodology, assumptions)
- Main Analysis (4-8 findings, 600-2,000 words each, cited)
- Synthesis & Insights (patterns, implications)
- Limitations & Caveats
- Recommendations
- Bibliography (COMPLETE - every citation, no placeholders)
- Methodology Appendix

**Output files (all to `~/Documents/[Topic]_Research_[YYYYMMDD]/`):**
- Markdown (primary source of truth)
- HTML (McKinsey style, optional)

**Quality standards:**
- 10+ sources, 3+ per major claim
- All claims cited immediately [N]
- No placeholders, no fabricated citations
- Prose-first (>=80%), bullets sparingly

---

## Invocation Triggers

Use this skill when the user says:
- "deep research on [topic]"
- "comprehensive analysis of [topic]"
- "research report on [topic]"
- "compare [X] vs [Y]"
- "analyze trends in [topic]"
- "state of the art in [topic]"
- "investigate [topic] thoroughly"
- "what does the literature say about [topic]"

**Do NOT trigger for:**
- Simple lookups (use WebSearch directly)
- Debugging (use standard tools)
- Questions answerable with 1-2 searches
- Quick time-sensitive queries

---

## Agent Responsibilities

The agent (not scripts) handles:
- **Orchestration:** Execute phases by reading methodology.md
- **Source evaluation:** Use LLM reasoning to score credibility
- **Citation tracking:** Maintain source provenance in context
- **Validation:** Check against quality-gates.md checklists
- **Report generation:** Create output following templates

---

## No Dependencies

This skill requires:
- No Python runtime
- No pip install
- No external dependencies

**All orchestration is handled by the agent following the methodology rules.**