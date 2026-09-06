# SIPA Web AI

AI integration for small businesses — website chat widgets, WhatsApp AI
agents, and Telegram bots — built and run by Soul In PsyAbstract LLC,
sibling brand to `web.sipa-os.org` (landing pages) and `web-secure.sipa-os.org`
(EilatSecure, security scanning).

**Live site:** https://web-ai.sipa-os.org

## What this is

Where the market sells this in pieces — one vendor builds your site, another
sells a chat-widget SaaS subscription, a third does WhatsApp integration, a
fourth does custom model fine-tuning for enterprise clients only — this is
the same set of capabilities offered as one product, one point of contact,
one invoice. Verified before building: existing multi-channel bot platforms
(Tidio, ChatMaxima, SleekFlow) bundle website+WhatsApp+Instagram chat, but
run on their own shared cloud, use a generic base model with prompt/RAG
customization (not real fine-tuning), and assume you already have a website.
Fine-tune-plus-self-host as a category exists too (deviniti.com, premai.io,
petronellatech.com) but is priced and positioned for banks/law firms, not
small local businesses. The bundle of all four — build the site, fine-tune
per business, deploy on the client's own VPS/VM, cover website+WhatsApp+
Telegram — did not turn up anywhere in search as of 2026-09-06.

## Services (as shown on the landing page)

| Service | Price | What you get |
|---|---|---|
| Website chat widget | from ₪1,800 setup | Trained on the business's own content, one script tag, HE/EN/RU |
| AI WhatsApp agent | from ₪4,000 + ₪600/mo | Same production AI WhatsApp agent already sold via `web.sipa-os.org`/EilatSecure — 24/7, HE/EN/RU |
| Telegram bot | from ₪1,500 add-on / ₪2,500 standalone | Same knowledge base as other channels |
| Enterprise: own model, own server | from ₪12,000 | Custom fine-tuned weights deployed on the client's own VPS/VM — no third-party inference provider ever sees the conversation, only the messaging platform (WhatsApp/Telegram) does, same as any bot on those networks |

## Honesty note on data flow (see `privacy.html` for the full version)

The messaging platform (WhatsApp/Telegram, i.e. Meta/Telegram) always sees
the raw message — that's a condition of using their APIs at all, independent
of anything below. Beyond that:

- **Standard tiers** (widget, WhatsApp, Telegram): messages are sent to a
  third-party LLM inference provider to generate a response. This is stated
  plainly in `privacy.html` — no "we never log your data" claim, because it
  would not be true.
- **Enterprise tier**: inference runs on infrastructure the client controls
  (their own VPS/VM), so no third-party AI provider is in the loop for that
  tier specifically. The messaging-platform layer above still applies
  regardless of tier — self-hosting removes one hop, not both.

## Repository layout

```
index.html          landing page (services, pricing, footer links to siblings)
privacy.html         Privacy Policy
terms.html           Terms of Service
faq.html              FAQ (with FAQPage JSON-LD)
*.sha256 / *.TAG      integrity seals (see below)
```

## Sibling sites — cross-linked, not merged

- `web.sipa-os.org` — AI-built landing pages for local businesses
- `web-secure.sipa-os.org` — EilatSecure, passive security scanning
- `web-ai.sipa-os.org` — this site

Each links to the other two in its footer. They remain separate products/
domains per the "2 domains = 2 apps" rule already in force across this
ecosystem — this is deliberate cross-promotion, not a merge.

## Deploying

```bash
export CLOUDFLARE_API_TOKEN="$CLOUDFLARE_WORKERS_TOKEN"
npx wrangler pages deploy . --project-name=sipa-web-ai --branch=main
```

## Integrity seals

Every content file in this repo is sealed with a `<file>.sha256` and
`<file>.TAG` sidecar pair, produced by the shared `reseal.py` tool used
across SIPA OS projects:

```bash
python3 /path/to/sipa-os-governance/scripts/reseal.py <path> [DEVICE]
```
