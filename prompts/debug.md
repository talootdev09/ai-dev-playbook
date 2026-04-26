# Debug Prompts

---

## Prompt: Isolate a Bug from an Error

**When to use:** You have an error message or wrong output but don't know the root cause yet.
**Model:** Sonnet
**Token tip:** Diagnose before fixing. Skipping to a fix without a confirmed root cause creates double bugs.

### Template
```
Bug: {{one-line description}}

Error / symptom:
{{paste exact error message and stack trace, or describe wrong output}}

Reproduction:
{{smallest code that triggers the bug, or steps to reproduce}}

Suspected location: {{file:function or "unknown"}}

Diagnose only — do not write a fix yet:
1. What is causing the bug (specific line/function)
2. Why it happens (the underlying reason)
3. What a fix needs to address

I'll confirm the diagnosis before you write any code.
```

### Example
```
Bug: Invoice totals wrong for fractional quantities

Error: calculateLineTotal({qty: 2.5, unitPrice: 19.99}) returns 39 instead of 49.975

Reproduction:
const total = calculateLineTotal({ qty: 2.5, unitPrice: 19.99 })
console.log(total) // 39 (wrong)

Suspected location: src/services/invoice.js — calculateLineTotal()

Diagnose only. No fix yet.
```

---

## Prompt: Fix a Bug (After Diagnosis)

**When to use:** Root cause is confirmed. Now fix it — with a test to prove it.
**Model:** Sonnet
**Token tip:** Include the confirmed root cause so Claude doesn't re-analyze. Saves half the context work.

### Template
```
Fix this bug.

Root cause (confirmed): {{paste diagnosis from previous step}}

File: {{path/to/file.js}}
Relevant code:
{{paste only the functions that need changing}}

Do this in order:
1. Write a failing test that would have caught this bug
2. Fix the bug — minimum change to address the root cause
3. Confirm: "This test would have caught the bug before the fix"

Output: test to add, then diff.
```

### Example
```
Fix this bug.

Root cause (confirmed): calculateLineTotal uses parseInt() on qty and unitPrice,
which truncates decimals. parseInt(2.5) → 2, so 2 × 19.99 = 39 instead of 49.975.

File: src/services/invoice.js
Relevant code:
function calculateLineTotal({ qty, unitPrice }) {
  return parseInt(qty) * parseInt(unitPrice)
}

Do this in order:
1. Write a failing test for fractional quantities
2. Fix by removing parseInt() — inputs are already numbers
3. Confirm the test would have caught this

Output: test to add, then diff.
```

---

## Prompt: Explain Unexpected Behavior

**When to use:** No exception is thrown but the output is wrong. You need to understand why before fixing.
**Model:** Sonnet
**Token tip:** Give Claude the exact failing case. The smaller the reproduction, the less irrelevant context it reads.

### Template
```
This function returns wrong output. No exception is thrown.

Function:
{{paste the function}}

Input: {{paste input}}
Actual output: {{actual}}
Expected output: {{expected}}

Walk through the function step by step with the failing input.
Identify the exact line where output diverges from expected.
Then propose the minimal fix.
```

### Example
```
This function returns wrong output. No exception is thrown.

Function:
function groupByDate(events) {
  return events.reduce((acc, event) => {
    const key = new Date(event.timestamp).toDateString()
    acc[key] = acc[key] || []
    acc[key].push(event)
    return acc
  }, {})
}

Input:
[
  { timestamp: '2026-04-26T23:00:00Z', name: 'event-1' },
  { timestamp: '2026-04-27T00:30:00Z', name: 'event-2' }
]
Actual output: both events in the same date key
Expected output: events in separate date keys

Walk through step by step. Identify where output diverges. Propose minimal fix.
```
