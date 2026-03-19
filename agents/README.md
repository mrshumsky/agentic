# Agent Communication System

This repository is the coordination layer for a hybrid AI workforce — human + AI agents working on shared tasks via GitHub Issues.

## Architecture

```
Kirill (human)
    │
    ├── Jarvis  — Claude Code (primary AI, deep reasoning, complex tasks)
    └── Lex     — OpenClaw (always-on assistant, Telegram interface, quick tasks)
```

Agents are connected over a private encrypted network (no public internet exposure). Each agent has its own capabilities and runs on different hardware, but shares this repository as the single source of truth for tasks.

## How Tasks Work

### GitHub Issues = Task Queue

Every task is a GitHub Issue in this repository.

**Lifecycle:**
1. **Open** — task created (by Kirill, Jarvis, or Lex)
2. **Claimed** — agent adds their label (`agent:jarvis` or `agent:lex`) and moves to `in-progress`
3. **Done** — agent closes the issue with a comment containing evidence of completion

**Rules:**
- Never claim a task without intending to work on it immediately
- Never close a task without a comment explaining what was done and proof it worked
- If a task is too complex for you, add comment and re-assign or ping Kirill

### Labels

| Label | Meaning |
|-------|---------|
| `agent:jarvis` | Assigned to or claimed by Jarvis |
| `agent:lex` | Assigned to or claimed by Lex |
| `status:in-progress` | Currently being worked on |
| `status:blocked` | Waiting on something |
| `priority:high` | Do this first |
| `type:research` | Research task |
| `type:draft` | Writing task |
| `type:action` | Action to take (send message, update, etc.) |

### Issue Format

```
Title: [short imperative description]

Body:
## Goal
What needs to be accomplished.

## Context
Relevant background.

## Acceptance Criteria
- [ ] Specific, verifiable outcome 1
- [ ] Specific, verifiable outcome 2
```

## Agent Capabilities

### Jarvis
- Deep research and analysis
- Long-form writing and drafting
- Complex reasoning and planning
- Code review and architecture
- Access to full PAI skill library

**Best for:** Tasks requiring depth, nuance, or extended reasoning.

### Lex
- Always-on (24/7, responds immediately)
- Telegram interface (reaches Kirill on mobile)
- Quick lookups and summaries
- Running scripts and automations on VPS
- Monitoring and scheduled tasks

**Best for:** Urgent tasks, scheduled jobs, anything needing immediate response.

## Agent-to-Agent Communication

When Lex needs Jarvis's capabilities for a complex task:
1. Lex calls Jarvis via the internal API (authenticated, encrypted)
2. Jarvis processes and returns the result
3. Lex delivers the result to the user

When Jarvis needs Lex to do something on the VPS or via Telegram:
1. Jarvis creates a GitHub Issue assigned to Lex
2. Lex picks it up on its next check-in
3. Lex executes and closes the issue with evidence

## Adding New Agents

To add a new agent to this system:

1. Create a new label: `agent:<name>`
2. Document capabilities in this folder: `agents/<name>.md`
3. Define how the agent connects (API endpoint, authentication method, network access)
4. Add the agent to the architecture diagram above

## Repository Structure

```
/
├── agents/
│   ├── README.md        ← this file
│   ├── jarvis.md        ← Jarvis capabilities and connection spec
│   └── lex.md           ← Lex capabilities and connection spec
└── .github/
    └── ISSUE_TEMPLATE/  ← issue templates (coming soon)
```
