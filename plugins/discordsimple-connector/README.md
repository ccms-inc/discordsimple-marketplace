# DISCORDsimple — Discord Announcements Connector for Claude

Let Claude post announcements and briefs to your Discord channels — new support
emails, project-management alerts, scheduled morning briefs, whatever you ask it
to announce — through **DISCORDsimple by CCMS Hosting**, a multi-tenant remote
MCP server that runs on CCMS infrastructure so your Discord bot credentials
never live in Claude.

This plugin is a thin, secret-free pointer to the DISCORDsimple endpoint. You
supply two values — the **endpoint URL** and your **API key** (`dsk_…`) — and
Claude connects.

> **You need a DISCORDsimple account.** This plugin connects to the
> DISCORDsimple service operated by CCMS Hosting; it does not include the
> server. Sign in at **https://discordsimple.ccmssolutions.com**, click
> **"Add to Discord"** to install the DISCORDsimple bot on your server, pick the
> channels Claude may post to, and mint an API key.

---

## What you get

Once connected, Claude can use these tools (scoped to the channels you picked):

| Tool | Type | What it does |
|------|------|--------------|
| `list_channels` | read  | List your configured channel aliases plus live server channels. Call first. |
| `post_message`  | write | Post text and/or a rich embed to a channel (alias or ID). `@everyone`/role pings are disabled. |
| `read_messages` | read  | Recent channel history, newest first — used to avoid double-announcing. Content is flagged untrusted. |

Read tools run without a permission prompt; **`post_message` always prompts**
(it carries `destructiveHint`). Channel scoping and plan limits are enforced on
the server regardless of what the model attempts.

---

## Install

### 1. Add the marketplace and plugin

```
/plugin marketplace add ccms-hosting/discordsimple-marketplace   # the public repo CCMS gives you
/plugin install discordsimple-connector@ccms-hosting
```

### 2. Provide your endpoint and key

The bundled MCP server config ([`.mcp.json`](.mcp.json)) reads two values by
**environment-variable interpolation**, so no secret is ever stored in the
plugin:

| Variable | Example | Notes |
|----------|---------|-------|
| `DISCORDSIMPLE_MCP_URL` | `https://discordsimple-mcp.ccmssolutions.com/` | The MCP endpoint. **HTTPS, trailing slash.** |
| `DISCORDSIMPLE_CONNECTOR_KEY` | `dsk_…` (minted at the portal) | Sent as `Authorization: Bearer <key>`. Treat as a password. |

Set them in the environment Claude Code runs in:

```bash
# macOS / Linux (add to your shell profile)
export DISCORDSIMPLE_MCP_URL="https://discordsimple-mcp.ccmssolutions.com/"
export DISCORDSIMPLE_CONNECTOR_KEY="dsk_your-key-here"
```

```powershell
# Windows PowerShell (persist for your user)
setx DISCORDSIMPLE_MCP_URL "https://discordsimple-mcp.ccmssolutions.com/"
setx DISCORDSIMPLE_CONNECTOR_KEY "dsk_your-key-here"
```

Then restart Claude Code so the plugin's MCP server picks up the values.

### 3. Verify

Ask Claude: *"List my Discord channels."* It should call `list_channels` and
return your configured channel aliases. If you get an auth error, re-check the
URL (HTTPS, trailing slash) and the key.

---

## Plans

| | Free | Pro ($7/month) |
|---|---|---|
| Discord servers | 1 | 10 |
| Channels | 2 | Unlimited |
| Posts | Text only | Text + rich embeds |
| Message reading | Recent history (dedup) | Full message reading |
| Posts per month | 200 | 5,000 |

Billing is handled by Stripe at **https://discordsimple.ccmssolutions.com**.
Limits are enforced server-side per account.

---

## Authentication & privacy

- **Header auth.** Your key is sent as an `Authorization: Bearer` header — not
  in the URL — so it doesn't leak into web-server logs, proxies, or history.
  OAuth sign-in is also supported on the endpoint.
- **Bot credentials stay on the server.** The Discord bot token lives on CCMS's
  DISCORDsimple server; it is never placed in this plugin and never sent to
  Anthropic.
- **No message-content storage.** Messages you post pass through to Discord;
  the service does not store message bodies.
- **HTTPS only.** The endpoint is always `https://`.

---

## Security notes

- **No secrets in this plugin.** The key and URL come from your environment at
  runtime. Never hardcode a real key into `.mcp.json`.
- **Your key is a bearer credential.** Anyone with it can post to the channels
  on your account. If it's ever exposed, rotate it at the portal.
- **Mention pings are disabled.** Posts can never ping `@everyone`, `@here`, or
  roles — this is enforced server-side.
- **Untrusted content.** `read_messages` output is flagged as untrusted so a
  malicious channel message can't easily steer the assistant.

See [`SECURITY.md`](../../SECURITY.md), the
[privacy policy](../../docs/PRIVACY.md), [terms](../../docs/TERMS.md), and
[acceptable use policy](../../docs/ACCEPTABLE-USE.md).

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| `401 Unauthorized` | Key must start with `dsk_` and match a key minted at the portal. Re-check both env vars, then **restart Claude Code** (env changes don't apply to a running session). |
| Connection fails | URL must be `https://discordsimple-mcp.ccmssolutions.com/` — HTTPS, trailing slash. |
| "Unknown channel" / channel missing from `list_channels` | The channel must be selected for your account at the portal. Add it there, then retry. |
| Posts rejected with a limit error | You've hit your plan's monthly post quota (200 Free / 5,000 Pro) or server/channel cap. Upgrade or wait for the cycle reset. |
| Embeds rejected | Rich embeds are a **Pro** feature; Free posts are text only. |
| Bot can't post to a channel | The DISCORDsimple bot needs **View Channel** and **Send Messages** in that channel. Re-run "Add to Discord" if the bot was removed. |
| `@everyone` / role mentions don't ping | By design — mass-mention pings are disabled and cannot be enabled from the plugin. |

Still stuck? Email **support@ccmssolutions.com**.

---

## Branding

`assets/icon.svg` is the plugin icon — a rounded-square chat bubble with a
megaphone motif in Discord blurple `#5865F2` on a transparent background. It is
an original CCMS mark (not Discord's logo) and a vector master that scales to
any size; export PNG rasters from it for directory listings.

---

_DISCORDsimple is built and operated by **CCMS Hosting** (Complete Content
Management Services, Inc.). Support: support@ccmssolutions.com · +1-954-693-6422._
