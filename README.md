# ProSignature — email signature skill + MCP servers

Connect any AI assistant to [ProSignature](https://prosignature.co): professional email signatures for Gmail and Google Workspace (Outlook, Microsoft 365 and Apple Mail by copy-paste), for one person or a whole team. Ask your assistant for an email signature and it designs 3–4 options, builds email-safe HTML, quotes Pro and Teams prices and gives install steps — without an account. With sign-in it works on your own ProSignature account.

This repository holds the **skill** (`skills/email-signature/SKILL.md`, the Agent Skills format read by Claude Code, Codex, Cursor, Gemini CLI, Copilot and others), the **plugin manifests** (Claude Code / claude.ai, Cursor, Gemini CLI, Perplexity) and the **MCP server configs**.

Two remote servers (Streamable HTTP), no software to install:

| Server | URL | Sign-in | What it does |
|---|---|---|---|
| Public | `https://prosignature.co/mcp` | none | design 3–4 signatures, build email-safe HTML, quote prices, install steps, facts |
| Account | `https://prosignature.co/mcp/account` | OAuth 2.1 (your ProSignature account) | read and save your signature, banner, team and rollout, checkout link |

Official registry name: `co.prosignature/email-signatures` · Docs: https://prosignature.co/for-ai-agents · Guide: https://prosignature.co/connect-ai

## Install the skill (any agent that reads SKILL.md)

```bash
npx skills add porshianboti-star/prosignature-mcp
```

**Claude Code plugin** (skill + both MCP servers)
```bash
claude plugin marketplace add porshianboti-star/prosignature-mcp
claude plugin install prosignature@prosignature
```

**Perplexity** — Plugins → Add custom → upload this repository (plugin.json + SKILL.md + mcp.json are read as-is).

## Install the MCP servers only

**Claude Code**
```bash
claude mcp add --transport http prosignature https://prosignature.co/mcp
claude mcp add --transport http prosignature-account https://prosignature.co/mcp/account
```

**claude.ai / Claude desktop** — Customize → Connectors → Add custom connector → paste `https://prosignature.co/mcp/account` → Add → Connect → Allow.

**ChatGPT** — chatgpt.com/plugins → + → Add custom MCP server → paste the URL → Create.

**Cursor** — [Add public server](cursor://anysphere.cursor-deeplink/mcp/install?name=prosignature&config=eyJ1cmwiOiJodHRwczovL3Byb3NpZ25hdHVyZS5jby9tY3AifQ==) · [Add account server](cursor://anysphere.cursor-deeplink/mcp/install?name=prosignature-account&config=eyJ1cmwiOiJodHRwczovL3Byb3NpZ25hdHVyZS5jby9tY3AvYWNjb3VudCJ9)

**VS Code** — [Install public server](https://vscode.dev/redirect?url=vscode%3Amcp%2Finstall%3F%257B%2522name%2522%253A%2522prosignature%2522%252C%2522type%2522%253A%2522http%2522%252C%2522url%2522%253A%2522https%253A%2F%2Fprosignature.co%2Fmcp%2522%257D) · [Install account server](https://vscode.dev/redirect?url=vscode%3Amcp%2Finstall%3F%257B%2522name%2522%253A%2522prosignature-account%2522%252C%2522type%2522%253A%2522http%2522%252C%2522url%2522%253A%2522https%253A%2F%2Fprosignature.co%2Fmcp%2Faccount%2522%257D)

**Gemini CLI / Antigravity**
```bash
gemini extensions install https://github.com/porshianboti-star/prosignature-mcp
```

**Any other MCP client** — use `mcp.json` from this repo.

## What the account server can and cannot do

It designs and saves your signature, sets the banner, adds and removes people on your team and reads their install links and rollout status, and returns a checkout link. It never pays, creates accounts or sends email by itself. Access lasts up to 90 days and can be revoked under Settings in the app.

## What the skill tells an assistant

Gather what it already knows about the person, call `design_signatures` (3–4 designs on one preview link), present the three honest ways to install (free do-it-yourself paste with a small "Created with ProSignature" credit line · Pro · Teams) with live prices from `pricing.json`, give install steps per mail client, and say plainly when ProSignature is not the right fit (server-side Microsoft 365 signatures, Outlook directory sync, analytics). It never creates accounts, pays or sends email for the person.

## Links for your own site

Cursor deeplink (public): `cursor://anysphere.cursor-deeplink/mcp/install?name=prosignature&config=eyJ1cmwiOiJodHRwczovL3Byb3NpZ25hdHVyZS5jby9tY3AifQ==`
VS Code: `vscode:mcp/install?%7B%22name%22%3A%22prosignature%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A//prosignature.co/mcp%22%7D`
