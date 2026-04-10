# tekknoschtev-plugins

A sample Claude Code plugin marketplace for testing and validating plugin distribution before rolling out to a team.

## Quick Start

### Register the marketplace

Add this repository as a plugin marketplace in your Claude Code settings. In your project's `.claude/settings.json` (or your user-level settings), add:

```json
{
  "plugins": {
    "marketplaces": [
      "https://github.com/tekknoschtev/tekknoschtev-plugins"
    ]
  }
}
```

Or, if working with a local clone:

```json
{
  "plugins": {
    "marketplaces": [
      "/path/to/tekknoschtev-plugins"
    ]
  }
}
```

### Install a plugin

Once the marketplace is registered, install individual plugins by name:

```
/install-plugin po-plugin
/install-plugin ui-dev-plugin
/install-plugin qa-plugin
```

## What's Included

### po-plugin — Product Owner Toolkit

Tools for creating and refining JIRA tickets with codebase-aware context.

| Type | Name | Description |
|------|------|-------------|
| Skill | **story-creator** | Create structured user stories from a conversation or brief description. Produces tickets with business problem, acceptance criteria, open items, and references. |
| Skill | **ticket-refiner** | Read an existing ticket and enrich it with technical context discovered from the codebase — relevant code paths, affected models/APIs, risks, and test coverage gaps. |

### ui-dev-plugin — Frontend Developer Toolkit

Accessibility auditing, component conventions, and commit-time quality checks for frontend engineers.

| Type | Name | Description |
|------|------|-------------|
| Skill | **a11y-audit** | Audit a component or page for WCAG 2.1 AA compliance. Checks semantic HTML, ARIA usage, keyboard navigation, color contrast, images, and forms. Produces a severity-ranked report with specific fix suggestions. |
| Hook | **pre-commit** | Block commits that introduce ESLint warnings or accessibility violations (missing alt text, invalid anchors, click handlers without keyboard support). |
| Rule | **ui-conventions** | Enforce component structure, prop patterns, accessibility requirements, styling rules, state management practices, and naming conventions. |

### qa-plugin — QA Engineer Toolkit

Test planning, structured bug reporting, and testing standards enforcement for QA engineers.

| Type | Name | Description |
|------|------|-------------|
| Skill | **test-plan-generator** | Generate a structured test plan from a ticket — happy path, edge case, negative, and integration tests with steps and expected results. Searches the codebase for existing coverage. |
| Skill | **bug-report-creator** | Walk through bug reproduction and generate a structured report with severity, repro steps, expected/actual behavior, impact, workarounds, and technical notes from codebase investigation. |
| Rule | **testing-standards** | Enforce coverage requirements, test organization, quality rules (no skipped tests on main, no sleep, deterministic data), test type guidance, and mocking best practices. |

## Jira Integration

Several skills in this marketplace are designed to work with the **Jira MCP server**. When the Jira MCP is available, skills will:

- **story-creator**: Create tickets directly in your Jira project
- **ticket-refiner**: Fetch tickets by ID and update them with technical context
- **test-plan-generator**: Attach test plans to tickets or create linked sub-tasks
- **bug-report-creator**: File bug tickets with the correct issue type and fields

When the Jira MCP is not available, all skills gracefully fall back to outputting markdown that can be copied into your issue tracker manually.

## Project Structure

```
tekknoschtev-plugins/
  .claude-plugin/
    marketplace.json          # Marketplace manifest
  plugins/
    po-plugin/
      .claude-plugin/
        plugin.json           # Plugin manifest
      skills/
        story-creator/
          SKILL.md
        ticket-refiner/
          SKILL.md
    ui-dev-plugin/
      .claude-plugin/
        plugin.json
      skills/
        a11y-audit/
          SKILL.md
      hooks/
        pre-commit.md
      rules/
        ui-conventions.md
    qa-plugin/
      .claude-plugin/
        plugin.json
      skills/
        test-plan-generator/
          SKILL.md
        bug-report-creator/
          SKILL.md
      rules/
        testing-standards.md
```

## Contributing

To add a new plugin:

1. Create a directory under `plugins/` with a `.claude-plugin/plugin.json` manifest
2. Add skills, hooks, and/or rules in subdirectories
3. Register the plugin in `.claude-plugin/marketplace.json`
4. Update this README with the plugin's description and contents
