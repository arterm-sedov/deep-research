# Report Output Guidelines

## Output Formats

### Primary: Markdown

All reports are generated as Markdown first:
- `./[Topic]_Research_[YYYYMMDD]/report.md`
- Primary source of truth
- Human-readable, version-controllable

### Optional: HTML

The agent can generate HTML reports following [html-generation.md](./html-generation.md):
- McKinsey-style professional formatting
- Auto-opened in browser
- Based on templates/mckinsey_report_template.md

### PDF (Legacy/Optional)

PDF generation requires WeasyPrint (Python). If PDF is needed, the agent can:
1. Generate HTML following the HTML generation guidelines
2. Hand off to user for PDF conversion, OR
3. User can run: `weasyprint report.html report.pdf`

**Note:** PDF is not a core requirement. Most users need Markdown + HTML only.

---

## Output Directory Structure

```
./[Topic]_Research_[YYYYMMDD]/
├── report.md          # Primary report (always generated)
├── report.html        # HTML version (optional)
└── sources.json       # Source metadata (for continuation)
```

---

## For Long Reports (>18K words)

See [continuation.md](./continuation.md) for handling reports that exceed context limits:
- Disk-persisted citations survive context compaction
- Recursive agent spawning for unlimited length
- Progressive file assembly

---

## Content Quality (Markdown)

### Required Sections

```markdown
# [Topic] Research Report

## Executive Summary
[200-400 words]

## Introduction
[Scope, methodology, assumptions]

## Main Analysis
### Finding 1: [Title]
[600-2,000 words, cited [1][2][3]]

### Finding 2: [Title]
...

## Synthesis & Insights
[Patterns, implications, novel insights]

## Limitations & Caveats
[Known gaps, assumptions, uncertainties]

## Recommendations
[Action items, next steps]

## Bibliography
[1] Author/Org (Year). "Title". Publication. URL
[2] ...
[N] ...

## Methodology Appendix
[Research process, sources consulted]
```

### Writing Standards

- **Prose-first**: >=80% flowing prose, minimal bullets
- **Citation density**: Major claims cited immediately [N]
- **No placeholders**: Zero "TODO", "TBD", "[Section continues]"
- **Evidence-rich**: Specific data points, statistics, quotes

---

## For HTML/PDF Conversion (If Needed)

If you need PDF output and have WeasyPrint installed:

### CSS for Print

```css
/* Prevent page breaks inside key elements */
.executive-summary,
.key-insight,
table {
    page-break-inside: avoid;
}

/* Headers should not be orphaned */
h2, h3, h4 {
    page-break-after: avoid;
}

/* Minimum lines at page boundaries */
p {
    orphans: 3;
    widows: 3;
}

/* Page setup */
@page {
    size: A4;
    margin: 25mm 20mm;
}

/* Typography for print */
body {
    font-family: Georgia, serif;
    font-size: 10pt;
    line-height: 1.6;
}
```

### Generation

```bash
weasyprint report.html report.pdf
```

See [html-generation.md](./html-generation.md) for full HTML stylingguidelines.