## ADDED Requirements

### Requirement: LLM-powered research synthesis

The agent SHALL synthesize findings using its LLM capabilities without external scripts.

#### Scenario: Pattern identification
- **WHEN** synthesizing research findings
- **THEN** the agent SHALL identify patterns across sources using LLM reasoning
- **AND** the agent SHALL document patterns in the synthesis section

#### Scenario: Novel insight generation
- **WHEN** connecting research findings
- **THEN** the agent SHALL generate insights beyond source material
- **AND** the agent SHALL use extended thinking for non-obvious connections

#### Scenario: Citation generation
- **WHEN** producing final report
- **THEN** the agent SHALL include complete bibliography
- **AND** the agent SHALL use consistent citation format throughout
- **AND** the agent SHALL verify each citation references a real source

### Requirement: Agent-orchestrated pipeline phases

The agent SHALL execute all8 phases following documented rules.

#### Scenario: SCOPE phase
- **WHEN** starting research
- **THEN** the agent SHALL decompose the question
- **AND** define scope boundaries
- **AND** establish success criteria

#### Scenario: PLAN phase
- **WHEN** planning research approach
- **THEN** the agent SHALL identify source types
- **AND** create search query strategy
- **AND** define quality gates

#### Scenario: RETRIEVE phase
- **WHEN** gathering information
- **THEN** the agent SHALL launch parallel searches using WebSearch
- **AND** spawn subagents for deep dives using Task tool
- **AND** collect sources with metadata

#### Scenario: TRIANGULATE phase
- **WHEN** verifying information
- **THEN** the agent SHALL cross-reference across 3+ sources
- **AND** flag contradictions
- **AND** assess source credibility

#### Scenario: SYNTHESIZE phase
- **WHEN** connecting insights
- **THEN** the agent SHALL identify patterns
- **AND** generate novel insights
- **AND** build argument structures

#### Scenario: CRITIQUE phase
- **WHEN** quality checking
- **THEN** the agent SHALL review for logical consistency
- **AND** check citation completeness
- **AND** identify gaps

#### Scenario: REFINE phase
- **WHEN** improving quality
- **THEN** the agent SHALL address gaps
- **AND** strengthen weak arguments
- **AND** verify revised content

#### Scenario: PACKAGE phase
- **WHEN** delivering report
- **THEN** the agent SHALL structure with clear hierarchy
- **AND** write executive summary
- **AND** compile bibliography
- **AND** output in Markdown format (optionally HTML)

### Requirement: Quality validation without Python

The agent SHALL validate report quality using documented checklists.

#### Scenario: Structure validation
- **WHEN** packaging report
- **THEN** the agent SHALL verify required sections exist
- **AND** the agent SHALL check section lengths meet standards

#### Scenario: Citation validation
- **WHEN** finalizing bibliography
- **THEN** the agent SHALL verify each citation has URL
- **AND** the agent SHALL check no placeholders exist