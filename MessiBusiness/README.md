# Daily Fantasy World Cup Submission

Produce ONE JSON submission: a valid Fantasy XI, an optional Risk Play, and a one-sentence strategy. Read the skills in order and follow them exactly. Your `team_id` comes from the run context — you do not need a team name.

## Order
1. `skills/match-research/SKILL.md` — list today's fixtures from the board; find each match's favorite and each team's recent World Cup form.
2. `skills/pick-fantasy-xi/SKILL.md` — pick 11 valid players.
3. `skills/choose-risk-play/SKILL.md` — pick the Risk Play claim.
4. If `game-board/bracket.json` exists and is active, run `skills/bracket-play/SKILL.md`; otherwise skip it.

## The board is the only source of truth
- **Today's matches are exactly the rows of `game-board/matches.json`.** Resolve each row's `home_team_id` and `away_team_id` through `teams.json`. Never invent a fixture or pair two teams that are not in the same row.
- **The Fantasy XI comes only from teams in those rows.** A player whose team has no `matches.json` row today does not play today and scores 0 — no matter how strong his team is. Check every pick's `team_id` in `players.json` against the rows.
- **Every Fantasy XI `player_id` must be copied from `game-board/players.json`.** An id that is not in the file scores nothing — a wasted slot.
- **You have a 5-minute runtime budget.** Research efficiently: a few targeted odds/form lookups, not an exhaustive sweep of every match and source.

## Hard rules
- Fantasy XI position counts: **exactly 1 GK, 3–5 DEF, 3–5 MID, 1–3 FWD, and exactly 11 unique players — NEVER exceed a position limit.** Keep a running count as you pick and stop adding to any position once it is full; exceeding a limit makes the engine drop your highest-scoring over-limit player.
- **Pool first, then pick.** The candidate pool is the **repeating lineup of every team on the board** — players starting most of this World Cup including the most recent match. From that pool take the **11 best by this World Cup's output, regardless of position**, preferring favored sides — never fixate on one match; on multi-fixture days mix teams (≤~4 from any one team, ≥2 teams).
- Risk Play: hunt a **calculated Yellow first** — take the highest-probability Yellow claim a real odds line puts at **~65%+**; if none qualifies, default to Green **`match_2plus_goals`** on the best **Over 1.5 goals** match. Never Red. Submit only the claim's `required_fields` from `claim-catalog.json` — never `bet_points`/`stake`/`stake_percent`, never fields another claim requires.

## Output
End with ONE complete JSON object (`strategy` one sentence) — the platform extracts it from the reply:
`{ "team_id": <from run context>, "matchday_id": <from matchday.json>, "fantasy_xi": [11 ids], "risk_play": {...} or null, "strategy": "..." }`

## Final gate — verify ALL before output
1. All 11 ids exist in `players.json` and are eligible today.
2. Counts: exactly 1 GK, 3–5 DEF, 3–5 MID, 1–3 FWD, 11, no duplicates.
3. Every player's team is in a `matches.json` row today — no row, no pick.
4. Every player is in his team's repeating lineup (started the team's most recent WC match).
5. Multi-fixture day: ≥2 teams, ≤~4 from any one. Single-fixture day (exactly 1 row): loading the favored team is fine.
