# ⚡ claude-code-helper

> A personal library of Claude Code slash commands, skills, and plugins — purpose-built for AI-assisted development workflows.

[![Last Commit](https://img.shields.io/github/last-commit/MathewT/claude-code-helper?style=flat-square&color=brightgreen)](https://github.com/MathewT/claude-code-helper/commits/master)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Claude%20Code-blueviolet?style=flat-square&logo=anthropic)](https://claude.ai/code)
[![Powered by AWS Bedrock](https://img.shields.io/badge/Powered%20by-AWS%20Bedrock-FF9900?style=flat-square&logo=amazonaws)](https://aws.amazon.com/bedrock/)
[![Made with Markdown](https://img.shields.io/badge/Made%20with-Markdown-1f425f?style=flat-square&logo=markdown)](https://daringfireball.net/projects/markdown/)

---

## 📖 Overview

`claude-code-helper` extends [Claude Code](https://claude.ai/code) with reusable slash commands, skills, and agent definitions that accelerate everyday development tasks. Rather than typing the same instructions repeatedly, each command encodes a repeatable workflow that Claude executes consistently every time.

This repo follows the [`anthropics/claude-plugins-official`](https://github.com/anthropics/claude-plugins-official) plugin conventions and is designed to be dropped into any project via Claude Code's commands path configuration.

---

## 📦 Contents

| Type | Name | Description | Status |
|------|------|-------------|--------|
| Command | [`/spec`](#-spec-command) | Turn a short idea into a feature spec file and git branch | ✅ Ready |
| Skill | — | *(coming soon)* | 🚧 Planned |
| Agent | — | *(coming soon)* | 🚧 Planned |
| Plugin | — | *(coming soon)* | 🚧 Planned |

---

## ⚡ `/spec` Command

Turn a rough idea into a structured feature spec and a clean git branch — in one command.

### Usage

```
/spec <short feature description>
```

**Examples:**
```
/spec Add dark mode toggle to settings panel
/spec User CSV export for the reports dashboard
/spec Rate limiting middleware for the public API
```

### What it does

1. **Guards your working tree** — aborts immediately if there are uncommitted, unstaged, or untracked files. You must commit or stash before proceeding.
2. **Parses your idea** — extracts a human-readable `Title Case` title and a git-safe `kebab-case` slug (max 40 chars).
3. **Creates a branch** — switches to `claude/feature/<slug>`. If the branch already exists, appends a version suffix (`-01`, `-02`, …).
4. **Drafts the spec** — writes a structured Markdown spec to `_specs/<slug>.md` using the project's spec template.
5. **Reports back** — prints a compact summary so you know exactly what was created.

### Workflow

```
Your idea
    │
    ▼
/spec "add rate limiting middleware"
    │
    ├─▶ 🔒 Check: clean working tree?  ──✗──▶ ⛔ Abort (commit/stash first)
    │                ✓
    ▼
🌿 git switch -c claude/feature/rate-limiting-middleware
    │
    ▼
📄 _specs/rate-limiting-middleware.md  (written to disk)
    │
    ▼
✅ Branch: claude/feature/rate-limiting-middleware
   Spec:   _specs/rate-limiting-middleware.md
   Title:  Rate Limiting Middleware
```

### Example Output

```
Branch: claude/feature/rate-limiting-middleware
Spec file: _specs/rate-limiting-middleware.md
Title: Rate Limiting Middleware
```

### Features

- **Dirty-tree guard** — prevents spec creation on an unclean repo, keeping branches tidy
- **Smart slugging** — normalises spaces, punctuation, and casing; collapses repeated hyphens; trims to 40 chars
- **Collision-safe branches** — auto-increments suffix if the branch name is already taken
- **No implementation details** — the spec stays at the requirements/acceptance-criteria level; no code examples leak in
- **Template-driven** — spec structure is consistent across all features (see below)

<details>
<summary>📄 Spec file structure (click to expand)</summary>

```markdown
# Spec for <feature-name>

branch: mathewt/feature/<feature-name>

## Summary
...

## Functional Requirements
- ...

## Possible Edge Cases
- ...

## Acceptance Criteria
- ...

## Open Questions
- ...

## Testing Guidelines
```

</details>

---

## 🚀 Quick Start

1. **Clone this repo** into your projects directory:
   ```bash
   git clone git@github.com:MathewT/claude-code-helper.git
   ```

2. **Point Claude Code at the commands folder** by adding this to your project's `.claude/settings.json`:
   ```json
   {
     "commandsDirectories": ["/path/to/claude-code-helper/commands"]
   }
   ```

3. **Verify** the command is available — open Claude Code in any project and type `/spec` to see it in the autocomplete menu.

---

## 🛠 Requirements

- [Claude Code](https://claude.ai/code) CLI installed and authenticated
- **AWS Bedrock** (this setup routes all Claude calls through Bedrock — requires an active SSO session):
  ```bash
  aws sso login --profile <your-aws-profile>
  ```

---

## 🤝 Contributing / Adding Commands

New slash commands live under `commands/<name>/<name>.md` with front matter:

```markdown
---
description: One-line description shown in the slash command menu
argument-hint: What to pass as $ARGUMENTS
allowed-tools: Read, Write, Bash(git switch:*)
---

Command instructions here...
```

See [`CLAUDE.md`](CLAUDE.md) for full plugin and skill authoring conventions.

---

## 📜 License

MIT © [MathewT](https://github.com/MathewT)
