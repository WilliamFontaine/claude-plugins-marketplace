---
description: "Audit UX complet avec scoring et recommandations priorisees"
argument-hint: "[scope: file-path, component-name, or 'project']"
model: opus
---

# Comprehensive UX Audit

Perform a full UX audit with quantitative scoring across 10 dimensions, Nielsen heuristic evaluation, issue classification by severity, and a prioritized action plan.

**Scope:** $ARGUMENTS

## Workflow

### Phase 1: Determine Scope

Parse `$ARGUMENTS`:

- **File path** (contains `/` or `.`): Audit that specific file
- **Component name**: Search the codebase for the component using Grep and Glob
- **"project"**: Audit the entire project's user-facing interface
- **Empty**: Ask the user what to audit using AskUserQuestion with options:
  - "Entire project" — Full UX audit of all user-facing pages
  - "Specific page/file" — Audit a single page or file (ask for path)
  - "Specific component" — Audit a single component (ask for name)

### Phase 2: Identify Context

1. If a file was specified, read it to determine the interface type
2. Classify as: `landing-page`, `saas-app`, `admin-portal`, or `other`
3. If the scope is "project", scan the project structure to identify all UI files and determine the primary interface type

### Phase 3: Load Knowledge Base

1. Read the `ux-audit-framework` skill (use Glob to find `skills/ux-audit-framework/SKILL.md`)
2. Read the `ux-foundations` skill for universal principles
3. Read the context-specific skill for the detected interface type
4. The ux-auditor agent will load additional references as needed

### Phase 4: Launch Auditor Agent

Launch the `ux-auditor` agent using the Task tool:

```
Task tool (ux-expert:ux-auditor):
  "Perform a comprehensive UX audit.

   Scope: [scope description]
   Context type: [detected type]
   Files to audit: [list of files if known]

   Produce a complete Audit Report following your output format,
   including scores for all 10 dimensions, Nielsen heuristic evaluation,
   issues classified by severity, and a prioritized action plan."
```

Wait for the Audit Report to be returned.

### Phase 5: Present Results

1. Present the full Audit Report to the user
2. Highlight the global score prominently
3. Call out any critical (severity 4) issues that need immediate attention
4. Summarize the top 3 quick wins from the action plan

### Phase 6: Offer Follow-Up Actions

"**Audit complete. Score: [XX/100]. What would you like to do?**

1. **Fix critical issues now** — Address severity 3-4 issues immediately
2. **Create task list** — Generate a TodoWrite with all recommended actions
3. **Deep dive on a dimension** — Explore a specific low-scoring area in detail
4. **Re-audit after fixes** — Run the audit again to measure improvement
5. **Design improvements** — Use `/ux-design` to redesign low-scoring areas

Which would you like to do?"

If the user wants to fix issues, help them work through the action plan in priority order, starting with the "Immediate" items.

## Key Principles

- **Quantitative**: Every evaluation produces numerical scores. No vague assessments.
- **Evidence-based**: Every issue cites a specific file and line number.
- **Prioritized**: Recommendations are ordered by impact x effort, not just severity.
- **Comprehensive**: All 10 dimensions and all 10 Nielsen heuristics must be evaluated.
- **Actionable**: Every issue includes a concrete, implementable fix.
- **Balanced**: Acknowledge strengths alongside weaknesses.
