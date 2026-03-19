---
description: "Explore a running web app to find bugs"
argument-hint: "[URL or scope description, e.g. 'http://localhost:3000' or 'the mandate creation flow']"
model: opus
---

# Exploratory Bug Hunting

Explore a running web application to discover bugs through black-box testing. Navigate, interact, monitor errors, and report findings.

**Scope:** $ARGUMENTS

## Workflow

### Phase 1: Parse Scope

Parse `$ARGUMENTS`:

- **URL** (starts with `http` or `localhost`): Use as the starting point for exploration
- **Description** (no URL): Ask the user for the URL using AskUserQuestion: "What URL should I explore? (e.g., http://localhost:3000)"
- **Empty**: Ask the user using AskUserQuestion with options:
  - "Provide a URL" — I'll explore the app at a given URL
  - "Describe the scope" — I'll focus on a specific area you describe

### Phase 2: Check Browser Connection

1. Call `list_pages` to verify browser connection
2. If it fails or returns no pages: "Browser connection not available. Make sure you launched Claude Code with `claude --chrome`."
3. If connected, create a new page with `new_page` and navigate to the target URL

### Phase 3: Load Knowledge Base

1. Use Glob to find `skills/qa-foundations/SKILL.md` and `skills/qa-web-testing/SKILL.md`
2. Read both skills to understand oracle layers, severity scales, and testing checklists
3. Keep the testing methodology in mind for the exploration

### Phase 4: Handle Authentication

If the page shows a login form or the user mentioned authentication:

Use AskUserQuestion: "This page requires authentication. How should I proceed?"
- "No login needed" — I'll explore as an anonymous user
- "I'll log in manually first" — Log in yourself, then tell me when ready
- "Here are credentials" — Provide username and password (I won't store them)

If credentials are provided for multiple roles, test with each role to find permission boundary issues.

### Phase 5: Launch Explorer

Dispatch the `qa-hunter:qa-explorer` agent using the Task tool:

```
Task tool (qa-hunter:qa-explorer):
  "Explore the web application for bugs.

   Target URL: [URL]
   Scope: [scope description from user]
   Authentication: [credentials if provided, or 'anonymous']
   User role(s): [role names if provided]

   Follow your exploration process. Test the happy path first,
   then edge cases. Check console and network after every interaction.
   Report all findings in your structured format."
```

Wait for the raw findings to be returned.

### Phase 6: Compile Report

Take the explorer's raw findings and dispatch the `qa-hunter:qa-reporter` agent:

```
Task tool (qa-hunter:qa-reporter):
  "Compile these raw findings into a structured QA Bug Report.

   Application: [URL]
   Scope: [scope description]
   Date: [today's date]

   Raw findings:
   [paste complete raw findings from explorer]

   Filter false positives, validate severity, and produce
   the full report following the report template."
```

Wait for the compiled report.

### Phase 7: Present Results

1. Present the full bug report to the user
2. If S0 or S1 bugs were found, highlight them prominently at the top:
   - "**[count] critical bugs found that need immediate attention:**"
   - List each S0/S1 bug title and one-line summary
3. Show the summary statistics (total bugs, by severity, by confidence)

### Phase 8: Offer Follow-Up

"**Exploration complete. What would you like to do next?**

1. **Explore another area** — Test a different part of the application
2. **Deep dive on a bug** — Re-test a specific finding with more detail
3. **Save report to file** — Write the report to `qa-report-YYYY-MM-DD.md`
4. **Systematic audit** — Run `/qa-audit` for targeted checklists (i18n, a11y, forms, etc.)
5. **Done** — End the QA session

Which would you like?"

If the user chooses to explore another area, restart from Phase 1 with the new scope.

## Key Principles

- **Report only** — Never suggest code fixes. Only report bugs and evidence.
- **Evidence-based** — Every bug must have console, network, or visual evidence.
- **Scope-guided** — Stay within what the user asked to test. Don't wander.
- **Role-aware** — If multiple roles are provided, test permission boundaries.
- **Honest** — Report what works well (PASS flows), not just failures.
