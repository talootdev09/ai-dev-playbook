# Workflows

Three things that will change how you work with AI agents.

---

## Section 1: How to Approach Any Task

### Step 1 — Don't start with the agent

When you get a task, resist the urge to open Claude Code immediately. Agents are powerful but expensive. You want them executing, not figuring out what you mean. The ambiguity tax is real: every clarification round costs the full context re-read.

### Step 2 — Use a free model to craft your prompt

Open ChatGPT (free) or Claude.ai (free). Describe your task conversationally:

> "I need to build X. My stack is Y. The existing code does Z. Help me write a clear prompt for an AI coding agent that captures exactly what needs to be built."

Let the free model help you structure:
- **The goal** — what exactly needs to be built, not what's wrong right now
- **The constraints** — what not to do, what files to leave alone, what API to use
- **The expected output** — specific files, functions, test coverage
- **The context** — only the relevant parts of the codebase, not everything

Refine the prompt until it's airtight. This takes 5–10 minutes and saves 30–60 minutes of agent correction rounds.

### Step 3 — Hand the refined prompt to your agent

Open Claude Code, Cursor, or your agent of choice. Paste the refined prompt. The agent executes with zero ambiguity — no back and forth, no wasted tokens explaining what you meant.

You've already done the thinking. The agent just builds.

### Step 4 — Review the diff, not the codebase

You don't need to read every line the agent wrote. You need to check: does the diff match what the prompt asked for? Do the tests pass? Is anything outside the scope of the task changed?

If yes — ship it. If no — your prompt was missing something. Fix the prompt, not the agent.

### Why This Works

```
Free model = cheap thinking
Agent = expensive execution

Keep thinking cheap. Keep execution focused.
```

A 10-minute prompt-crafting session with a free model is worth more than an hour of correction rounds with a paid agent. This one habit cuts agent token usage by 40–60%.

---

## Section 2: TDD with AI Agents

### Why TDD + Agents

Without tests, agents drift. They make assumptions, go off-scope, and produce code you can't verify without reading every line. With tests, the contract is explicit. The agent's job is to pass them — not to interpret what you want.

Tests are not extra work when using agents. They're the mechanism that makes agents reliable.

### The Loop

```
  Task arrives
      ↓
  Craft prompt (free model — 5-10 min)
      ↓
  Agent writes failing tests first
      ↓
  Agent writes implementation to pass tests
      ↓
  Run tests
      ↓
  Pass? → Review diff → Done
  Fail? → Agent self-corrects (max 2 retries)
      ↓
  Still failing? → You step in, adjust the prompt, retry from the top
```

### In Practice

**Prompt 1 — Write failing tests first:**
```
Before writing any implementation, write failing tests for [feature].
Cover: happy path, empty input, invalid input, edge cases.
Use [Jest / Vitest / your test runner]. Tests must fail before any implementation exists.
Do not write implementation code yet.
```

**Prompt 2 — Implement to pass the tests:**
```
Now write the implementation for [feature] that passes all the tests you just wrote.
Only create or modify files needed. Do not change the tests.
Show me the diff only, not the full files.
```

**Prompt 3 — Review and close:**
```
Run the tests and tell me the results. If any fail, fix the implementation (not the tests).
Summarize what you changed in 3 bullet points.
```

### Why This Saves Tokens

TDD feels like more work upfront. The math says otherwise:

| Path | Token cost |
|------|-----------|
| Write tests (10 cases) | ~3,000 tokens |
| Implement against tests | ~4,000 tokens |
| **TDD total** | **~7,000 tokens** |
| Implement without tests | ~4,000 tokens |
| + 1 correction round | ~10,000 tokens |
| + 2 correction rounds | ~20,000 tokens |
| **No-TDD, 2 corrections** | **~34,000 tokens** |

Corrections are the most expensive part of working with agents. TDD eliminates most of them.

---

## Section 3: Token Efficiency

### The Formula

```
Total tokens processed = S × N(N+1) / 2
```

`S` = average tokens per exchange. `N` = number of messages.

Don't let N get big. Every message compounds the cost of every previous message. Message 20 costs 20× more than message 1 in context re-read overhead.

### The 10 Habits

**1. Edit your prompt, don't send a follow-up**
Click edit on your last message → fix it → regenerate. Follow-ups stack context history. Edits replace it. This is the single highest-ROI habit.

**2. Start fresh every 15–20 messages**
Long sessions burn tokens re-reading old, irrelevant history. Ask Claude to summarize → copy the summary → open a new chat → paste it as context. Fresh start, zero wasted re-reads.

**3. Batch your questions**
Three separate prompts = three full context loads. One prompt with three tasks = one context load. Never send a message when you could add it to the one you're already writing.

**4. Use Projects for recurring files**
Upload a file to a Project once → it's cached. The same PDF or codebase file in multiple chats gets tokenized every single time. Projects fix this.

**5. Set Memory and Preferences**
Stop spending 3–5 messages on setup every chat. Set your role, coding style, and preferences once in Settings. Claude applies them automatically.

**6. Turn off unused features**
Web search and connectors add tokens to every response even when you don't use them. Only enable them when you need them, then turn them off.

**7. Use the right model**
```
Haiku  → formatting, grammar, brainstorming, simple Q&A
Sonnet → real dev work: features, debugging, tests, code review
Opus   → architecture decisions, complex refactors, orchestration
```
Using Opus for a formatting task is like hiring a principal engineer to rename a variable. Don't.

**8. Spread work across the day**
Claude uses a rolling 5-hour usage window, not a daily reset. Split heavy work into 2–3 sessions across the day. You'll stay under limits and keep latency low.

**9. Work off-peak**
Peak usage: 5AM–11AM PT weekdays. Same query costs more in latency and sometimes quality at peak. Run heavy agentic loops evenings or weekends when you can.

**10. Enable overage as a safety net**
Settings → Usage → Extra Usage. Set a monthly cap. A mid-session cutoff when you're deep in a refactor costs more in context reconstruction than the overage would have.

### Developer Habit #11

**Use claude.md to eliminate setup tokens.**

Never explain your stack, conventions, or preferences in chat. Put it in `claude.md` once, in your project root. Claude reads it at the start of every session. You get ~200–500 tokens of setup back, every single session, forever.

Across 100 sessions on a project that's 20,000–50,000 tokens returned to you for free. Just from filling in a file once.
