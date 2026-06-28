---
name: bracket-play
description: Knockout bracket predictions — runs only when bracket play is open.
---

# Bracket Play

Run this ONLY if a bracket board (`bracket.json`) exists and has active data. If it is missing or empty, produce no output and skip. This is a **bracket-only** run — never emit Fantasy XI or Risk Play. Read `rules/bracket-play.md` for lock timing.

## Inputs — read the workspace first
- **`bracket.json`** — the knockout tree: every required match with its `match_id`, `round`, and the slots/candidate teams feeding it. This is the only source of match and team IDs.
- **`world-cup-standings.json`** and **`bracket-context.json`** — provided standings and likely paths. Use these to reason about advancement without inventing IDs or structure.
- **`teams.json`** — id ↔ team lookup.

## Research each tie
- **Odds (strongest signal)** — win odds on renowned sportsbooks (DraftKings, FanDuel, BetMGM, Bet365, Pinnacle, Oddschecker) and prediction markets (Kalshi, Polymarket).
- **Standings / group form** — points, goal difference, goals scored/conceded from the provided files.
- **FIFA ranking** and **knockout pedigree** as tie-breakers.

## Predict each advancing team
Weight odds and form first, then ranking, then pedigree and the matchup. For a close tie, favor the better defensive record, more knockout experience, or host-region advantage (the 2026 tournament is in USA/Canada/Mexico).

**Advancement must be consistent.** Build the tree forward round by round — R32 → R16 → QF → SF → final — and only ever advance a team you already advanced: a later-round `winner_team_id` must be one of the two winners you picked in the matches feeding it. No upset chain that contradicts an earlier pick.

If the board includes a **third-place match**, pick it too (round `third_place`).

## Points grow deeper in the bracket
R32 +5 · R16 +8 · QF +12 · SF +18 · Champion +30.

Later rounds are worth far more — get the deep rounds and champion right even at the cost of an early pick. For champion, pick one of the 3–4 strongest teams (best form + ranking + odds, ideally in the easier half). Never gamble the +30 on a dark horse.

## Output — one JSON object, this exact shape
```
{
  "team_id": <from run context>,
  "bracket_id": <from bracket.json>,
  "picks": [
    { "round": "<round_of_32|round_of_16|quarterfinal|semifinal|third_place|final>",
      "match_id": "<from bracket.json>",
      "winner_team_id": "<official team id>" }
    // one entry per required match
  ],
  "champion_team_id": "<must equal the winner_team_id of the final pick>",
  "strategy": "<one sentence>"
}
```
Plain JSON only — no fences, no extra text, no Fantasy XI, no Risk Play, no extra keys. Copy every `match_id` and `winner_team_id` from `bracket.json`; never invent one. One pick per required match — a pick beats a blank (partial brackets score partial points).

**Keep research OUT of the final message.** Do all odds/form lookups in tool calls or a scratch file; the final assistant response must be the single JSON object and nothing before or after it — never paste search results, odds tables, or prose. Research per tie (who plays whom), not generic tournament-winner odds.

## Final gate — verify ALL before output
1. Every `match_id` and `winner_team_id` exists in `bracket.json`; nothing invented.
2. Each pick's `round` is one of the allowed values; one pick per required match (incl. third place if present).
3. Every later-round `winner_team_id` is one of the two winners feeding that match.
4. `champion_team_id` equals the final pick's `winner_team_id`.
5. Output is bracket-only — no `fantasy_xi`, no `risk_play`, no keys beyond the schema.
