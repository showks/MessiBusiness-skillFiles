---
name: match-research
description: List today's board fixtures and find each match's favorite plus each team's recent World Cup form (who has been starting and scoring).
---

# Match Research

You have a 5-minute total runtime budget for the whole submission, so research efficiently — focus on the favored teams, a couple of targeted searches each, not an exhaustive sweep.

## 1. List today's fixtures from the board — resolve every id to a name BEFORE any search
Read `game-board/teams.json` and `game-board/matches.json`. For each row, look up BOTH `home_team_id` and `away_team_id` in `teams.json`, copy the two team names, and write: `match_id — HomeName vs AwayName`. **Every id on the board has a name in `teams.json`** — if you haven't found it, re-read the file until you have. **Never put a raw id in a search query** (no "Team 1090"-style searches) **and never fill in an opponent from memory or the bracket** — a search query may contain only two names copied from the same row.
**These rows are the only fixtures, and each fixture is real because its row exists.** Finding no odds or coverage for one never means it doesn't exist — it means your sources don't list it; continue with form research and the fallbacks. If a search result describes a different pairing than the row (a different opponent, or a team not in it), it is about some other match — discard it; the board is always right.

## 2. For each fixture, find two things
- **The favorite.** Check win odds on Kalshi/Polymarket. Note the stronger team and whether 2+ goals look likely.
- **Recent World Cup form (this is the main signal).** Look up each team's earlier 2026 World Cup match(es) and record **who started, who played the full match, and who scored or assisted**. Search the match report or `"[Team] World Cup 2026 lineup"` on The Guardian or Sports Mole. Note each match's date — the team's **most recent** match defines its repeating lineup. Also check whether a **predicted or confirmed lineup for today's match** is already published (Sports Mole/Guardian preview) and record it — it outranks older lineups. Past results are facts — use them, not guesses about today's lineup.

## 3. Pass forward, per team (favored sides first)
- Its **repeating lineup** — who started its most recent WC match and most of its matches.
- Its **goal/assist producers** and its **main star / penalty taker**.
- Anyone **reported injured or suspended** for today (exclude them).

Use only Kalshi, Polymarket, The Guardian, Sports Mole for this step — choose-risk-play additionally reads betting lines on US sportsbooks. If a fixture has no data, mark it board-only. Never invent a lineup, stat, or scoreline.
