# AGENTS.md

## Cursor Cloud specific instructions

### Project overview

JOKEY is a cross-platform audio joke-sharing mobile app built with Expo (React Native) + TypeScript. The main project lives in `rork-JOKEY-main/expo/`. There is no monorepo — it is a single Expo app with an embedded tRPC/Hono backend.

### Package manager

**Bun** is the package manager (lockfile: `bun.lock`). Always use `bun install` (or `bun i`) for dependency management — never npm or yarn. If bun is not on PATH, source it: `export BUN_INSTALL="$HOME/.bun" && export PATH="$BUN_INSTALL/bin:$PATH"`.

### Key commands (run from `rork-JOKEY-main/expo/`)

| Task | Command |
|------|---------|
| Install deps | `bun install` |
| Lint | `bunx expo lint` |
| TypeScript check | `bunx tsc --noEmit` |
| Start web dev server | `bunx expo start --web --port 8081` |
| Start mobile dev server | `bun run start` (uses Rork CLI + tunnel) |

### Running the web dev server

Use `bunx expo start --web --port 8081` rather than `bun run start-web`, which invokes the Rork CLI with `--tunnel` and may not work in headless/cloud environments. The direct Expo command is more reliable.

### External services

- **Supabase** (hosted): credentials are hardcoded in `lib/supabase.ts`. No env vars needed for the primary data flow.
- **SurrealDB** (tRPC backend): requires `EXPO_PUBLIC_RORK_DB_*` env vars. Optional — the client-side flow uses Supabase directly.

### Codebase notes

- Pre-existing lint errors (unescaped entities in `delete-account.tsx`) and TS errors (implicit `any` types in `backend/trpc/` routes, undefined `playbackIntervalRef` in `AppContext.tsx`) exist in the repo. These are not regressions.
- The `Classement` (Ranking/Top 100) tab currently throws a runtime error in web mode — this is a pre-existing issue.
- The app supports French (primary), English, and Arabic via `constants/translations.ts`.
