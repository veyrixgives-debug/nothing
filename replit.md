# BloxGifts

A Roblox rewards platform where users can complete quests to earn Robux, Discord Nitro, and other prizes.

## Run & Operate

- `pnpm --filter @workspace/bloxgifts run dev` — run the frontend (port assigned by workflow)
- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: Vite + React (static site served via Vite)
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/bloxgifts/` — the BloxGifts frontend (Vite artifact at `/`)
- `artifacts/bloxgifts/public/` — pre-built static assets (CSS, JS bundles, images)
- `artifacts/bloxgifts/index.html` — main HTML entry point loading the pre-built bundles
- `artifacts/api-server/` — backend Express API server
- `lib/api-spec/openapi.yaml` — API spec (source of truth)
- `lib/db/` — Drizzle ORM schema and database connection

## Architecture decisions

- The imported BloxGifts site was a pre-built static app (not Next.js). It ships its own compiled JS (`cdc03e.js`) and CSS (`5aee6c.css`) bundles.
- Rather than decompiling and converting the pre-built bundles to React components, we serve the original bundles directly from Vite's `public/` directory. Vite acts as a static file server for the pre-built assets.
- The `index.html` loads the original static bundles directly, bypassing the React entrypoint — the site has its own React bundle internally.
- All images and assets (webp, flags, resources) are in `public/` and served as-is.

## Product

BloxGifts is a Roblox reward platform. Users select a reward (Robux, Discord Nitro, etc.), enter their username, and complete quests to earn prizes. Features include a live chat feed showing recent claimants, a help FAQ panel, and multi-language support.

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- Do NOT run `pnpm dev` or `pnpm run dev` at the workspace root — Replit apps run via workflows.
- The pre-built JS/CSS bundles in `public/` are minified — do not attempt to edit them directly. If you need to modify the app's functionality, the source code is not available in this repo (it was a pre-built export).
- A woff2 font fails to decode (OTS parsing error) — this is a pre-existing issue from the original site and is cosmetic only.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
