---
description: "Systematic QA audit with specific checklists"
argument-hint: "[audit-type] [URL] — types: i18n, a11y, console, network, forms, full"
model: opus
---

# Systematic QA Audit

Run a targeted QA audit using specific testing checklists. Unlike `/qa-explore` which freely explores, this command systematically goes through every item on the selected checklist.

**Arguments:** $ARGUMENTS

## Workflow

### Phase 1: Parse Arguments

Extract audit type and URL from `$ARGUMENTS`:

1. **Audit type** — Look for one of: `i18n`, `a11y`, `console`, `network`, `forms`, `full`
2. **URL** — Look for a URL starting with `http` or `localhost`

If audit type is missing, use AskUserQuestion with options:
- "i18n" — Translation & localization issues (missing keys, text overflow, date/number formats)
- "a11y" — Accessibility compliance (keyboard nav, ARIA, contrast, screen reader)
- "console" — JavaScript errors & warnings (exceptions, failed resources, CSP)
- "network" — API failures & performance (4xx/5xx, slow responses, CORS)
- "forms" — Form validation & submission (required fields, edge cases, double-submit)
- "full" — All checklists combined (comprehensive audit)

If URL is missing, use AskUserQuestion: "What URL should I audit? (e.g., http://localhost:3000)"

### Phase 2: Load Targeted Checklist

Read the relevant reference file from `qa-web-testing/references/` (use Glob to find):

| Audit Type | Reference File | Section |
|------------|---------------|---------|
| `i18n` | `i18n-a11y-checks.md` | Internationalization section |
| `a11y` | `i18n-a11y-checks.md` | Accessibility section |
| `console` | `technical-checks.md` | Console Monitoring section |
| `network` | `technical-checks.md` | Network Monitoring section |
| `forms` | `functional-checks.md` | Forms section |
| `full` | All 3 reference files | All sections |

Also read `qa-foundations/SKILL.md` for severity and oracle classification.

### Phase 3: Check Browser and Authenticate

Same as `/qa-explore` Phases 2 and 4:
1. Verify browser connection with `list_pages`
2. Handle authentication if needed

### Phase 4: Launch Targeted Explorer

Dispatch the `qa-hunter:qa-explorer` agent with specific audit focus:

```
Task tool (qa-hunter:qa-explorer):
  "Run a targeted [audit-type] audit on [URL].

   FOCUS: Only test [audit-type] checklist items. Be systematic —
   go through every item on the checklist below.

   Checklist to follow:
   [paste the relevant checklist content from the reference file]

   For each checklist item:
   1. Navigate to relevant pages
   2. Perform the test action
   3. Record PASS or FAIL
   4. If FAIL: document as a finding with full evidence

   Produce a checklist results table AND raw findings for failures."
```

### Phase 5: Compile and Present

1. Dispatch `qa-hunter:qa-reporter` with the raw findings (same as `/qa-explore` Phase 6)
2. Present the report with an additional **Checklist Coverage** section:

```markdown
### Checklist Coverage: [audit-type]
| # | Check Item | Status | Bug ID |
|---|-----------|--------|--------|
| 1 | [item] | PASS/FAIL | [BUG-xxx or —] |
| 2 | [item] | PASS/FAIL | [BUG-xxx or —] |
| ... | ... | ... | ... |

**Coverage:** [X/Y] items tested ([Z]% coverage)
```

3. Highlight any FAIL items prominently

### Phase 6: Offer Follow-Up

"**Audit complete: [audit-type] on [URL]. Score: [PASS count]/[total] checks passed.**

1. **Run another audit type** — Test a different checklist on the same app
2. **Deep dive on failures** — Re-test specific failed items
3. **Save report** — Write to `qa-audit-[type]-YYYY-MM-DD.md`
4. **Full exploration** — Run `/qa-explore` for broader bug hunting
5. **Done** — End the QA session

Which would you like?"

## Key Principles

- **Systematic** — Unlike free exploration, this goes through every checklist item
- **Checklist-driven** — The reference file IS the test plan. Cover every item.
- **Measurable** — Produce a pass/fail count and coverage percentage
- **Targeted** — Stay focused on the selected audit type. Don't drift into other areas.
- **Evidence-based** — Same evidence requirements as `/qa-explore`
