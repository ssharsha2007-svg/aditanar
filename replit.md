# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Each package manages its own dependencies.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally

See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details.

## Artifacts

- `artifacts/api-server` — Express API. Custom routes in `src/routes/site.ts` provide `/api/site` (GET/POST JSON site state), `/api/login`, `/api/logout`, `/api/me`, `/api/credentials`. Auth uses bcryptjs + cookie sessions (`adithanar_sid`). Default admin seeded on first boot: `admin / adithanar2024`. Persistence in two raw-SQL Postgres tables: `site_state(id, data jsonb, updated_at)` and `admin_user`/`admin_session`.
- `artifacts/adithanar-legacy` — The Adithanar Legacy site. Hand-written `index.html` (no React tree mounted; `src/main.tsx` is a no-op, kept for Vite HMR). All admin/CMS behavior lives in the inline script and talks to `/api/*` (proxied to the api-server). Site state is loaded from `/api/site` on boot and saved with the in-panel "Save Changes" button — no HTML download.
