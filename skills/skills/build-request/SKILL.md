---
name: build-request
description: Use when Noah asks to build, create, or revise a small project or automation for the agent pipeline. Interviews for a complete spec before any planner/builder subagent work starts, for both new builds and revisions.
---

# Build Request Intake

Turn a one-line ask into a complete `spec.md` the pipeline can execute without mid-build course-correction. Ask only what's genuinely missing — don't re-ask what Noah already stated, and don't pad the interview with questions the answer to which wouldn't change the build.

## Step 1 — Determine build vs. revision

- Slugify the project name (lowercase, spaces/underscores → hyphens) and check whether the repo exists. Run this yourself; don't ask Noah:
  ```sh
  curl -sf -o /dev/null -H "Authorization: token $GITEA_TOKEN" \
    "$GITEA_URL/api/v1/repos/$GITEA_ORG/<slugified-project-name>"
  ```
- Decide from curl's exit status: **0** (repo exists) → revision; **22** (HTTP 404) → new build, skip to Step 2. Any other status (e.g. 7 connection refused, 28 timeout) means Gitea is unreachable, so report that and stop rather than guessing.
- **Found** → this is a revision. Read the existing repo's README and `spec.md` (if present) before asking anything — don't make Noah repeat context that's already in the repo.

## Step 2 — Interview checklist

Skip any item already answered by the request itself. Ask the rest in one pass, not one question per turn, unless an answer changes what else needs asking.

1. **Goal** — one sentence: what this does, for whom/what it serves
2. **Inputs / outputs** — what it reads, what it produces, any external services or files it touches
3. **Environment** — where this runs (dev VM, another host, a container, a cron job, systemd service) and any OS/runtime constraints
4. **Dependency policy** — is installing new packages acceptable, or should the builder stay within stdlib / what's already in the repo (this directly informs the builder subagent's minimalism ladder)
5. **Interface** — CLI, script, systemd service, web UI, library import?
6. **Out of scope** — anything explicitly *not* wanted, so the builder doesn't over-scope
7. **Acceptance check** — a concrete way to confirm it works: a command to run, expected output, a test to pass

For a revision, replace 1–7 with: what should change, and does the existing acceptance check still apply or does it need updating.

## Step 3 — Write spec.md

```markdown
# <project-name>

## Goal


## Inputs / Outputs


## Environment


## Dependency policy


## Interface


## Out of scope


## Acceptance check


## Revision history
- (empty on first build; the publisher subagent appends one line per revision)
```

## Step 4 — Confirm before handoff

Show Noah the finished `spec.md`. Get explicit confirmation before the orchestrator dispatches the planner. Do not start planning or building from within this skill — this skill's only job is producing a spec the pipeline can build from unattended.
