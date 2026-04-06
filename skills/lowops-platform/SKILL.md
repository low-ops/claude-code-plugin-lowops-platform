---
name: lowops-platform
description: Context for using the LowOps Platform from Claude Code. Use when the user asks about deploys, environments, services, or platform operations exposed via this plugin's MCP server.
---

# LowOps Platform (placeholder)

This skill is a **lightweight guide**. Authoritative behavior lives in the **MCP tools** provided by your LowOps MCP server once it is reachable.

## Setup

1. Install the `lowops-platform` plugin from this repo's marketplace (see repository README).
2. When prompted, set **MCP base URL** and **API token** (`userConfig`).
3. Confirm the MCP connection in Claude Code (`/mcp`).

## Using the platform

- Prefer **MCP tool calls** for list/get/deploy/status/log flows instead of guessing URLs or APIs.
- Replace placeholder URLs and tokens with values from your team **before** production use.

## References

- Repository README: installation, versioning (release-please), and security notes.
