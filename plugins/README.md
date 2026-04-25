# Plugins

This directory contains configuration and integration files for extending Claude Code with external tools and Model Context Protocol (MCP) servers.

## What are plugins?

Plugins for Claude Code are [MCP (Model Context Protocol)](https://modelcontextprotocol.io/) servers that provide Claude with additional tools — such as the ability to query a database, search the web, interact with external APIs, or run specialised analysis tools.

MCP servers are configured in Claude Code's settings file:

- **Project-level:** `.claude/settings.json` (committed to the repo, shared with the team)
- **User-level:** `~/.claude/settings.json` (personal, not committed)

## Configuration format

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@some-scope/mcp-server"],
      "env": {
        "API_KEY": "${MY_API_KEY}"
      }
    }
  }
}
```

## Included plugin configurations

| Plugin | Description | Config file |
|--------|-------------|------------|
| GitHub | Query issues, PRs, and repository data | [`github.json`](github.json) |
| PostgreSQL | Run read-only queries against a database | [`postgres.json`](postgres.json) |
| Filesystem | Extended file operations with configurable roots | [`filesystem.json`](filesystem.json) |
| Brave Search | Web search via Brave Search API | [`brave-search.json`](brave-search.json) |
| Sentry | Fetch and analyse Sentry error events | [`sentry.json`](sentry.json) |

## Usage

Copy the relevant JSON snippet from a plugin config file into your `.claude/settings.json` under `"mcpServers"`, then set any required environment variables.

### Example: enabling GitHub MCP

1. Copy the contents of [`github.json`](github.json) into `.claude/settings.json`.
2. Set the `GITHUB_PERSONAL_ACCESS_TOKEN` environment variable (or use a `.env` file with a tool like `direnv`).
3. Restart Claude Code.

## Security notes

- **Never commit secrets.** Always use environment variable references (`${VAR_NAME}`) in config files.
- **Review MCP servers before enabling.** An MCP server runs locally and can execute code on your machine. Only use servers from trusted sources.
- **Principle of least privilege.** Grant each MCP server only the permissions it needs (e.g., read-only database access).

## Adding a new plugin

1. Find or build an MCP-compatible server for the tool you need.
2. Create a new `<plugin-name>.json` file in this directory following the format above.
3. Document it in the table above.
4. Add any required environment variables to your shell profile or a `.env` file (never commit them).
