---
name: bracket-play
description: Knockout bracket prediction skill that activates when bracket play is open, using FIFA rankings, web research, and tournament form to predict advancement.
---

# Bracket Play Skill

This skill activates ONLY when bracket play is open. Check if `game-board/bracket.json` exists and contains active bracket data. If it does not exist or is empty, skip this skill entirely.

## Step 1: Check Bracket Availability

1. Attempt to read `game-board/bracket.json`.
2. If the file does not exist or contains no active bracket data, this skill produces no output.
3. If bracket play is open, read `rules/bracket-play.md` for specific rules and lock timing.

## Step 2: Research Phase

For each knockout matchup in the bracket:

1. Check the prediction markets (kalshi.com / polymarket.com) for tournament-winner and per-tie odds, and search CBS Sports / Fox Sports / The Guardian for "[Team A] vs [Team B] World Cup 2026 prediction" and "World Cup 2026 bracket predictions." (ESPN/BBC removed — not reachable from the sandbox.)
2. Search FIFA.com for current FIFA rankings and tournament group stage results.
3. Search Wikipedia for historical World Cup knockout round performance for each team.

## Step 3: Prediction Framework

For each matchup, assess which team advances using these weighted factors:

| Factor | Weight | How to Assess |
|--------|--------|---------------|
| FIFA ranking | 20% | Higher-ranked team has an edge |
| Group stage performance | 30% | Goals scored, goals conceded, points earned |
| Historical knockout pedigree | 15% | Teams with deep WC runs tend to repeat |
| Key player availability | 20% | Injuries to star players change outcomes |
| Tactical matchup | 15% | Defensive teams can upset attacking teams in knockouts |

### Tiebreaker rules for close matchups:
- Favor the team with better defensive record (fewer goals conceded in groups)
- Favor the team with more knockout stage experience
- Favor the team with home/regional advantage (2026 WC is in USA/Canada/Mexico)

## Step 4: Bracket Scoring Awareness

Points increase deeper in the bracket:
- Round of 32: +5 points per correct pick
- Round of 16: +8 points per correct pick
- Quarterfinal: +12 points per correct pick
- Semifinal: +18 points per correct pick
- Champion: +30 points

This means **later-round picks are worth much more**. When uncertain about early rounds, favor teams you believe will go deep — it's better to get the semifinal and champion right than to nail every Round of 32 pick but miss the deep rounds.

## Step 5: Champion Selection Strategy

For the tournament winner prediction:
1. Identify the 3-4 strongest teams based on group stage performance + FIFA ranking + squad depth.
2. Consider bracket draw — a team in an easier bracket half has better odds.
3. Weight toward teams with tournament-winning pedigree (Brazil, Germany, France, Argentina, Spain, Italy).
4. Use web research to see which teams bookmakers/pundits favor.
5. Do not pick a dark horse for champion — the +30 points is too valuable to gamble on a low-probability pick.

## Step 6: Output

Submit bracket picks using the exact IDs from `bracket.json`. Follow the format specified in the bracket rules. A bracket pick is a *prediction*, so making a reasoned forecast here is expected — not fabrication (which means inventing source data you did not read). If any pick is uncertain, still submit your best evidence-based pick from the valid `bracket.json` IDs rather than leaving it blank — partial bracket submissions may score partial points.
