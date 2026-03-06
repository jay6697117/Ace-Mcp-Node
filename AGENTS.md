# AGENTS.md

## Cursor Cloud specific instructions

### Overview

Acemcp-Node is an MCP (Model Context Protocol) server providing codebase indexing and semantic search for AI assistants. It is a single-package Node.js/TypeScript project (not a monorepo).

### Key commands

See `package.json` scripts for the full list. The most important ones:

| Command | Purpose |
|---|---|
| `npm run build` | Compile TypeScript + copy web templates to `dist/` |
| `npm run dev` | Dev mode with hot-reload via `tsx watch` (stdio MCP mode) |
| `npm start` | Run compiled MCP server (stdio mode) |
| `npm start -- --web-port 8080` | Run with web management UI on port 8080 |
| `npm test` | Run `test-server.js` (requires `dist/` to exist — run build first) |

### Gotchas

- **Build before test**: `npm test` imports from `dist/`, so you must run `npm run build` before `npm test`.
- **No ESLint/Prettier configured**: The project does not have a linter config. TypeScript compilation (`tsc`) is the primary static check.
- **External API required for full functionality**: The `search_context` tool needs a real remote indexing API (`BASE_URL` + `TOKEN` in `~/.acemcp/settings.toml`). Without it, indexing/search calls will fail with connection errors. The server itself still starts and the web UI works fine.
- **Config auto-created on first run**: The `~/.acemcp/` directory (with `settings.toml`, `data/`, `log/`) is created automatically on first startup or test run.
- **stdio mode conflicts with terminal output**: When running `npm start` without `--web-port`, the server communicates via stdin/stdout (MCP protocol). Do not mix terminal interaction with stdio mode. Use `--web-port` for interactive/debugging use.
- **Dev mode (`npm run dev`)**: Uses `tsx watch` which only supports stdio MCP transport. To also get the web UI in dev mode, pass `--web-port`: `npm run dev -- --web-port 8080`.
