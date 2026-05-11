# Rilo

Lifestyle product mockups for Etsy and print-on-demand sellers.

Marketing site and waitlist capture for [rilohq.com](https://rilohq.com).

---

## What this repo is

The static landing page served at `rilohq.com`. Single-file HTML, no build step.

The associated waitlist Worker lives in a separate directory locally and is deployed independently — see "Waitlist Worker" below.

## Stack

- **Hosting:** Cloudflare Pages, connected to this repo. Every push to `main` redeploys automatically.
- **Page:** Plain HTML + CSS in `index.html`. No framework, no build.
- **Fonts:** Fraunces (display) + JetBrains Mono (accents) via Google Fonts.
- **Waitlist form:** POSTs to `/api/waitlist`, which is handled by the `rilo-waitlist` Cloudflare Worker (not in this repo).

## Domains

| Domain | Status | Notes |
|---|---|---|
| `rilohq.com` | Canonical | Live landing page |
| `truemockups.com` | 301 redirect | → `rilohq.com` |
| `mocktable.com` | 301 redirect | → `rilohq.com` |

All three live in the dedicated Rilo Cloudflare account (separate from any other projects).

## Waitlist Worker

The form on the landing page POSTs to `https://rilohq.com/api/waitlist`. That endpoint is a Cloudflare Worker (`rilo-waitlist`) which:

- Validates the email
- Dedupes against existing entries
- Writes the entry to a KV namespace called `WAITLIST`
- Captures email, free-text context, timestamp, IP, country, and user-agent

The Worker source lives at `~/Documents/rilo/rilo-waitlist/` locally (not in this repo — deployed via `wrangler deploy`). KV namespace ID is recorded in the local `wrangler.toml`.

To view signups: Cloudflare dashboard → Workers & Pages → KV → `WAITLIST`.

## Local development

There is no build step. To preview the page locally:

```bash
# Just open it in a browser
open index.html
```

Or serve it via any static server if you want to test absolute paths:

```bash
npx serve .
```

## Deploy

Push to `main`. Cloudflare Pages picks up the change automatically (~30 seconds to live).

## Status

Pre-launch. Waitlist open. Product not yet built — this is the validation phase.
