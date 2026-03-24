# Deep Research Skill

Enterprise-grade research skill for AI agents. Agent-orchestrated, Python-free, free-first providers.

## Why This Skill Exists

Research is one of the most common tasks AI agents perform, yet existing solutions often rely on Python dependencies, paid APIs, or rigid pipelines that break when rate limits hit. This skill exists to solve those problems:

- **No dependencies to install** — The agent is the research engine. No Python, no pip, no environment setup.
- **Free-first providers** — Uses built-in web search first, falls back to free options before paid ones.
- **Resilient to failures** — When a provider hits rate limits, it pivots automatically instead of failing.
- **Transparent methodology** — You see the 8-phase pipeline: SCOPE → PLAN → RETRIEVE → TRIANGULATE → SYNTHESIZE → CRITIQUE → REFINE → PACKAGE.

## Repository

- **GitHub**: [https://github.com/anomalyco/deep-research](https://github.com/anomalyco/deep-research)
- **Skill README**: [skills/deep-research/README.md](skills/deep-research/README.md)

## Installation

```bash
# Clone and copy to your skills directory
git clone https://github.com/arterm-sedov/deep-research.git
cp -r deep-research/skills/deep-research ~/.agents/skills/
```

Or use your AI assistant's skill command:

```
npx skills add deep-research
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
    └── mckinsey_report_template.md
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

## Inspired By

This skill was inspired by [browser-switch](https://github.com/arterm-sedov/browser-switch) — a skill that helps AI agents choose between different browser automation tools. The clear, concise "Why This Skill Exists" format in this README follows that pattern.

## License

MIT