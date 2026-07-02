---
name: pick-fantasy-xi
description: Pick the 11 best-scoring valid players from players.json — proven World Cup starters across the day's favored teams, built into a locked legal shape so the position caps can never be exceeded.
---

# Pick Fantasy XI

Pick the 11 best-scoring players, each one a real id copied from `game-board/players.json`.

## How points score (rank candidates by these)
Start +2 · plays 60+ min +2 · goal **+6** · assist **+4** · clean sheet (DEF/GK, needs 60 min) **+4** · GK 3+ saves +2 · yellow −1 · red −3 · own goal −3.
A confirmed starter floors at +4; goals and assists are the upside — attackers and penalty-takers have the highest ceilings, full-90 defenders/keepers on a favorite add clean-sheet value.

## Lock your shape FIRST — this is how we stop busting the forward cap
Before naming a single player, pick ONE legal shape from this menu and write it at the top of your work (`GK-DEF-MID-FWD`, each sums to 11):

> **1-4-3-3** (default — we lean attack) · 1-3-4-3 · 1-4-4-2 · 1-3-5-2 · 1-5-3-2

**No legal shape has 4 forwards.** Once you commit to a shape you fill *exactly* that many of each position — never one more — so 4 FWD becomes impossible by construction. Default to a 3-forward shape; only drop to 2 forwards if you genuinely cannot find a third starting forward worth a slot.

## Lock the team menu SECOND — only teams playing today
Before naming a single player, copy today's fixtures from `game-board/matches.json` — one line per row, `match_id — Home vs Away` (names resolved via `teams.json`) — and mark each row's favored side from match-research. **These teams are the whole menu. Every player you pick must belong to one of them** — check his `team_id` in `players.json` against it. A team with no row today is not on the menu, however strong or famous; its players cannot score today.

## Who to pick — across the day's fixtures
- Build one candidate pool from the **favored side of every menu row**, rank by expected points, take the best 11 — never fixate on one match.
- **Multi-row day (2+ rows in `matches.json`):** spread across teams — at most ~4 players from any single team, ≥2 teams.
- **Single-row day (exactly 1 row):** the spread rule does not apply — load the favored team; the position caps below still do.
- Each candidate must be a **proven starter this World Cup** (started and played his team's recent 2026 WC match — from match-research), on the **favored side**, not injured or suspended today.
- Include each favored team's **star / talisman and penalty taker** if they have been starting; on an already-qualified team prefer nailed-on stars over rotation candidates.

## How to pick — build the spine first, forwards LAST, every id from players.json
Fill your locked shape in this order, keeping a running count:
1. **GK (1):** the favored side's starting keeper.
2. **DEF (your shape's count):** best proven starters; full-90 defenders on a favorite for clean-sheet value.
3. **MID (your shape's count):** best proven starters; prioritise penalty-takers and goal threats.
4. **FWD LAST (your shape's count — never more than 3):** add forwards only now, best-first, and **STOP the instant you reach your shape's forward count.** Tempted by one more forward? You have the wrong shape or the wrong player — take a midfielder instead.

For each player, find him in `players.json` and copy his exact `player_id`, `position`, and `team_id`. **Use the file's `position`, not your own guess** — a winger the file labels MID counts as MID. His `team_id` must be on your menu, and if the file lists `eligible_matchday_ids` it must include today. If he is not in the file, pick someone else; never invent, guess, or shorten an id.

## Final gate — write all 11 as `id — name — position — team`, then tally OUT LOUD
1. **Count each position on its own line:** `GK=_ DEF=_ MID=_ FWD=_ TOTAL=_`. It must read **GK=1, DEF=3–5, MID=3–5, FWD=1–3, TOTAL=11**, no duplicates, and match your locked shape. **If FWD>3, delete the weakest forward(s) and replace each with the best available DEF/MID before anything else, then re-tally.**
2. Every id is in `players.json` and eligible today (`eligible_matchday_ids` includes today when the field exists).
3. **Every player's team is on the team menu** — his team has a `matches.json` row today. One player failing this = replace him before anything else.
4. Every player is a proven starter this World Cup.
5. 2+ rows: ≥2 teams, ≤~4 from one. Exactly 1 row: loading the favored team is fine.

Do not output until every line passes.

## Output
Return the 11 `player_id` strings for `fantasy_xi`, ordered GK → DEF → MID → FWD.
