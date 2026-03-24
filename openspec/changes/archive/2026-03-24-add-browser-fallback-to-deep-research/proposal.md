## Why

The deep-research skill currently relies on API-based content extraction (WebFetch, searxng-extract, tavily-extract). When these fail due to:
- JavaScript-rendered content (SPAs, dynamic pages)
- CAPTCHA challenges
- Login-required pages
- Rate limiting
- Private content (Telegram channels, etc.)

The research hits a dead end. We need a browser-based fallback that can handle these cases.

Additionally, Telegram channels have special handling that already exists in the `telegram-scraper` skill. Deep-research should reference this rather than duplicate functionality.

## What Changes

- **Add browser-scraping.md** - Reference document for browser fallback chain
- **Update methodology.md Phase 3** - Add browser scraping as fallback extraction method
- **Update SKILL.md** - Reference browser-scraping.md in execution section

**Key principles:**
1. **Tool-agnostic** - Detect which browser tools are available (playwright-cli, MCP playwright, etc.)
2. **Reference existing skills** - Don't duplicate playwright-cli or telegram-scraper
3. **Human-in-the-loop** - Pause-and-wait protocol for CAPTCHA/login
4. **Fallback chain** - Try extraction APIs first, then browser automation

## Capabilities

### New Capabilities

- `browser-fallback`: When API extraction fails, detect and use available browser tools
- `human-handoff`: Pause-and-wait protocol for CAPTCHA and login challenges
- `telegram-extraction`: Reference telegram-scraper skill for Telegram channels

### Modified Capabilities

- `deep-research`: Phase 3 RETRIEVE now includes browser fallback extraction

## Impact

- **New file**: `deep-research/reference/browser-scraping.md`
- **Modified**: `deep-research/reference/methodology.md` (Phase 3)
- **Modified**: `deep-research/SKILL.md` (execution section)

## Dependencies

- **Existing skills referenced**:
  - `playwright-cli` (browser automation)
  - `telegram-scraper` (Telegram-specific extraction)
- **No new dependencies** - Uses available tools