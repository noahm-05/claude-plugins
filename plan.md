## File list
- `agents/agents/planner.md` — copy of agent-pipeline's planner subagent definition
- `agents/agents/builder.md` — copy of agent-pipeline's builder subagent definition
- `agents/agents/reviewer.md` — copy of agent-pipeline's reviewer subagent definition
- `agents/agents/revisor.md` — copy of agent-pipeline's revisor subagent definition
- `agents/agents/publisher.md` — copy of agent-pipeline's publisher subagent definition
- `skills/skills/build-request/SKILL.md` — copy of agent-pipeline's build-request skill
- `agents/agents/.gitkeep` — delete; no longer needed now that real content lives in this directory
- `skills/skills/.gitkeep` — delete; no longer needed now that real content lives in this directory
- `.claude-plugin/marketplace.json` — modify: add two new plugin entries (`ponytail`, `i-have-adhd`) with external git-url sources
- `README.md` — modify: update to reflect that `agents/agents/` and `skills/skills/` now contain real content instead of empty placeholders, and document the two external plugin entries

## Source → destination mapping (copied files)
| Source (agent-pipeline) | Destination (claude-plugins) |
|---|---|
| `/home/noah/projects/agent-pipeline/.claude/agents/planner.md` | `/home/noah/projects/claude-plugins/agents/agents/planner.md` |
| `/home/noah/projects/agent-pipeline/.claude/agents/builder.md` | `/home/noah/projects/claude-plugins/agents/agents/builder.md` |
| `/home/noah/projects/agent-pipeline/.claude/agents/reviewer.md` | `/home/noah/projects/claude-plugins/agents/agents/reviewer.md` |
| `/home/noah/projects/agent-pipeline/.claude/agents/revisor.md` | `/home/noah/projects/claude-plugins/agents/agents/revisor.md` |
| `/home/noah/projects/agent-pipeline/.claude/agents/publisher.md` | `/home/noah/projects/claude-plugins/agents/agents/publisher.md` |
| `/home/noah/projects/agent-pipeline/.claude/skills/build-request/SKILL.md` | `/home/noah/projects/claude-plugins/skills/skills/build-request/SKILL.md` |

Copies must be byte-for-byte unmodified (acceptance check requires this explicitly).

## New marketplace.json entries
To be appended to the `plugins` array in `/home/noah/projects/claude-plugins/.claude-plugin/marketplace.json`, using the nested external-source object form confirmed against real examples on this machine:

```json
{
  "name": "ponytail",
  "source": {
    "source": "url",
    "url": "https://github.com/DietrichGebert/ponytail.git"
  },
  "description": "<one-line description of ponytail, written by builder — see Ambiguity 2>"
},
{
  "name": "i-have-adhd",
  "source": {
    "source": "url",
    "url": "https://github.com/ayghri/i-have-adhd.git"
  },
  "description": "<one-line description of i-have-adhd, written by builder — see Ambiguity 2>"
}
```

Note: real examples found on this machine (official marketplace, and the two plugins' own marketplace caches) additionally pin with `"sha": "..."` or `"ref": "main"`. Plan is to omit that pin — see Ambiguity 1.

## Build order
1. Read the 5 agent `.md` files and `SKILL.md` from `/home/noah/projects/agent-pipeline` (spec-authorized exception to same-repo boundary — spec.md's Inputs section names this repo explicitly).
2. Copy the 5 agent files into `agents/agents/`, unmodified.
3. Copy `SKILL.md` into `skills/skills/build-request/`, unmodified.
4. Delete `agents/agents/.gitkeep` and `skills/skills/.gitkeep` now that both directories hold real content.
5. Modify `.claude-plugin/marketplace.json` to add the `ponytail` and `i-have-adhd` entries (external `source: url` form, per mapping above).
6. Update `README.md` to describe the now-populated `agents/agents/` and `skills/skills/` directories and the two external plugin references, removing the "ships scaffolding only / empty placeholders" language that's now stale.
7. Commit locally. Do not push or touch the GitHub repo — that's a separate publish stage after review passes.

## Ambiguities
1. **External-source pinning.** Real marketplace.json examples on this machine pin external sources with `sha`/`ref`, but spec's acceptance check literally specifies the minimal `{"source": "url", "url": "..."}` form, and spec's Out of scope note says these stay external "so they still auto-update from their own repos" — pinning would work against that. Resolved as: unpinned minimal form, per spec's literal acceptance check.
2. **Descriptions for the two new entries.** Builder writes a short, factual one-liner for each, grounded in the third-party plugin's own stated purpose — not invented.
3. **README update.** Not explicitly in spec's Outputs/Acceptance check, but current README says the content dirs are "empty placeholders" — now false. Included for accuracy.
4. **plugin.json version numbers.** No bump requested or tested; no change planned.

None of these are true contradictions — judgment calls within spec's stated constraints.

## Acceptance check
- `.claude-plugin/marketplace.json` and both `plugin.json` files are valid
  JSON and match the required marketplace/plugin schema fields, including
  the two new external-source entries (`{"source": "url", "url": "..."}`
  form, matching the schema used by real marketplace.json examples on
  this machine).
- The 5 agent `.md` files and `build-request/SKILL.md` are present under
  `agents/agents/` and `skills/skills/build-request/` with valid
  frontmatter, unmodified from agent-pipeline's originals.
- Repo pushed to GitHub as private under noahm-05.
- `gh repo view noahm-05/claude-plugins` confirms it exists and is private.

(Note: the push/repo-verify portion runs in the separate publish stage, not by the builder.)
