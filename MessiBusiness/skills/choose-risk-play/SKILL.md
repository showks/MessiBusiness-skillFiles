---
name: choose-risk-play
description: Take the Risk Play from real odds — hunt a calculated Yellow claim first, fall back to Green match_2plus_goals read off the Over 1.5 goals line.
---

# Choose Risk Play

The agent runs on US Mountain Time, so US sportsbooks and prediction markets are reachable. Stay within the 5-minute budget — a few targeted lookups, not every match.

## Hunt a calculated Yellow FIRST
Stake is a % of your same-day Fantasy XI score: Green 15%, Yellow 25%, Red 35%; a wrong claim subtracts the same. **EV = stake% × (2P − 1)** — a Yellow at ~70% probability matches the best Green's EV and pays more when it lands, so check the Yellow menu every day before settling for Green. Read real lines on renowned sportsbooks (DraftKings, FanDuel, BetMGM, Bet365, Pinnacle, Oddschecker) and Kalshi/Polymarket; convert decimal odds to probability (P = 1 ÷ odds). Take the **highest-probability Yellow a real line puts at ~65% or better**:
- `team_scores_first` (+`team_id`) — a strong favorite against a weak defense.
- `match_over_2_5_goals` — read the **Over 2.5 goals** line.
- `both_teams_score` — read the BTTS line.
- `match_2plus_yellow_cards` — a derby or physical tie; read the cards lines.
- `player_scores` (+`player_id`) — an elite scorer's anytime line (rarely reaches the bar).

No qualifying line, no Yellow — never take one on feel or an invented number.

## Fallback: Green `match_2plus_goals`
If no Yellow reaches ~65%, take `match_2plus_goals` on the match with the **highest Over 1.5 goals probability** (usually ~85%+). Over 1.5 = 2+ total goals — do NOT read the Over 2.5 line for this claim; that is a different, harder claim. If odds are unclear or unreachable, take it on the biggest mismatch, or return `null`.

**Never take a Red claim** — all sit below 50% probability, so their EV is negative.

## Build the claim
From `game-board/claim-catalog.json`: submit `claim_id` plus **exactly that claim's `required_fields` — nothing more** (`match_2plus_goals` needs only `match_id`; add `team_id`/`player_id`/`home_score`+`away_score` only when the catalog lists them for your claim). The `match_id` must be a row in today's `matches.json`. Do NOT include `bet_points`, `stake`, or `stake_percent`. Never invent an odds figure — use only lines you actually read.
