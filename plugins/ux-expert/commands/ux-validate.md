---
description: "Validation UX rapide d'un composant ou page"
argument-hint: "[file-path or description]"
model: sonnet
---

# Quick UX Validation

Perform a fast, conversational UX review of a component or page during development. Not a full audit — just the most impactful observations to guide the developer.

**Input:** $ARGUMENTS

## Workflow

### Step 1: Identify What to Validate

Parse `$ARGUMENTS`:

- **If a file path** (contains `/` or `.`): Read the file with the Read tool
- **If a description**: Use it as the evaluation context
- **If empty**: Ask the user what they want to validate using AskUserQuestion

### Step 2: Detect Context Type

From the code or description, identify the interface type:

- Look for hero sections, CTAs, pricing -> `landing-page`
- Look for dashboards, sidebars, navigation, settings -> `saas-app`
- Look for data tables, CRUD forms, user management -> `admin-portal`
- If unclear, default to general UX principles

### Step 3: Load Knowledge

1. Read the `ux-foundations` skill (use Glob to find `skills/ux-foundations/SKILL.md`)
2. Read the detected context-specific skill
3. If needed for a specific pattern, read the relevant reference file

### Step 4: Evaluate

Review the code or description against UX principles. Focus on:

1. **Visual hierarchy** — Is the most important content prominent?
2. **Accessibility** — Are there obvious WCAG violations? (missing alt text, low contrast, no keyboard nav)
3. **User flow** — Can users accomplish their goal without friction?
4. **Feedback states** — Are loading, error, empty, and success states handled?
5. **Mobile readiness** — Will this work on smaller screens?
6. **Context-specific patterns** — Does it follow best practices for its type?

### Step 5: Respond Conversationally

Output format — keep it short and actionable:

```markdown
## UX Quick Check

**What works well:**
- [Positive observation 1]
- [Positive observation 2]

**Points d'attention:**
1. **[Issue]** — [Why it matters] -> [Quick fix suggestion]
2. **[Issue]** — [Why it matters] -> [Quick fix suggestion]
3. ...max 5 most impactful issues

**Quick wins:**
- [Immediately actionable improvement]
- [Immediately actionable improvement]
```

## Key Principles

- **Fast and focused**: This is NOT a full audit. Maximum 5 issues, ordered by impact.
- **Conversational tone**: Write as a colleague giving friendly feedback, not a formal report.
- **Actionable**: Every point must include a concrete fix suggestion.
- **Balanced**: Always acknowledge what's done well before pointing out issues.
- **No false alarms**: Only flag issues you're confident about. When in doubt, skip it.

## When to Suggest a Full Audit

If you find more than 3 critical issues, end with:

"There are several significant UX concerns here. Consider running `/ux-audit` for a comprehensive evaluation with scoring and a prioritized action plan."
