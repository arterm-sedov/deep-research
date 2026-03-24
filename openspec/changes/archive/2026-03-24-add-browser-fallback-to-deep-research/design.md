## Context

The deep-research skill extracts content via APIs (WebFetch, searxng-extract, tavily-extract). These work for static pages but fail for:
- JavaScript-rendered content (React, Vue, SPAs)
- CAPTCHA-protected pages
- Login-required content (LinkedIn, Twitter, private Telegram)
- Rate-limited APIs

Browser automation can handle these cases by rendering JavaScript and allowing human intervention for CAPTCHA/login.

**Existing resources:**
- `playwright-cli` skill exists at `.config/opencode/skills/playwright/SKILL.md`
- `telegram-scraper` skill exists at `.agents/skills/telegram-scraper/SKILL.md`

**Constraints:**
- Must be tool-agnostic (don't assume playwright-cli only)
- Must reference existing skills rather than duplicate
- Must support human-in-the-loop for CAPTCHA/login
- Must maintain free-first philosophy

## Goals / Non-Goals

**Goals:**
- Add browser fallback extraction when API methods fail
- Be tool-agnostic (detect available browser tools)
- Reference existing skills (playwright-cli, telegram-scraper)
- Support pause-and-wait for human intervention
- Maintain extraction priority (free first)

**Non-Goals:**
- Not creating a new browser skill
- Not duplicating telegram-scraper functionality
- Not requiring any specific browser tool

## Decisions

### Decision 1: Browser Fallback Chain

**Choice**: Try extraction APIs first, then browser automation

**Rationale**:
- APIs are faster and don't require human intervention
- Browser automation is slower and may need human help
- Matches existing free-first philosophy

**Fallback Chain**:
```
1. WebFetch (native, always available)
2. searxng-extract (free, local, 2500 chars)
3. tavily-extract (if configured)
4. Browser automation (detect available tool)
   └─ Headless first, then headed + human handoff
5. Report limitation if all fail
```

### Decision 2: Tool Detection Approach

**Choice**: Agent detects available browser tools at runtime

**Rationale**:
- Can't assume playwright-cli is the only option
- MCP tools might be available (mcp__playwright__*)
- Other browser tools might exist

**Detection Pattern**:
```markdown
Available browser tools (check in order):
1. playwright-cli skill (allowed-tools: Bash(playwright-cli:*))
2. MCP playwright tools (mcp__playwright__*)
3. Other browser MCPs (puppeteer, selenium)
```

### Decision 3: Human Handoff Protocol

**Choice**: Pause-and-wait with clear messaging

**Rationale**:
- CAPTCHA and login require human intervention
- Agent should pause, not fail
- User credentials never exposed to agent (human logs in directly)

**Protocol**:
```
1. Detect CAPTCHA/login requirement
2. Open browser in headed mode
3. Pause execution
4. Notify user: "Please complete the [CAPTCHA/login] to continue."
5. Wait for user confirmation: "done"
6. Resume extraction
```

### Decision 4: Telegram Handling

**Choice**: Delegate to telegram-scraper skill

**Rationale**:
- telegram-scraper already handles public, private, and paywalled channels
- Has specific extraction logic for Telegram DOM
- Supports multiple methods (web fetch, Playwright, Desktop export)

**Implementation**:
- Reference telegram-scraper in browser-scraping.md
- Add note in methodology.md: "For Telegram channels, use telegram-scraper skill"

### Decision 5: File Location

**Choice**: `deep-research/reference/browser-scraping.md`

**Rationale**:
- Co-located with other reference docs
- Follows existing pattern (methodology.md, providers.md, etc.)
- SKILL.md execution section references it

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    PHASE 3: RETRIEVE                        │
└─────────────────────────────────────────────────────────────┘
                           │
                           ▼
         ┌─────────────────────────────────────┐
         │     Try API Extraction              │
         │  WebFetch → searxng → tavily        │
         └─────────────────────────────────────┘
                           │
                           ▼
                   ┌───────────────┐
                   │  Success?      │
                   └───────────────┘
                    │           │
                   YES          NO
                    │           │
                    ▼           ▼
              Continue     ┌─────────────────────────────────┐
                           │  Check browser-scraping.md       │
                           │  Detect available browser tools  │
                           │  Try browser automation          │
                           └─────────────────────────────────┘
                                          │
                                          ▼
                                  ┌───────────────┐
                                  │ CAPTCHA/Login? │
                                  └───────────────┘
                                   │           │
                                  NO          YES
                                   │           │
                                   ▼           ▼
                             Extract    ┌────────────────────────┐
                                        │ Open headed browser    │
                                        │ Pause for human        │
                                        │ Wait for confirmation  │
                                        │ Resume extraction      │
                                        └────────────────────────┘
```

## Risks / Trade-offs

| Risk | Mitigation |
|------|------------|
| Browser tools not available | Document as limitation, continue with API-only |
| Human doesn't respond to handoff | Add timeout, report partial results |
| Different browser tools have different syntax | Focus on common patterns (open, snapshot, eval) |
| Browser automation slower than APIs | Only use as fallback, not primary method |

## Open Questions

1. Should we add a timeout for human handoff? (Decision: Yes, 5-minute default)
2. Should we save browser state for reuse? (Decision: Yes, reference playwright state-save)
3. Should we log which extraction method was used? (Decision: Yes, in methodology appendix)