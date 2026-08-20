---
name: trophy-guide-hunt
description: >-
  Find the best guide for a specific trophy or game — written roadmaps first,
  video only when it is genuinely better. Use when the user asks "how do I
  get \<trophy\>" or wants a guide for a game.
---

# trophy-guide-hunt

The whole point of this plugin: replacing hours of video scrubbing with
the right link in seconds.

## Workflow

1. Get the exact trophy name and game from the user (or from
   `get_trophy_list`). Search with the exact quoted trophy name — trophy
   names are unique strings, quoting kills noise.
2. Try sources in this order:
   - **PowerPyx** (`powerpyx.com`) — written roadmaps, per-trophy text
     guides, timestamped video per trophy. Default best answer.
   - **PSNProfiles** (`psnprofiles.com`) — forum threads for obscure
     trophies; also rarity reality-check (does anyone actually have it?).
   - **PlayStationTrophies.org** (`playstationtrophies.org`) — guides that
     pre-categorize trophies as missable/online/grind; spoiler-free
     roadmaps. Good second opinion on missable classification.
   - **TrueTrophies** (`truetrophies.com`) — Server Closures Hub: check
     here before planning any online trophy; if the game is listed, the
     platinum may be dead.
   - **YouTube** — only for spatial/execution trophies (jumps, routes,
     boss patterns) where video beats text. PS5Trophies channel does one
     video per trophy. The link from `get_trophy_list` is the seed query.
   - **Reddit** (`r/Trophies`) — for broken/patched trophies and online
     boosting partners.
   - **Korean** — `games.gg` has some Korean-language platinum guides
     (step-structured); otherwise Naver blog posts. English sources are
     far more complete; prefer them unless the user asks for Korean.
3. For each trophy the user asked about, output:
   - best link + source
   - one-line summary of the method (from the guide, not invented)
   - estimated effort, and whether it is currently obtainable
     (delisted/servers shut / patched out → say so).

## Rules

- Never invent a method. If no guide is found, say so and describe only
  what the trophy description itself supports.
- Prefer text + maps over video. Link video only when movement/route
  matters or the user asks for video.
