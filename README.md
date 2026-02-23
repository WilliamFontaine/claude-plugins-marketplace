# Claude Plugins Marketplace

Custom [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugins for design, UX, and development workflows.

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

Each skill includes detailed reference files (48,000+ words of UX knowledge).

## Installation

```bash
# Clone the repo
git clone git@github.com:WilliamFontaine/claude-plugins-marketplace.git

# Install the plugin locally
claude plugin add ./plugins/ux-expert
```

Or add manually to `~/.claude/plugins/installed_plugins.json`:

```json
"ux-expert@local": [
  {
    "scope": "user",
    "installPath": "/absolute/path/to/plugins/ux-expert",
    "version": "1.0.0"
  }
]
```

Then enable in `~/.claude/settings.json`:

```json
"enabledPlugins": {
  "ux-expert@local": true
}
```

## Structure

```
plugins/
└── ux-expert/
    ├── .claude-plugin/
    │   └── plugin.json
    ├── agents/
    │   ├── ux-researcher.md
    │   ├── ux-designer.md
    │   └── ux-auditor.md
    ├── commands/
    │   ├── ux-design.md
    │   ├── ux-validate.md
    │   └── ux-audit.md
    └── skills/
        ├── ux-foundations/
        ├── ux-landing-page/
        ├── ux-saas-app/
        ├── ux-admin-portal/
        └── ux-audit-framework/
```

## License

[MIT](LICENSE)
