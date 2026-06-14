# DISCORDsimple — Discord Announcements Connector for Claude

**DISCORDsimple** is a remote MCP connector that lets Claude post announcements and structured briefs to your Discord channels — new support emails, project alerts, scheduled morning digests, and more.

This repository is the **public marketplace listing** for DISCORDsimple. It contains the plugin manifest, MCP server config, and icon assets needed to install the connector in Claude.

> **Source code is proprietary.** The server that powers DISCORDsimple is operated exclusively by [CCMS Hosting](https://ccmshightech.com). This repo contains listing artifacts only — no server code.

---

## What it does

- Post text messages or rich embeds to any Discord channel your bot can access
- List available channels across your guilds
- Read recent channel history to avoid duplicate announcements
- Works as a Streamable HTTP remote MCP server — no local process required

> `@everyone` and role pings are disabled. Message content is never stored on CCMS servers.

---

## Installation

1. Sign up at **[ccmshightech.com/discordsimple](https://ccmshightech.com/discordsimple/)** to get your connector key and MCP endpoint URL.
2. In Claude, install this plugin using the manifest at `.claude-plugin/marketplace.json`.
3. Set the required environment variables in your Claude environment:

```
DISCORDSIMPLE_MCP_URL=<your endpoint URL from the account portal>
DISCORDSIMPLE_CONNECTOR_KEY=<your connector key>
```

---

## Files in this repo

| Path | Purpose |
|---|---|
| `.claude-plugin/marketplace.json` | Marketplace manifest (plugin registry entry) |
| `plugins/discordsimple-connector/.claude-plugin/plugin.json` | Plugin manifest |
| `plugins/discordsimple-connector/.mcp.json` | MCP server config (reads env vars) |
| `plugins/discordsimple-connector/assets/` | Icons (SVG + PNG at 512, 180, 32 px) |
| `docs/nginx-discordsimple-mcp.conf` | NGINX snippets for the MCP endpoint subdomain |
| `docs/nginx-discordsimple.conf` | NGINX snippets for the account portal subdomain |

---

## Operated by

**CCMS Hosting** — [ccmshightech.com](https://ccmshightech.com)  
Support: [ccmshightech@gmail.com](mailto:ccmshightech@gmail.com)
