# QA Hunter

Automated exploratory bug hunting on web applications. This Claude Code plugin explores running web apps via browser automation, detects bugs through systematic testing, and produces structured bug reports.

## Prerequisites

QA Hunter uses browser automation to test web applications. You need a Chrome DevTools MCP server configured. The recommended option:

- **Claude Code with Chrome:** Launch with `claude --chrome`

## Commands

| Command | Description |
|---------|-------------|
| `/qa-explore [URL or scope]` | Explore a running web app to find bugs |
| `/qa-audit [type] [URL]` | Run a systematic audit with specific checklists |
| `/qa-report [file]` | Compile raw findings into a structured bug report |

### `/qa-explore`

The main command. Navigates your app, interacts with forms and buttons, monitors console and network for errors, and reports bugs.

```
/qa-explore http://localhost:3000
/qa-explore the mandate creation flow
/qa-explore http://localhost:3000/admin
```

### `/qa-audit`

Runs a systematic audit against a specific checklist. More thorough than `/qa-explore` for a given area, but narrower in scope.

Available audit types: `i18n`, `a11y`, `console`, `network`, `forms`, `full`

```
/qa-audit i18n http://localhost:3000
/qa-audit a11y http://localhost:3000/dashboard
/qa-audit full http://localhost:3000
```

### `/qa-report`

Compiles raw findings into a structured bug report. Can take findings from the conversation or from a file.

```
/qa-report
/qa-report ./raw-findings.md
```

## Agents

| Agent | Role |
|-------|------|
| `qa-explorer` | Navigates the app via browser tools, interacts with UI elements, monitors console and network errors, classifies bugs by oracle type |
| `qa-reporter` | Validates findings, filters false positives (framework noise, dev-mode warnings), deduplicates, and compiles the structured bug report |

## Skills

| Skill | Domain |
|-------|--------|
| `qa-foundations` | Bug classification methodology: oracle layers (Crash/Convention/Permission/Visual/Semantic), severity scale (S0-S4), confidence levels, report template |
| `qa-web-testing` | Comprehensive testing checklists: forms, navigation, CRUD, authentication, search, console errors, network failures, i18n, accessibility, edge cases |

## Bug Report Format

QA Hunter produces structured reports with:

- **Summary** — Bug count by severity and confidence
- **Individual bugs** — Each with severity, confidence, oracle type, steps to reproduce, expected vs actual, and evidence (console errors, network failures, visual descriptions)
- **Tested flows** — What was tested, with PASS/FAIL status
- **Not tested** — Areas that were out of scope

## Key Principles

- **Report only** — Never modifies source code. Only observes, interacts, and reports.
- **Evidence-based** — Every bug includes console output, network response, or visual evidence.
- **Black-box** — Tests from the user's perspective, with no access to source code during exploration.
- **Role-aware** — Can test with multiple user credentials to find permission boundary issues.
- **Generic** — Works on any web application. App-specific context comes from project CLAUDE.md files.

## Installation

```bash
claude plugin install qa-hunter@willdev-plugins
```
