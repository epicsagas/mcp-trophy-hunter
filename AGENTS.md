# AGENTS.md — mcp-trophy-hunter

> Shared agent guide. Claude Code, Codex, agy, and hermes all load this file.

## Role

MCP server (TypeScript, stdio) exposing PSN trophy data: platinum progress
(`get_my_games`), trophy lists with rarity (`get_trophy_list`), and
next-trophy suggestions (`suggest_next_trophy`). PSN auth via one-time NPSSO
token (`setup_psn`). The authoritative usage guide is
`skills/mcp-trophy-hunter/SKILL.md`; host-discovery copies live under
`.claude/skills/`, `.codex/skills/`, and `.hermes/skills/` and mirror it.

## Running the server

```bash
npm install && npm run build   # produces self-contained dist/bundle.mjs
node dist/bundle.mjs           # stdio MCP server, no node_modules needed
```

Register as MCP server per host (server itself, not the skill, needs this):

- Claude Code: bundled `.mcp.json` (plugin) or `claude mcp add`
- Codex: `[mcp_servers.trophy-hunter]` in `~/.codex/config.toml`
- agy: `mcpServers` in `~/.antigravity/settings.json`
- hermes: `hermes mcp add`

## Development

See CLAUDE.md for full rules. All specs, comments, commit messages in English.
