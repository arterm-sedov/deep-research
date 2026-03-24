## ADDED Requirements

### Requirement: Agent-managed content extraction

The agent SHALL extract content from URLs using its native tool capabilities.

#### Scenario: WebFetch for URL content
- **WHEN** the agent needs content from a specific URL
- **THEN** the agent SHALL use WebFetch to retrieve and parse the content
- **AND** the agent SHALL extract relevant passages for synthesis

#### Scenario: SearXNG content extraction
- **WHEN** SearXNG is running and content extraction is needed
- **THEN** the agent MAY use searxng-extract for full page content
- **AND** the agent SHALL note content truncation if applicable

#### Scenario: JavaScript-rendered content
- **WHEN** content requires JavaScript rendering
- **THEN** the agent SHALL note the limitation
- **AND** the agent SHALL seek alternative sources or note incomplete data

### Requirement: No Python extraction dependencies

The skill SHALL not require Python libraries for content extraction.

#### Scenario: URL extraction without Python
- **WHEN** extracting content from URLs
- **THEN** the agent uses WebFetch (native) or searxng-cli (HTTP)
- **AND** no BeautifulSoup, requests, or similar Python packages needed