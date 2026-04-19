# Event JSONL Schema

Use one JSON object per event, written as a single line. Events from the same
durable workstream share a `thread_id`. Events from the same contiguous agent
or harness capture lifecycle also share a `session_id` and are ordered by
`sequence`.

## Required Fields

- `schema_version`: integer, start with `4`
- `record_type`: string, use `conversation_event`
- `thread_id`: stable id for the durable visible workstream
- `session_id`: stable id shared by every event in one contiguous capture lifecycle
- `event_id`: unique id for this event
- `sequence`: integer, monotonically increasing within the session
- `timestamp`: ISO-8601 timestamp
- `event_type`: event kind

## Core Event Types

- `session_start`: conversation/session metadata
- `user_message`: exact user message text when available
- `assistant_message`: exact assistant message text when available
- `tool_call`: tool name, arguments, and intent
- `tool_result`: tool output summary, status, and safe excerpts
- `decision`: decision made during the conversation
- `annotation`: tags, constraints, uncertainty, corrections, or evaluation notes
- `artifact`: file, diff, commit, PR, deployment, or generated output
- `session_end`: final status, result, validation, and follow-up

## Recommended Fields

- `agent_id`: stable id for the agent or harness instance when available
- `turn_id`: groups related user, assistant, and tool events
- `parent_event_id`: links a response/result to the event it answers
- `actor`: `user`, `assistant`, `tool`, `harness`, or `system`
- `content`: event-specific structured payload
- `metadata`: model, workspace, skill, token, latency, or harness metadata
- `redactions`: descriptions of omitted secrets or private output

## Content Shapes

## ID Semantics

- `thread_id`: groups related work across one or more sessions. Use this for
  the durable user-visible workstream, such as a Codex thread, issue, PR,
  project task, or long-running topic.
- `session_id`: groups one contiguous agent or harness capture lifecycle inside
  a thread. In interactive Codex, this is usually one assistant execution after
  a user message. A resumed or restarted thread usually gets a new `session_id`
  while keeping the same `thread_id`.
- `turn_id`: groups the events caused by one user interaction unit, such as a
  user message plus the assistant/tool activity it triggers.
- `event_id`: identifies exactly one event.
- `agent_id`: identifies the agent or harness instance when available. Do not
  use `agent_id` as the work grouping key because one agent can serve multiple
  threads.

In simple interactive logging, one `turn_id` often maps to one `session_id`.
Keep both fields because one turn can spawn multiple sessions through sub-agents
or retries, and one long-running session can include multiple turns when the
user clarifies while the same assistant run is still active.

`session_start` content:

- `title`
- `project`
- `workspace`
- `initial_prompt`
- `instructions_summary`

`*_message` content:

- `text`
- `attachments`
- `quoted_from`

`tool_call` content:

- `tool_name`
- `arguments`
- `intent`

`tool_result` content:

- `tool_name`
- `status`
- `summary`
- `safe_output_excerpt`

`decision` content:

- `decision`
- `rationale`
- `alternatives_considered`

`annotation` content:

- `tags`
- `constraints`
- `uncertainty`
- `corrections`

`artifact` content:

- `kind`
- `path`
- `summary`
- `identifier`

`session_end` content:

- `status`
- `result_summary`
- `validation`
- `follow_up`

## Example

```json
{"schema_version":4,"record_type":"conversation_event","thread_id":"thread-rensemble-convlog","session_id":"2026-04-18T18:42:10Z-rensemble-convlog-test","event_id":"e0001","sequence":1,"timestamp":"2026-04-18T18:42:10Z","event_type":"session_start","actor":"assistant","agent_id":"codex","content":{"title":"Conversation logging skill trial","project":"rensemble","workspace":"rensemble","initial_prompt":"can you add a skill to record our prompt and conversation also?","instructions_summary":"Capture as much as possible for later analysis"}}
{"schema_version":4,"record_type":"conversation_event","thread_id":"thread-rensemble-convlog","session_id":"2026-04-18T18:42:10Z-rensemble-convlog-test","event_id":"e0002","sequence":2,"timestamp":"2026-04-18T18:42:10Z","event_type":"user_message","actor":"user","turn_id":"t0001","content":{"text":"can you add a skill to record our prompt and conversation also? im not sure what the structure should look like, lets brainstorm?"}}
{"schema_version":4,"record_type":"conversation_event","thread_id":"thread-rensemble-convlog","session_id":"2026-04-18T18:42:10Z-rensemble-convlog-test","event_id":"e0003","sequence":3,"timestamp":"2026-04-18T18:42:10Z","event_type":"decision","actor":"assistant","turn_id":"t0004","content":{"decision":"Use event-level JSONL as the canonical long-term format.","rationale":"It preserves turn-by-turn structure and can generate human summaries later.","alternatives_considered":["conversation-level JSONL","Markdown summaries"]}}
```
