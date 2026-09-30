# Security overview — DISCORDsimple

DISCORDsimple posts messages into your Discord server on your behalf, so
security is central to how it's built. This page summarizes the posture for
users of the connector.

## How your account is protected

- **Per-tenant isolation.** DISCORDsimple is multi-tenant: your servers,
  channels, and API keys are scoped to your account and enforced on every
  request. One account can never post to, or read from, another account's
  channels.
- **Server-side scoping.** Claude can only reach the channels you selected at
  the portal, and only within your plan's limits — enforced on the server
  regardless of what the model attempts.
- **Bot credentials stay on the server.** The Discord bot token lives on CCMS
  infrastructure; it is never placed in this plugin and never sent to
  Anthropic.
- **Encrypted in transit.** All connections use HTTPS/TLS — between Claude and
  the MCP endpoint, and between the service and Discord's API.
- **Not in this plugin.** This plugin contains no secrets. Your API key
  (`csk_…`) is supplied from your own environment at runtime and sent only as
  an `Authorization: Bearer` header (OAuth is also supported).

## What the connector does and doesn't do

- Messages are posted **on demand** at your direction; channel history is
  retrieved on demand (for de-duplication) — message content is **not stored**
  by the service.
- Posts can never ping `@everyone`, `@here`, or roles — mass-mention pings are
  disabled server-side.
- Channel content returned to the assistant is flagged as **untrusted** to
  reduce the risk of a malicious message manipulating the assistant.
- Your Discord data is **not** used to train AI models and is **not** sold.

## Handling your API key

Your `csk_` key is a bearer credential — anyone holding it can post to the
channels on your account. Keep it secret, never commit it, and use HTTPS only.
If it may have been exposed, rotate it immediately at
<https://discordsimple.ccmssolutions.com>.

## Scope

In scope for security reports: this packaging repository, the DISCORDsimple
portal (`https://discordsimple.ccmssolutions.com`), and the MCP endpoint
(`https://discordsimple-mcp.ccmssolutions.com`). Discord itself and Anthropic's
services are out of scope — report issues with those to their respective
programs.

## Reporting a vulnerability

Email **support@ccmssolutions.com** with details and steps to reproduce. Please do
not open a public issue for security reports, and allow a reasonable period for
remediation before any disclosure. We do not pursue action against good-faith
research that respects user data and service availability.

---

_Full data-handling details are in the [Privacy Policy](docs/PRIVACY.md)._
