# AGENTS.md

## Cursor Cloud specific instructions

This repo is **Ant Design Pro** — a frontend-only React 19 + TypeScript SPA built on **UmiJS Max** (`@umijs/max`). There is **no backend in this repo**; `/api/*` calls are served by built-in **mockjs** mocks (default in dev), so the app runs fully self-contained with no database or external services.

### Running (dev)
- Use `npm start` (`UMI_ENV=dev max dev`) to run the dev server **with mocks enabled** on **http://localhost:8000**. This is the recommended self-contained way to run/test locally.
- Do **not** use `npm run dev` for local testing: it sets `MOCK=none` and relies on a proxy that is commented out in `config/proxy.ts`, so API calls will fail offline.
- Default mock login credentials: username `admin` (or `user`) / password `ant.design` (see `mock/user.ts`).
- Startup prints many `... is duplicated in mock/...` warnings — these are harmless (mock files overlap) and do not indicate a problem.

### Lint / Test / Build
- Standard commands live in `package.json` scripts. Lint = `npm run lint` (Biome lint + `tsc --noEmit`); test = `npm test` (Jest); build = `npm run build` (`max build`).
- `npm run lint` may report Biome warnings/info (e.g. `useOptionalChain`) but still exits 0; only treat errors as failures.
- Jest may print React `act(...)` warnings and "Jest did not exit one second after the test run" — these are noise; check the pass/fail summary.
- The build can be memory-hungry; CI sets `NODE_OPTIONS=--max_old_space_size=4096`. Use the same if you hit OOM during `npm run build`.

### Dependencies
- No lockfile is committed. `.npmrc` sets `legacy-peer-deps=true` (required because of React 19 peer-dep conflicts) — keep this when installing with npm. CI uses `bun install`, but `npm install` works locally.
- `postinstall` runs `max setup` automatically (generates `src/.umi` types); a fresh install is needed before `tsc`/lint/build will pass.
