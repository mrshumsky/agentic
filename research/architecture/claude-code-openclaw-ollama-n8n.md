# Claude Code + OpenClaw + Ollama + n8n

Status: requested
Issue: #1

## Research brief

Design a practical, opinionated workflow for using these tools together in one personal AI stack:
- Claude Code
- OpenClaw
- Ollama
- n8n

Core question:

> Where does n8n add real value, and where is it just extra complexity?

## Context

This is for a solo builder / product operator setup, not an enterprise team.

Desired properties:
- practical
- low-maintenance
- automation-friendly
- easy to extend over time
- useful for both AI-assisted work and background workflows

## Questions to answer

### 1. Role of each component
Define the ideal role of:
- Claude Code
- OpenClaw
- Ollama
- n8n

### 2. When n8n is clearly justified
Focus especially on:
- multi-step automations
- webhook/event-driven workflows
- SaaS integrations
- branching logic
- retries and failure handling
- human approval steps
- scheduling/orchestration across tools
- observability and auditability

### 3. When n8n is unnecessary overhead
Point out scenarios better handled by:
- OpenClaw directly
- Claude Code directly
- cron jobs
- lightweight scripts
- GitHub Actions
- simple local automations

### 4. Recommended starter architecture
Recommend a minimal starter architecture that avoids overengineering.

The recommendation should answer:
- what to set up first
- what to postpone
- what boundaries each tool should have
- how they should interact without turning into spaghetti

### 5. Concrete scenario ideas
Give 5-10 specific automation scenarios for this stack.

Useful categories:
- GitHub issues / project workflows
- research intake / summarization
- content pipelines
- reminders / follow-ups
- messaging / notifications
- personal or home ops
- async delegation between agents/tools

## Output preference
Be practical and opinionated.
Prefer:
- concise architecture thinking
- concrete examples
- explicit tradeoffs
- a recommendation that a solo operator could implement this week

Avoid generic enterprise architecture fluff.

## Notes

- Primary tracking issue: https://github.com/mrshumsky/agentic/issues/1
- Intended agent label: `agent:jarvis`
- Initial status label: `status:todo`
