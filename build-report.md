## What was built
This revision populates the previously-empty `agents/agents/` and
`skills/skills/` plugin content directories, per plan.md's file list and
build order:

- Read the 5 agent `.md` files and `build-request/SKILL.md` from the
  sibling `agent-pipeline` repo (spec-authorized exception to the
  same-repo boundary, per spec.md's Inputs section).
- Copied all 6 files byte-for-byte unmodified into
  `agents/agents/{planner,builder,reviewer,revisor,publisher}.md` and
  `skills/skills/build-request/SKILL.md`. Verified with `diff` against the
  source — no differences.
- Deleted `agents/agents/.gitkeep` and `skills/skills/.gitkeep`, now that
  both directories hold real content.
- Updated `README.md` to drop the stale "ships scaffolding only / empty
  placeholders" language and describe what's actually in
  `agents/agents/` and `skills/skills/` now.
- Committed in two steps: (1) the 6 copied files + `.gitkeep` removals,
  (2) the README update.

## Deviations from plan.md
**`.claude-plugin/marketplace.json` was NOT updated** — plan.md's step 5
(add `ponytail` and `i-have-adhd` as external `source: url` entries) was
blocked. Every attempt to make that edit (tried twice, identical content)
was rejected by the Claude Code auto-mode permission classifier with
reason `[Untrusted Code Integration]`, and its instructions were explicit
that I should not retry via another tool, different quoting, or any other
workaround — that a spec/plan written by another pipeline stage does not
itself count as the user's consent for a permission-system-level block.
So I stopped, did not attempt a workaround, and left `marketplace.json`
exactly as it was before this build (still only listing the `agents` and
`skills` plugins — no `ponytail`/`i-have-adhd` entries).

Because that step didn't land, I also trimmed my first draft of the
README update: it had already described `marketplace.json` as
referencing the two third-party plugins, which would have been
inaccurate given the blocked edit, so I removed that paragraph and kept
only the (true) description of `agents/agents/` and `skills/skills/`
content. **Noah needs to either grant permission for that specific edit
(adding `{"source": "url", "url": "https://github.com/<owner>/<repo>.git"}`
entries to `marketplace.json`) or add the two entries himself**, then
the README paragraph describing them can be restored — I left that out
rather than guess at wording for content that isn't there yet.

Everything else — the 6 copied files, `.gitkeep` removals, README's
agents/skills paragraph — matches plan.md exactly, no scope changes.

## Acceptance check result
PARTIAL PASS — one item blocked, reported honestly rather than worked
around.

- `.claude-plugin/marketplace.json` and both `plugin.json` files are
  valid JSON (verified with `python3 -c "import json; json.load(...)"`,
  all three `OK`) and match the required schema fields — **but
  `marketplace.json` does NOT yet include the two new external-source
  entries**, because that edit was blocked by the permission system (see
  Deviations above). FAIL on this specific sub-check.
- The 5 agent `.md` files and `build-request/SKILL.md` are present under
  `agents/agents/` and `skills/skills/build-request/`, each starts with
  `---` frontmatter, and are byte-for-byte identical to agent-pipeline's
  originals (confirmed via `diff`, no output = no differences). PASS.
- Repo pushed to GitHub as private under noahm-05 — not attempted, out of
  scope for this stage per plan.md's explicit note ("push/repo-verify
  portion runs in the separate publish stage, not by the builder").
- `gh repo view noahm-05/claude-plugins` — not run, same reason.

Local commit history added this build (`git log --oneline`, newest first):
```
51045aa Update README: agents/agents and skills/skills now hold real content
3e85ef9 Copy agent-pipeline's 5 agent defs and build-request skill into plugin dirs
```

**Action needed from Noah:** grant permission for the `marketplace.json`
edit adding the `ponytail`/`i-have-adhd` external-source entries (exact
content is in plan.md's "New marketplace.json entries" section), or add
them directly — then re-run this build step (or apply just that one
edit) and restore the README's third-party-plugins paragraph.
