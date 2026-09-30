# claude-plugins

## Goal
A private GitHub repo (`noahm-05/claude-plugins`) that serves as a Claude Code
plugin marketplace, so Noah can install his own plugins/agents/skills — and
references to third-party ones — consistently across every dev environment.

## Inputs / Outputs
- Inputs: none at scaffold time. Follow-up revisions will add Noah's own
  plugin content (e.g. the agents-pipeline setup) and marketplace entries
  pointing at externally-published plugins on GitHub.
- Outputs: a git repo structured per Anthropic's Claude Code plugin
  marketplace spec — root `.claude-plugin/marketplace.json`, with each
  plugin as its own top-level directory containing a `.claude-plugin/plugin.json`.

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
- Migrating Noah's existing agents/skills content into the plugins —
  scaffold only, content comes in a follow-up revision.
- Adding actual external plugin references — follow-up revision once Noah
  picks which ones.
- Any CI, web UI, or publishing automation.

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
  JSON and match the required marketplace/plugin schema fields.
- Repo pushed to GitHub as private under noahm-05.
- `gh repo view noahm-05/claude-plugins` confirms it exists and is private.

## Revision history
- 2026-09-30: Initial scaffold build (agents/skills plugin dirs, marketplace.json). Pushed to https://github.com/noahm-05/claude-plugins (private). Tag: v1.
