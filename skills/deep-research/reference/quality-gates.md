# Quality Gates and Standards

All validation is performed by the agent following these checklists. No Python scripts required.

---

## Citation Verification Checklist

After generating any report, verify citations:

- [ ] **DOI/URL resolution**: Each citation URL is accessible
- [ ] **Title/year matching**: Citation metadata matches source
- [ ] **No suspicious entries**: No fabricated citations, no placeholders
- [ ] **Citation format**: [N] Author/Org (Year). "Title". Publication. URL

**On suspicious citations:** Review flagged entries, remove/replace fabricated ones, re-verify.

---

## Structure & Quality Validation Checklist

**9 required checks:**

1. [ ] **Executive summary length**: 200-400 words
2. [ ] **Required sections present**: Exec Summary, Introduction, Analysis, Synthesis, Limitations, Recommendations, Bibliography, Methodology
3. [ ] **Citations formatted**: [1], [2], [3] style throughout
4. [ ] **Bibliography complete**: Every in-text citation has bibliography entry
5. [ ] **No placeholder text**: No "TODO", "TBD", "[Section continues]"
6. [ ] **Word count reasonable**: 500-10,000 words (mode-dependent)
7. [ ] **Minimum sources**: 10+ distinct sources
8. [ ] **No broken internal links**: All section references work
9. [ ] **Prose-first**: >=80% flowing prose, <20% bullets

**Failure handling:**
- Attempt 1: Fix formatting/links
- Attempt 2: Review and correct
- After 2 failures: STOP, report issues

---

## Validation Loop Protocol

**After generating ANY report, run this checklist:**

1. Read through citation verification checklist
2. Read through structure validation checklist
3. If ANY check fails:
   - Identify the specific issue
   - Fix it immediately
   - Re-verify from the top
4. Maximum 3 retry cycles. If still failing after 3 cycles: STOP and report issues.

**Do NOT skip validation.** Every report must pass all checks before delivery.

---

## Anti-Fatigue Protocol

### Quality Check (Apply to EVERY Section)

Before considering section complete:
- [ ] **Paragraph count**: >=3 paragraphs for major sections
- [ ] **Prose-first**: <20% bullets (>=80% flowing prose)
- [ ] **No placeholders**: Zero "Content continues", "Due to length", "[Sections X-Y]"
- [ ] **Evidence-rich**: Specific data points, statistics, quotes
- [ ] **Citation density**: Major claims cited in same sentence

**If ANY fails:** Regenerate section before continuing.

### Bullet Point Policy

- Use bullets SPARINGLY: Only for distinct lists (product names, company roster, enumerated steps)
- NEVER use bullets as primary content delivery
- Each finding requires substantive prose (3-5+ paragraphs)
- Convert: "* Market size: $2.4B" -> "The global market reached $2.4 billion in 2023, driven by increasing consumer demand [1]."

---

## Bibliography Requirements (ZERO TOLERANCE)

**Report is UNUSABLE without complete bibliography.**

**MUST:**
- Include EVERY citation [N] used in report body
- Format: [N] Author/Org (Year). "Title". Publication. URL (Retrieved: Date)
- Each entry on its own line, complete

**NEVER:**
- Placeholders: "[8-75] Additional citations", "...continue...", "etc."
- Ranges: "[3-50]" instead of individual entries
- Truncation: Stop at 10 when 30 cited

---

## Writing Standards

### Core Principles

| Principle | Description |
|-----------|-------------|
| Narrative-driven | Flowing prose, story with beginning/middle/end |
| Precision | Every word deliberately chosen |
| Economy | No fluff, eliminate fancy grammar |
| Clarity | Exact numbers embedded in sentences |
| Directness | State findings without embellishment |
| High signal-to-noise | Dense information, respect reader time |

### Precision Examples

| Bad | Good |
|-----|------|
| "significantly improved outcomes" | "reduced mortality 23% (p<0.01)" |
| "several studies suggest" | "5 RCTs (n=1,847) show" |
| "potentially beneficial" | "increased biomarker X by 15%" |
| "* Market: $2.4B" | "The market reached $2.4 billion in 2023 [1]." |

---

## Source Attribution Standards

**Immediate citation:** Every factual claim followed by [N] in same sentence.

**Quote sources directly:**
- "According to [1]..."
- "[1] reports..."

**Distinguish fact from synthesis:**
- GOOD: "Mortality decreased 23% (p<0.01) in the treatment group [1]."
- BAD: "Studies show mortality improved significantly."

**No vague attributions:**
- NEVER: "Research suggests...", "Studies show...", "Experts believe..."
- ALWAYS: "Smith et al. (2024) found..." [1]

**Label speculation:**
- GOOD: "This suggests a potential mechanism..."
- BAD: "The mechanism is..." (presented as fact)

**Admit uncertainty:**
- GOOD: "No sources found addressing X directly."
- BAD: Fabricating a citation

---

## Anti-Hallucination Protocol

- **Source grounding:** Every factual claim MUST cite specific source immediately [N]
- **Clear boundaries:** Distinguish FACTS (from sources) from SYNTHESIS (your analysis)
- **Explicit markers:** Use "According to [1]..." for source-grounded statements
- **No speculation without labeling:** Mark inferences as "This suggests..."
- **Verify before citing:** If unsure source says X, do NOT fabricate citation
- **When uncertain:** Say "No sources found for X" rather than inventing references

---

## Report Quality Standards

**Every report must have:**
- 10+ sources (document if fewer)
- 3+ sources per major claim
- Executive summary 200-400 words
- Full citations with URLs
- Credibility assessment
- Limitations section
- Methodology documented
- No placeholders

**Priority:** Thoroughness over speed. Quality > speed.

---

## Error Handling

**Stop immediately if:**
- 2 validation failures on same error
- <5 sources after exhaustive search
- User interrupts/changes scope

**Graceful degradation:**
- 5-10 sources: Note in limitations, extra verification
- Time constraint: Package partial, document gaps
- High-priority critique: Address immediately

**Error format:**
```
Issue: [Description]
Context: [What was attempted]
Tried: [Resolution attempts]
Options:
   1. [Option 1]
   2. [Option 2]
```