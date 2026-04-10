# Pre-Commit Hook: UI Quality Gate

This hook runs before each commit to catch common frontend quality issues early.

## Setup

Add the following to your project's `.claude/settings.json`:

```json
{
  "hooks": {
    "pre-commit": [
      {
        "command": "npx eslint --max-warnings 0 $(git diff --cached --name-only --diff-filter=ACM -- '*.ts' '*.tsx' '*.js' '*.jsx')",
        "description": "Lint staged frontend files with zero warnings tolerance"
      },
      {
        "command": "npx eslint --no-eslintrc --rule '{\"jsx-a11y/alt-text\": \"error\", \"jsx-a11y/anchor-is-valid\": \"error\", \"jsx-a11y/no-noninteractive-element-interactions\": \"error\", \"jsx-a11y/click-events-have-key-events\": \"error\", \"jsx-a11y/no-static-element-interactions\": \"error\"}' $(git diff --cached --name-only --diff-filter=ACM -- '*.tsx' '*.jsx')",
        "description": "Block commits with accessibility violations in JSX files"
      }
    ]
  }
}
```

## What It Checks

| Check | Why |
|-------|-----|
| Zero ESLint warnings on staged files | Warnings that pile up become invisible — enforce them at commit time |
| `alt-text` on images | Screen readers need alt text; decorative images need explicit `alt=""` |
| Valid anchors | `<a>` without `href` or with `href="#"` breaks keyboard nav and screen readers |
| Key events on interactive elements | Click handlers on non-interactive elements need `onKeyDown`/`onKeyUp` for keyboard users |
| No static element interactions | `<div onClick>` without a role is invisible to assistive technology |

## Dependencies

These checks assume the project has:
- `eslint` installed
- `eslint-plugin-jsx-a11y` installed (for accessibility rules)

If your project doesn't use JSX, adapt the file globs and rules to match your templating approach.

## Customization

- To add more `jsx-a11y` rules, see the [full rule list](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y#supported-rules).
- To make the hook less strict during early adoption, change `"error"` to `"warn"` for specific rules and remove `--max-warnings 0`.
