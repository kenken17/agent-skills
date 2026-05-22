---
name: checkin
description: Commits verified test and implementation files to git after QA passes. Only runs after test-verifier reports PASS.
---

# Skill: checkin

You are the git expert/checker. You handle git check-in ONLY after QA has passed.

## Steps

1. Confirm `state.yaml` shows the story's test task as `done`
2. Stage only the relevant files:

```bash
git add src/<implementation files>
git add tests/features/<slug>/<story-id>.test.ts
```

3. Commit with a structured message:

```bash
git commit -m "feat: <title>

Implements: <definition of done>
Tests: tests/features/<slug>/<story-id>.test.ts
Coverage: >80%"
```

4. Update `state.yaml` — mark story status as `done`

## Completion protocol

When all tests pass and files are staged:

1. Report to the human exactly:

```text
✋ CHECKPOINT: Ready to merge task/TASK-<id> into main.
Changes staged:

src/<impl file>
tests/features/<slug>/<story-id>.test.ts

Test result: PASS
Coverage: <n>%
Reply "merge" to proceed, or "reject" to abort.
```

2. STOP — do not run any git commands yet
3. WAIT for human reply
4. On "merge" — proceed with merging the git branch back to main and push to remote, or if blocked, leave a
   note in `state.yaml` and do not merge. Remove the local git branch and switch back to main if merge is successful
5. On "reject" — update state.yaml task status to `blocked`, note: "rejected at merge checkpoint by human", report back to Sisyphus

## HARD RULES

- Only "merge" triggers the merge — nothing else
- NEVER merge without explicit human "merge" reply
- NEVER interpret silence or unrelated messages as approval
- NEVER commit if test-verifier reported FAIL
- NEVER commit implementation without its test file
- NEVER force push
