---
name: pick-fantasy-xi
description: Pick the 11 best-scoring valid players from players.json — proven World Cup starters across all of the day's favored teams, strictly within the position limits.
---

# Pick Fantasy XI

Pick the 11 best-scoring players, each one a real id copied from `game-board/players.json`.

## How points score (rank candidates by these)
Start +2 · plays 60+ min +2 · goal **+6** · assist **+4** · clean sheet (DEF/GK, needs 60 min) **+4** · GK 3+ saves +2 · yellow −1 · red −3 · own goal −3.
A confirmed starter floors at +4; goals and assists are the upside — attackers and penalty-takers have the highest ceilings, full-90 defenders/keepers on a favorite add clean-sheet value.

## Position limits — NEVER exceed (the rule we most often break)
Exactly **1 GK · 3–5 DEF · 3–5 MID · 1–3 FWD · 11 total**. Keep a **live count of each position** as you add players. The instant a position reaches its max — GK 1, DEF 5, MID 5, FWD 3 — you may add NO more of that position; take the next-best player in a position that still has room. (Exceeding a limit makes the engine drop your highest-scoring over-limit player, wasting your best pick there.) Minimums matter too: you must end with ≥1 GK, ≥3 DEF, ≥3 MID, ≥1 FWD.

## Who to pick — across the WHOLE matchday
- **Consider the favored side of EVERY fixture in `matches.json`, not just one match.** Build a single candidate pool from all of the day's favored teams, rank it by expected points, and take the best 11. Never build the XI around one fixture — at most ~4 players from any single team.
- Each candidate must be a **proven starter this World Cup** (started and played his team's recent 2026 WC match — from match-research), on the **favored side**, and not injured or suspended today.
- Include each favored team's **star / talisman and penalty taker** if they have been starting. On an already-qualified team, prefer its nailed-on stars over rotation candidates.

## How to pick — every id from players.json
1. For each player you want, **find him in `players.json`** and copy his exact `player_id` and `position`. If he is not in the file, pick someone else — never invent, guess, or shorten an id.
2. Add players best-first, respecting the live position count above. Stop at 11.

## Final gate — write all 11 out as `id — name — position`, then verify
1. **Counts: exactly 1 GK, 3–5 DEF, 3–5 MID, 1–3 FWD, 11 total, no duplicates** — tally them now from the list.
2. Every id is in `players.json` and eligible today.
3. Every player is a proven starter this World Cup.
4. Players come from multiple fixtures (≥2–3 teams, ≤~4 from one).

If any check fails, fix it and re-tally. Do not output until all four pass.

## Output
Return the 11 `player_id` strings for `fantasy_xi`, ordered GK → DEF → MID → FWD.
