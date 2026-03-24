# Deep Research Skill

Enterprise-grade research skill for AI agents. Agent-orchestrated, Python-free, free-first providers.

## Installation

```bash
# Clone and copy to your skills directory
git clone https://github.com/YOUR_REPO/deep-research.git
cp -r deep-research/skills/deep-research ~/.agents/skills/
```

**No Python dependencies.** The agent orchestrates research by following methodology rules.

## What's Included

```
skills/deep-research/
├── SKILL.md              # Skill entry point
├── README.md             # Documentation
├── reference/
│   ├── methodology.md    # 8-phase pipeline
│   ├── providers.md      # Search providers (free-first)
│   ├── quality-gates.md  # Validation checklists
│   ├── report-assembly.md
│   ├── html-generation.md
│   ├── continuation.md
│   └── weasyprint_guidelines.md
└── templates/
    ├── report_template.md
    └── mckinsey_report_template.html
```

## Key Features

- **No Python dependencies** - Agent is the research engine
- **Free-first providers** - WebSearch → SearXNG → Exa MCP → search-cli (fallback)
- **8-phase pipeline** - SCOPE → PLAN → RETRIEVE → TRIANGULATE → SYNTHESIZE → CRITIQUE → REFINE → PACKAGE
- **Rate-limit resilient** - Pivots automatically when hitting provider limits
- **Quality gates** - Citation verification, structure validation, anti-hallucination protocols

## Provider Priority

| Priority | Provider | Cost | Setup |
|----------|----------|------|-------|
| 1 | WebSearch | FREE | Built-in |
| 2 | SearXNG | FREE | Docker (optional) |
| 3 | Exa MCP | FREE tier | MCP config (optional) |
| 4 | search-cli | PAID | API keys (fallback only) |

## Rate Limit Handling

When a provider hits rate limits, pivot immediately:

```
Tavily rate limit (429) → Switch to SearXNG
SearXNG unavailable    → Switch to WebSearch
All fail               → Report limitation, use cached sources
```

**Example:**
> I hit a rate limit with Tavily, so I'm pivoting to SearXNG and native WebSearch to maintain the momentum. I'll continue the deep-dive across the same five tracks to ensure I capture the full 200 unique links without interruption.

## Usage

```
deep research on the current state of quantum computing
```

```
deep research in ultradeep mode: compare PostgreSQL vs Supabase for our stack
```

## Research Modes

| Mode | Phases | Duration | Best For |
|------|--------|----------|----------|
| Quick | 3 | 2-5 min | Initial exploration |
| Standard | 6 | 5-10 min | Most research questions |
| Deep | 8 | 10-20 min | Complex topics, critical decisions |
| UltraDeep | 8+ | 20-45 min | Comprehensive reports, maximum rigor |

## Output

Reports saved to `~/Documents/[Topic]_Research_[YYYYMMDD]/`:
- Markdown (primary source of truth)
- HTML (McKinsey style, optional)

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 3.0 | 2026-03-24 | Removed Python dependencies, agent-orchestrated, free-first providers, rate-limit handling |
| 2.3.1 | 2026-03-19 | Template/validator harmonization, structured evidence, critique loop-back |
| 2.3 | 2026-03-19 | Search-cli integration, dynamic year detection, disk-persisted citations |
| 2.2 | 2025-11-05 | Auto-continuation system |
| 1.0 | 2025-11-04 | Initial release |

## License

MIT