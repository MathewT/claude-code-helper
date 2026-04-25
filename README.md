# claude-code-helper

A curated collection of reusable **commands**, **skills**, and **plugins** for [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview) — Anthropic's AI coding assistant.

## Contents

- [Commands](#commands) — Custom slash commands you can invoke inside Claude Code
- [Skills](#skills) — Reusable prompt patterns and methodologies for complex tasks
- [Plugins](#plugins) — MCP server configurations that extend Claude Code with external tools

---

## Commands

Custom slash commands live in `.claude/commands/`. Each `.md` file becomes a `/command-name` you can invoke directly in Claude Code. Commands can accept arguments via the `$ARGUMENTS` placeholder.

### Installation

**Project-level (shared with your team):**
Copy the `.claude/` directory into your project root:
```bash
cp -r .claude/ /path/to/your/project/
```

**User-level (available in every project):**
Copy commands to your global Claude Code directory:
```bash
cp .claude/commands/*.md ~/.claude/commands/
```

### Available commands

| Command | Usage | Description |
|---------|-------|-------------|
| `/commit` | `/commit` | Inspect staged changes and generate a conventional commit message |
| `/debug` | `/debug <issue description>` | Systematically debug an issue using a structured methodology |
| `/document` | `/document [target]` | Add or improve documentation and docstrings |
| `/explain` | `/explain <code or file>` | Explain what code does in plain English |
| `/fix` | `/fix <bug description>` | Find and fix a bug or failing test |
| `/optimize` | `/optimize [target]` | Identify and fix performance bottlenecks |
| `/pr` | `/pr [context]` | Generate a pull request description from the current branch |
| `/refactor` | `/refactor [target]` | Refactor code for quality without changing behaviour |
| `/review` | `/review [target]` | Perform a thorough, structured code review |
| `/security` | `/security [target]` | Run a security audit against common vulnerability classes |
| `/test` | `/test [target]` | Write comprehensive unit and integration tests |

### Examples

```
# Generate a commit message for staged changes
/commit

# Debug a specific error
/debug TypeError: Cannot read properties of undefined (reading 'map')

# Review the current file
/review

# Review a specific file
/review src/auth/middleware.ts

# Add documentation to a module
/document src/utils/validators.py

# Audit a controller for security issues
/security app/controllers/users_controller.rb
```

---

## Skills

Skills are detailed reference guides and prompt patterns for common development tasks. They are designed to be used as context for Claude — either pasted directly into a conversation or referenced by a command.

```
skills/
├── code-quality/
│   ├── code-review.md          # Structured code review methodology and severity levels
│   └── refactoring-patterns.md # Catalogue of common refactoring techniques with examples
├── testing/
│   ├── test-patterns.md        # AAA structure, parameterised tests, fakes vs mocks
│   └── tdd.md                  # Red-Green-Refactor cycle with worked examples
└── devops/
    └── ci-cd.md                # Pipeline design, GitHub Actions patterns, debugging
```

### Using a skill

You can instruct Claude to apply a skill by referencing it in your prompt:

```
Using the refactoring patterns in skills/code-quality/refactoring-patterns.md,
refactor the PaymentProcessor class in src/payments/processor.py
```

Or load it as context at the start of a session:

```
Please read skills/testing/tdd.md and then help me implement the UserService
class using TDD.
```

---

## Plugins

Plugins are [MCP (Model Context Protocol)](https://modelcontextprotocol.io/) server configurations that give Claude Code access to external tools and data sources.

```
plugins/
├── README.md            # Full usage guide and security notes
├── github.json          # GitHub issues, PRs, and repository data
├── postgres.json        # Read-only PostgreSQL queries
├── filesystem.json      # Extended file operations
├── brave-search.json    # Web search via Brave Search API
└── sentry.json          # Sentry error event analysis
```

### Quick start

1. Copy the relevant JSON snippet from a plugin file.
2. Add it to your `.claude/settings.json` under `"mcpServers"`.
3. Set any required environment variables.
4. Restart Claude Code.

See [`plugins/README.md`](plugins/README.md) for detailed instructions and security guidance.

---

## Contributing

Contributions are welcome. To add a new command:

1. Create a `.md` file in `.claude/commands/` named after the command (lowercase, hyphens for spaces).
2. Write the prompt. Use `$ARGUMENTS` where the user should supply input.
3. Add a row to the commands table in this README.

To add a skill, create a `.md` file in the most relevant `skills/` subdirectory and document it in the skills section above.

To add a plugin, create a JSON config file in `plugins/` following the existing format, and document it in `plugins/README.md`.

---

## License

[MIT](LICENSE)
