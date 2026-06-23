# Risk Assessment Reference Guide

## Expected Value Calculation

The stake is `stake_pct × your same-day Fantasy XI score`. For each claim, expected value determines if it's worth taking:

```
EV = (win_probability * stake) - (loss_probability * stake)
EV = stake * (2 * win_probability - 1)        # stake = stake_pct * same-day XI score
```

A claim is only +EV when win probability exceeds 50%. Maximize `stake_pct × (2P − 1)`: a high-confidence Green is the default, and Red claims (sub-50% P) are bad bets.

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
- Every real Red claim (`team_wins_by_3plus` ~18%, `exact_score` ~10%, `player_scores_2plus` ~12%) is well under 50% → **negative EV**. Do not take Red on a normal day.

## World Cup Statistical Baselines

These historical averages inform claim probability estimates:

### Goals
- Average goals per World Cup match (2022): 2.55
- Matches with 2+ goals: ~75%
- Matches with 3+ goals: ~52%
- Matches with both teams scoring: ~50%
- Goals in first 10 minutes: ~20-25% of matches — and noticeably HIGHER when a heavy favorite plays (they press from kickoff). `no_goal_first_10` is ~75-80% at best, not 85-90%.
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

**NEVER take `no_goal_first_10` in this profile** — heavy favorites often score early (~60-65% claim probability here, far below Green standards). Take `match_2plus_goals` instead.

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

## Rank Does Not Drive Risk

The stake is a percentage of one day's Fantasy XI score, so the math is identical whether you are 1st or last:

- **Every rank plays the same claim: the highest-EV one, almost always Green `match_2plus_goals` on the strongest favorite.**
- Do not raise aggression because you are behind — the stake is too small to catch up via variance, and Red is negative-EV.
- The only legitimate upgrade is Yellow `match_2plus_yellow_cards` when read evidence puts a physical match at ~70%+.

## Calibration Notes

- Default to Green `match_2plus_goals` on the strongest favorite.
- Never take `no_goal_first_10` against a heavy favorite — they press from kickoff.
- Never take a Red claim on a normal day — all are negative-EV.
