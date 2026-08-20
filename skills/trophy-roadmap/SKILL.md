---
name: trophy-roadmap
description: >-
  Build a platinum roadmap for a specific PSN game. Use when the user names a
  game and wants a plan to platinum it: trophy classification, ordering,
  missable warnings, and time estimates.
---

# trophy-roadmap

Turns a raw trophy list into an ordered platinum plan.

## Workflow

1. `check_psn_auth` — if expired, stop and walk the user through NPSSO re-auth.
2. `get_my_games` — confirm the game is in the trophy list (played at least
   once). If missing, tell the user to launch it once; PSN API cannot see
   unplayed games.
3. `get_trophy_list(game)` — full list with rarity, sorted easiest-first.
4. Classify every trophy into exactly one bucket:
   - **story** — unlocks through main campaign, cannot be missed
   - **missable** — tied to a point of no return (verify via guide, never guess)
   - **side** — side quests / optional content
   - **collectible** — collect-all objectives
   - **grind** — repetition (kills, currency, playtime)
   - **online/co-op** — requires servers or a partner; check shutdown risk
   - **difficulty** — finish on hard+, sometimes stackable in one run
   - **dlc** — separate from base platinum; list separately
5. Order the plan: story run (stack difficulty if allowed) → missables at
   their story trigger → side + collectibles cleanup → grind → online LAST
   unless shutdown risk is known, then do online FIRST.
6. Output one markdown table: order, trophy, bucket, rarity, est. time,
   risk note. Below it: total est. hours, platinum difficulty verdict
   (easy / moderate / hard / ultra), and the top 3 things most likely to
   waste the user's time.

## Rules

- Missable classification must be verified against a guide (web search or the
  trophy-guide-hunt skill). Guessing creates ruined playthroughs.
- Rarity comes from the data: Common >50%, Rare 15-50%, Ultra Rare <15%.
- Time estimates are ranges (e.g. 2-4h), never single numbers.
