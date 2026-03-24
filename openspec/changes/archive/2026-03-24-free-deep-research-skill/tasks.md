## 1. Documentation Enhancement

- [x] 1.1 Enhance reference/methodology.md with complete phase logic (consolidate from research_engine.py)
- [x] 1.2 Create reference/providers.md documenting provider priority (WebSearch → SearXNG → Exa MCP)
- [x] 1.3 Update SKILL.md to be self-contained with phase summary and provider priority
- [x] 1.4 Update reference/quality-gates.md with validation checklist (from validate_report.py logic)
- [x] 1.5 Update README.md to reflect free-first architecture

## 2. Python Removal

- [x] 2.1 Delete scripts/research_engine.py (logic moved to methodology.md)
- [x] 2.2 Delete scripts/source_evaluator.py (LLM evaluation replaces)
- [x] 2.3 Delete scripts/citation_manager.py (agent tracks in context)
- [x] 2.4 Delete scripts/validate_report.py (quality-gates.md checklist replaces)
- [x] 2.5 Delete scripts/verify_citations.py (agent verifies with WebFetch)
- [x] 2.6 Delete scripts/md_to_html.py (agent generates HTML from templates)
- [x] 2.7 Delete scripts/verify_html.py (agent validates following html-generation.md)
- [x] 2.8 Delete tests/ directory (no Python to test)
- [x] 2.9 Delete requirements.txt (no dependencies)
- [x] 2.10 Delete deep-research/requirements.txt at project root if it exists

## 3. Provider Support

- [x] 3.1 Document WebSearch as primary (always available)
- [x] 3.2 Document SearXNG detection (curl localhost:8000/health)
- [x] 3.3 Document Exa MCP usage (if available)
- [x] 3.4 Remove search-cli from requirements (make it undocumented fallback)
- [x] 3.5 Add provider fallback logic to methodology.md

## 4. Template Cleanup

- [x] 4.1 Keep templates/report_template.md (agent reads template)
- [x] 4.2 Keep templates/mckinsey_report_template.html (agent uses as reference)
- [x] 4.3 Simplify weasyprint_guidelines.md or consider removing PDF generation

## 5. SKILL.md Enhancement

- [x] 5.1 Add provider detection section
- [x] 5.2 Add clear invocation triggers
- [x] 5.3 Reference all required documents
- [x] 5.4 Include quality standards summary
- [x] 5.5 Document output format expectations

## 6. Testing and Validation

- [ ] 6.1 Test research execution with only WebSearch (manual testing required)
- [ ] 6.2 Test research execution with SearXNG available (manual testing required)
- [ ] 6.3 Test research execution with Exa MCP configured (manual testing required)
- [x] 6.4 Verify no Python dependencies required (verified - scripts/ and requirements.txt deleted)
- [ ] 6.5 Verify output format matches original (Markdown + HTML) (manual testing required)

## 7. Installation Updates

- [x] 7.1 Update README.md installation instructions (no pip install) - Done in task 1.5
- [x] 7.2 Remove any Python setup requirements - No Python setup in current README
- [x] 7.3 Document SearXNG setup as optional enhancement - Done in README
- [x] 7.4 Update version history in README.md - Done in README (version 3.0)