# Copilot Instructions – Specs Agent

This repository uses **BDD (Behavior-Driven Development)** with Gherkin `.feature` files as the primary source of truth for application behavior. Follow the guidelines below whenever you assist with code in this project.

---

## Core Principles

1. **Feature files drive development.** Before writing or modifying any implementation code, read the relevant `.feature` files to understand the intended behavior.
2. **Keep specs and code in sync.** If you change behavior in code, update the corresponding `.feature` file (or flag the discrepancy clearly).
3. **Scenarios are acceptance criteria.** A feature is only complete when all its scenarios pass.

---

## Working with `.feature` Files

### Reading specs
- Treat every `Scenario` as a self-contained acceptance test.
- `Background` steps are preconditions shared by all scenarios in the file — account for them in your implementation.
- `Scenario Outline` + `Examples` tables mean the scenario runs once per row — your implementation must handle every combination.

### Writing new specs
Use valid Gherkin syntax and follow this style:

```gherkin
Feature: <short description of the feature>
  In order to <business value>
  As a <role>
  I want to <capability>

  Scenario: <description of the happy path>
    Given <an initial context>
    When <an action is taken>
    Then <an expected outcome>
```

Style rules:
- One behavior per scenario; keep scenarios to 5–7 steps.
- Write from the **user/stakeholder perspective**, not the implementation perspective.
- Use **declarative** style: describe *what* happens, not *how*.
- Avoid implementation detail in step text (no CSS selectors, no SQL, no method names).
- Use `Scenario Outline` + `Examples` for data-driven scenarios.
- Apply tags (`@smoke`, `@regression`, `@wip`) to organize runs.

### Updating existing specs
- Preserve the original intent of every scenario unless explicitly asked to change behavior.
- When a scenario is no longer valid, remove it and explain why in the PR description.
- Never silently skip or comment out a failing scenario — fix the code or update the spec with explicit justification.

---

## Step Definitions

- Each Gherkin step must map to exactly one step definition.
- Step definitions should be **thin** — delegate business logic to domain objects or service classes.
- Reuse existing step definitions before creating new ones; check the existing step definitions directory first.
- Name step definition files to mirror the feature file they serve (e.g., `login.feature` → `login.steps.ts`).

---

## Test Execution

- Run the BDD suite before and after making changes to catch regressions.
- If a scenario fails, report which step failed and the full error message.
- Do not modify tests to make them pass — fix the underlying implementation (unless the test itself is incorrect, in which case update the spec and explain the change).

---

## File Layout Conventions

```
features/
  <domain>/
    <feature-name>.feature
step-definitions/
  <domain>/
    <feature-name>.steps.<ext>
support/
  hooks.<ext>
  world.<ext>
```

Adapt this layout to the existing project structure if it already differs.

---

## General Coding Guidelines

- Write code that is easy to test in isolation (pure functions, dependency injection).
- Prefer small, focused functions that correspond to single Gherkin steps.
- Commit `.feature` files alongside the implementation code that satisfies them.
- Include a brief comment at the top of new step-definition files referencing the `.feature` file they implement.
