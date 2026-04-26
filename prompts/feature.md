# Feature Prompts

---

## Prompt: New Feature (TDD First)

**When to use:** Building something new from a spec or ticket.
**Model:** Sonnet
**Token tip:** Write tests first. One correction round costs more tokens than the entire test-writing step.

### Template
```
Feature: {{feature name}}

Goal: {{what it should do, inputs, outputs}}

Constraints:
- {{e.g. no new dependencies}}
- {{e.g. must match existing error handling pattern}}

Do this in order:
1. Write failing tests. Cover: happy path, empty input, invalid input.
   Use {{Jest / Vitest / your framework}}. Tests must fail before implementation exists.
2. Write implementation to pass the tests.
3. Show me: test file, then implementation diff.
   Do not change the tests in step 2.
```

### Example
```
Feature: slugify utility

Goal: Takes a string (article title), returns a URL-safe slug.
Lowercases, replaces spaces and special chars with hyphens, strips leading/trailing hyphens.
Returns empty string for empty or whitespace-only input.

Constraints:
- No external dependencies
- Named export only from src/utils/slugify.js

Do this in order:
1. Write failing tests in tests/slugify.test.js
   Cover: normal title, special chars, empty string, whitespace-only, already-valid slug
2. Write implementation to pass the tests
3. Show test file, then implementation diff
```

---

## Prompt: Extend an Existing Feature

**When to use:** Adding new behavior to something that already exists.
**Model:** Sonnet
**Token tip:** Paste only the relevant function, not the whole file.

### Template
```
I need to extend an existing function.

Current function (do not change the signature):
{{paste the function}}

Add: {{describe exactly what new behavior to add}}

Tests that must still pass: {{test file path}}
New test cases to write first: {{describe the new cases to cover}}

Output: new test cases first, then the updated function diff.
```

### Example
```
I need to extend an existing function.

Current function:
export function slugify(str) {
  return str.toLowerCase().replace(/[^a-z0-9]+/g, '-').replace(/^-|-$/g, '')
}

Add: transliterate accented characters to ASCII before slugifying
(e.g. "café" → "cafe", "naïve" → "naive"). Emoji should be stripped.

Tests that must still pass: tests/slugify.test.js
New test cases to write first: accented chars, mixed ASCII/Unicode, emoji

Output: new test cases, then implementation diff.
```

---

## Prompt: Plan a Multi-File Feature

**When to use:** Any feature touching more than 2 files. Plan before you build.
**Model:** Sonnet
**Token tip:** Planning costs ~1K tokens. A wrong implementation costs 10K+ to reverse.

### Template
```
Plan this feature — do not write any code yet.

Feature: {{name and description}}
Files likely involved: {{list what you think will change}}

Output:
1. Each subtask with the file it touches
2. Order to implement (note dependencies)
3. Any ambiguities or missing info you need resolved

I'll approve the plan, then we implement subtask by subtask.
```
