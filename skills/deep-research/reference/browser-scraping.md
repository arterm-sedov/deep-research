# Browser Scraping Fallback Guide

When API-based extraction fails, use browser automation as a fallback. This guide covers tool detection, human handoff, and extraction patterns.

## Extraction Fallback Chain

**Try these methods in order:**

| Priority | Method | Cost | Best For |
|----------|--------|------|----------|
| 1 | WebFetch | FREE (native) | Static pages, simple content |
| 2 | searxng-extract | FREE (local) | Quick extraction, 2500 chars |
| 3 | tavily-extract | Paid (if configured) | JS rendering, full content |
| 4 | Browser automation | FREE (tools required) | JS, CAPTCHA, login, rate limits |
| 5 | Report limitation | - | Document what couldn't be extracted |

**When to escalate to browser:**
- WebFetch returns empty/minimal content (JS not rendered)
- Coverage less than expected after API extraction
- CAPTCHA or login detected in response
- Rate limit errors (429)
- JavaScript-dependent content (SPAs, dynamic updates)
- Private content (Telegram channels, LinkedIn profiles)

---

## Browser Tool Detection

**Detect available browser tools at runtime:**

```
Check in order:
1. playwright-cli skill (Bash tool with playwright-cli:*)
2. MCP playwright tools (mcp__playwright__*)
3. Other browser MCPs (puppeteer, selenium, etc.)
4. Report limitation if none available
```

**Pattern for detection:**
```markdown
If playwright-cli skill exists:
  Use playwright-cli commands

If MCP playwright tools exist:
  Use mcp__playwright__* tools

If neither:
  Report: "No browser automation available. Content from [URL] could not be extracted."
  Continue with available sources
```

---

## Playwright CLI Patterns

If playwright-cli is available, use these patterns:

### Headless Mode (Primary)

For JavaScript content without CAPTCHA/login:

```bash
# Navigate
playwright-cli open "https://example.com"

# Get page structure
playwright-cli snapshot

# Extract text
playwright-cli eval "document.body.innerText"

# Extract links
playwright-cli eval "Array.from(document.querySelectorAll('a[href]')).map(a => a.href)"
```

### Headed Mode (Human Handoff)

For CAPTCHA or login:

```bash
# Open visible browser
playwright-cli open "https://example.com" --headed

# Snapshot to see current state
playwright-cli snapshot

# If CAPTCHA/login detected, pause for human
# ... human intervention ...

# Continue extraction
playwright-cli eval "document.body.innerText"
```

### Browser State Management

Save and restore state to avoid repeated logins:

```bash
# Save after login
playwright-cli state-save auth-state.json

# Restore for subsequent requests
playwright-cli state-load auth-state.json
```

**For detailed commands, reference the playwright-cli skill.**

---

## Human Handoff Protocol

When human intervention is required:

### Trigger Conditions

- CAPTCHA challenge detected
- Login page appeared
- Age verification / terms acceptance
- Payment wall (decide: skip or ask)

### Process

```
1. Detect need:
   - Page shows CAPTCHA
   - Login form detected
   - "Sign in to continue" message
   - Age/terms overlay

2. Switch to headed mode:
   playwright-cli open "URL" --headed

3. Pause and notify human:
   > "I need your help to access [URL].
   > Reason: [CAPTCHA/login required]
   > The browser window is open. Please complete the challenge.
   > Type 'done' when ready to continue."

4. Wait for confirmation:
   - Human completes challenge
   - Human confirms "done"

5. Resume extraction:
   playwright-cli snapshot
   playwright-cli eval "document.body.innerText"
   # Continue with research
```

### Timeout

- Wait up to **5 minutes** for human response
- If no response, close browser and report limitation
- Document in methodology appendix: "Could not access [URL] - human handoff timeout"

---

## Telegram Channels

**Use the telegram-scraper skill for Telegram extraction.**

Do NOT duplicate Telegram-specific logic here. The telegram-scraper skill handles:
- Public channels (web fetch / Playwright preview)
- Private/closed channels (Playwright headed + login)
- Telegram Desktop exports

**Pattern:**
```markdown
If URL matches t.me/* or telegram.me/*:
  Reference telegram-scraper skill
  Follow its extraction methods
```

**See:** `.agents/skills/telegram-scraper/SKILL.md`

---

## Search Engine Scraping

When all search APIs are rate limited, scrape search results directly:

### DuckDuckGo (Recommended)

No CAPTCHA, reliable:

```bash
playwright-cli open "https://duckduckgo.com/?q=your+query"
playwright-cli snapshot
# Extract result links from snapshot
```

### Google (Use Sparingly)

Will trigger CAPTCHA after ~50 requests:

```bash
playwright-cli open "https://www.google.com/search?q=your+query"
playwright-cli snapshot
# If CAPTCHA, switch to headed mode
```

### SearX (Self-hosted)

If you have SearX running:

```bash
playwright-cli open "http://localhost:8999/search?q=your+query"
playwright-cli snapshot
```

---

## Common Extraction Patterns

### Get Page Text

```bash
playwright-cli eval "document.body.innerText"
```

### Get All Links

```bash
playwright-cli eval "Array.from(document.querySelectorAll('a[href]')).map(a => ({url: a.href, text: a.innerText}))"
```

### Get Specific Elements

```bash
playwright-cli eval "Array.from(document.querySelectorAll('article')).map(a => a.innerText)"
```

### Get Meta Tags

```bash
playwright-cli eval "Array.from(document.querySelectorAll('meta')).map(m => ({name: m.name, content: m.content}))"
```

### Get JSON Data

```bash
playwright-cli eval "document.querySelector('script[type=\"application/json\"]')?.textContent"
```

---

## Rate Limit Considerations

| Source | API Limit | Browser Limit |
|--------|-----------|---------------|
| Google Search | 429 after ~100 | CAPTCHA after ~50 |
| DuckDuckGo | No API | No CAPTCHA |
| LinkedIn | Strict | Works with login |
| Twitter/X | 429 after ~450 | Works with login |

**Recommendation:** Use DuckDuckGo or self-hosted SearX for search scraping.

---

## Integration with Deep Research

In Phase 3 (RETRIEVE), after API extraction attempts:

1. **Detect extraction failure**
   - Empty/minimal content
   - Rate limit (429)
   - CAPTCHA/login detected

2. **Check browser tools available**

3. **Launch browser automation**
   - Headless first
   - Headed if blocked

4. **Apply human handoff if needed**

5. **Extract and continue**

**See also:**
- [playwright-cli skill](~/.config/opencode/skills/playwright/SKILL.md) for detailed commands
- [telegram-scraper skill](~/.agents/skills/telegram-scraper/SKILL.md) for Telegram extraction
- [providers.md](./providers.md) for API extraction options