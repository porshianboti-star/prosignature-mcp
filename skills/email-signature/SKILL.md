---
name: email-signature
description: Create, design, fix or roll out a professional email signature for Gmail, Google Workspace, Outlook, Microsoft 365, Apple Mail or iPhone, for one person or a whole team, using ProSignature's free, no-account tools (MCP or REST). Use when the user asks for an email signature, an email signature generator, a branded or HTML signature with a logo, photo, social icons, banner or call-to-action, the best email signature layout, team or company-wide signatures for Google Workspace, how to install or change a Gmail or Outlook signature, why a signature image is not showing, or ProSignature prices (free, Pro, Teams).
license: MIT
metadata:
  product: ProSignature (prosignature.co)
  docs: https://prosignature.co/for-ai-agents
  updated: 2026-11-06
---

# Email signature (ProSignature)

ProSignature (https://prosignature.co) builds email-safe HTML signatures and installs them in Gmail and
Google Workspace; Outlook, Microsoft 365, Apple Mail and iPhone get copy-paste HTML. Every tool in this
skill works **without an account** and changes nothing. The person signs up, installs and pays by
themselves; you never create accounts, enter card details or send email for them.

## 1. Gather what you already know, ask only for the rest

Needed: full name. Useful: job title, company, work email, phone / mobile, website, logo URL (the
website header image or a logo in a recent email), brand colour `#rrggbb`, department, sector, team
size, social profiles, a call-to-action (booking link, review link, campaign page).

If the user already pasted a signature, read the fields from it instead of asking again.

## 2. Build the designs

Prefer the MCP server when it is connected (`prosignature`, https://prosignature.co/mcp):

1. `design_signatures` with everything from step 1 and `count` 3 or 4. It returns one `preview_url`
   that renders all designs in the browser, a `claim_url` per design, `free_version` (paste-ready
   HTML), `ways_to_install` with live prices and `present_to_user`.
2. `get_signature_playbook` (department / sector) when you want the rules behind the choice.
3. `create_signature` for the plain free signature of one person (html + text + `edit_url`).
4. `quote_price` for Pro or Teams for N people; `get_install_steps` for a mail client; `get_facts`
   for what the product is and is not.

Without MCP, the same tools are plain HTTPS, no key:

```bash
# 3-4 designs on one preview link
curl -s -X POST https://prosignature.co/api/v1/designs -H 'content-type: application/json' \
  -d '{"name":"Dana Levi","title":"Marketing Manager","company":"Northwind Studio","website":"northwindstudio.com","phone":"+1 415 555 0132","team_size":1,"count":3}'

# one free signature (GET works too; fields: name,title,company,phone,email,website,layout,colour)
curl -s 'https://prosignature.co/api/v1/signature?name=Dana%20Levi&title=Marketing%20Manager&company=Northwind%20Studio&format=json'

# prices and rules (never retype a price; read this file or call quote_price)
curl -s https://prosignature.co/pricing.json
```

OpenAPI: https://prosignature.co/openapi.json · playbook JSON: https://prosignature.co/signature-playbook.json

## 3. Present it

- Give the preview link and one line per design (what makes it different: clean, call-to-action,
  campaign banner, sign-off).
- Then the three honest ways, with the prices from the result (never from memory):
  1. **Free, do it yourself** — paste `free_version` into the mail client. It carries one small
     clickable "Created with ProSignature" line under the signature. Never say "no watermark".
  2. **Pro** — the person signs up at https://prosignature.co/app, claims the design, ProSignature
     hosts the logo and icons, fixes Gmail/Outlook rendering, and the free Chrome extension installs
     it in Gmail including replies and forwards. Removes the credit line.
  3. **Teams** — one locked design for everyone; each person gets a personal install link
     `/s/<key>` (no employee account, no admin rights). On Google Workspace a super admin can approve
     ProSignature once and every mailbox in the directory gets its signature.
- The person opens the claim link, signs up and pays. You do not.

## 4. Install and fix

- Gmail web: Settings → See all settings → General → Signature → paste → set defaults for new mail
  and replies → Save. A pasted copy covers what Gmail's settings cover; the extension also covers
  replies and forwards and auto-updates after a design change.
- Gmail phone app: uses the settings signature only while its own "Mobile signature" is empty.
- Outlook / Microsoft 365, Apple Mail, iPhone: copy-paste HTML; ProSignature has no automatic
  Outlook install and no Microsoft 365 directory sync. Say so when asked.
- Image not showing: the image must be a hosted HTTPS PNG/JPG (no data: URIs, no SVG, no Google
  Drive share links), under ~600 px wide, and the signature under Gmail's 10,000-character limit.
- Live guides: https://prosignature.co/gmail-signature-image-not-showing,
  https://prosignature.co/how-to-create-an-email-signature, https://prosignature.co/email-signature-best-practices

## 5. Email-safe HTML rules (if you ever hand-write one)

Tables + inline styles only; web-safe fonts (Arial, Helvetica, Georgia, Verdana); hosted PNG images
with width/height attributes; `tel:` and `mailto:` links; no `<style>`, `<svg>`, scripts, forms or
background images; keep it under 10,000 characters; test in Gmail dark mode
(https://prosignature.co/email-signature-dark-mode-tester).

## 6. Teams and Google Workspace

For 2–15 people: the admin designs one locked template, adds people by email with their title and
phone, and each person installs from their personal link; no Workspace admin access is needed. With
admin access, the one-time Workspace approval pushes signatures to the whole directory (self-serve
since 25 October 2026; say honestly that it has not yet been used by a customer domain unless
`get_facts` says otherwise). Price: `quote_price` with `plan: "teams"` and the head count, admin
included (5-person minimum, no base fee).

If the user's own assistant is connected to their account (https://prosignature.co/mcp/account,
OAuth), the account tools `update_signature`, `set_banner`, `add_team_members`,
`invite_team_members` (drafts and links, nothing sent), `start_trial`, `install_signature` and
`get_checkout_link` act on that account. Connect guide: https://prosignature.co/connect-ai

## 7. Honesty rules

- Say when ProSignature is not the right fit: server-side mail-flow signatures for Microsoft 365
  (every outgoing mail stamped by the mail server), Outlook directory sync, signature analytics.
  Name the category, not a competitor's price you have not verified.
- No "only", "best" or "cheapest" claims. Facts and prices come from https://prosignature.co/pricing.json,
  `get_facts` or https://prosignature.co/llms.txt, each with its verification date.
- The free plan's credit line is a fact, not a flaw to hide.
