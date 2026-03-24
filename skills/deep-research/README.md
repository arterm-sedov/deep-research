# Deep Research Skill forClaude Code

Enterprise-grade research engine for Claude Code. Produces citation-backed reports using the agent's native capabilities. **No Python dependencies required.**

## Installation

```bash
# Clone into Claude Code skills directory
git clone https://github.com/199-biotechnologies/claude-deep-research-skill.git ~/.agents/skills/deep-research
```

**That's it.** No pip install, no dependencies. The agent is the research engine.

### Optional Enhancements

**SearXNG (FREE local search):**
```bash
# Docker-based local search engine (Tavily-compatible API)
git clone https://github.com/vakovalskii/searxng-docker-tavily-adapter
cd searxng-docker-tavily-adapter
cp config.example.yaml config.yaml
python -c "import secrets; print(secrets.token_urlsafe(32))"  # Add to config.yaml
docker compose up -d

# Check health
curl -s http://localhost:8000/health
```

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

## Pipeline

Scope → Plan → **Retrieve** (parallel search + agents) → Triangulate → Outline Refinement → Synthesize → Critique (with loop-back) → Refine → Package

Key features:
- **Step 0**: Retrieves current date before searches (prevents stale year assumptions)
- **Parallel retrieval**: 5-10 concurrent searches + 2-3 focused subagents returning structured evidence
- **First Finish Search**: Adaptive quality thresholds by mode
- **Critique loop-back**: Phase 6 can return to Phase 3 with delta-queries if critical gaps found
- **Multi-persona red teaming**: Skeptical Practitioner, Adversarial Reviewer, Implementation Engineer (Deep/UltraDeep)
- **Agent-orchestrated**: All coordination by agent following methodology rules

## Output

Reports saved to `~/Documents/[Topic]_Research_[Date]/`:
- Markdown (primary source of truth)
- HTML (McKinsey-style, optional)

Reports >18K words auto-continue via recursive agent spawning with context preservation.

## Quality Standards

- 10+ sources, 3+ per major claim
- Executive summary 200-400 words
- Findings 600-2,000 words each, prose-first (>=80%)
- Full bibliography with URLs, no placeholders
- Validation checklists in quality-gates.md
- Validation loop: validate → fix → retry (max 3 cycles)

## Search Providers

**FREE-FIRST priority:**

| Priority | Provider | Cost | Setup |
|----------|----------|------|-------|
| 1 | WebSearch | FREE | Built-in |
| 2 | SearXNG | FREE | Docker (optional) |
| 3 | Exa MCP | FREE tier | MCP config (optional) |
| 4 | search-cli | PAID | API keys (fallback only) |

**Always use free options first.** The skill works without any paid services.

See [providers.md](./reference/providers.md) for details.

## Architecture

```
deep-research/
├── SKILL.md                           # Skill entry point
├── reference/
│   ├── methodology.md                 # 8-phase pipeline details
│   ├── providers.md                   # Search provider options
│   ├── quality-gates.md               # Validation checklists
│   ├── report-assembly.md             # Progressive generation
│   ├── html-generation.md             # HTML conversion
│   └── continuation.md                 # Auto-continuation
├── templates/
│   ├── report_template.md             # Report structure
│   └── mckinsey_report_template.html   # HTML styling
├── requirements.txt                   # (empty - no dependencies)
└── README.md
```

**No Python required.** The agent orchestrates research by following methodology.md rules.

## How It Works

1. **Agent reads SKILL.md** on invocation
2. **Agent loads methodology.md** for phase instructions
3. **Agent uses WebSearch** (or SearXNG/Exa if available) for research
4. **Agent follows quality-gates.md** checklists for validation
5. **Agent generates output** following templates

All coordination happens through agent reasoning, not Python scripts.

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 3.0 | 2026-03-24 | Removed Python dependencies, agent-orchestrated, free-first providers |
| 2.3.1 | 2026-03-19 | Template/validator harmonization, structured evidence, critique loop-back |
| 2.3 | 2026-03-19 | Contract harmonization, search-cli integration, dynamic year detection |
| 2.2 | 2025-11-05 | Auto-continuation system for unlimited length |
| 2.1 | 2025-11-05 | Progressive file assembly |
| 1.0 | 2025-11-04 | Initial release |

## License

MIT - modify as needed for your workflow.