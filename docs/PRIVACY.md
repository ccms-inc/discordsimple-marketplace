# DISCORDsimple — Privacy Policy

**Effective date:** August 19, 2026
**Service:** DISCORDsimple ("the Service")
**Provider:** Complete Content Management Services, Inc. d/b/a CCMS Hosting
("CCMS", "we", "us", "our")
**Address:** 3641 SW 21st Ct, Fort Lauderdale, FL 33312, United States
**Contact:** support@ccmssolutions.com · +1-954-693-6422

This Service is offered from the United States and is intended for U.S. users
(see §10).

---

## 1. What DISCORDsimple is

DISCORDsimple is a multi-tenant Model Context Protocol (MCP) connector that
lets an AI assistant (such as Anthropic's Claude) post announcements and briefs
to Discord channels **you** choose, over a secure HTTPS connection. You sign in
at the DISCORDsimple portal, install the DISCORDsimple bot on your Discord
server, select the channels the assistant may use, and mint an API key. It does
not host Discord and does not store your conversations.

## 2. Information we process

- **Account & identity data** — the identifier and email address from your
  sign-in (via our identity provider), used to create your account.
- **Discord integration data** — your Discord server (guild) IDs, the IDs and
  names of the channels you select, and channel aliases you configure. We do
  **not** receive your Discord password; the bot is installed through Discord's
  own authorization flow.
- **Message content posted on your behalf (transient)** — the text and embeds
  the assistant posts at your direction pass through the Service to Discord's
  API. We do **not** store message bodies.
- **Channel history (on demand, transient)** — recent messages read via
  `read_messages` to avoid double-announcing, retrieved only to fulfill a
  request and not stored.
- **API keys** — the `dsk_` keys you mint, used to authenticate your requests.
- **Billing data** — for paid plans, your plan status and a Stripe customer
  identifier. Card numbers are handled by Stripe; we do not store them.
- **Operational data** — server logs and a per-account audit log of post
  actions (action, channel, timestamp, post counts for plan enforcement —
  never message bodies).

## 3. How we use information

We use the information solely to provide and operate the Service: authenticate
you, post to and read the channels you authorized as you direct, enforce plan
limits, process payments, and maintain security and an audit trail. We do
**not**:

- sell or rent your personal information;
- "share" it for cross-context behavioral advertising;
- use your message content or Discord data to train any AI model;
- store the content of messages posted or read through the Service;
- post to your channels except at the direction of your assistant under your
  key, or read your channels except to fulfill a request you make.

## 4. Discord platform compliance

DISCORDsimple accesses Discord through Discord's official bot and API
mechanisms and adheres to the
[Discord Developer Terms of Service](https://discord.com/developers/docs/policies-and-agreements/developer-terms-of-service)
and [Developer Policy](https://discord.com/developers/docs/policies-and-agreements/developer-policy),
including their restrictions on data retention and use. Mass-mention pings
(`@everyone`, `@here`, roles) are disabled by the Service. DISCORDsimple is not
affiliated with Discord Inc.

## 5. Disclosures to third parties

We share information only with providers needed to run the Service:

| Provider | Purpose | Data involved |
|---|---|---|
| Hosting (CCMS server infrastructure) | Run the Service | Account, integration, and operational data |
| Identity provider | Sign-in | Your login identity |
| Stripe | Billing for paid plans | Email, plan, payment details (handled by Stripe) |
| Discord | The servers/channels you connect | Channel IDs and the message content you direct the assistant to post or read |
| Anthropic (Claude) | The AI assistant you direct | The channel content the assistant retrieves, under Anthropic's terms |

We may also disclose information to comply with law, enforce our Terms, or
protect rights and safety. We do not otherwise sell or share your data.

> **About the AI assistant.** When you use DISCORDsimple through Claude, the
> content the assistant retrieves is processed by Anthropic under Anthropic's
> terms and privacy policy, which we do not control. Channel messages are
> returned to the assistant flagged as untrusted content to reduce the risk of
> a malicious message manipulating the assistant.

## 6. Data retention

- **Integration data and API keys** are retained until you disconnect the
  server, revoke the key, or delete your account, then deleted from active
  systems.
- **Message content** is not stored — it transits the Service only long enough
  to deliver or return it.
- **Account/billing records** are retained as needed for the account and for
  legal/tax obligations.
- **Logs/audit entries** are retained for a limited operational period as needed
  for operations and security, and exclude message bodies.
- Backups are purged on our routine cycle.

## 7. Security

We use per-tenant isolation enforced on every request, bearer-key
authentication over TLS, server-side channel scoping and plan enforcement, rate
limiting, and origin validation; the Discord bot token is held in server
configuration outside the web root. No system is perfectly secure, but we
design so that one account can never reach another account's servers or
channels. If a breach affecting your data occurs, we will notify you as
required by applicable U.S. state law.

## 8. Your U.S. privacy rights

Depending on your state (e.g. California's CCPA/CPRA and similar laws in other
states), you may have the right to **access**, **correct**, **delete**, or
obtain a **copy** of your personal information, and to **not be discriminated
against** for exercising these rights. Because we do not sell or share personal
information for cross-context behavioral advertising, there is nothing to opt
out of in that respect. You can exercise most rights directly in the portal
(disconnect a server, revoke a key, delete your account) or by contacting us at
the address above; we will verify and respond as required by law.

## 9. Children

The Service is not directed to children under 13 (or under 16 where
applicable), and we do not knowingly collect their information. Note that
Discord itself requires users to meet its minimum-age requirements. If you
believe a child has provided us information, contact us and we will delete it.

## 10. U.S.-only; international users

The Service is operated from the United States and intended for users in the
United States. It is **not** directed to individuals in the European Economic
Area, the United Kingdom, or other regions with data-transfer or GDPR-style
requirements, and we do not offer GDPR-specific mechanisms. If you access the
Service from outside the U.S., you do so on your own initiative and are
responsible for compliance with local law; do not use the Service if local law
prohibits this processing.

## 11. Changes

We may update this policy; the effective date above reflects the latest
version, and we will notify account holders of material changes.

## 12. Contact

Complete Content Management Services, Inc. (CCMS Hosting)
3641 SW 21st Ct, Fort Lauderdale, FL 33312 · support@ccmssolutions.com ·
+1-954-693-6422
