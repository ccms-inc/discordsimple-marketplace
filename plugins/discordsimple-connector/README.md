# DISCORDsimple — Discord Announcements Connector for Claude

Let Claude post announcements and briefs to your Discord channels — new support
emails, project-management alerts, scheduled morning briefs, whatever you ask it
to announce — through **DISCORDsimple by CCMS Hosting**, a multi-tenant remote
MCP server that runs on CCMS infrastructure so your Discord bot credentials
never live in Claude.

This plugin is a thin, secret-free pointer to the DISCORDsimple endpoint. You
supply one value — your **API key** (`csk_…`) — and Claude connects. (The
endpoint URL is built in; see [Install](#install) to override it.)

> **You need a DISCORDsimple account.** This plugin connects to the
> DISCORDsimple service operated by CCMS Hosting; it does not include the
> server. Full walkthrough: **https://discordsimple.ccmssolutions.com/setup-guide**.
> In short:
>
> 1. **Sign up and choose a plan** at **https://account.ccmssolutions.com**.
> 2. **Generate a key** at **https://account.ccmssolutions.com** under
>    **API Keys** (choose DiscordSimple). It starts with `csk_` and is shown
>    once.
> 3. **Connect your server** at **https://discordsimple-mcp.ccmssolutions.com** —
>    sign in, choose **Connect a Discord server** to install the DISCORDsimple
>    bot, and pick the channels Claude may post to.

---

## What you get

Once connected, Claude can use these tools (scoped to the channels you picked):

| Tool | Type | What it does |
|------|------|--------------|
| `list_channels` | read  | List your configured channel aliases plus live server channels. Call first. |
| `post_message`  | write | Post text and/or a rich embed to a channel (alias or ID). `@everyone`/role pings are disabled. |
| `read_messages` | read  | Recent channel history, newest first — used to avoid double-announcing. Content is flagged untrusted. |

`list_channels` and `read_messages` are marked read-only; `post_message` is a
write tool. Channel scoping and plan limits are enforced on the server
regardless of what the model attempts.

---

## Plans

| | Free | Pro ($9/month) |
|---|---|---|
| Discord servers | 1 | 10 |
| Channels | 2 | Unlimited |
| Posts | Text only | Text + rich embeds |
| Message reading | Recent history (dedup) | Full message reading |
| Actions per month | 50 | 5,000 |

An **action** is one tool call Claude makes through DISCORDsimple: listing
channels, reading messages, and posting a message each count as one. Setting up
and signing in do not.

Subscriptions, invoices, and API keys all live at your CCMS account portal,
**https://account.ccmssolutions.com** (payments processed by Stripe). Limits are
enforced server-side per account.

---

## Install

### 1. Add the marketplace and plugin

In Claude Code:

```
/plugin marketplace add ccms-inc/discordsimple-marketplace
/plugin install discordsimple-connector@ccms-hosting
```

### 2. Provide your key (and, optionally, the endpoint)

The bundled MCP server config ([`.mcp.json`](.mcp.json)) reads its settings by
**environment-variable interpolation**, so no secret is ever stored in the
plugin:

| Variable | Value | Notes |
|----------|-------|-------|
| `DISCORDSIMPLE_CONNECTOR_KEY` | `csk_…` (from account.ccmssolutions.com → API Keys) | **Required.** Sent as `Authorization: Bearer <key>`. Treat as a password. |
| `DISCORDSIMPLE_MCP_URL` | `https://discordsimple-mcp.ccmssolutions.com/mcp` | Optional; this is the default. **HTTPS and the `/mcp` path** — the host root serves the sign-in app, not the connector. |

Set them in the environment Claude Code runs in:

```bash
# macOS / Linux (add to your shell profile)
export DISCORDSIMPLE_MCP_URL="https://discordsimple-mcp.ccmssolutions.com/mcp"
export DISCORDSIMPLE_CONNECTOR_KEY="csk_your-key-here"
```

```powershell
# Windows PowerShell (persist for your user)
setx DISCORDSIMPLE_MCP_URL "https://discordsimple-mcp.ccmssolutions.com/mcp"
setx DISCORDSIMPLE_CONNECTOR_KEY "csk_your-key-here"
```

Then restart Claude Code so the plugin's MCP server picks up the values.

### 3. Verify

Ask Claude: *"List my Discord channels."* It should call `list_channels` and
return your configured channel aliases. If you get an auth error, re-check the
key. A `404` means the URL is missing the `/mcp` path; a `402` means the
subscription lapsed — settle it on the **Billing** page at
https://account.ccmssolutions.com.

---

## Other MCP clients

DISCORDsimple is a standard remote MCP server (Streamable HTTP). It works with
any MCP client that can send an `Authorization: Bearer` header: point the client
at `https://discordsimple-mcp.ccmssolutions.com/mcp` and send your `csk_…` key as
`Authorization: Bearer csk_…`.

The custom-connector flows in the Claude Desktop and claude.ai apps expect an
OAuth sign-in, which DISCORDsimple does not offer yet, so pasting a key there
will not connect. Use Claude Code with this plugin.

---

## Authentication & privacy

- **Header auth.** Your key is sent as an `Authorization: Bearer` header — not
  in the URL — so it doesn't leak into web-server logs, proxies, or history.
- **Bot credentials stay on the server.** The Discord bot token lives on CCMS's
  DISCORDsimple server; it is never placed in this plugin and never sent to
  Anthropic.
- **No message-content storage.** Messages you post pass through to Discord;
  the service does not store message bodies. See the
  [privacy policy](https://discordsimple.ccmssolutions.com/privacy-policy).
- **HTTPS only.** The endpoint is always `https://`.

---

## Security notes

- **No secrets in this plugin.** The key comes from your environment at
  runtime. Never hardcode a real key into `.mcp.json`.
- **Your key is a bearer credential.** Anyone with it can post to the channels
  on your account. If it's ever exposed, revoke and regenerate it under
  **API Keys** at https://account.ccmssolutions.com — the old key stops working
  at once.
- **Mention pings are disabled.** Posts can never ping `@everyone`, `@here`, or
  roles — this is enforced server-side.
- **Untrusted content.** `read_messages` output is flagged as untrusted so a
  malicious channel message can't easily steer the assistant.

See [`SECURITY.md`](../../SECURITY.md) in this repository, and the hosted
[privacy policy](https://discordsimple.ccmssolutions.com/privacy-policy),
[terms of service](https://discordsimple.ccmssolutions.com/terms-of-service), and
[acceptable use policy](https://discordsimple.ccmssolutions.com/acceptable-use).

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| `401 Unauthorized` | Key must start with `csk_` and be active under **API Keys** at account.ccmssolutions.com. Re-check the key, then **restart Claude Code** (env changes don't apply to a running session). |
| `402 Subscription inactive` | The DiscordSimple subscription lapsed or the account is suspended — settle it on the **Billing** page at account.ccmssolutions.com and retry. |
| Connection fails, or `404` | URL must be `https://discordsimple-mcp.ccmssolutions.com/mcp` — the `/mcp` path, not the host root (the root is the sign-in app). |
| "Unknown channel" / channel missing from `list_channels` | The channel must be selected for your account at `https://discordsimple-mcp.ccmssolutions.com`. Add it there, then retry. |
| "Monthly usage limit reached" / posts rejected with a limit error | You've used your plan's monthly actions (50 Free / 5,000 Pro) or hit the server/channel cap. Upgrade at account.ccmssolutions.com or wait for the monthly reset. |
| Embeds rejected | Rich embeds are a **Pro** feature; Free posts are text only. |
| Bot can't post to a channel | The DISCORDsimple bot needs **View Channel** and **Send Messages** in that channel. Choose **Connect a Discord server** again if the bot was removed. |
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
