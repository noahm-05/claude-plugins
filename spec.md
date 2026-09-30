# claude-plugins

## Goal
A private GitHub repo (`noahm-05/claude-plugins`) that serves as a Claude Code
plugin marketplace, so Noah can install his own plugins/agents/skills — and
references to third-party ones — consistently across every dev environment.

## Inputs / Outputs
- Inputs: the `agent-pipeline` repo's own `.claude/agents/*.md` (planner,
  builder, reviewer, revisor, publisher) and `.claude/skills/build-request/`.
- Outputs: those files copied into `agents/agents/` and `skills/skills/`
  respectively, plus two new marketplace entries for `ponytail`
  (github.com/DietrichGebert/ponytail) and `i-have-adhd`
  (github.com/ayghri/i-have-adhd) — referenced by external git source, not
  copied, since they're published plugins Noah installs from elsewhere.

## Environment
GitHub-hosted, private, personal account (noahm-05). No server or runtime —
it's a static git repo. Consumed from any environment via:
`/plugin marketplace add noahm-05/claude-plugins`.

## Dependency policy
None. Plain JSON/Markdown — no code, no package dependencies.

## Interface
Git repo / Claude Code plugin marketplace, added and installed from via the
`/plugin` command family.

## Out of scope
- Any content beyond agent-pipeline's agents/build-request skill and the
  two named external plugins — no other repos' agents/skills go in yet.
- Any CI, web UI, or publishing automation.
- Copying ponytail/i-have-adhd's actual source — they stay external
  references so they still auto-update from their own repos.

## Scaffold structure
```
claude-plugins/
  .claude-plugin/
    marketplace.json       # lists the plugins below
  agents/
    .claude-plugin/
      plugin.json
    agents/                # Noah's subagent .md files go here later
  skills/
    .claude-plugin/
      plugin.json
    skills/                # Noah's skill folders go here later
  README.md
  .gitignore
```
Two plugins to start — `agents` and `skills` — each independently
installable, so Noah can pull in just what an environment needs. More
plugin directories (e.g. for hooks or MCP configs) can be added the same
way later without restructuring.

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

## Revision history
- 2026-09-30: Initial scaffold build (agents/skills plugin dirs, marketplace.json). Pushed to https://github.com/noahm-05/claude-plugins (private). Tag: v1.
- 2026-09-30: Added agent-pipeline's 5 agents and build-request skill into agents/agents/ and skills/skills/, plus ponytail and i-have-adhd as external marketplace entries. Tag: v2.
