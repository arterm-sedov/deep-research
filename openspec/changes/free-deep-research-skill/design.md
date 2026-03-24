## Context

The current deep-research skill uses Python scripts to orchestrate an 8-phase research pipeline. However, this architecture doesn't leverage the agent's native capabilities. The agent IS the research engine - it can:
- Orchestrate itself following SKILL.md instructions
- Evaluate source credibility using LLM reasoning
- Track citations in its context
- Validate reports against quality gates
- Verify citations using WebFetch/WebSearch
- Generate HTML directly from templates

The Python scripts are essentially **duplicating what the agent already does better**.

**Current State (Python-orchestrated):**
```
research_engine.py calls → Agent executes phases
         ↓
Python manages state files, phase instructions, output paths
```

**Proposed State (Rule-guided agent):**
```
SKILL.md + reference/ → Agent executes following rules
         ↓
Agent manages own context, memory, outputs
```

**Constraints:**
- Must maintain same 8-phase pipeline quality
- Must produce same output formats (Markdown, HTML, PDF)
- Must use agent's native tools (WebSearch, WebFetch, Task)
- Must work without any Python dependencies

## Goals / Non-Goals

**Goals:**
- Delete Python scripts - agent manages itself
- Consolidate all phase instructions into reference/methodology.md
- Add free provider priority (WebSearch → SearXNG → Exa MCP)
- Simplify skill to pure markdown documentation
- Leverage agent's LLM for evaluation and reasoning
- Maintain or improve research quality

**Non-Goals:**
- Not changing the research methodology (same 8 phases)
- Not changing output formats (same templates)
- Not adding new features beyond provider support
- Not maintaining backward compatibility with Python invocation

## Decisions

### Decision 1: Delete scripts/ directory entirely

**Choice**: Remove all Python orchestration code

**Rationale**:
- research_engine.py: Agent follows SKILL.md naturally
- source_evaluator.py: LLM evaluates credibility better
- citation_manager.py: Agent tracks in context
- validate_report.py: quality-gates.md rules suffice
- verify_citations.py: Agent uses WebFetch to verify
- md_to_html.py: Agent generates HTML from templates

**Alternatives Considered**:
- Keep scripts as helpers: Unnecessary complexity
- Rewrite in another language: Still misses the point

### Decision 2: Embed phase instructions in methodology.md

**Choice**: Comprehensive methodology.md with all phase logic

**Rationale**:
- Single source of truth for research methodology
- Agent reads one comprehensive document
- Easier to maintain than scattered scripts
- methodology.md already contains 90% of the logic

**Alternatives Considered**:
- Split into per-phase files: More files to read
- Put everything in SKILL.md: Too long for single file

### Decision 3: Free-first provider priority

**Choice**: WebSearch (always) → SearXNG (if local) → Exa MCP (if configured) → search-cli (only if insisting)

**Rationale**:
- WebSearch: Built into agent, always available, free
- SearXNG: Free local Docker, Tavily-compatible API
- Exa MCP: Free tier available, semantic search
- search-cli: Requires paid API keys, should be last resort

**Alternatives Considered**:
- Auto-detect first available: Simpler but may pick paid first
- Require configuration: More setup burden

### Decision 4: Remove requirements.txt

**Choice**: Delete requirements.txt entirely

**Rationale**:
- No Python dependencies means no requirements file
- Simplicity - pure markdown skill
- Agent uses built-in tools, not Python packages

**Alternatives Considered**:
- Keep empty file: Unnecessary

### Decision 5: Agent uses Task tool for parallel retrieval

**Choice**: Explicitly specify using Task tool with subagents for Phase 3

**Rationale**:
- methodology.md mentions Task tool but not prominently
- Parallel retrieval is key to efficiency
- Subagents return structured evidence objects
- Agent's native capability, no Python needed

## Risks / Trade-offs

| Risk | Mitigation |
|------|------------|
| Quality variation without scripts | methodology.md provides explicit quality gates, LLM checks |
| Agent context limits for long reports | continuation.md already handles >18K word reports |
| SearXNG not running | Graceful fallback to WebSearch, documented in providers.md |
| Users expect Python | README clearly documents new architecture, migration guide |

## Migration Path

1. **Phase 1: Documentation consolidation**
   - Enhance methodology.md with complete phase logic
   - Create providers.md with detection logic
   - Update SKILL.md to be self-contained

2. **Phase 2: Template simplification**
   - Keep templates (report_template.md, mckinsey_report_template.html)
   - Agent reads templates and generates output

3. **Phase 3: Python removal**
   - Delete scripts/ directory
   - Delete tests/ directory
   - Delete requirements.txt
   - Update README.md

## Open Questions

1. Should we keep PDF output or simplify to Markdown + HTML only? (WeasyPrint adds complexity)
2. Should validation be a separate quality-gates checklist or embedded in methodology?
3. Should we create a diagnostic command to check available providers?