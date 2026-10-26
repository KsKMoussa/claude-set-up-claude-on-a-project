# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

A small Express JSON API (users + health) used as the working codebase for the Claude Code course projects.

## Commands

```bash
npm install
npm run dev                      # node --watch server.js — API on http://localhost:3000
npm test                         # node --test — discovers tests/**/*.test.js
node --test tests/users.test.js  # run one test file
node --test --test-name-pattern "returns ok"   # run one test by name
npm run lint                     # eslint .
```

CI (`.github/workflows/ci.yml`) runs `npm install`, `npm run lint`, then `npm test` on Node 22 for every push and PR — both must pass.

## Architecture

- `server.js` builds and exports the `app`; it only calls `app.listen` when run directly (`require.main === module`). Tests import `app` and drive it with `supertest`, so no port is opened. Keep this guard intact — removing it hangs the test run.
- `routes/` holds one router per resource, mounted in `server.js` under its path prefix (`routes/users.js` → `/users`). A new resource means a new file in `routes/` plus one `app.use` line.
- `db/store.js` is the only data access layer — an in-memory array exposed through named functions. Route handlers never touch the array directly, and state resets on every restart, so tests must not assume a user created by one test exists in another.

## Conventions

- CommonJS only (`require` / `module.exports`). ESLint is configured with `sourceType: "script"`; `import`/`export` will fail lint.
- Node's built-in test runner (`node:test` + `node:assert`) and `supertest` — no Jest, Mocha, or Chai.
- Express only; `express` is the single runtime dependency. Prefer the standard library over adding packages.
- Double-quoted strings and semicolons, matching the existing files.
- Validate required fields in the route handler and return `res.status(4xx).json({ error: "..." })` — errors are always a JSON object with an `error` key, never a bare string.
- Read config from `process.env` with an inline fallback (`process.env.PORT || 3000`). Document any new variable in `.env.example`; never put real values there.
