# ♿ Claude Accessibility Audit Skill

> **A full-featured WCAG 2.1/2.2 web accessibility audit skill for Claude — by Anish**

Audit any public website for accessibility barriers, get per-page findings mapped to WCAG success criteria, and receive technology-specific fix code — all delivered as a polished downloadable **Word (.docx)** and **PDF** report.

---

## ✨ What It Does

- 🔍 **Crawls your entire site** — not just the homepage
- 🧠 **Detects your tech stack** automatically (React, Next.js, Vue, Angular, WordPress, Shopify, Vanilla HTML, and more)
- 📋 **Checks every WCAG 2.1/2.2 criterion** assessable from HTML across all four POUR principles
- 🎯 **Maps issues per page** with WCAG criterion, level (A/AA/AAA), severity, affected users, and fix complexity
- 🛠️ **Provides stack-specific fix code** — JSX for React, Liquid for Shopify, PHP templates for WordPress, etc.
- 📄 **Generates a professional report** — cover page, executive summary, per-page findings, priority roadmap, automated testing guide
- ⚖️ **Notes legal context** — ADA Title III, Section 508, EN 301 549

### Report Sections

| Section | Contents |
|---|---|
| Cover Page | Site name, severity counts at a glance |
| Executive Summary | Compliance verdict, POUR scores, affected user groups |
| Pages Audited | All crawled URLs with issue count and highest severity |
| Detailed Findings | Per-page issue tables + fix code blocks for every Critical/Serious issue |
| WCAG Criterion Reference | Every issue mapped to its criterion, sorted by level |
| Priority Fix Roadmap | Sprint 1 (Critical) → Sprint 2 (Serious) → Sprint 3 (Backlog) |
| Automated Testing Setup | Stack-specific CI/CD integration guide |
| What's Already Accessible | Genuine strengths with evidence |
| Glossary | WCAG, ARIA, POUR, and key terms explained |

### Severity Levels

| Level | Meaning | Example |
|---|---|---|
| 🔴 Critical | Completely blocks assistive technology users | Form with no labels, keyboard trap |
| 🟠 Serious | Significantly impairs usability | Missing focus indicators, low contrast |
| 🟡 Moderate | Creates friction, workarounds exist | Generic link text ("read more") |
| 🟢 Minor | Best-practice gap, minimal impact | Missing `lang` attribute |

### Supported Tech Stacks

`React` · `Next.js` · `Vue` · `Nuxt` · `Angular` · `WordPress` · `Shopify` · `Drupal` · `Wix` · `Squarespace` · `Vanilla HTML/CSS`

---

## 📦 Installation

### Requirements

- A Claude account ([claude.ai](https://claude.ai) or API access)
- The `.skill` file from this repository

Download the latest release:

```
accessibility-audit.skill
```

---

## 🖥️ How to Use on Claude.ai (Web & Desktop)

### Step 1 — Install the skill

1. Go to **[claude.ai](https://claude.ai)** and sign in
2. Click your profile icon → **Settings**
3. Navigate to **Skills** (or **Extensions**, depending on your plan)
4. Click **Install Skill** or drag and drop `accessibility-audit.skill` into the skills panel
5. The skill appears in your list as **accessibility-audit** — toggle it **On**

> **Note:** Skills are available on Claude Pro, Team, and Enterprise plans. Check [claude.ai/upgrade](https://claude.ai/upgrade) if you don't see the Skills tab.

### Step 2 — Start a new conversation

Open a new chat at [claude.ai](https://claude.ai).

### Step 3 — Run your audit

Just describe what you want in plain English:

```
Audit https://yourwebsite.com for accessibility issues
```

```
Do a full WCAG 2.1 AA accessibility audit of https://mystore.com
```

```
Quick a11y check on https://example.com — I need to know the biggest issues
```

```
Check if https://myblog.com is ADA compliant
```

### Step 4 — Choose audit depth

Claude will ask:

> **"Would you like a Quick Audit (homepage + key pages — 2-3 minutes) or a Full Audit (all pages, comprehensive — 5-10 minutes)?"**

- **Quick Audit** — Homepage + up to 6 high-signal pages. Great for a fast overview or before a client meeting.
- **Full Audit** — Every meaningful page crawled. Best for compliance work, pre-launch reviews, or legal preparation.

### Step 5 — Receive your report

Claude will:
1. Show a brief **chat summary** — severity counts, top 3 critical fixes, biggest strength
2. Automatically generate and offer **two downloadable files**:
   - `a11y-audit-yoursite-com-2025-06-15.docx` — editable Word document
   - `a11y-audit-yoursite-com-2025-06-15.pdf` — print-ready PDF

Click the download links to save both files.

### Tips for Claude.ai

- You can ask follow-up questions after the report: *"Show me the fix code for the form labels issue on /contact"*
- Request a re-audit after making changes: *"I fixed the skip link — re-run the audit on /contact"*
- Ask for competitor comparison: *"Also audit https://competitor.com and compare"*

---

## 💻 How to Use in Claude Code

Claude Code gives you more power — you can pipe URLs in from scripts, chain audits, and save reports directly to your project.

### Step 1 — Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

Authenticate:

```bash
claude login
```

### Step 2 — Install the skill

Copy the skill file into your Claude Code skills directory:

```bash
# macOS / Linux
cp accessibility-audit.skill ~/.claude/skills/

# Windows (PowerShell)
Copy-Item accessibility-audit.skill "$env:USERPROFILE\.claude\skills\"
```

Verify it's recognized:

```bash
claude skills list
# Should show: accessibility-audit ✓
```

### Step 3 — Run an audit from the terminal

**Interactive mode** — Claude asks you questions as it works:

```bash
claude "Audit https://yourwebsite.com for WCAG 2.1 AA accessibility issues. Full audit please."
```

**Non-interactive / scripted mode** — pass everything upfront:

```bash
claude --no-interactive "Full accessibility audit of https://yourwebsite.com. Save the DOCX and PDF to ./reports/"
```

**Pipe a list of URLs:**

```bash
cat sites.txt | while read url; do
  claude --no-interactive "Quick accessibility audit of $url. Save reports to ./reports/"
done
```

**With a custom output directory:**

```bash
claude "Full accessibility audit of https://yourwebsite.com. Save all outputs to /Users/anish/audits/$(date +%Y-%m-%d)/"
```

### Step 4 — Find your reports

By default, reports are saved to the current working directory (or wherever you specify). Look for:

```
a11y-audit-yourwebsite-com-2025-06-15.docx
a11y-audit-yourwebsite-com-2025-06-15.pdf
```

### Step 5 — Integrate into your dev workflow

**Pre-deploy audit in a Makefile:**

```makefile
audit:
	claude --no-interactive "Quick accessibility audit of https://staging.yoursite.com. Save to ./audit-reports/"
```

**GitHub Actions — audit on PR:**

```yaml
name: Accessibility Audit
on:
  pull_request:
    branches: [main]

jobs:
  a11y:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Copy skill
        run: |
          mkdir -p ~/.claude/skills
          cp accessibility-audit.skill ~/.claude/skills/

      - name: Run audit
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          claude --no-interactive "Quick accessibility audit of ${{ vars.STAGING_URL }}. Save to ./audit-reports/"

      - name: Upload report
        uses: actions/upload-artifact@v4
        with:
          name: accessibility-audit
          path: audit-reports/
```

**VS Code task (`tasks.json`):**

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Accessibility Audit",
      "type": "shell",
      "command": "claude",
      "args": [
        "--no-interactive",
        "Quick accessibility audit of ${input:auditUrl}. Save to ${workspaceFolder}/audit-reports/"
      ],
      "group": "test",
      "presentation": { "reveal": "always" }
    }
  ],
  "inputs": [
    {
      "id": "auditUrl",
      "type": "promptString",
      "description": "URL to audit"
    }
  ]
}
```

### Claude Code Tips

- Use `claude --model claude-opus-4-5` for the most thorough crawl on complex sites
- Chain with other skills: after the audit, ask Claude to *"now generate GitHub issues for each Critical finding"*
- Combine with your codebase: *"Audit https://staging.myapp.com and cross-reference findings with the React components in ./src/components/"*

---

## 🔍 What Gets Checked

<details>
<summary><strong>Perceivable (WCAG 1.x)</strong></summary>

- Image `alt` attributes — present, descriptive, empty for decorative
- SVG and icon font labels (`aria-label`, `<title>`)
- Video captions (`<track kind="captions">`)
- Audio transcripts
- Heading hierarchy (single H1, no skipped levels)
- Form label associations (`<label for>`, `aria-labelledby`)
- Table `<th scope>` and `<caption>`
- Landmark regions (`<main>`, `<nav>`, `<header>`, `<footer>`)
- Color-alone information conveyance
- Font sizing units (`px` vs `rem`/`em`)
- Viewport meta for reflow
- Tooltip dismissibility

</details>

<details>
<summary><strong>Operable (WCAG 2.x)</strong></summary>

- Keyboard operability of custom widgets
- Skip navigation link
- Focus visibility (`outline: none` detection)
- Positive `tabindex` values
- `onclick` on non-interactive elements
- Modal focus trapping
- Descriptive link text (no "click here")
- Unique, descriptive `<title>` per page
- Auto-playing media pause controls
- Animation/flicker signals

</details>

<details>
<summary><strong>Understandable (WCAG 3.x)</strong></summary>

- `<html lang>` attribute
- `lang` on language-switch passages
- Form error identification in text
- Required field indication
- Error recovery suggestions
- `onfocus`/`onchange` context-change warnings
- Navigation consistency across pages

</details>

<details>
<summary><strong>Robust (WCAG 4.x)</strong></summary>

- Duplicate `id` attributes
- Valid ARIA roles and states
- `aria-expanded`, `aria-live`, `aria-required` correctness
- Custom widget name/role/value exposure
- `role="status"` for dynamic content
- HTML parse error signals

</details>

---

## ⚠️ Limitations

Some WCAG criteria require live browser rendering or assistive technology that cannot be assessed from HTML alone. The skill will flag these and tell you exactly which tool to use:

| What needs tooling | Recommended tool |
|---|---|
| Exact color contrast ratios | [TPGi Colour Contrast Analyser](https://www.tpgi.com/color-contrast-checker/) (free) |
| Screen reader behavior | NVDA + Chrome (Windows, free) · VoiceOver + Safari (Mac/iOS, built-in) |
| Animation frame rates | Browser DevTools → Performance tab |
| JavaScript-rendered content | axe DevTools browser extension |
| Cognitive load assessment | Hemingway Editor for readability |

---

## 📖 WCAG Quick Reference

| Level | Meaning | Required for ADA/EN 301 549? |
|---|---|---|
| A | Minimum baseline | Yes |
| AA | Standard compliance target | Yes |
| AAA | Enhanced / best practice | No (aspirational) |

**Legal context (not legal advice):**
- 🇺🇸 **ADA Title III** — applies to US businesses' websites. WCAG 2.1 AA is the accepted standard.
- 🇺🇸 **Section 508** — US federal agencies and contractors. WCAG 2.0 AA minimum.
- 🇪🇺 **EN 301 549** — EU/UK public sector and increasingly private sector.

Consult a qualified attorney for compliance determinations.

---

## 🙏 Credits

**Skill created by Anish M Mathew**

Powered by [Claude](https://claude.ai) · Built on the Claude Skills platform by Anthropic

---

## 📄 License

MIT License — free to use, modify, and distribute. Attribution appreciated.

---

*Last updated: May 2026*