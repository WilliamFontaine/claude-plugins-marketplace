# Claude Plugins Marketplace

Custom [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugins for design, UX, QA, and development workflows.

## Plugins

### ux-expert

Expert UI/UX plugin covering the full design lifecycle: conception, validation, and audit.

**Commands:**

| Command | Description |
|---------|-------------|
| `/ux-design` | Full UX design pipeline with web research and structured specs |
| `/ux-validate` | Quick UX validation during development |
| `/ux-audit` | Comprehensive UX audit with scoring (/100) and action plan |

**Agents:**

| Agent | Role |
|-------|------|
| `ux-researcher` | Web research on UX best practices, trends, and benchmarks |
| `ux-designer` | Produces structured UX Specifications with wireframes |
| `ux-auditor` | Evaluates interfaces against Nielsen heuristics and WCAG |

**Skills (auto-activated):**

| Skill | Domain |
|-------|--------|
| `ux-foundations` | Nielsen heuristics, psychology laws, WCAG 2.2 AA, design systems |
| `ux-landing-page` | Conversion optimization, hero sections, CTAs, pricing tables |
| `ux-saas-app` | Navigation, onboarding, dashboards, notifications |
| `ux-admin-portal` | Data tables, CRUD, bulk actions, RBAC, search/filters |
| `ux-audit-framework` | Severity scales, scoring rubric, audit methodology |

---

### qa-hunter

Automated exploratory bug hunting on web applications. Explores via browser automation, detects bugs, and produces structured reports.

**Prerequisites:** Requires a Chrome DevTools MCP server (launch with `claude --chrome`).

**Commands:**

| Command | Description |
|---------|-------------|
| `/qa-explore` | Explore a running web app to find bugs (main command) |
| `/qa-audit` | Systematic audit with specific checklists (i18n, a11y, forms, etc.) |
| `/qa-report` | Compile raw findings into a structured bug report |

**Agents:**

| Agent | Role |
|-------|------|
| `qa-explorer` | Navigates apps via browser, interacts with UI, discovers bugs |
| `qa-reporter` | Validates findings, filters false positives, compiles reports |

**Skills (auto-activated):**

| Skill | Domain |
|-------|--------|
| `qa-foundations` | Oracle layers, bug severity (S0-S4), confidence levels, report format |
| `qa-web-testing` | Testing checklists: forms, navigation, CRUD, console, network, i18n, a11y |

## Installation

```bash
# Add the marketplace
claude plugin marketplace add WilliamFontaine/claude-plugins-marketplace

# Install a plugin
claude plugin install ux-expert@willdev-plugins
claude plugin install qa-hunter@willdev-plugins
```

Each plugin will be available in your next Claude Code session.

## Structure

```
plugins/
├── ux-expert/
│   ├── .claude-plugin/
│   │   └── plugin.json
│   ├── agents/
│   ├── commands/
│   └── skills/
│       ├── ux-foundations/
│       ├── ux-landing-page/
│       ├── ux-saas-app/
│       ├── ux-admin-portal/
│       └── ux-audit-framework/
└── qa-hunter/
    ├── .claude-plugin/
    │   └── plugin.json
    ├── agents/
    ├── commands/
    └── skills/
        ├── qa-foundations/
        └── qa-web-testing/
```

## License

[MIT](LICENSE)
