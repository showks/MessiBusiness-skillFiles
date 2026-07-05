---
name: choose-risk-play
description: Take a sure-shot risky Risk Play from real odds every day — the surest Yellow on the board, or a Red when a real line makes it damn sure; Green only as a no-data emergency.
---

# Choose Risk Play

The agent runs on US Mountain Time, so US sportsbooks and prediction markets are reachable. Stay within the 5-minute budget — a few targeted lookups, not every match.

## Yellow or Red every day — never settle for safe
Stake is a % of your same-day Fantasy XI score: Green 15%, Yellow 25%, Red 35%; correct adds the stake, wrong subtracts it. Read real lines on renowned sportsbooks (DraftKings, FanDuel, BetMGM, Bet365, Pinnacle, Oddschecker) and Kalshi/Polymarket; convert decimal odds to probability (P = 1 ÷ odds). **The daily play is the SUREST Yellow or Red on the board — a sure-shot risky claim, never a safe one:**

1. **Red — take it over everything when a real line makes one damn sure by Red standards (~45%+, near even money or better):**
   - `team_wins_by_3plus` (+`team_id`) — a crushing favorite's −2.5/−3 handicap or winning-margin line priced near even.
   - `player_scores_2plus` (+`player_id`) — an elite striker's "scores 2 or more" line against a weak defense priced near even.
2. **Yellow — the everyday play otherwise: the single highest-probability Yellow any real line supports today.** Read all five and take the surest — a ~70%+ line is a sure shot, grab it; the day's best still wins even below that:
   - `team_scores_first` (+`team_id`) — a strong favorite against a weak defense.
   - `match_over_2_5_goals` — read the **Over 2.5 goals** line.
   - `both_teams_score` — read the BTTS line.
   - `match_2plus_yellow_cards` — a derby or physical tie; read the cards lines.
   - `player_scores` (+`player_id`) — an elite scorer's anytime line.
3. **Green is not a choice — it is the no-data emergency only.** If and only if no Yellow or Red line is readable anywhere today: take Green `match_2plus_goals` on the match with the highest **Over 1.5 goals** probability (Over 1.5 = 2+ total goals — never read the Over 2.5 line for it), or on the biggest mismatch if odds are unreachable everywhere, or return `null`.

Search lines only by the two team names copied from the board row (never a raw team id), and read them only on the named sites' real domains — numbers on mirror/aggregator sites are not read lines. A market missing on one site is not a dead end (check another named book); a claim listed nowhere is simply unavailable today. Never invent an odds figure — a claim you have no genuinely read line for is not available.

## Build the claim
From `game-board/claim-catalog.json`: submit `claim_id` plus **exactly that claim's `required_fields` — nothing more** (`match_2plus_goals` needs only `match_id`; add `team_id`/`player_id`/`home_score`+`away_score` only when the catalog lists them for your claim). The `match_id` must be a row in today's `matches.json`. Do NOT include `bet_points`, `stake`, or `stake_percent`.
