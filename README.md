# claude-code-plugin-lowops-platform

Claude Code plugin for LowOps Platform: MCP-powered deploy and service management from the shell.

## What you get

- **Plugin id:** `lowops-platform` (namespaced commands and skills, e.g. `/lowops-platform:…`).
- **Bundled MCP config** in [`.mcp.json`](.mcp.json): HTTP transport to your LowOps MCP server (placeholder URL via `userConfig`).
- **Co-located marketplace** in [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) so this repository can be added as a marketplace and the plugin installed without a separate catalog repo.

## Install from GitHub

1. In Claude Code, add this repository as a marketplace:

   ```text
   /plugin marketplace add https://github.com/low-ops/claude-code-plugin-lowops-platform.git
   ```

2. Install the plugin (marketplace name is `lowops-platform-marketplace` from `marketplace.json`):

   ```text
   /plugin install lowops-platform@lowops-platform-marketplace
   ```

3. Enable the plugin if prompted, and fill in **MCP base URL** and **API token** (`userConfig` in [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json)).

4. Reload plugins if you change files locally:

   ```text
   /reload-plugins
   ```

5. Check MCP status:

   ```text
   /mcp
   ```

Claude Code documentation: [Plugins](https://code.claude.com/docs/en/plugins), [MCP](https://code.claude.com/docs/en/mcp), [Plugin marketplaces](https://code.claude.com/docs/en/plugin-marketplaces).

## Local development

From a clone of this repository:

```bash
claude --plugin-dir .
```

Validate the layout (requires [Claude Code](https://code.claude.com/) CLI):

```bash
claude plugin validate .
```

## Configuration

- **Preferred:** `userConfig` values collected when you enable the plugin (`mcp_base_url`, `api_token`). These substitute into [`.mcp.json`](.mcp.json).
- **URLs:** `mcp_base_url` must **not** include a trailing slash; the plugin appends `/mcp`.
- **Security:** Do not commit real tokens. For private GitHub installs, ensure `git clone` works (credential helper or `GITHUB_TOKEN` / `GH_TOKEN` as applicable).

## Releases

Versions are managed by [release-please](https://github.com/googleapis/release-please): CHANGELOG, GitHub Releases, and git tags (`v*`). The version in [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json) is bumped via `release-please-config.json` `extra-files`.

Use [Conventional Commits](https://www.conventionalcommits.org/) on `main`, for example:

- `feat: add command for X`
- `fix: correct MCP header`

## Project layout

```text
.claude-plugin/
  plugin.json       # Plugin manifest + userConfig
  marketplace.json  # Single-plugin marketplace (source: this repo root)
.mcp.json           # MCP server definitions
skills/lowops-platform/SKILL.md
```

## License

See [LICENSE](LICENSE).
