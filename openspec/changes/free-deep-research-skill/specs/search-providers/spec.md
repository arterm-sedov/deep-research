## ADDED Requirements

### Requirement: Provider priority for research

The agent SHALL use search providers in priority order: free first, paid last.

#### Scenario: WebSearch always available
- **WHEN** the agent starts a research task
- **THEN** the agent SHALL use WebSearch as the primary search tool
- **AND** WebSearch requires no configuration or setup

#### Scenario: SearXNG local search available
- **WHEN** SearXNG Docker container is running at localhost:8000
- **THEN** the agent MAY use searxng-search for broader search coverage
- **AND** the agent SHALL check SearXNG health before suggesting it

#### Scenario: Exa MCP configured
- **WHEN** Exa MCP is configured in the agent environment
- **THEN** the agent MAY use mcp__Exa__exa_search for semantic/neural search
- **AND** the agent SHALL fall back to WebSearch if Exa is unavailable

#### Scenario: No paid services required
- **WHEN** the user has no API keys configured
- **THEN** deep research SHALL still function using WebSearch + optional SearXNG
- **AND** no error or warning about missing paid services

### Requirement: Agent-orchestrated research pipeline

The skill SHALL rely on the agent to orchestrate all phases without external scripts.

#### Scenario: Phase execution
- **WHEN** the agent invokes deep-research
- **THEN** the agent SHALL read SKILL.md and reference/methodology.md
- **AND** the agent SHALL execute each phase following the documented instructions
- **AND** the agent SHALL manage state in its own context

#### Scenario: Source evaluation
- **WHEN** evaluating source credibility
- **THEN** the agent SHALL use LLM reasoning to score sources
- **AND** the agent SHALL consider: domain expertise, recency, bias, corroboration
- **AND** the agent SHALL output credibility scores as part of synthesis

#### Scenario: Citation tracking
- **WHEN** gathering sources
- **THEN** the agent SHALL track citations in structured format
- **AND** the agent SHALL maintain source provenance throughout context
- **AND** the agent SHALL output complete bibliography

### Requirement: No Python dependencies

The skill SHALL operate without any Python runtime or packages.

#### Scenario: Fresh installation
- **WHEN** a user installs the skill
- **THEN** no pip install or requirements.txt execution is required
- **AND** no Python packages are downloaded

#### Scenario: Research execution
- **WHEN** the agent executes research
- **THEN** no Python subprocess calls are made
- **AND** all orchestration happens via agent following rules