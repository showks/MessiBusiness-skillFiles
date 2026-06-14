---
name: choose-risk-play
description: Adaptive risk play selection using prediction website probabilities, statistical models, and standings-aware risk management to pick the highest expected-value claim.
---

# Risk Play Selection Skill

Select the optimal risk play claim by combining prediction site data with adaptive risk management based on your current standings position.

## Step 1: Assess Your Position

1. Read `game-board/standings-before.json` — find your team's rank and `tournament_total`.
2. Count total teams in standings to calculate your percentile position.
3. Determine your risk appetite:

| Your Position | Risk Level | Preferred Claims |
|--------------|------------|-----------------|
| Top 10% | Very Conservative | GREEN only — protect your lead |
| 11-25% | Conservative | GREEN, or YELLOW only with 65%+ probability |
| 26-50% | Moderate | YELLOW preferred, with 55%+ probability |
| 51-75% | Aggressive | YELLOW or RED with strong evidence |
| Bottom 25% | Very Aggressive | RED with any reasonable evidence |
| Below 15 points | Maximum Aggression | RED — the downside is trivial (35% of 15 = 5 pts) |

4. Note your exact point total — this determines actual stake size:
   - Green: 15% of total (e.g., 50 pts → 8 pts at risk)
   - Yellow: 25% of total (e.g., 50 pts → 13 pts at risk)
   - Red: 35% of total (e.g., 50 pts → 18 pts at risk)

## Step 2: Research Match Probabilities from Prediction Sites

For EACH match today, search prediction websites for specific probabilities:

### Search Forebet (highest priority for risk play):
- Search: `"[Team A] vs [Team B]" site:forebet.com`
- Extract: 1X2 percentages, over/under 2.5 probability, BTTS probability, predicted score
- Forebet uses a mathematical model with historical data — treat these as the most reliable probabilities

### Search ESPN/BBC for expert predictions:
- Search: `"[Team A] vs [Team B] prediction" site:espn.com`
- Search: `"[Team A] vs [Team B] prediction" site:bbc.com`
- Extract: Expert predicted scores, key match narrative

### Search Sportskeeda for detailed predictions:
- Search: `"[Team A] vs [Team B] prediction World Cup 2026" site:sportskeeda.com`
- Extract: Predicted scoreline, predicted goalscorers, match analysis

### Search for card predictions:
- Search: `"[Team A] vs [Team B] cards prediction"` on available prediction sites
- Check if Forebet provides card predictions for the match

### Cross-reference predictions:
- If 3+ sources predict the same outcome → **High confidence** (80%+)
- If 2 sources agree → **Moderate confidence** (60-70%)
- If sources disagree → **Low confidence** (below 60%)

## Step 3: Map Predictions to Claims

### GREEN CLAIMS (15% stake) — Target 75%+ win probability

| Claim | When Prediction Sites Say... | Base Probability |
|-------|------------------------------|-----------------|
| `match_2plus_goals` | Default — ~75% of WC matches have 2+ goals; near-lock when a strong favorite plays (Forebet over 1.5 > 70%) | ~78% (~85%+ with a heavy favorite) |
| `goal_before_halftime` | Forebet over 0.5 first half > 65%, attacking match or heavy favorite | ~75% |
| `match_2plus_cards` | Physical teams, competitive match, or Forebet cards prediction | ~78% |
| `no_goal_first_10` | ONLY in even, cagey matchups. **NEVER with a heavy favorite** — favorites press from kickoff (warmup: Argentina scored inside 10' vs Iceland and this claim lost our 8-pt stake) | ~75% even match; ~60-65% with heavy favorite |
| `no_goal_stoppage_time` | Cagey/defensive matches only — modern stoppage time is long, making this riskier than it looks | ~75% |

**Default GREEN pick**: `match_2plus_goals` on the match with the strongest favorite. Calibration: both warmup matches (3-0 and 1-2) hit it, and the teams that took it gained +8 while our `no_goal_first_10` lost -8.

### YELLOW CLAIMS (25% stake) — Target 60%+ win probability

| Claim | When Prediction Sites Say... | Expected Probability |
|-------|------------------------------|---------------------|
| `match_2plus_yellow_cards` | Forebet/experts predict physical match, 3+ cards expected | ~70% |
| `team_scores_first` | One team is 70%+ favorite AND predicted to score first | ~60% |
| `match_over_2_5_goals` | Forebet over 2.5 > 60%, multiple experts predict 3+ goals | ~55% |
| `both_teams_score` | Forebet BTTS > 60%, two attacking teams facing each other | ~55% |
| `player_scores` | Star forward on dominant team (Mbappe, Vinicius, Kane), confirmed starter | ~40% |

**Best YELLOW pick**: `match_2plus_yellow_cards` — highest probability Yellow claim.
**Best upside YELLOW**: `team_scores_first` on a heavy favorite backed by Forebet data.

### RED CLAIMS (35% stake) — Only with strong evidence

| Claim | When Prediction Sites Say... | Expected Probability |
|-------|------------------------------|---------------------|
| `team_wins_by_3plus` | Massive mismatch (e.g., top 5 team vs debutant), 3+ predictions agree | ~18% |
| `exact_score` | Forebet + 2 experts predict same scoreline, common score (2-0, 1-0) | ~10% |
| `player_scores_2plus` | Elite striker vs very weak defense, confirmed starter, penalty taker | ~12% |
| `red_card_shown` | Heated rivalry, aggressive teams, history of red cards | ~10% |
| `team_comeback_win` | **AVOID** — too unpredictable even with research | ~6% |
| `match_goes_to_extra_time` | Knockout only, very even teams, multiple sources say close match | ~22% |
| `match_goes_to_penalties` | Knockout only, defensive teams, experts predict extra time | ~13% |

**Best RED pick (group stage)**: `team_wins_by_3plus` on a massive mismatch.
**Best RED pick (knockout)**: `match_goes_to_extra_time` on an even knockout match.

## Step 4: Decision Algorithm

```
1. Get your risk appetite from Step 1 (Conservative / Moderate / Aggressive)
2. Get prediction probabilities from Step 2

IF risk appetite is Conservative:
    → Find the GREEN claim with highest probability from research
    → Prefer: match_2plus_goals on the strongest favorite's match (~85%),
      or match_2plus_cards (~78%) in a physical competitive match
    → Only use no_goal_first_10 when ALL matches are even/cagey — never against a heavy favorite
    → Pick the match with the clearest prediction consensus

ELSE IF risk appetite is Moderate:
    → Check if any YELLOW claim has 60%+ probability based on predictions
    → YES → Take that Yellow claim
    → NO → Fall back to best GREEN claim
    → Best options: match_2plus_yellow_cards, team_scores_first

ELSE IF risk appetite is Aggressive:
    → Check if any RED claim has strong multi-source support
    → YES AND match is a clear mismatch → Take the Red claim
    → YES BUT uncertain → Take best Yellow claim instead
    → NO → Take best Yellow claim
    → Never take exact_score or team_comeback_win unless overwhelmingly supported

SPECIAL CASE: Your team has very few points (< 15):
    → Even Red claims only risk 5 points max
    → Be aggressive: take the Red claim with the best narrative support
    → team_wins_by_3plus on any lopsided match is the go-to
```

## Step 5: Select Match and Construct the Claim

1. Choose the match with the highest-confidence prediction data.
2. Build the risk play object with ALL required fields from `claim-catalog.json`:
   - `claim_id`: The selected claim
   - `match_id`: From `matches.json` — must be a valid match for today
   - `team_id` (if required): Must be home or away team in the match
   - `player_id` (if required): Must be in `players.json` AND belong to a team in the match — AND injury-cleared: never stake a player-based claim (`player_scores`, `player_scores_2plus`) on anyone flagged OUT or DOUBTFUL by the Injury Watchlist in `../pick-fantasy-xi/references/world-cup-2026-knowledge.md` or by a fresh `"[player] injury"` search; he must be in the confirmed/predicted starting lineup
   - `home_score` / `away_score` (if exact_score): Integers matching the predicted scoreline

3. **NEVER include**: `bet_points`, `stake`, `stake_percent` — the tournament calculates these.

## Step 6: Validate

Before finalizing:
- [ ] `claim_id` exists in `claim-catalog.json`
- [ ] All required fields for that claim are present
- [ ] `match_id` is from today's `matches.json`
- [ ] `team_id` (if used) is home_team_id or away_team_id in that match
- [ ] `player_id` (if used) is in `players.json` and belongs to a team in that match
- [ ] No prohibited fields (bet_points, stake, stake_percent) included

If validation fails, fall back to: `{ "claim_id": "match_2plus_goals", "match_id": "[the match with the strongest favorite]" }`

## Step 7: Safety Net

If ALL of the following are true:
- Web research returned no useful prediction data
- You cannot confidently assess any claim's probability
- You have no prediction site data to work with

Then choose ONE of:
- `risk_play: null` (skip entirely — no risk, no reward)
- `{ "claim_id": "match_2plus_goals", "match_id": "[the match whose teams look most mismatched on the board data]" }` (safest blind Green — ~75% baseline, higher in mismatches; never blind-pick no_goal_first_10, it lost our warmup stake)

A wrong claim loses points. A skipped claim loses nothing. When truly blind, skipping is better than guessing on Yellow/Red.

**Never fabricate a probability.** Do not invent a Forebet/ESPN figure or a percentage to justify a claim — if you did not read it from a reachable source, you do not have it. Base the blind Green only on what the board data plainly shows (e.g., a top-tier team vs a debutant). A Yellow or Red claim requires real read evidence; absent it, take the blind Green or skip. This is the choose-risk-play half of the package-wide No-Fabrication Policy in the team `README.md`.
