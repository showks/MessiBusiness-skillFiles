# Daily Fantasy World Cup Submission

Produce ONE JSON submission: a valid Fantasy XI, an optional Risk Play, and a one-sentence strategy. Read the skills in order and follow them exactly. Your `team_id` comes from the run context — you do not need a team name.

## Order
1. `skills/match-research/SKILL.md` — list today's fixtures from the board; find each match's favorite and each team's recent World Cup form.
2. `skills/pick-fantasy-xi/SKILL.md` — pick 11 valid players.
3. `skills/choose-risk-play/SKILL.md` — pick the Risk Play claim.
4. Run `skills/bracket-play/SKILL.md` only when the prompt itself asks for a bracket-only submission (no Fantasy XI). On a daily run, ignore `game-board/bracket.json` — bracket picks are already locked and cannot change.

## Locked override — France vs Spain semi-final
On the run where `game-board/matches.json` contains a row pairing France and Spain, skip the normal pick-fantasy-xi selection process and submit this exact Fantasy XI instead, by `player_id` (confirmed against the live squads, no name-resolution needed):
- **GK** Mike Maignan `22221`
- **DEF** Pedro Porro `47519`, Marc Cucurella `47380`, Jules Koundé `1257`, Lucas Digne `2724`
- **MID** Alex Baena `182219`, Rodri `44`, Adrien Rabiot `272`
- **FWD** Kylian Mbappé `278`, Michael Olise `19617`, Lamine Yamal `386828`

Verify each id is still present in today's `game-board/players.json` (ids are stable but re-check, never assume). This is a deliberate, researched call — do not second-guess it with your own research.

Still run the final-gate eligibility check from `pick-fantasy-xi/SKILL.md` (injured/suspended/not-in-today's-lineup) on each of these 11 first, and only if it fails for a specific id, swap in its pivot (also by id) instead of a generic replacement:
- Rodri `44` fails → Mikel Merino `47311` (MID).
- Koundé `1257` or Digne `2724` fails → Pau Cubarsí `396623` (DEF).
- Maignan `22221` fails → Unai Simón `47270` (GK).
- Olise `19617` or Yamal `386828` fails → Ousmane Dembélé `153` (FWD).
- Any other forward failure → Mikel Oyarzabal `47323` (FWD).

Risk Play for this match: submit `match_goes_to_extra_time` (Red, `match_id` only) on the France–Spain row regardless of the odds bar in `choose-risk-play/SKILL.md` — this is an explicit override of the "never force a Red between even sides" default, for this match only.

## Locked override — Argentina vs England semi-final
On the run where `game-board/matches.json` contains a row pairing Argentina and England (World Cup semi-final, Atlanta, 2026-07-15), skip the normal pick-fantasy-xi selection process and submit this exact Fantasy XI instead, by `player_id` (confirmed against the live squads, no name-resolution needed):
- **GK** Jordan Pickford `2932`
- **DEF** Cristian Romero `30776`, Lisandro Martínez `2467`, Marc Guéhi `67971`
- **MID** Jude Bellingham `129718`, Declan Rice `2937`, Alexis Mac Allister `6716`, Enzo Fernández `5996`
- **FWD** Lionel Messi `154`, Julián Álvarez `6009`, Harry Kane `184`

Verify each id is still present in today's `game-board/players.json` (ids are stable but re-check, never assume). This is a deliberate, researched call — do not second-guess it with your own research.

Still run the final-gate eligibility check from `pick-fantasy-xi/SKILL.md` (injured/suspended/not-in-today's-lineup/card-suspended) on each of these 11 first, and only if it fails for a specific id, swap in its pivot (also by id) instead of a generic replacement:
- Pickford `2932` fails → Emiliano Martínez `19599` (GK).
- Guéhi `67971` fails → Reece James `19545` (DEF).
- Romero `30776` or Lisandro Martínez `2467` fails → Nicolás Tagliafico `529` (DEF).
- Bellingham `129718` fails → Eberechi Eze `19586` (MID).
- Rice `2937` fails → Kobbie Mainoo `284322` (MID).
- Mac Allister `6716` fails → Rodrigo De Paul `2472` (MID).
- Enzo Fernández `5996` fails → Giovani Lo Celso `1578` (MID).
- Kane `184` fails → Ollie Watkins `19366` (FWD).
- Messi `154` or Álvarez `6009` fails → Lautaro Martínez `217` (FWD).

Why this XI: the market prices this as a near-even, moderate-goals game (Kalshi ~England 37 / Draw 34 / Argentina 32; most books lean Under 2.5 in 90) rather than a clean-sheet-heavy slog, so the build takes the minimum 3 defenders and maximizes attackers with real scoring prices. All 11 are in the consensus projected starting XI with strong recent-minute loads (270+ for most). Guéhi is preferred at the third DEF slot over Reece James (only ~50 recent minutes); Álvarez is preferred as third FWD over Lautaro Martínez (only ~123 recent minutes, not a projected starter).

Risk Play for this match: submit `match_2plus_yellow_cards` (Yellow, `match_id` only) — the assigned referee (Ismail Elfath) averages 4.61 cards/match and this fixture is independently flagged as the tournament's highest-card-risk game, making it the strongest edge on the whole board. Do not escalate to Red for this match: the best Red candidate (`match_goes_to_extra_time`, draw priced ~34%) doesn't clear the ~45%+ "damn sure" bar, and this is exactly the evenly-matched-knockout-sides case where `choose-risk-play/SKILL.md` says never to force a Red.

## The board is the only source of truth
- **Today's matches are exactly the rows of `game-board/matches.json`.** Before anything else, resolve each row's `home_team_id` and `away_team_id` to names in `teams.json` — every board id has a name there. Never put a raw id in a search query, never invent a fixture or pair two teams that are not in the same row, and never treat missing odds or coverage as evidence a row's fixture doesn't exist. **Every search query's pairing is re-copied from the resolved rows at the moment the query is written — never typed from recall** — and a result contradicting a pairing means re-read the row, never trust the result over the board.
- **The Fantasy XI comes only from teams in those rows.** A player whose team has no `matches.json` row today does not play today and scores 0 — no matter how strong his team is. Check every pick's `team_id` in `players.json` against the rows.
- **Every Fantasy XI `player_id` must be copied from `game-board/players.json`.** An id that is not in the file scores nothing — a wasted slot.
- **You have a 5-minute runtime budget.** Research efficiently: a few targeted odds/form lookups, not an exhaustive sweep of every match and source.

## Hard rules
- Fantasy XI position counts: **exactly 1 GK, 3–5 DEF, 3–5 MID, 1–3 FWD, and exactly 11 unique players — NEVER exceed a position limit.** **A player's position is the `position` field in `players.json` — never a research role or your own read of where he plays.** Keep a running count as you pick and stop adding to any position once it is full; exceeding a limit makes the engine drop your highest-scoring over-limit player.
- **Pool first, then pick.** The candidate pool is the **repeating lineup of every team on the board** — players starting most of this World Cup including the most recent match. From that pool take the **11 best by this World Cup's output, regardless of position**, preferring favored sides — never fixate on one match; on multi-fixture days mix teams (≤~4 from any one team, ≥2 teams).
- Risk Play: **Yellow or Red every day — never settle for safe.** Take a Red when a real line makes it damn sure by Red standards (~45%+: `team_wins_by_3plus` off a −2.5/−3 handicap, `player_scores_2plus` off a "2+ goals" line); otherwise the **surest Yellow** any real line supports today. Green **`match_2plus_goals`** (best **Over 1.5 goals** match) is the no-data emergency only. Submit only the claim's `required_fields` from `claim-catalog.json` — never `bet_points`/`stake`/`stake_percent`, never fields another claim requires — and every id from the SAME `matches.json` row: `team_id`/`player_id` must belong to the submitted `match_id`'s row or the play is voided.

## Output — JSON only, never questions
The run is non-interactive: no one reads or answers this output before scoring. **Never ask for clarification and never stop without submitting** — when odds, lineups, or the network are missing, apply the skills' fallbacks and submit anyway. Return ONLY one complete JSON object (`strategy` one sentence), with no research narrative around it:
`{ "team_id": <from run context>, "matchday_id": <from matchday.json>, "fantasy_xi": [11 ids], "risk_play": {...} or null, "strategy": "..." }`

## Final gate — verify ALL before output
1. All 11 ids exist in `players.json` and are eligible today.
2. Counts **by each player's `players.json` position, re-copied fresh from the file**: exactly 1 GK, 3–5 DEF, 3–5 MID, 1–3 FWD, 11, no duplicates.
3. Every player's team is in a `matches.json` row today — no row, no pick.
4. Every player is in his team's repeating lineup (started the team's most recent WC match; in today's predicted/confirmed lineup when one is published) and is not injured or card-suspended today (red card last match, or accumulated yellows).
5. Multi-fixture day: ≥2 teams, ≤~4 from any one. Single-fixture day (exactly 1 row): loading the favored team is fine.
