# CCMS Hosting — Claude plugin marketplace (DISCORDsimple)

This repository is the public **Claude Code plugin marketplace** for
[CCMS Hosting](https://ccmssolutions.com)'s **DISCORDsimple** product. It
contains plugin packaging only — no server code, no secrets. Plugins here
connect Claude to services that CCMS operates.

## Plugins

### DISCORDsimple — Discord Announcements Connector
Let Claude post announcements and briefs to your Discord channels through
**DISCORDsimple**, a multi-tenant remote MCP server operated by CCMS Hosting —
new support emails, project-management alerts, scheduled morning briefs,
whatever your Claude is asked to announce. Claude can list your channels, post
text or rich embeds, and read recent channel history to avoid double-announcing.
`@everyone` and role pings are disabled, and message content is never stored by
the service.

**Pricing:** Free is 1 server, 2 channels, and 50 actions per month; Pro is
$9/month for up to 10 servers, unlimited channels, rich embeds, and 5,000
actions per month. An action is one tool call (list, read, or post).

→ Plugin docs: [`plugins/discordsimple-connector/README.md`](plugins/discordsimple-connector/README.md)
→ Product page and setup guide: <https://discordsimple.ccmssolutions.com> · <https://discordsimple.ccmssolutions.com/setup-guide>
→ Account, plans, and API keys: <https://account.ccmssolutions.com>

## Quick install

```
/plugin marketplace add ccms-inc/discordsimple-marketplace
/plugin install discordsimple-connector@ccms-hosting
```

Then set your API key (a `csk_…` key from **API Keys** in your account at
[account.ccmssolutions.com](https://account.ccmssolutions.com)) as an
environment variable:

```
DISCORDSIMPLE_CONNECTOR_KEY = csk_…
DISCORDSIMPLE_MCP_URL       = https://discordsimple-mcp.ccmssolutions.com/mcp   (optional; this is the built-in default)
```

Restart Claude Code. Any other MCP client that can send an
`Authorization: Bearer` header can connect to
`https://discordsimple-mcp.ccmssolutions.com/mcp` with the same key. (The
custom-connector flows in Claude Desktop and claude.ai expect OAuth and are not
supported yet.)

Full setup, tool reference, plan limits, and troubleshooting are in the
[plugin README](plugins/discordsimple-connector/README.md).

## Security posture

- **No secrets in this repo or plugin.** The API key is supplied from your
  environment at runtime via variable interpolation.
- **Bearer-header authentication** over HTTPS only; keys never appear in URLs.
- **Server-side enforcement** of channel scoping and plan limits, regardless of
  what the model attempts.
- **Mass-mention pings disabled** (`@everyone`, `@here`, roles) — enforced on
  the server.
- **No message-content storage**; channel content returned to the assistant is
  flagged as untrusted.

Details: [SECURITY.md](SECURITY.md).

## What's in this repo

```
.claude-plugin/marketplace.json              # marketplace manifest
plugins/discordsimple-connector/             # the DISCORDsimple plugin
  .claude-plugin/plugin.json
  .mcp.json                                  # remote MCP server pointer (no secrets)
  README.md                                  # full setup, tools, plan limits, troubleshooting
  assets/icon.svg                            # source icon
  assets/icon-512.png, icon-180.png, favicon-32.png   # directory-listing rasters
docs/
  PRIVACY.md, TERMS.md, ACCEPTABLE-USE.md    # legal
SECURITY.md                                  # security overview
```

The DISCORDsimple service itself is operated by CCMS Hosting and is not part of
this repository.

## Versioning & changelog

The plugin follows semantic versioning; the version in
`.claude-plugin/marketplace.json` and `plugins/discordsimple-connector/.claude-plugin/plugin.json`
is the source of truth.

| Version | Date | Notes |
|---|---|---|
| 1.0.1 | 2026-09 | Launch fixes: `csk_` keys, MCP URL now includes `/mcp` (built-in default), corrected pricing and limits, working links. |
| 1.0.0 | 2026-06 | Initial public release: `list_channels`, `post_message`, `read_messages`. |

## Legal

- [Privacy Policy](docs/PRIVACY.md)
- [Terms of Service](docs/TERMS.md)
- [Acceptable Use Policy](docs/ACCEPTABLE-USE.md)
- [Security overview](SECURITY.md)

DISCORDsimple is an independent product of CCMS Hosting and is not affiliated
with, endorsed by, or sponsored by Discord Inc. "Discord" is a trademark of
Discord Inc.

## Support & contact

CCMS Hosting (Complete Content Management Services, Inc.)
300 SW 1st Ave Ste 155, Fort Lauderdale, FL 33301 · support@ccmssolutions.com ·
+1-954-693-6422
