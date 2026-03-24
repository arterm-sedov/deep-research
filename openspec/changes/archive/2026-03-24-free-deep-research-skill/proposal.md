## Why

The existing `deep-research` skill contains Python orchestration scripts that create unnecessary dependencies and don't leverage the agent's native capabilities. The agent **IS** the research engine - it can orchestrate, evaluate, track citations, and validate reports using its LLM reasoning and built-in tools.

**Problems with current architecture:**

1. **Python scripts duplicate agent capabilities**:
   - `research_engine.py` - Agent naturally orchestrates itself following SKILL.md
   - `source_evaluator.py` - LLM evaluates credibility better than scripts
   - `citation_manager.py` - Agent tracks context naturally
   - `validate_report.py` - Rules in quality-gates.md suffice
   - `verify_citations.py` - Agent can verify with WebFetch
   - `md_to_html.py` - Agent can generate HTML directly

2. **`search-cli` in requirements.txt** - Paid multi-provider aggregator, should be optional

3. **No SearXNG support** - Free Tavily-compatible local search not listed

4. **No clear provider priority** - Agents need guidance on free-first approach

**Solution:**

Transform the skill from "Python-orchestrated" to "Rule-guided agent execution".
The agent follows well-designed rules in SKILL.md and reference docs, using its
native capabilities for everything the Python scripts used to do.

## What Changes

- **Delete Python scripts entirely** - Agent is the engine
- **Enhance SKILL.md** - Include clear phase-by-phase instructions
- **Update reference docs** - Add provider priority, improve methodology
- **Simplify requirements.txt** - Remove all Python dependencies
- **Add SearXNG support** - Free local search alternative
- **Establish free-first provider priority** - WebSearch → SearXNG → Exa MCP

## Capabilities

### Modified Capabilities

- `deep-research`: Transform from Python-orchestrated to rule-guided
  - Remove scripts/ directory entirely
  - Enhance SKILL.md with embedded phase instructions
  - Update reference docs to be comprehensive guides

### New Documentation

- `reference/providers.md`: Provider detection and priority (free-first)
- Enhanced `reference/methodology.md`: Complete phase instructions

## Architecture Change

```
BEFORE (Python-orchestrated):
deep-research/
├── SKILL.md (thin, points to scripts)
├── reference/ (methodology docs)
├── templates/
├── scripts/        ← DELETED
│   ├── research_engine.py
│   ├── citation_manager.py
│   ├── source_evaluator.py
│   ├── validate_report.py
│   ├── verify_citations.py
│   └── md_to_html.py
├── tests/          ← DELETED
├── requirements.txt (lists search-cli)
└── README.md

AFTER (Rule-guided agent):
deep-research/
├── SKILL.md (rich, includes phase instructions)
├── reference/
│   ├── methodology.md (enhanced)
│   ├── providers.md    (NEW)
│   ├── report-assembly.md
│   ├── quality-gates.md
│   ├── html-generation.md
│   └── continuation.md
├── templates/
│   ├── report_template.md
│   └── mckinsey_report_template.html
├── requirements.txt (EMPTY or deleted)
└── README.md
```

## Impact

- **Simplified architecture** - No Python, no dependencies
- **Better agent utilization** - Uses LLM for evaluation, reasoning
- **Free-first** - No paid services required by default
- **Smaller footprint** - ~10KB skill vs ~50KB with scripts
- **No breaking changes** - Same phases, same outputs
- **Agent-native** - Leverages context management, parallel tool calls