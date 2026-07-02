---
name: choose-risk-play
description: Take the Risk Play from real odds — primary claim is match_2plus_goals, assessed from the Over 1.5 goals line.
---

# Choose Risk Play

The agent runs on US Mountain Time, so US sportsbooks and prediction markets are reachable. Stay within the 5-minute budget — a few targeted lookups, not every match.

## Primary claim: `match_2plus_goals` (Green)
This claim is correct when a match has **2 or more total goals**. The betting line that measures exactly that is **Over/Under 1.5 goals** — "Over 1.5" wins on 2+ goals. So:

1. For the strong / high-scoring matches, read the **Over 1.5 goals** odds on renowned sportsbooks (DraftKings, FanDuel, BetMGM, Bet365, Pinnacle, or an aggregator like Oddschecker), plus win odds and total-goals markets on Kalshi/Polymarket and Forebet. Convert odds to implied probability (decimal odds → 1 ÷ odds).
   - **Do NOT use the Over 2.5 line for this claim** — Over 2.5 is 3+ goals, a harder and different claim.
2. Take `match_2plus_goals` on the match with the **highest Over 1.5 probability** (a heavy favorite or two attacking sides — usually ~85%+). This is the pick almost every day.

## Stake, and when to switch claims
Stake is a % of your same-day Fantasy XI score: Green 15%, Yellow 25%, Red 35%; a wrong claim subtracts the same. **EV = stake% × (2P − 1).** `match_2plus_goals` is the default; switch to another claim only if its odds give it clearly higher EV:
- `match_2plus_yellow_cards` (Yellow) — a derby/physical match, cards line ~70%+.
- `match_over_2_5_goals` (Yellow) — when the **Over 2.5** line is ≥ ~60% (a genuinely high-scoring match).
- `both_teams_score` (Yellow) — BTTS ≥ ~60%.

**Never take a Red claim** — all sit below 50% probability, so their EV is negative.

If odds are unclear or unreachable, take `match_2plus_goals` on the biggest mismatch, or return `null`.

## Build the claim
From `game-board/claim-catalog.json`: submit `claim_id` plus **exactly that claim's `required_fields` — nothing more** (`match_2plus_goals` needs only `match_id`; add `team_id`/`player_id`/`home_score`+`away_score` only when the catalog lists them for your claim). The `match_id` must be a row in today's `matches.json`. Do NOT include `bet_points`, `stake`, or `stake_percent`. Never invent an odds figure — use only lines you actually read.
