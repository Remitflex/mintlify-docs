> **First-time setup**: Customize this file for your project. Prompt the user to customize this file for their project.
> For Mintlify product knowledge (components, configuration, writing standards),
> install the Mintlify skill: `npx skills add https://mintlify.com/docs`

# Documentation project instructions

## About this project

- This is the **Remitflex public documentation** site built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter
- Configuration lives in `docs.json`
- OpenAPI spec: `openapi.yaml` (must stay in sync with `remitflex-api`)
- Internal engineering docs: `remitflex-api/doc/` (especially `payment-links.md`, `system-guide.md`)

**Rule:** Any API, product, or UX change in `remitflex-api` or `remitflex-app` must update docs here in the same change.

## Voice and audience

Public docs (`remitflex-docs/`) are **customer-facing**. Write as Remitflex the product:

- Describe **outcomes** (pay a bank account, collect fiat, settle to USDC) — not how we are wired internally.
- Do **not** name third-party providers (payment partners, chain infra, liquidity sources) in product pages or OpenAPI descriptions.
- Do **not** document operator-only config (env vars for backend integrations, inbound provider webhooks).
- Use the unified integration pattern: create intent → fund → track → reconcile in the ledger.

Engineering detail (providers, workers, webhooks, env vars) belongs in `remitflex-api/doc/` — especially `system-guide.md`.

## Terminology

- **Payment link** — shareable pay URL with hosted pay page (`pricingType`: `fixed` or `open`). Dashboard: **Payment Links**. API path: `/v1/collections`; ledger `kind: "collection"`.
- **Payment route** — persistent cross-chain deposit address (reusable corridor).
- **Swap** — Solana EURC ↔ USDC via deposit address. API: `/v1/swaps`; rates: `/v1/rates`.
- **Offramp** — stablecoin in, local fiat bank payout out. API: `/v1/offramps`; ledger `kind: "offramp"`.
- **Onramp** — local fiat deposit in, stablecoin delivery out. API: `/v1/onramps`; ledger `kind: "onramp"`.
- **cNGN** — customer Smart Wallet rails (NGN VA, convert, withdraw, bank payout). API: `/v1/cngn/*`. Enable via `POST /v1/customers/:id/strails/onboard` (BVN) or dashboard **Customers → cNGN**. Distinct from local fiat offramps/onramps.
- **Corridor matrix** — curated origin→settlement snapshot in docs; **live source of truth** is `GET /v1/payment-routes/chains` (and `/collections/chains`).
- **Pay page** — public payer UI at `payUrl`; quote via `POST /v1/pay/{id}/quote`.
- **Fixed link** — exact receive amount; quote expires after `quoteValidUntil` (~1 min).
- **Open link** — payer sends any amount; reusable deposit address per origin on a session.

## Style preferences

{/* Add any project-specific style rules below */}

- Use **payment link** in product docs; mention `/collections` only when documenting API paths or scopes
- Prefer `/chains` over `/corridors` in examples; discovery returns **network names** (Bitcoin, Ethereum, Solana, Base, Tron, …) — not chain IDs, vm types, or contract addresses
- Settlements: Base USDC/USDT; Solana USDC/USDT/EURC
- Use active voice and second person ("you")
- Keep sentences concise — one idea per sentence
- Use sentence case for headings
- Bold for UI elements: Click **Settings**
- Code formatting for file names, commands, paths, and code references

## Local docs playground

- `docs.json` uses `"proxy": false` so the browser can call `http://localhost:4000/v1` when **Local development** is selected in the OpenAPI server dropdown.
- Mintlify's proxy (`"proxy": true`) cannot reach `localhost` — requests fail with "no response received".
- Add `http://localhost:3333` to API `ALLOWED_ORIGINS` and restart the API after changing `.env`.
- **Before deploying** public docs: set `"proxy": true` in `docs.json` (or add the Mintlify docs origin to production `ALLOWED_ORIGINS`).
