## File list
- `.gitignore` — root-level ignore rules for OS/editor junk (repo has no code/dependencies, so nothing else to exclude)
- `agents/.claude-plugin/plugin.json` — plugin manifest for the `agents` plugin
- `agents/agents/.gitkeep` — placeholder so the empty `agents/agents/` content directory is tracked by git
- `skills/.claude-plugin/plugin.json` — plugin manifest for the `skills` plugin
- `skills/skills/.gitkeep` — placeholder so the empty `skills/skills/` content directory is tracked by git
- `.claude-plugin/marketplace.json` — marketplace manifest listing the `agents` and `skills` plugins
- `README.md` — repo overview, structure explanation, and the `/plugin marketplace add noahm-05/claude-plugins` install instructions

## Build order
1. `.gitignore` — set up ignore rules before anything else gets staged
2. `agents/.claude-plugin/plugin.json` — define the agents plugin manifest
3. `agents/agents/.gitkeep` — placeholder to track the empty agents content dir
4. `skills/.claude-plugin/plugin.json` — define the skills plugin manifest
5. `skills/skills/.gitkeep` — placeholder to track the empty skills content dir
6. `.claude-plugin/marketplace.json` — marketplace manifest referencing both plugin dirs (build after the plugin dirs exist so paths/names are final)
7. `README.md` — document the repo once the final structure is settled
8. Commit locally. Do not push or create the GitHub repo — that happens in a separate publish stage after review passes.

## Ambiguities
1. spec.md says marketplace.json and plugin.json must "match the required marketplace/plugin schema fields" but does not enumerate those fields inline. Use Claude Code's documented plugin-marketplace schema (marketplace.json: name, owner, plugins array with each entry's name/source/description; plugin.json: name, version, description, author) rather than inventing field names — spec gives no explicit version numbers or author info to use.
2. `.gitignore` contents are unspecified. Since there's no code or package dependencies, use reasonable judgment (e.g. OS/editor cruft like `.DS_Store`, `*.swp`) rather than leaving it empty or pulling in an unrelated language template.

Neither is a true contradiction — both are implementation judgment calls within the spec's stated constraints.

## Acceptance check
- `.claude-plugin/marketplace.json` and both `plugin.json` files are valid
  JSON and match the required marketplace/plugin schema fields.
- Repo pushed to GitHub as private under noahm-05.
- `gh repo view noahm-05/claude-plugins` confirms it exists and is private.

(Note: the push/repo-creation half of this check runs in the separate publish stage, not by the builder.)
