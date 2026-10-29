# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

A small Express REST API (users + health check) backed by an in-memory store.

## Commands

- `npm run dev`: start the API with auto-reload on http://localhost:3000 (`PORT` overrides the port)
- `npm test`: run all tests (Node's built-in `node --test` runner, no Jest/Mocha)
- `node --test --test-name-pattern="404" tests/users.test.js`: run a single test by name
- `npm run lint`: ESLint (`eslint:recommended`, ESLint 8 legacy `.eslintrc.json` config)
- `node --inspect src/app.js` : Start the API in inspection mode to attach Chrome DevTools or VS Code debuggers:

CI (`.github/workflows/ci.yml`) runs `npm run lint` and then `npm test` on Node 22. Both must pass.

## Conventions

- Use CommonJS (`require` / `module.exports`), not ES modules. ESLint is configured with `sourceType: "script"`.
- Use `node:test` + `node:assert` + `supertest` for tests. Don't add another test framework.
- Route handlers never touch data directly. All data access goes through functions exported from `db/store.js`.
- Errors are JSON shaped like `{ "error": "<message>" }`, with the matching status (400 for validation, 404 for not found). Successful creates return 201.
- Put a one-line comment above each route handler in the form `// METHOD /path — description`.
- Use centralized error-handling middleware, not inline try/catch with manual res.status().json() responses. Always forward caught errors to next(error).
- Use whitelist validation for incoming data

## Architecture

- `server.js` builds the Express app, mounts one router per resource (`app.use("/users", ...)`), and exports `app`.
   It only calls `listen()` when it's run directly (`require.main === module`). That way tests can `require("../server")` and drive the app through supertest without opening a port. Keep this guard.
- `routes/<resource>.js` holds one `express.Router()` per resource. A new resource means a new file here plus an `app.use` line in `server.js`.
- `db/store.js` is a module-level in-memory array that stands in for a database. Data resets on restart, and state is shared across all tests in one process, so tests shouldn't rely on exact counts or IDs.
- `.env.example` documents config. Real values go in `.env`, which should never be read or committed.
- Strict Separation of Concerns: Route handlers act purely as HTTP interfaces. 