---
description: "Design UX complet avec recherche et specs"
argument-hint: "[context-type] [description]"
model: opus
---

# UX Design Pipeline

Orchestrate a complete UX design workflow: research best practices, then produce a structured UX Specification ready for implementation.

**Arguments:** $ARGUMENTS

## Workflow

### Phase 1: Parse Context

1. Parse `$ARGUMENTS` to extract:
   - **Context type** (first word): `landing-page`, `saas-app`, `admin-portal`
   - **Description** (remaining words): what the user wants to build

2. If context type is missing or unclear, ask the user:

   Use AskUserQuestion with these options:
   - "Landing Page" — Marketing/conversion page
   - "SaaS App" — Application dashboard or feature
   - "Admin Portal" — Back-office or management interface

3. If description is missing, ask: "Describe briefly what you want to build."

### Phase 2: Load Knowledge Base

1. Read the `ux-foundations` skill for universal UX principles (use Glob to find `skills/ux-foundations/SKILL.md`)
2. Read the context-specific skill:
   - `landing-page` -> `skills/ux-landing-page/SKILL.md`
   - `saas-app` -> `skills/ux-saas-app/SKILL.md`
   - `admin-portal` -> `skills/ux-admin-portal/SKILL.md`
3. Keep these principles in mind for the full pipeline

### Phase 3: Research

Launch the `ux-researcher` agent using the Task tool:

```
Task tool (ux-expert:ux-researcher):
  "Research UX best practices for: [context-type] — [description].
   Produce a complete Research Brief following your output format."
```

Wait for the Research Brief to be returned.

### Phase 4: Design

Launch the `ux-designer` agent using the Task tool, passing the Research Brief:

```
Task tool (ux-expert:ux-designer):
  "Create a UX Specification for: [context-type] — [description].

   Research Brief:
   [paste the complete Research Brief from Phase 3]

   Produce a complete UX Spec following your output format."
```

Wait for the UX Spec to be returned.

### Phase 5: Present Results

1. Present the UX Spec to the user in a clear, readable format
2. Highlight the most important design decisions and why they were made
3. Call out any trade-offs or areas where the user should make a choice

### Phase 6: Next Steps

Offer transition options:

"**UX Spec complete. Next steps:**

1. **Implement with frontend-design** — Use the `frontend-design` skill to turn this spec into code
2. **Refine the spec** — Adjust specific sections based on your feedback
3. **Audit later** — Use `/ux-audit` after implementation to verify UX quality

Which would you like to do?"

If the user chooses implementation, provide the UX Spec Section 8 (Implementation Instructions) as context for the frontend-design skill or development workflow.

## Key Principles

- **Research before design**: Always gather data before making design decisions
- **Structured output**: The UX Spec must follow the exact template format for agent-to-agent consumption
- **Stack agnostic**: Never prescribe specific frameworks or libraries in the UX Spec
- **Accessibility first**: WCAG 2.2 AA is the minimum standard in all designs
- **Actionable**: Every recommendation must be specific enough to implement directly
