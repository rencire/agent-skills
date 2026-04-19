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

Prefer the most specific existing repo-local log, but keep event-level JSONL in
the repo's metadata area when the capture is harness-oriented:

- `.agents/conversation-events.jsonl`
- `.agents/<project>/conversation-events.jsonl` when a repo scopes logs per project
- `docs/projects/<project>/conversation-events.jsonl` only when the project
  explicitly treats the log as user-facing documentation
- `docs/epics/*-events.jsonl` and `docs/tasks/*-events.jsonl` only when the log
  is meant to live with those planning documents
- `AGENTS.md` only for durable repo-wide lessons, not routine dialogue notes

If a project already has a Markdown prompt log, keep it for human browsing and
add the JSONL event stream alongside it.

## Canonical Shape

Store one JSON object per event, one line per object.

Read [schema.md](references/schema.md) for the field layout.

## IDs And Lifecycles

Use these ids for different levels of grouping:

- `thread_id`: the durable user-visible workstream, such as a Codex thread,
  issue, PR, project task, or long-running topic. Keep this stable across
  multiple agent runs that continue the same work.
- `session_id`: one contiguous agent or harness capture lifecycle inside a
  thread. In interactive Codex, this is usually one assistant execution after a
  user message, ending when the assistant sends a final answer, is interrupted,
  errors, or hands off.
- `turn_id`: one user interaction unit and the assistant/tool activity caused
  by it.
- `event_id`: one JSONL record.

A simple Codex exchange often has one `session_id` per `turn_id`, but do not
define them as the same thing. One user turn can spawn multiple sessions when
sub-agents, retries, or restarts are involved. One session can also include
multiple turns when the user clarifies while the same assistant run is still
active.

## Writing Rules

- Capture raw user and assistant message text when available.
- Give every event a stable `thread_id`, stable `session_id`, unique
  `event_id`, and monotonic `sequence`.
- Use `thread_id` for the user's visible work thread or task thread. Use
  `session_id` for a contiguous agent or harness capture lifecycle within that
  thread.
- Include tool calls and tool outputs as separate events when they matter for
  later analysis.
- Add compact decision, annotation, artifact, and run-end events instead of
  hiding important structure inside prose.
- Do not record secrets, private keys, tokens, or private command output.

## Experiment Note

This is the experimental version of the logging path. The longer-term goal is
to have the harness write the same JSON records automatically, with the skill
serving as the schema and workflow guide. Human-readable summaries should be
derived from the event stream, not treated as the source of truth. Repos that
want harness or metadata capture to stay out of docs should keep the canonical
log under `.agents/`.
