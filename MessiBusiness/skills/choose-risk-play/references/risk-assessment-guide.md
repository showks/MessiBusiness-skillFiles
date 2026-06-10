# Risk Assessment Reference Guide

## Expected Value Calculation

For each claim, the expected value (EV) determines if it's worth taking:

```
EV = (win_probability * stake) - (loss_probability * stake)
EV = stake * (2 * win_probability - 1)
```

A claim is only +EV when win probability exceeds 50%.

### Green Claims (15% stake)
- Break-even: 50% win rate
- At 80% win rate: EV = +0.15 * 0.6 * points = +9% of points
- At 85% win rate: EV = +0.15 * 0.7 * points = +10.5% of points

### Yellow Claims (25% stake)
- Break-even: 50% win rate
- At 65% win rate: EV = +0.25 * 0.3 * points = +7.5% of points
- At 70% win rate: EV = +0.25 * 0.4 * points = +10% of points

### Red Claims (35% stake)
- Break-even: 50% win rate
- At 60% win rate: EV = +0.35 * 0.2 * points = +7% of points
- Most Red claims have <50% win rate = negative EV
- Only take Red when behind and need high variance

## World Cup Statistical Baselines

These historical averages inform claim probability estimates:

### Goals
- Average goals per World Cup match (2022): 2.55
- Matches with 2+ goals: ~75%
- Matches with 3+ goals: ~52%
- Matches with both teams scoring: ~50%
- Goals in first 10 minutes: ~20-25% of matches — and noticeably HIGHER when a heavy favorite plays (they press from kickoff; warmup evidence: Argentina scored inside 10' vs Iceland). `no_goal_first_10` is ~75-80% at best, not 85-90%.
- Goals before halftime: ~75% of matches
- Goals in stoppage time: ~22-25% of matches (modern stoppage periods are long) — `no_goal_stoppage_time` is ~75%, not 80%+

### Cards
- Average cards per match: 3.8 (2022 World Cup)
- Matches with 2+ card events: ~82%
- Matches with 2+ yellow cards specifically: ~75%
- Red cards shown: ~8% of matches (very rare)

### Knockout Stage Specific
- Matches going to extra time: ~25% of knockout matches
- Matches going to penalties: ~15% of knockout matches
- Comeback wins (concede first, win): ~12% of all matches

### Score Patterns (2022 World Cup)
- 1-0 results: ~18%
- 2-1 results: ~14%
- 0-0 draws: ~7%
- 2-0 results: ~11%
- 3+ goal winning margin: ~15%

## Claim Selection by Match Profile

### Profile A: Heavy Favorite vs Weak Team
Best claims:
1. GREEN `match_2plus_goals` (~85% probability) — the default for this profile
2. YELLOW `team_scores_first` for the favorite (~70%)
3. RED `team_wins_by_3plus` if mismatch is extreme (~20-25%)

**NEVER take `no_goal_first_10` in this profile** — heavy favorites often score early (~60-65% claim probability here, far below Green standards). This exact mistake cost us 8 points in the warmup (Argentina scored inside 10' vs Iceland; the final was 3-0, so `match_2plus_goals` would have won).

### Profile B: Two Strong Teams
Best claims:
1. GREEN `match_2plus_cards` (~85% — intense physical matches)
2. YELLOW `both_teams_score` (~55-60%)
3. YELLOW `match_2plus_yellow_cards` (~80%)

### Profile C: Two Evenly Matched Mid-Tier Teams
Best claims:
1. GREEN `match_2plus_cards` (~80% — even matches are scrappy)
2. GREEN `no_goal_first_10` (~78% — acceptable here, and ONLY here)
3. GREEN `no_goal_stoppage_time` (~75%)
4. YELLOW `match_2plus_yellow_cards` (~70%)

### Profile D: Knockout Match (when applicable)
Best claims:
1. GREEN `match_2plus_goals` (~70% — knockout matches tend to be cagier)
2. RED `match_goes_to_extra_time` (~25% — if teams are evenly matched)
3. GREEN `no_goal_first_10` (~80% — knockout matches start cautiously, but still avoid when one side is a heavy favorite)

## Adaptive Risk Table

| Your Rank Percentile | Points | Recommended Risk Level | Rationale |
|----------------------|--------|----------------------|-----------|
| Top 10% | High | GREEN only | Protect lead, compound safely |
| 10-25% | Moderate-High | GREEN, occasional YELLOW | Maintain position |
| 25-50% | Moderate | YELLOW preferred | Need gains to climb |
| 50-75% | Moderate-Low | YELLOW or careful RED | Need to make moves |
| Bottom 25% | Low | RED when supported | Nothing to lose, need big swings |
| Very low points (<15) | Any | Aggressive RED | Stake amounts are trivial |

## Compound Effect Example

Starting at 50 points, hitting a Green claim each day:
- Day 1: 50 + (50 * 0.15) = 57.5 → 58 points (rounding)
- Day 5: ~76 points
- Day 10: ~102 points

Starting at 50 points, hitting a Yellow claim each day:
- Day 1: 50 + (50 * 0.25) = 62.5 → 63 points
- Day 5: ~95 points
- Day 10: ~155 points

The compounding effect makes consistent correct risk plays extremely powerful over a full tournament. This is why claim probability matters more than claim size.

## Calibration Log (update after every matchday)

| Date | Claim taken | Match context | Result | Lesson |
|------|-------------|---------------|--------|--------|
| 2026-06-10 (warmup) | `no_goal_first_10` (Green, 8-pt stake) | Argentina (heavy favorite) vs Iceland | LOST -8 — Argentina scored inside 10' | Never `no_goal_first_10` against a heavy favorite. Final was 3-0 and the other match 1-2, so `match_2plus_goals` would have won on either match — that's what the +8 teams above us took. |
