---
name: bracket-play
description: Knockout bracket predictions — runs only when bracket play is open.
---

# Bracket Play

Run this ONLY if `game-board/bracket.json` exists and has active data. If it is missing or empty, produce no output and skip. When active, read `rules/bracket-play.md` for the format and lock timing.

## Research each tie
- **Odds (strongest signal)** — win odds for each tie on renowned sportsbooks (DraftKings, FanDuel, BetMGM, Bet365, Pinnacle, or Oddschecker) and prediction markets (Kalshi, Polymarket).
- **Group-stage form** — goals scored/conceded and points each team earned this World Cup.
- **FIFA ranking** (fifa.com) and **knockout history** (en.wikipedia.org) as tie-breakers.

## Predict each advancing team
Weight odds and group form first, then ranking, then pedigree and the tactical matchup. For a close tie, favor the team with the better defensive record, more knockout experience, or regional advantage (the 2026 World Cup is in the USA/Canada/Mexico).

## Points grow deeper in the bracket
R32 +5 · R16 +8 · QF +12 · SF +18 · Champion +30.

**Later rounds are worth far more — get the deep rounds and the champion right even at the cost of an early-round pick.** For champion, pick one of the 3–4 strongest teams (best group form + ranking + odds, ideally in the easier bracket half). Never pick a dark horse for champion — the +30 is too valuable to gamble.

## Output
Return the **full knockout tree** — every Round-of-32, Round-of-16, quarterfinal, semifinal, and final winner — plus `champion_team_id` matching your predicted final winner. Use only the official team IDs from `bracket.json`. Make a best evidence-based pick for every tie (a pick beats a blank — partial brackets score partial points). Never invent data you did not read.
