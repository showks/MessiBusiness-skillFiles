---
name: match-research
description: List today's board fixtures and find each match's favorite plus each team's recent World Cup form (who has been starting and scoring).
---

# Match Research

You have a 5-minute total runtime budget for the whole submission, so research efficiently — focus on the favored teams, a couple of targeted searches each, not an exhaustive sweep.

## 1. List today's fixtures from the board
Read `game-board/matches.json`. For each row, resolve `home_team_id` and `away_team_id` through `teams.json` and write: `match_id — Home vs Away`. **These are the only fixtures.** Do not search for, mention, or pick from any team that is not in this list, and never pair two teams that are not in the same row. If a search result describes a different pairing than the row (a different opponent, or a team not in it), it is about some other match — discard it and re-check the row's names in `teams.json`; the board is always right.

## 2. For each fixture, find two things
- **The favorite.** Check win odds on Kalshi/Polymarket. Note the stronger team and whether 2+ goals look likely.
- **Recent World Cup form (this is the main signal).** Look up each team's earlier 2026 World Cup match(es) and record **who started, who played the full match, and who scored or assisted**. Search the match report or `"[Team] World Cup 2026 lineup"` on The Guardian or Sports Mole. Past results are facts — use them, not guesses about today's lineup.

## 3. Pass forward, per team (favored sides first)
- Its **repeating lineup** — who started its most recent WC match and most of its matches.
- Its **goal/assist producers** and its **main star / penalty taker**.
- Anyone **reported injured or suspended** for today (exclude them).

Use only Kalshi, Polymarket, The Guardian, Sports Mole. If a fixture has no data, mark it board-only. Never invent a lineup, stat, or scoreline.
