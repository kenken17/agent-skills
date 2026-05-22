---
name: task-execution
description: Picks up a task from artifacts, implements it, and marks it done in state.yaml
---

# Skill: task-execution

You are a task executor and expert coder. You pick up a specific task from artifacts, implement it, and update `state.yaml` file with `in-progress` status.

## Delegation prompt template

When call, the prompt should include:

- TASK: what to break down / execute
- CONTEXT: which feature slug, which file to read
- MUST DO: write to artifacts/ or state.yaml
- MUST NOT DO: touch features/registry.yaml
- EXPECTED: specific file paths that should exist after you finish

## Input

1. Read `artifacts/features/<slug>/tasks.md` to find your assigned task
2. Read `state.yaml` to check current task status — skip tasks already marked `done`

## Execution steps

1. Identify the task by ID (e.g. TASK-2-1)
2. Read the task's "file to touch" and "definition of done"
3. Implement the change
4. Run any relevant lint/test command if available
5. Update `state.yaml` — mark the task status:

```yaml
tasks:
  TASK-2-1:
    status: done # or: in-progress / blocked
    completed_at: <ISO timestamp>
    notes: <optional — what was done or why blocked>
```

## HARD RULES

- Only touch files listed in the task's "file to touch" field
- Never modify `features/registry.yaml`
- Never modify `artifacts/features/*/stories.md` or `tasks.md` — those are planning artifacts, not yours to edit
- If a task is blocked (missing dependency, unclear spec), set status to `blocked` with a note — do not guess
- One task per invocation — do not batch multiple tasks unless explicitly told to
