---
name: mcp-trophy-hunter
description: >-
  PlayStation trophy hunting via the trophy-hunter MCP server. Use when the user
  asks about platinum progress, trophy lists, trophy rarity, or what trophy to
  grind next in a PSN game.
---

# mcp-trophy-hunter

MCP server wrapping PSN trophy data (`psn-api`). Runs on stdio as
`node dist/bundle.mjs` from the plugin root — a single self-contained
bundle (no node_modules needed).

## Tools

| Tool | What it does |
|------|--------------|
| `setup_psn(npsso)` | One-time PSN auth. Saves tokens to `~/.trophy-hunter/credentials.json` |
| `check_psn_auth` | Verify auth status and token expiry |
| `get_my_games` | Top-20 games closest to platinum: progress %, remaining trophy counts |
| `get_trophy_list(game)` | Full trophy list sorted easiest-first, rarity tiers, YouTube fallback link |
| `suggest_next_trophy(limit?)` | Top-3 game recommendations with specific next trophies |

## Auth flow (first run only)

1. User opens https://ca.account.sony.com/api/v1/ssocookie while logged in to PSN
2. Copies the `npsso` cookie value
3. Call `setup_psn` with it — tokens are saved and auto-refreshed
   (access ~1h silently, refresh ~2 months then repeat steps 1-3)

## Intents -> actions

| User intent | Action |
|-------------|--------|
| "how close am I to platinum", "my trophy progress" | `get_my_games` |
| "trophy list for \<game\>", "rarest trophy in \<game\>" | `get_trophy_list` |
| "what should I grind next", "next trophy" | `suggest_next_trophy` |
| "is PSN auth still valid" | `check_psn_auth` |

## Notes

- Server is a data layer only. Guide hunting: use web search; `get_trophy_list`
  returns a YouTube search link as fallback.
- Rarity tiers from `trophyEarnedRate`: Common >50%, Rare 15-50%, Ultra Rare <15%.
- A game must be in the user's trophy list (played at least once) — PSN API limitation.
