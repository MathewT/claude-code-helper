# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This repository is a personal library of custom Claude Code skills, commands, and plugins. There is no application code, build system, or test runner — all artifacts are Markdown-based definitions consumed by the Claude Code plugin system.

## Plugin and Skill Structure

Follow the `anthropics/claude-plugins-official` conventions when authoring content here:

```
claude-code-helper/
├── .claude-plugin/
│   └── plugin.json          # Plugin metadata: name, description, author
├── skills/
│   └── <skill-name>/
│       └── SKILL.md         # Skill prompt (YAML front matter + Markdown body)
├── commands/                # Slash command definitions (optional)
└── agents/                  # Agent definitions (optional)
```

### SKILL.md format

Each skill file must have YAML front matter followed by the instruction body:

```markdown
---
name: skill-name
description: One-line description shown in the Claude Code UI
license: MIT
---

Skill instructions here...
```

### plugin.json format

```json
{
  "name": "plugin-name",
  "description": "What this plugin provides",
  "author": "MathewT"
}
```

## Environment

Claude Code in this environment runs via **AWS Bedrock** (not the Anthropic API directly). Authentication requires an active SSO session:

```bash
aws sso login --profile <your-aws-profile>
```

Model routing and Bedrock credentials are configured in `~/.claude/settings.json` — do not modify model ARNs or `CLAUDE_CODE_USE_BEDROCK` in project settings.
