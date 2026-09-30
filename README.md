# claude-plugins

A Claude Code plugin marketplace for Noah's own plugins/agents/skills, so
they can be installed consistently across every dev environment — plus,
later, references to third-party plugins worth pulling in.

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

This repo currently ships scaffolding only — the `agents/agents/` and
`skills/skills/` content directories are empty placeholders. Actual
subagent and skill content, plus any third-party plugin references, land in
follow-up revisions.

## Install

```
/plugin marketplace add noahm-05/claude-plugins
```

Then install whichever plugin(s) you need:

```
/plugin install agents@claude-plugins
/plugin install skills@claude-plugins
```
