---
name: pick-fantasy-xi
description: Build the day pool from the repeating lineups of today's teams, then pick the pool's best 2026 WC performers into a locked legal shape — every player slotted by his players.json position, caps never exceeded.
---

# Pick Fantasy XI

Pick the 11 best-scoring players, each one a real id copied from `game-board/players.json`.

## How points score (rank candidates by these)
Start +2 · plays 60+ min +2 · goal **+6** · assist **+4** · clean sheet (DEF/GK, needs 60 min) **+4** · GK 3+ saves +2 · yellow −1 · red −3 · own goal −3.
A confirmed starter floors at +4; goals and assists are the upside — attackers and penalty-takers have the highest ceilings, full-90 defenders/keepers on a favorite add clean-sheet value.

## Lock your shape FIRST
Before naming a single player, pick ONE legal shape from this menu and write it at the top of your work (`GK-DEF-MID-FWD`, each sums to 11):

> **1-4-3-3** (default — we lean attack) · 1-3-4-3 · 1-4-4-2 · 1-3-5-2 · 1-5-3-2

**No legal shape has 4 forwards.** Once you commit to a shape you fill *exactly* that many of each position — never one more — so 4 FWD becomes impossible by construction. Default to a 3-forward shape; only drop to 2 forwards if you genuinely cannot find a third starting forward worth a slot.

## Lock the team menu SECOND — only teams playing today
Before naming a single player, copy today's fixtures from `game-board/matches.json` — one line per row, `match_id — Home vs Away` (names resolved via `teams.json`) — and mark each row's favored side from match-research. **These teams are the whole menu. Every player you pick must belong to one of them** — check his `team_id` in `players.json` against it. A team with no row today is not on the menu, however strong or famous; its players cannot score today.

## Build the day pool THIRD — repeating lineups only, ids and positions copied from players.json
Teams repeat their World Cup lineups. For each menu team, write its **repeating lineup**: the players who started its most recent 2026 WC match and have started most of its matches (from match-research), minus anyone injured or suspended today — including card bans (red card last match, or accumulated yellows) found in match-research. If a predicted or confirmed lineup for today's match is published, it wins — anyone not in it leaves the pool. **This pool is the only place picks may come from** — no bench options, no one whose starting spot is a guess. If research found nothing for a team (board-only), fill its share of the pool from `players.json` — best by the file's World Cup metrics when present, else its most renowned names at the needed positions — never stall or leave slots empty.

For every pool player, find him in `players.json` and copy his exact `player_id`, `team_id`, and `position` next to his name; if the file lists `eligible_matchday_ids` it must include today. **The file's `position` IS his position — final.** Research roles ("winger", "attacking midfielder", a formation diagram) set nothing: a player the file labels FWD fills only a FWD slot, whatever reports call him, and a file-labelled MID counts as MID. Not in the file = not in the pool; never invent, guess, or shorten an id.

## Pick the XI — the pool's best performers, mixed across teams
- Rank the pool by **this World Cup's actual output**: goals and assists first, then talisman / penalty-taker status, then full 90s (clean-sheet value for DEF/GK on a favorite). Prefer favored sides — never fixate on one match.
- **Best-first at every position: a favored team's top scorer / talisman / penalty taker goes in before any lesser player at the same file position.** Never leave a favored side's biggest star out while a weaker same-position player is in.
- **Multi-row day (2+ rows in `matches.json`):** mix teams — at most ~4 players from any single team, ≥2 teams.
- **Single-row day (exactly 1 row):** the spread rule does not apply — load the favored team; the position caps still do.

## How to pick — build the spine first, forwards LAST, file positions only
Fill your locked shape in this order, slotting every player by his copied `players.json` position and keeping a running count:
1. **GK (1):** the best pool keeper, favored side.
2. **DEF (your shape's count):** the best pool defenders; full-90 defenders on a favorite for clean-sheet value.
3. **MID (your shape's count):** the best pool midfielders; prioritise penalty-takers and goal threats.
4. **FWD LAST (your shape's count — never more than 3):** add forwards only now, best-first, and **STOP the instant you reach your shape's forward count.** Tempted by one more forward? You have the wrong shape or the wrong player — take a midfielder instead.

## Final gate — write all 11 as `id — name — position — team`, then tally OUT LOUD
1. **Re-look up all 11 in `players.json` and re-copy each `position` fresh from the file** — never from research, memory, or your earlier notes. Count each position on its own line: `GK=_ DEF=_ MID=_ FWD=_ TOTAL=_`. It must read **GK=1, DEF=3–5, MID=3–5, FWD=1–3, TOTAL=11**, no duplicates, and match your locked shape. **If FWD>3, delete the weakest forward(s) and replace each with the best available DEF/MID before anything else, then re-tally.**
2. Every id is in `players.json` and eligible today (`eligible_matchday_ids` includes today when the field exists).
3. **Every player's team is on the team menu** — his team has a `matches.json` row today. One player failing this = replace him before anything else.
4. Every player is in the day pool — his team's repeating lineup — and not injured or card-suspended today.
5. 2+ rows: ≥2 teams, ≤~4 from one. Exactly 1 row: loading the favored team is fine.

Do not output until every line passes.

## Output
Return the 11 `player_id` strings for `fantasy_xi`, ordered GK → DEF → MID → FWD.
