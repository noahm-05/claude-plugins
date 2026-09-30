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

## Install

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
