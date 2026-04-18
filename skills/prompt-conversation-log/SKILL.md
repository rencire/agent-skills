---
name: prompt-conversation-log
description: Use when a user wants durable repo-local event-level JSONL capture of a prompt, conversation, tool calls, decisions, outcomes, and validation for later analysis or factory improvement.
---

# Prompt Conversation Log

Use this skill when the user wants structured conversation capture for later
analysis. The canonical output is event-level JSONL: one JSON object per
message, tool event, decision, artifact, or run boundary. Events from parallel
or long-lived workstreams are grouped with `thread_id`.

## Where To Log

Prefer the most specific existing project log, but write event-level JSONL:

- `docs/projects/<project>/conversation-events.jsonl`
- `docs/epics/*-events.jsonl`
- `docs/tasks/*-events.jsonl`
- `AGENTS.md` only for durable repo-wide lessons, not routine dialogue notes

If a project already has a Markdown prompt log, keep it for human browsing and
add the JSONL event stream alongside it.

## Canonical Shape

Store one JSON object per event, one line per object.

Read [schema.md](references/schema.md) for the field layout.

## Writing Rules

- Capture raw user and assistant message text when available.
- Give every event a stable `thread_id`, stable `session_id`, unique
  `event_id`, and monotonic `sequence`.
- Use `thread_id` for the user's visible work thread or task thread. Use
  `session_id` for a contiguous capture session within that thread.
- Include tool calls and tool outputs as separate events when they matter for
  later analysis.
- Add compact decision, annotation, artifact, and run-end events instead of
  hiding important structure inside prose.
- Do not record secrets, private keys, tokens, or private command output.

## Experiment Note

This is the experimental version of the logging path. The longer-term goal is
to have the harness write the same JSON records automatically, with the skill
serving as the schema and workflow guide. Human-readable summaries should be
derived from the event stream, not treated as the source of truth.
