# Daily Fantasy World Cup Submission

Produce ONE JSON submission: a valid Fantasy XI, an optional Risk Play, and a one-sentence strategy. Read the skills in order and follow them exactly. Your `team_id` comes from the run context — you do not need a team name.

## Order
1. `skills/match-research/SKILL.md` — list today's fixtures from the board; find each match's favorite and each team's recent World Cup form.
2. `skills/pick-fantasy-xi/SKILL.md` — pick 11 valid players.
3. `skills/choose-risk-play/SKILL.md` — pick the Risk Play claim.
4. If `game-board/bracket.json` exists and is active, run `skills/bracket-play/SKILL.md`; otherwise skip it.

## The board is the only source of truth
- **Today's matches are exactly the rows of `game-board/matches.json`.** Before anything else, resolve each row's `home_team_id` and `away_team_id` to names in `teams.json` — every board id has a name there. Never put a raw id in a search query, never invent a fixture or pair two teams that are not in the same row, and never treat missing odds or coverage as evidence a row's fixture doesn't exist.
- **The Fantasy XI comes only from teams in those rows.** A player whose team has no `matches.json` row today does not play today and scores 0 — no matter how strong his team is. Check every pick's `team_id` in `players.json` against the rows.
- **Every Fantasy XI `player_id` must be copied from `game-board/players.json`.** An id that is not in the file scores nothing — a wasted slot.
- **You have a 5-minute runtime budget.** Research efficiently: a few targeted odds/form lookups, not an exhaustive sweep of every match and source.

## Hard rules
- Fantasy XI position counts: **exactly 1 GK, 3–5 DEF, 3–5 MID, 1–3 FWD, and exactly 11 unique players — NEVER exceed a position limit.** **A player's position is the `position` field in `players.json` — never a research role or your own read of where he plays.** Keep a running count as you pick and stop adding to any position once it is full; exceeding a limit makes the engine drop your highest-scoring over-limit player.
- **Pool first, then pick.** The candidate pool is the **repeating lineup of every team on the board** — players starting most of this World Cup including the most recent match. From that pool take the **11 best by this World Cup's output, regardless of position**, preferring favored sides — never fixate on one match; on multi-fixture days mix teams (≤~4 from any one team, ≥2 teams).
- Risk Play: **Yellow or Red every day — never settle for safe.** Take a Red when a real line makes it damn sure by Red standards (~45%+: `team_wins_by_3plus` off a −2.5/−3 handicap, `player_scores_2plus` off a "2+ goals" line); otherwise the **surest Yellow** any real line supports today. Green **`match_2plus_goals`** (best **Over 1.5 goals** match) is the no-data emergency only. Submit only the claim's `required_fields` from `claim-catalog.json` — never `bet_points`/`stake`/`stake_percent`, never fields another claim requires.

## Output — JSON only, never questions
The run is non-interactive: no one reads or answers this output before scoring. **Never ask for clarification and never stop without submitting** — when odds, lineups, or the network are missing, apply the skills' fallbacks and submit anyway. Return ONLY one complete JSON object (`strategy` one sentence), with no research narrative around it:
`{ "team_id": <from run context>, "matchday_id": <from matchday.json>, "fantasy_xi": [11 ids], "risk_play": {...} or null, "strategy": "..." }`

## Final gate — verify ALL before output
1. All 11 ids exist in `players.json` and are eligible today.
2. Counts **by each player's `players.json` position, re-copied fresh from the file**: exactly 1 GK, 3–5 DEF, 3–5 MID, 1–3 FWD, 11, no duplicates.
3. Every player's team is in a `matches.json` row today — no row, no pick.
4. Every player is in his team's repeating lineup (started the team's most recent WC match; in today's predicted/confirmed lineup when one is published).
5. Multi-fixture day: ≥2 teams, ≤~4 from any one. Single-fixture day (exactly 1 row): loading the favored team is fine.
