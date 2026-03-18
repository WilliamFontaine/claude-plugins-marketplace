---
description: "Compile QA findings into a structured bug report"
argument-hint: "[optional: path to raw findings file]"
model: sonnet
---

# Compile QA Bug Report

Take raw QA findings (from exploration, audit, or manual testing) and compile them into a structured, prioritized bug report.

**Input:** $ARGUMENTS

## Workflow

### Phase 1: Gather Findings

Parse `$ARGUMENTS`:

- **File path** (contains `/` or `.`): Read the file with the Read tool
- **Empty**: Use findings from the current conversation context
- **No findings available**: "No findings to compile. Run `/qa-explore` or `/qa-audit` first to generate findings, or provide a file path to raw findings."

If findings come from the conversation context, extract all bug-related content (findings, errors, observations) from the recent messages.

### Phase 2: Launch Reporter

Dispatch the `qa-hunter:qa-reporter` agent:

```
Task tool (qa-hunter:qa-reporter):
  "Compile these raw findings into a structured QA Bug Report.

   Raw findings:
   [paste all findings gathered in Phase 1]

   Follow the report template. Filter false positives,
   validate severity and confidence, deduplicate,
   and produce the complete report."
```

Wait for the compiled report.

### Phase 3: Present Report

Present the full compiled report to the user.

### Phase 4: Offer to Save

Use AskUserQuestion: "Save this report to a file?"
- "Yes" — Write to `qa-report-YYYY-MM-DD.md` in the project root
- "Yes, custom path" — Ask for the desired file path
- "No" — End

If saving, write the report using the Write tool and confirm the path.

## Key Principles

- **Formatting only** — This command formats, it doesn't explore or test
- **Filter noise** — The reporter agent filters false positives and duplicates
- **Prioritized** — Bugs are ordered by severity and confidence
- **Complete** — Every section of the report template is filled
