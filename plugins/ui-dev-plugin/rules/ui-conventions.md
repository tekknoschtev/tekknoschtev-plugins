# UI Development Conventions

These rules are enforced when Claude assists with frontend code in projects that use this plugin.

## Component Structure

- One component per file. The file name must match the default export name in PascalCase (e.g., `UserProfile.tsx` exports `UserProfile`).
- Co-locate component files: keep the component, its styles, its tests, and its stories in the same directory.
  ```
  components/
    UserProfile/
      UserProfile.tsx
      UserProfile.styles.ts   (or .css/.module.css)
      UserProfile.test.tsx
      UserProfile.stories.tsx
      index.ts                (re-export only)
  ```
- Keep components focused. If a component exceeds ~200 lines, consider extracting sub-components into the same directory.

## Props & Types

- Define prop types as a named `interface` (not inline or `type` alias) directly above the component: `interface UserProfileProps { ... }`.
- Destructure props in the function signature: `function UserProfile({ name, avatar }: UserProfileProps)`.
- Avoid boolean prop names that imply negation (`isNotVisible`). Prefer positive names (`isVisible`).
- Provide sensible defaults for optional props via default parameter values rather than internal fallback logic.

## Accessibility (Non-Negotiable)

- Every `<img>` must have an `alt` attribute. Decorative images use `alt=""`.
- Interactive elements must be focusable and operable via keyboard. Use native `<button>` and `<a>` over `<div onClick>`.
- Form inputs must have an associated `<label>` or `aria-label`.
- Color must never be the sole means of conveying information.
- Modals must trap focus and return focus to the trigger on close.

## Styling

- Use the project's established styling approach (CSS modules, styled-components, Tailwind, etc.) — do not introduce a second system.
- Avoid inline styles except for truly dynamic values (e.g., calculated positions).
- Use design tokens / theme variables for colors, spacing, and typography. Do not hard-code hex values or pixel sizes.

## State Management

- Keep state as local as possible. Lift state only when sibling components need it.
- Avoid prop drilling beyond 2 levels — use context or the project's state management solution.
- Side effects belong in `useEffect` (React) or the framework's equivalent, not in render logic.

## Testing

- Every component must have at least one test that verifies it renders without crashing.
- Test user behavior, not implementation details — prefer `getByRole`, `getByLabelText` over `getByTestId`.
- Accessibility: include at least one test using an a11y testing utility (e.g., `jest-axe`) for components with interactive elements.

## Naming

- Components: `PascalCase`
- Hooks: `camelCase` starting with `use` (e.g., `useUserProfile`)
- Utilities: `camelCase`
- Constants: `UPPER_SNAKE_CASE`
- CSS classes / modules: `camelCase`
- Event handlers: `handleEventName` (e.g., `handleClick`, `handleSubmit`)
