## ADDED Requirements

### Requirement: Browser fallback extraction

The skill SHALL attempt browser-based extraction when API methods fail.

#### Scenario: JavaScript-rendered content
- **WHEN** WebFetch returns empty or minimal content
- **AND** the page likely has JavaScript-rendered content
- **THEN** the skill SHALL attempt browser extraction
- **AND** use available browser tools (playwright-cli, MCP, etc.)

#### Scenario: CAPTCHA detected
- **WHEN** extraction returns a CAPTCHA challenge
- **THEN** the skill SHALL open browser in headed mode
- **AND** pause for human intervention
- **AND** resume after human confirmation

#### Scenario: Login required
- **WHEN** content requires authentication
- **THEN** the skill SHALL open browser in headed mode
- **AND** ask human to log in
- **AND** resume after human confirmation

#### Scenario: Rate limited
- **WHEN** API extraction returns rate limit error (429)
- **THEN** the skill SHALL attempt browser extraction as fallback
- **AND** switch to headed mode if CAPTCHA appears

### Requirement: Tool-agnostic browser detection

The skill SHALL detect available browser tools at runtime.

#### Scenario: playwright-cli available
- **WHEN** playwright-cli skill is available
- **THEN** the skill SHALL use playwright-cli commands
- **AND** follow patterns: open, snapshot, eval

#### Scenario: MCP playwright available
- **WHEN** mcp__playwright tools are available
- **THEN** the skill SHALL use MCP playwright tools
- **AND** follow MCP tool patterns

#### Scenario: No browser tools available
- **WHEN** no browser automation tools are detected
- **THEN** the skill SHALL report limitation
- **AND** continue with available API methods
- **AND** note the unextractable URLs in methodology appendix

### Requirement: Human handoff protocol

The skill SHALL support pause-and-wait for human intervention.

#### Scenario: During research execution
- **WHEN** CAPTCHA or login is encountered
- **THEN** the skill SHALL pause execution
- **AND** notify the user with clear instructions
- **AND** wait up to 5 minutes for human response
- **AND** resume after "done" confirmation

#### Scenario: Human does not respond
- **WHEN** human does not confirm within 5 minutes
- **THEN** the skill SHALL close the browser
- **AND** report partial results
- **AND** note the timeout in methodology appendix

#### Scenario: Human credentials handling
- **WHEN** login is required
- **THEN** the human SHALL log in directly
- **AND** the skill SHALL never see credentials
- **AND** browser state persists for subsequent requests

### Requirement: Telegram channel handling

The skill SHALL delegate Telegram extraction to telegram-scraper skill.

#### Scenario: Telegram channel URL detected
- **WHEN** URL matches t.me/* or telegram.me/*
- **THEN** the skill SHALL reference telegram-scraper skill
- **AND** follow telegram-scraper extraction patterns
- **AND** NOT duplicate Telegram-specific logic

#### Scenario: Private Telegram channel
- **WHEN** Telegram channel is private/closed
- **THEN** the skill SHALL follow telegram-scraper Method 2 (Playwright headed + login)
- **AND** apply same human handoff protocol