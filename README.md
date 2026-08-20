# mcp-trophy-hunter

MCP server for PlayStation trophy hunting. Gives Claude access to your PSN data — games closest to platinum, full trophy lists with rarity, and personalized next-step suggestions.

## Tools

| Tool | Description |
|------|-------------|
| `setup_psn` | One-time PSN authentication using your NPSSO token |
| `check_psn_auth` | Verify auth status and token expiry |
| `get_my_games` | Top-20 games closest to platinum, sorted by progress |
| `get_trophy_list` | Full trophy list for a game with rarity (Common/Rare/Ultra Rare), sorted easiest first + YouTube guide link |
| `suggest_next_trophy` | Analyze your profile and recommend the best trophies to go for next |

## Installation

### Option 1 — mcpize.com (recommended, no setup)

Connect via the mcpize.com marketplace — no Node.js, no config file editing required:

👉 [psn-trophy-hunter on mcpize.com](https://mcpize.com/mcp/psn-trophy-hunter)

The server runs on mcpize infrastructure — free, direct access, no signup.

### Option 2 — Self-hosted via npm

Install globally first:

```bash
npm install -g @pavlo-skuibida/mcp-trophy-hunter
```

Then add to Claude Desktop config using the full path to your Node.js binary:

```bash
which node   # copy this path
```

```json
{
  "mcpServers": {
    "trophy-hunter": {
      "command": "/full/path/to/node",
      "args": ["/full/path/to/node_modules/@pavlo-skuibida/mcp-trophy-hunter/dist/index.js"]
    }
  }
}
```

> **Note for nvm users:** Claude Desktop does not inherit your shell PATH, so `npx` or `node` without a full path will fail. Always use the absolute path from `which node`.

Then restart Claude Desktop.

### Option 3 — Install as a plugin (Claude Code · Codex · agy · hermes)

This repo also ships as a self-contained multi-host plugin. It bundles three skills
(`mcp-trophy-hunter` usage guide, `trophy-roadmap`, `trophy-guide-hunt`), a
`trophy-strategist` subagent (Claude Code), and the MCP server itself —
`dist/bundle.mjs` is committed, so it runs with just Node 18+, no `npm install`.

```bash
# Claude Code — marketplace + plugin (MCP server auto-registered)
claude plugin marketplace add epicsagas/mcp-trophy-hunter
claude plugin install mcp-trophy-hunter@mcp-trophy-hunter

# Codex — skills via plugin; MCP server registered separately (below)
codex plugin marketplace add epicsagas/mcp-trophy-hunter
codex plugin add mcp-trophy-hunter@mcp-trophy-hunter

# agy — skills via plugin; MCP server registered separately (below)
agy plugin install https://github.com/epicsagas/mcp-trophy-hunter
agy plugin enable mcp-trophy-hunter

# hermes — skills via plugin; MCP server registered separately (below)
hermes plugins install https://github.com/epicsagas/mcp-trophy-hunter
hermes plugins enable mcp-trophy-hunter
```

Only the Claude Code plugin auto-registers the MCP server (via the bundled
`.mcp.json`). For the other hosts, register the server once:

```bash
git clone https://github.com/epicsagas/mcp-trophy-hunter /path/to/mcp-trophy-hunter
```

```toml
# Codex — append to ~/.codex/config.toml
[mcp_servers.trophy-hunter]
command = "node"
args = ["/path/to/mcp-trophy-hunter/dist/bundle.mjs"]
```

```json
// agy — merge into ~/.antigravity/settings.json
{
  "mcpServers": {
    "trophy-hunter": {
      "command": "node",
      "args": ["/path/to/mcp-trophy-hunter/dist/bundle.mjs"]
    }
  }
}
```

```bash
# hermes
hermes mcp add trophy-hunter --command node --args /path/to/mcp-trophy-hunter/dist/bundle.mjs
```

## First-time Setup

You need to authenticate with PSN once per session (mcpize) or once every ~2 months (self-hosted):

1. **Open this URL in your browser** (must be logged in to PSN):
   ```
   https://ca.account.sony.com/api/v1/ssocookie
   ```

2. You'll see a JSON response like:
   ```json
   {"npsso":"4ab6c...your-token..."}
   ```

3. Copy the `npsso` value and tell Claude:
   ```
   setup_psn 4ab6c...your-token...
   ```

When self-hosting, tokens are saved to `~/.trophy-hunter/credentials.json` and auto-refreshed.

## Usage Examples

```
Which of my games is closest to platinum?

Show me the trophy list for God of War Ragnarök

What trophy should I go for next?

Suggest something easy to platinum this weekend
```

## Local Development

```bash
npm install
npm run build
npm run inspector   # opens MCP Inspector at localhost:5173
```

## License

MIT
