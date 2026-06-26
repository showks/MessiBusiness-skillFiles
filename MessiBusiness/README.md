# Daily Fantasy World Cup Submission

Produce ONE JSON submission: a valid Fantasy XI, an optional Risk Play, and a one-sentence strategy. Read the skills in order and follow them exactly. Your `team_id` comes from the run context — you do not need a team name.

## Order
1. `skills/match-research/SKILL.md` — list today's fixtures from the board; find each match's favorite and each team's recent World Cup form.
2. `skills/pick-fantasy-xi/SKILL.md` — pick 11 valid players.
3. `skills/choose-risk-play/SKILL.md` — pick the Risk Play claim.
4. If `game-board/bracket.json` exists and is active, run `skills/bracket-play/SKILL.md`; otherwise skip it.

## The board is the only source of truth
- **Today's matches are exactly the rows of `game-board/matches.json`.** Resolve each row's `home_team_id` and `away_team_id` through `teams.json`. Never invent a fixture or pair two teams that are not in the same row.
- **Every Fantasy XI `player_id` must be copied from `game-board/players.json`.** An id that is not in the file scores nothing — a wasted slot.
- **You have a 5-minute runtime budget.** Research efficiently: a few targeted odds/form lookups, not an exhaustive sweep of every match and source.

## Hard rules
- Fantasy XI position counts: **exactly 1 GK, 3–5 DEF, 3–5 MID, 1–3 FWD, and exactly 11 unique players — NEVER exceed a position limit.** Keep a running count as you pick and stop adding to any position once it is full; exceeding a limit makes the engine drop your highest-scoring over-limit player.
- Pick the **11 best players by expected points, regardless of position**, from the **favored side of EVERY fixture on the board** — never fixate on one match. Only players who have been **starting this World Cup**; spread across the day's favored teams (≤~4 from any one team).
- Risk Play: default **`match_2plus_goals`**, judged by the **Over 1.5 goals** odds, on the match most likely to reach 2+ goals. Switch to another claim only if the odds give it clearly higher EV. Never Red. Do not include `bet_points`/`stake`/`stake_percent`.

## Output
Return plain JSON only — no fences, no extra text, `strategy` one sentence:
`{ "team_id": <from run context>, "matchday_id": <from matchday.json>, "fantasy_xi": [11 ids], "risk_play": {...} or null, "strategy": "..." }`

## Final gate — verify ALL before output
1. All 11 ids exist in `players.json` and are eligible today.
2. Counts: exactly 1 GK, 3–5 DEF, 3–5 MID, 1–3 FWD, 11, no duplicates.
3. Every player has been starting in this World Cup.
4. At least 2 teams, no more than ~4 from any one team.
