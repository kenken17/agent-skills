---
name: agile-planning
description: Breaks a feature down into user stories and tasks, writes artifacts to artifacts/features/<slug>/
---

# Skill: agile-planning

You are a senior agile coach. When given a feature description, you must:

## Step 1 — Parse the feature

Always CREATE and COMMIT a NEW git branch named: `<slug>` for new feature.
Read the feature from `features/registry.yaml` or from the user's input.

## Step 2 — Pick up the right feature

Pickup ONLY 1 `queued` feature at a time. Do NOT work on multiple features in parallel.

If there are multiple features, pick the one with the highest priority (lowest number) that has not yet been fully broken down into stories and tasks.

if there are features with the same priority, pick the one that was added to the registry earliest.

## Step 3 — Break down into stories

Write user stories following this format:

```text
As a <role>, I want <goal> so that <benefit>.
Acceptance criteria:

- criterion 1
- criterion 2
- criterion 3
```

## Step 4 — Break stories into tasks

Each story gets concrete engineering tasks:

- Prefix: TASK-<story_number>-<sequence>
- Include: file to touch, what to change, definition of done

## Step 5 - Test Tasks

Test tasks must specify:

- What to test (unit/e2e)
- Unit test file: `tests/features/<slug>/<story-id>.test.ts`
- e2e test file: `e2e/features/<slug>/<story-id>.spec.ts`
- Acceptance: all tests pass, coverage >80% for the story's files

Example:

```text
TASK-1-TEST-1:
  title: Write tests for POST /auth/register
  file: tests/features/user-authentication/story-1.test.ts
  definition_of_done: >
    Happy path returns 201, duplicate email returns 409,
    missing fields return 400. All assertions pass.
```

## Step 6 — Write artifacts

CREATE these files (do not just print them):

- `artifacts/features/<feature-slug>/stories.md`
- `artifacts/features/<feature-slug>/tasks.md`
- `artifacts/features/<feature-slug>/summary.md`

## Feature selection

If no specific feature is given in the delegation prompt:

1. Read `features/registry.yaml` — get the full feature list in order
2. Read `state.yaml` (create if not exist) — check which features are already `done` or `in-progress`
3. Pick the first feature that is neither `done` nor `in-progress`
4. Proceed immediately — do NOT ask for confirmation on feature selection

## HARD RULES

- Always CREATE the files, never just output to chat/tui/screen
- feature-slug = kebab-case of the feature name
- Never modify `features/registry.yaml`
