# AI Dev Playbook

**The best AI developer gets the most done per token, with reliable output.**

---

## Why This Exists

Most developers use AI like a search engine with superpowers — firing off questions, getting answers, sending corrections, drifting through long sessions. It feels productive. It isn't. Token costs compound quadratically. Agents without direction drift. Corrections cost more than getting it right the first time.

This playbook is built on two ideas. First: how you structure your prompt before you touch the agent determines 80% of the output quality. Second: tests are the only reliable way to keep agents on track. Together they form a workflow that produces better code with fewer tokens.

Two pillars. No fluff.

- **Token Efficiency** — specific habits that reduce cost without reducing output
- **Agentic Dev with TDD** — a loop that keeps agents honest and keeps you reviewing diffs instead of codebases

---

## How I Work (The Approach)

> When I get a task, I don't go straight to Claude Code or an agent.
> First, I use a free AI tool — ChatGPT free, Claude.ai free — to think through
> the task and craft a clear, structured prompt. Only once I have a prompt that
> captures exactly what needs to be done do I hand it to the agent.
>
> Why? Because agents are expensive and powerful. You don't want them figuring
> out what you mean. You want them executing what you mean.
> The free model does the thinking. The agent does the building.
>
> This one habit alone cuts agent token usage by 40–60%.

Here's the full loop:

1. **Receive the task.** Resist the urge to open Claude Code immediately.
2. **Open ChatGPT (free) or Claude.ai (free).** Describe the task conversationally. Ask it to help you write a clear, structured prompt for your coding agent.
3. **Refine the prompt** until it captures: the goal, the constraints, the tech stack, the expected output, and what NOT to do.
4. **Paste the refined prompt into Claude Code** (or Cursor, or your agent of choice). The agent executes with zero ambiguity — no back and forth, no correction rounds.
5. **Review the diff, not the codebase.** Tests pass? Output matches the prompt? Ship it.

Free model = cheap thinking. Agent = expensive execution. Keep thinking cheap.

---

## Quick Start

```bash
# 1. Clone
git clone https://github.com/talootdev09/ai-dev-playbook.git

# 2. Drop claude.md into your project
cp ai-dev-playbook/claude.md your-project/claude.md

# 3. Open WORKFLOWS.md and pick a workflow
```

Edit `claude.md` once — fill in your project, stack, and conventions. Claude reads it at the start of every session. You never explain your setup again.

---

## What's Inside

| File | What it does |
|------|-------------|
| `claude.md` | Drop into any project — gives Claude instant context without setup tokens |
| `WORKFLOWS.md` | The full approach: free-model-first, TDD agent loop, 10 token efficiency habits |
| `prompts/feature.md` | Copy-paste prompts for building new features |
| `prompts/tdd-cycle.md` | Prompts for each step of the TDD loop |
| `prompts/debug.md` | Prompts for isolating and fixing bugs |
| `cheatsheet/index.html` | Deploy to Vercel, share the URL with your team |

---

## The Token Cost Formula

This is the number most developers don't know, and it changes how you work once you do:

```
Total tokens processed = S × N(N+1) / 2
```

Where `S` is your average exchange size in tokens and `N` is the number of messages.

At ~500 tokens per exchange:

| Messages | Total Tokens | What That Means |
|----------|-------------|-----------------|
| 5 | 7,500 | One focused task |
| 10 | 27,500 | ~4× the cost of 5 messages |
| 20 | 105,000 | ~14× the cost of 5 messages |
| 30 | 232,500 | Message 30 costs 31× more than message 1 |

The session length is the variable with the most leverage. Keep N small. Use fresh sessions. Batch everything.

---

## Contributing

PRs welcome. Open an issue first for anything that changes structure or philosophy. Everything added should make the repo simpler to use, not more complete.
