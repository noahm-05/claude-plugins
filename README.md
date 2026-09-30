# claude-plugins

A Claude Code plugin marketplace for Noah's own plugins/agents/skills, so
they can be installed consistently across every dev environment — plus
references to third-party plugins worth pulling in.

## Structure

```
claude-plugins/
  .claude-plugin/
    marketplace.json       # lists the plugins below
  agents/
    .claude-plugin/
      plugin.json
    agents/                # Noah's subagent .md files go here
  skills/
    .claude-plugin/
      plugin.json
    skills/                # Noah's skill folders go here
```

Each top-level directory (`agents/`, `skills/`) is an independently
installable Claude Code plugin, so an environment can pull in just what it
needs. More plugin directories (e.g. for hooks or MCP configs) can be added
the same way later without restructuring.

`agents/agents/` holds Noah's 5 agent-pipeline subagent definitions
(planner, builder, reviewer, revisor, publisher), copied unmodified from
the `agent-pipeline` repo. `skills/skills/` holds the `build-request`
skill, copied the same way.

`marketplace.json` also lists two external plugins Noah depends on, each
referenced by its own GitHub source rather than copied in, so they keep
auto-updating from upstream:
- [`ponytail`](https://github.com/DietrichGebert/ponytail) — lazy senior
  dev mode
- [`i-have-adhd`](https://github.com/ayghri/i-have-adhd) — ADHD-friendly
  output shaping

## What's a plugin / skill / agent?

**Plugin.** A folder Claude Code can install. It's the delivery unit —
bundles skills, agents, hooks, or MCP servers together so they install,
update, and uninstall as one thing. `agents` and `skills` are each their
own plugin here on purpose: install only what an environment needs.

**Skill.** One `SKILL.md` file. A set of instructions Claude loads when a
task matches its `description` — a checklist, a workflow, a house style.
Runs in your *main* conversation, not a separate one. `build-request` is
a skill: it interviews you before a build starts.

**Agent.** One `.md` file with its own system prompt, tool list, and
model. A subagent Claude dispatches to do one job in its own context
window, then reports back — keeps that work from bloating your main
conversation. `planner`, `builder`, `reviewer`, `revisor`, `publisher`
are agents: each is one stage of the build pipeline.

One-line version: **plugin = the box, skill = a checklist that runs
inline, agent = a worker Claude hands a job to.**

## Install — local, desktop, IDE

```
/plugin marketplace add noahm-05/claude-plugins
```

Then install whichever plugin(s) you need:

```
/plugin install agents@claude-plugins
/plugin install skills@claude-plugins
/plugin install ponytail@claude-plugins
/plugin install i-have-adhd@claude-plugins
```

This covers the terminal, the desktop app's local sessions, and the VS
Code extension — they share the same settings files, so installing in one
makes it available in the others.

## Install — cloud sessions

A [cloud session](https://code.claude.com/docs/en/cloud-environments) (the
browser at claude.ai/code, `claude --cloud`, routines) starts from a fresh
VM and has no `/plugin` browser. It also does **not** read plugins your
local machine has installed, or ones a repo's own `.claude/settings.json`
turns on — those settings files live outside the cloud VM. The supported
way in is a **setup script**, which runs as root in the VM before Claude
Code launches and is exactly what `claude plugin install` is documented
to be used from.

1. Open the environment selector (the cloud icon above the message box at
   claude.ai/code) → the environment you want this in → the settings gear
   → **Setup script**.
2. Paste:
   ```bash
   #!/bin/bash
   claude plugin marketplace add noahm-05/claude-plugins || true
   claude plugin install agents@claude-plugins || true
   claude plugin install skills@claude-plugins || true
   claude plugin install ponytail@claude-plugins || true
   claude plugin install i-have-adhd@claude-plugins || true
   ```
   (`|| true` so one failed line doesn't fail the whole script and block
   the session from starting.)
3. Save. The script runs once, Anthropic snapshots the result, and every
   new session in that environment starts with these plugins already
   installed — no reinstalling per session.

**This repo is private.** The setup script's `git clone` goes through the
session's GitHub credentials, which are scoped to repos attached to that
session — a cloud session on some *other* project won't automatically have
access to this one. If the marketplace add fails in the setup-script log,
the simplest fix is making `claude-plugins` public (it's just plugin
config, no secrets in it); otherwise attach it as an extra repo source on
the environment, where that's supported.

Changed which plugins you want? Edit the setup script — Claude Code
reruns it and rebuilds the cached snapshot automatically.

## Add new stuff

**A whole new plugin** (yours, or a third party's):
1. Own plugin → add a folder at the repo root with `<name>/.claude-plugin/plugin.json` (copy an existing `plugin.json`, change `name`/`description`), plus its `agents/`, `skills/`, etc. subfolders.
2. Third-party plugin → no folder needed, just reference its source.
3. Either way, add an entry to `.claude-plugin/marketplace.json`'s `plugins` array:
   ```json
   { "name": "my-plugin", "source": "./my-plugin", "description": "..." }
   ```
   or, for an external repo:
   ```json
   { "name": "their-plugin", "source": { "source": "url", "url": "https://github.com/owner/repo.git" }, "description": "..." }
   ```
4. Commit, push. Environments already using `/plugin marketplace update` (or a fresh cloud setup-script run) pick it up.

**A new skill you build or find**, into the existing `skills` plugin:
1. Drop its folder under `skills/skills/<skill-name>/SKILL.md`.
2. No `marketplace.json` change needed — it ships with the `skills` plugin automatically.
3. Bump nothing; `/plugin marketplace update` (or a fresh install) picks it up.

**A new agent you build or find**, into the existing `agents` plugin:
1. Drop its `.md` file under `agents/agents/<agent-name>.md`, with `name`/`description`/`tools`/`model` frontmatter.
2. Same as skills — no `marketplace.json` change, ships with the `agents` plugin.
