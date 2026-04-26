# TDD Cycle Prompts

One prompt per step. Use them in order.

---

## Prompt 1: Write Failing Tests

**When to use:** Before any implementation exists. The spec is the prompt — not existing code.
**Model:** Sonnet
**Token tip:** Do NOT give Claude the implementation file. Tests must come from the spec, not reverse-engineered from code.

### Template
```
Write failing tests for: {{function or feature name}}

Spec:
{{describe what it does: inputs, outputs, edge cases, error conditions}}

Function signature: {{e.g. export function validateEmail(email: string): boolean}}
Test file: {{tests/name.test.js}}
Test runner: {{vitest / jest}}

Requirements:
- Minimum 5 tests: happy path, 2 edge cases, invalid input, null/undefined
- Every test must FAIL before implementation exists
- Use toBe(true) / toBe(false) — not toBeTruthy / toBeFalsy
- Test names describe behavior: "returns false for email without @"

Output: test file only. No implementation.
```

### Example
```
Write failing tests for: validateEmail

Spec: Takes any value. Returns true if it's a string in valid email format (after trimming).
Returns false otherwise. Never throws.

Function signature: export function validateEmail(email: string): boolean
Test file: tests/validate-email.test.js
Test runner: vitest

Requirements:
- Min 5 tests covering: valid email, missing @, empty string, null, non-string type
- Tests must fail before implementation exists
- Use toBe(true/false)

Output: test file only.
```

---

## Prompt 2: Implement to Pass Tests

**When to use:** After tests are written and confirmed failing.
**Model:** Sonnet
**Token tip:** List only the test file in "files you may read." The implementation should satisfy the tests, nothing more.

### Template
```
Implement to pass the failing tests.

Test file: {{tests/name.test.js}}
Implementation file: {{src/path/to/file.js}}

Files you may read:
- {{tests/name.test.js}}

Constraints:
- Make the minimum change to pass all tests
- Do not add functions or behavior beyond what the tests require
- Do not modify the test file

Output: implementation file only. End with: "Run: {{npm test -- name}}"
```

---

## Prompt 3: Run Tests and Report

**When to use:** After implementation. Confirms green before review.
**Model:** Haiku
**Token tip:** Haiku is enough for this — it's just running a command and summarizing output.

### Template
```
Run the tests: {{npm test -- name}}

Report:
- How many passed / failed
- If any failed: exact test name and the assertion that failed
- If all passed: confirm "All tests green. Ready for review."

Do not change any code.
```

---

## Prompt 4: Refactor Without Breaking Tests

**When to use:** After tests pass. Improving structure without changing behavior.
**Model:** Sonnet
**Token tip:** State the goal precisely — "better" is not a goal. All tests must pass unchanged.

### Template
```
Refactor: {{file or function name}}

Goal: {{what the code should look like after — be specific}}

Rules:
- All tests in {{test file}} must pass without modification
- Do not change the public API / exported signatures
- Do not add new functionality
- Show diff only, not the full file

After the diff: confirm "All tests still pass. No behavior changed."
```
