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

→ Plugin docs: [`plugins/discordsimple-connector/README.md`](plugins/discordsimple-connector/README.md)
→ Get an account: <https://discordsimple.ccmssolutions.com>
→ Product page: <https://ccmssolutions.com/discordsimple/>

## Quick install

```
/plugin marketplace add ccms-inc/discordsimple-marketplace
/plugin install discordsimple-connector@ccms-hosting
```

Then set your endpoint URL and API key as environment variables:

```
DISCORDSIMPLE_MCP_URL       = https://discordsimple-mcp.ccmssolutions.com/
DISCORDSIMPLE_CONNECTOR_KEY = dsk_…   (minted at https://discordsimple.ccmssolutions.com)
```

Full setup, tool reference, plan limits, and troubleshooting are in the
[plugin README](plugins/discordsimple-connector/README.md).

## Security posture

- **No secrets in this repo or plugin.** The endpoint URL and API key are
  supplied from your environment at runtime via variable interpolation.
- **Bearer-header authentication** over HTTPS only (OAuth also supported on the
  endpoint); keys never appear in URLs.
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
  nginx-discordsimple.conf                   # NGINX snippets — account portal subdomain
  nginx-discordsimple-mcp.conf               # NGINX snippets — MCP endpoint subdomain
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
