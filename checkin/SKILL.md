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
Tests: {tests,e2e}/features/<slug>/<story-id>.test.ts
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

2. STOP — do not merge or push until you get a human reply
3. Make a git pull to ensure you have the latest main branch and to check for merge conflicts. If there are conflicts, do not attempt to resolve — report back to human:

```text
⚠️ MERGE CONFLICT: There are merge conflicts with the latest main branch.
Please resolve the conflicts manually. No changes have been merged or pushed.
```

4. If there are no merge conflicts, CREATE A PULL REQUEST to the remote repository with a title and description/notes based on the commit message.
5. On "merge" — proceed with merging the git branch back to main and push to remote, or if blocked, leave a note in `state.yaml` and do not merge. Remove the local git branch and switch back to main if merge is successful
6. On "reject" — update state.yaml task status to `blocked`, note: "rejected at merge checkpoint by human", report back to orchestrator.

## HARD RULES

- Only "merge" triggers the merge — nothing else
- ALWAYS CREATE A PULL REQUEST (remote) — do not merge directly to main
- NEVER merge without explicit human "merge" reply
- NEVER interpret silence or unrelated messages as approval
- NEVER commit if test-verifier reported FAIL
- NEVER commit implementation without its test file
- NEVER force push
