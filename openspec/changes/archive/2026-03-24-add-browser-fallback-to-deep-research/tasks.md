## 1. Create browser-scraping.md Reference Document

- [x] 1.1 Create `deep-research/reference/browser-scraping.md`
- [x] 1.2 Document extraction fallback chain (WebFetch → searxng → tavily → browser)
- [x] 1.3 Document browser tool detection (playwright-cli, MCP playwright, etc.)
- [x] 1.4 Document common browser patterns (open, snapshot, eval)
- [x] 1.5 Document human handoff protocol (pause-and-wait, 5-minute timeout)
- [x] 1.6 Reference playwright-cli skill for detailed commands
- [x] 1.7 Reference telegram-scraper skill for Telegram extraction
- [x] 1.8 Add extraction priority table (free first)

## 2. Update methodology.md Phase 3 RETRIEVE

- [x] 2.1 Add "Step 3: Browser Scraping Fallback" section after parallel search
- [x] 2.2 Document when to use browser fallback (JS content, CAPTCHA, login)
- [x] 2.3 Add link to browser-scraping.md for detailed instructions
- [x] 2.4 Include Telegram delegation note (use telegram-scraper skill)
- [x] 2.5 Add human handoff trigger conditions

## 3. Update SKILL.md Execution Section

- [x] 3.1 Add browser-scraping.md to reference files list
- [x] 3.2 Update retrieval description to include browser fallback

## 4. Update providers.md

- [x] 4.1 Add "Browser Automation" section after search providers
- [x] 4.2 Document browser tool priority (playwright-cli > MCP > others)
- [x] 4.3 Add example commands for common extraction scenarios

## 5. Update weasyprint_guidelines.md (if needed)

- [x] 5.1 No changes expected - browser extraction doesn't affect PDF generation

## 6. Testing and Validation

- [x] 6.1 Test browser fallback is referenced correctly in methodology
- [x] 6.2 Verify browser-scraping.md links to playwright-cli skill
- [x] 6.3 Verify browser-scraping.md references telegram-scraper for Telegram
- [x] 6.4 Verify human handoff protocol is documented
- [x] 6.5 Verify tool detection is documented as agnostic