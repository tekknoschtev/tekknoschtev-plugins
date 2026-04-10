# Testing Standards

These rules are enforced when Claude assists with test-related code in projects that use this plugin.

## Test Coverage Requirements

- **New features**: Every new feature must include tests before it is considered complete. At minimum: happy path + one edge case + one negative case.
- **Bug fixes**: Every bug fix must include a regression test that fails without the fix and passes with it.
- **Refactors**: Refactored code must maintain existing test coverage. If tests break due to refactoring, update them — do not delete them unless the tested behavior was intentionally removed.

## Test Organization

- Test files live next to the code they test, named `<module>.test.<ext>` (e.g., `UserService.test.ts`).
- Group tests with `describe` blocks that match the module/class/function being tested.
- Name tests as behavior descriptions: `it('returns an error when the user is not authorized')`, not `it('test case 3')`.
- One logical assertion per test. Multiple `expect` calls are fine if they verify a single behavior.

## Test Quality

- **No skipped tests on main**: `it.skip`, `xit`, `@Disabled`, `@pytest.mark.skip` — these must not be merged to the main branch. Fix or remove them.
- **No sleep in tests**: Avoid `sleep()` or fixed delays. Use polling, waitFor utilities, or mock timers instead.
- **No test interdependence**: Each test must pass in isolation. Never rely on execution order or shared mutable state between tests.
- **No snapshot overuse**: Snapshot tests are acceptable for serialized output (e.g., API responses, config). Do not use snapshots to test UI rendering — they are brittle and rarely catch real bugs.
- **Deterministic data**: Use factories or builders for test data, not production database copies. Tests must produce the same result every run.

## Test Types & When to Use Them

| Type | Scope | When Required |
|------|-------|---------------|
| **Unit** | Single function/class in isolation | Always — this is the default test type |
| **Integration** | Multiple components working together | When the feature spans service boundaries, database calls, or API interactions |
| **End-to-end** | Full user workflow | Critical user journeys only — sign up, checkout, core CRUD flows |
| **Contract** | API request/response shape | When the service exposes or consumes an API used by other teams |

## Mocking Guidelines

- Mock external dependencies (APIs, databases, third-party services), not internal modules.
- If you find yourself mocking more than 2-3 things in a single test, the code under test may need refactoring — flag this rather than adding more mocks.
- Prefer fakes (in-memory implementations) over mocks for complex dependencies like databases when integration tests aren't feasible.
- Always verify that mocked behavior matches the real dependency's contract.

## Naming Conventions

- Test files: `<Module>.test.<ext>`
- Test factories: `create<Entity>` (e.g., `createUser`, `createOrder`)
- Test fixtures: `<entity>Fixture` or placed in a `__fixtures__` directory
- Test utilities: placed in a `__testutils__` or `test/helpers` directory
