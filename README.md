# Remitflex documentation

Public API and product documentation for [Remitflex](https://remitflex.io), built with [Mintlify](https://mintlify.com).

## Local development

Install the [Mintlify CLI](https://www.npmjs.com/package/mint):

```bash
npm i -g mint
```

From this directory:

```bash
npm install
npm run dev
```

Preview at `http://localhost:3333/introduction`. If port 3333 is in use (e.g. by the dashboard app), pick another port:

```bash
npx mint dev --port 3334
```

Then set `NEXT_PUBLIC_DOCS_URL` in `remitflex-app/.env.local` to match.

The API playground sends requests **directly from your browser** (`proxy: false`). It defaults to **Local development** (`http://localhost:4000/v1`) — ensure the API is running and `http://localhost:3333` is in `ALLOWED_ORIGINS`.

`api.remitflex.io` may not resolve until production DNS is live; use **Local development** for local testing. Before publishing docs to customers, put **Production** first in `openapi.yaml` `servers` and set `"proxy": true` in `docs.json`.

## Publishing

Changes deploy automatically when pushed to the default branch (Mintlify GitHub app).

## Source of truth

Keep docs in sync with `remitflex-api` and `remitflex-app`. Internal engineering notes live in `remitflex-api/doc/`.
