# Claude Code + OpenClaw + Ollama + n8n

Status: completed
Issue: #1
Author: Jarvis

---

## Role of each component

| Tool | Role in the stack |
|---|---|
| **Claude Code** | Interactive deep work — coding, debugging, file ops, complex reasoning. Human-in-the-loop by default. The "power tool" you pick up intentionally. |
| **OpenClaw** | Always-on ambient agent. Telegram interface. Routes tasks to Claude/Ollama. The layer you ping from your phone at 11pm. |
| **Ollama** | Local inference backend. Fast, free, private. Best for repetitive/lightweight tasks: classification, summarization, routing, template filling. Feeds into OpenClaw and n8n. |
| **n8n** | Workflow orchestration for multi-step, event-driven, scheduled automations involving external services. The glue between SaaS tools. |

Each tool occupies a distinct lane. They don't compete — they compose.

---

## When n8n is clearly justified

Use n8n when **two or more** of these are true:

1. **Multi-step cross-service flow** — e.g., GitHub event → transform → write to Notion → notify Telegram. Three services, one workflow.
2. **Webhook receiver** — incoming events from GitHub, Stripe, Typeform, etc. that need to trigger downstream logic.
3. **SaaS integrations without custom code** — Gmail, Calendar, Airtable, Notion, Slack. n8n has 400+ nodes; writing this from scratch is waste.
4. **Scheduling with retry and failure handling** — cron doesn't retry, n8n does. Matters for workflows that depend on external APIs.
5. **Human-in-the-loop approval steps** — n8n has a Wait node that pauses until you confirm via Telegram or email.
6. **Observability required** — n8n logs every execution with inputs/outputs. Useful when you need to audit "what ran and when."
7. **Background orchestration** — things that should run without a human present, on a schedule, reliably.

**The signal:** if you're writing a bash script with curl calls to 3 different APIs and a cron job to run it — that's n8n territory.

---

## When n8n is unnecessary overhead

Skip n8n when:

- **Single-step task** → cron + script is simpler, easier to version-control, easier to debug.
- **Pure AI conversation or delegation** → OpenClaw handles it. No workflow needed.
- **Code-heavy or file-heavy task** → Claude Code directly. n8n is not a code runner.
- **One-off or ad-hoc task** → run it manually or via GitHub Actions. Don't build a workflow for something you do once.
- **Fast local AI call** → call Ollama via OpenClaw or a script. n8n adds latency and complexity for simple inference.
- **GitHub CI/CD task** → GitHub Actions already does this. Don't duplicate.

**The anti-pattern:** using n8n as a fancy HTTP client for things a 10-line script handles fine.

---

## Recommended starter architecture

### Phase 1 — Set up first (week 1)

```
[You] ←→ Claude Code          # interactive work, code, complex tasks
[You] ←→ OpenClaw (Telegram)  # ambient agent, mobile-first, async delegation
              ↓
           Ollama              # fast local inference for OpenClaw tasks
```

This covers 80% of solo builder needs. You have:
- A power tool for focused work (Claude Code)
- An always-available agent you can ping anywhere (OpenClaw)
- Free local inference for lightweight tasks (Ollama)

No n8n yet. No SaaS glue yet.

### Phase 2 — Add when you have recurring multi-step workflows

```
[External events] → n8n → OpenClaw/Ollama/Claude → [outputs]
GitHub webhooks       ↓
Scheduled jobs    → Notion / Telegram / Files
SaaS triggers
```

**Add n8n only when** you have at least 2-3 specific workflows that require:
- external webhooks
- multi-service orchestration
- scheduled background automation

If you can't name 3 concrete workflows today, postpone n8n.

### Boundaries

| What | Who owns it |
|---|---|
| Interactive coding | Claude Code |
| Ad-hoc AI queries | OpenClaw + Ollama |
| Background automation | n8n |
| Local/cheap inference | Ollama (called by OpenClaw or n8n) |
| CI/CD, repo automation | GitHub Actions |
| Simple scheduled scripts | cron |

**Don't route everything through n8n.** It becomes the bottleneck and the spaghetti monster.

---

## Concrete scenario ideas

### Use n8n

1. **GitHub issue → research pipeline**
   GitHub webhook → n8n → trigger OpenClaw/Claude with issue context → write output to file → comment on issue with result link.

2. **Daily briefing**
   Scheduled n8n flow → pull Todoist tasks + calendar + weather → Ollama summarizes → Telegram message at 8am.

3. **Research intake from URL**
   URL dropped in Telegram → OpenClaw passes to n8n → scrape page → Ollama extracts key points → save to Notion research database.

4. **Service health monitoring**
   n8n pings endpoints every 5 min → if down → Telegram alert with error context + last known good state.

5. **Weekly review digest**
   Saturday 9am → n8n pulls GitHub commits + Todoist completed tasks → Claude summarizes week → Telegram message.

### Use OpenClaw + Ollama (no n8n)

6. **Quick lookup from mobile**
   Telegram message → OpenClaw → Ollama → instant response. Pure conversation. No workflow needed.

7. **Code task delegation from phone**
   "Review this PR" message → OpenClaw → routes to Claude → returns summary in Telegram.

8. **Summarize a pasted article**
   Paste text into Telegram → OpenClaw → Ollama → 5-bullet summary back.

### Use Claude Code directly

9. **Refactor or debug session**
   Open terminal, run Claude Code, work interactively. This is not a workflow — it's a tool session.

### Use cron / GitHub Actions

10. **Repo sync, backup, or maintenance script**
    If it's a single command run on a schedule — cron. If it's triggered by a git event — GitHub Actions. No n8n.

---

## Summary judgment on n8n

n8n earns its place when you're orchestrating **events + multiple services + background logic**. For a solo builder, that usually means 3-5 specific workflows, not the whole stack.

Most AI-native tasks belong to OpenClaw. Most code tasks belong to Claude Code. Most cheap inference belongs to Ollama. n8n is the connective tissue — valuable, but only where the seams are.

**Start without n8n. Add it when you feel the pain of not having it.**
