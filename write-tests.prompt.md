---
description: "Write unit tests for selected code or a file. Use when: writing tests, creating test cases, adding unit tests, testing a service, testing a function, testing a component"
argument-hint: "File path or describe the code to test"
agent: "agent"
tools: [vscode/askQuestions, vscode/toolSearch, execute, read, agent, edit, search]
---

## Write Unit Tests

### Step 1 — Prime (Silent)

Before doing anything else:

1. Read `package.json` to identify the test framework (Vitest, Jest, etc.) and the test run command.
2. Search for 1-2 existing test files in the project (e.g. `*.test.ts`, `*.spec.ts`). Read them to extract:
   - Test structure (`describe/it`, `describe/test`, flat)
   - Mocking approach (`vi.fn()`, `vi.mock()`, `vi.spyOn()`, `jest.fn()`, etc.)
   - Test data style (inline literals, factory functions, fixtures)
   - Setup/teardown patterns (`beforeEach`, `beforeAll`, inline)
   - File naming and co-location conventions

If no existing test files are found, use these defaults:
- **Framework**: Vitest
- **Structure**: `describe/it` BDD style — `it('should ...')`
- **Mocking**: `vi.fn()` for injected dependencies, `vi.spyOn()` for module-level methods, `vi.mock()` when module imports can't be injected
- **Test data**: Factory functions — e.g. `const createMockUser = (overrides = {}) => ({ id: '1', email: 'test@test.com', ...overrides })`
- **Setup**: `beforeEach` with fresh instances to prevent test pollution
- **Co-location**: test file lives next to the source file — `auth.service.test.ts` next to `auth.service.ts`

Priming is fully silent unless something critical is missing.

---

### Step 2 — Analyse the Target Code

Read the selected code or the file provided.

For each function, method, or component, understand:
- What it does
- Its inputs and expected outputs
- Its dependencies (what it calls, what could be mocked)
- Its failure scenarios and edge cases

---

### Step 3 — List Test Cases

Before writing any code, output a numbered list of proposed test cases grouped by function/method/component:

```
UserService.findById
  1. Returns user when ID exists
  2. Returns null when ID does not exist
  3. Throws DatabaseError when database call fails

UserService.create
  4. Creates and returns user with hashed password
  5. Throws ConflictError when email already exists
  6. Does not store plain text password
```

Ask: **"Add, remove, or adjust any test cases, then say 'approve' to proceed."**

Wait for explicit approval before continuing.

---

### Step 4 — Write the Tests

After approval, write the full test file:
- Match the style extracted in Step 1 (or the defaults if no existing tests were found)
- Cover every approved test case
- Use descriptive test names that read as sentences
- Keep each test focused on one behaviour — no multi-assertion tests unless they test the same logical outcome
- Mock only what crosses a boundary (DB, HTTP, file system, external services) — do not mock the code under test

Show the complete test file in a code block with the filename as the header.

Ask: **"Write this to file? (yes / no / adjust)"**

If the user says adjust, make the changes and show the updated file before asking again.

---

### Step 5 — Optionally Run the Tests

After writing the file, ask: **"Run the tests now? (yes / no)"**

If yes, run the test command from `package.json` (e.g. `npm run test` or `npx vitest run <filename>`). Read the output — if any tests fail, diagnose the failure and offer a fix.

## Complaint Logging

If the user says anything like "log a complaint", "report a bug", "this isn't working", or "something is wrong with you" — call the `Bug Reporter` agent, passing:
- **filename**: `write-tests.prompt.md`
- **complaint**: the user's description of what went wrong

Show the user the Bug Reporter's output before continuing.
