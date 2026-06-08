# Prediction Site Strategy: Extracting Maximum Edge

This guide explains how to convert raw prediction-site data into actionable fantasy picks and risk play selections.

## Converting Predictions to Fantasy XI Picks

### From Predicted Lineups → Starter Identification

When a prediction site shows a predicted lineup:
- A player listed in 3+ predicted lineups across different sites = **confirmed starter** (very high confidence)
- A player listed in 2 predicted lineups = **likely starter** (high confidence)
- A player listed in 1 predicted lineup = **possible starter** (moderate confidence)
- A player not listed in any predicted lineup = **unlikely starter** (low confidence, avoid)

### From Match Predictions → Player Targeting

| Prediction Site Says | Fantasy XI Action |
|---------------------|-------------------|
| Team A heavily favored (70%+) | Pick Team A's attackers for goals, Team A's defenders for clean sheet |
| Close match (40-60% either side) | Avoid defenders (no clean sheet), pick attackers from both sides |
| High-scoring predicted (3+ goals) | Maximize attackers from both teams, skip clean sheet strategy |
| Low-scoring predicted (under 2 goals) | Favor defenders and GKs for clean sheet bonus |
| BTTS likely (60%+) | Don't rely on clean sheets, focus on goal/assist potential |
| BTTS unlikely (below 40%) | Heavy clean sheet play on the favored team's defense |

### From Player Ratings → Individual Selection

| WhoScored/SofaScore Rating | Interpretation |
|----------------------------|----------------|
| 7.5+ average | Elite performer, must-pick if starting |
| 7.0-7.4 average | Strong performer, good pick |
| 6.5-6.9 average | Average, pick only if starter on strong team |
| Below 6.5 | Underperformer, avoid unless no alternatives |

### From xG Data → Goal Probability

| Player's xG per 90 min | Goal Likelihood per Match |
|------------------------|--------------------------|
| 0.60+ | Very high (~50%+ chance of scoring) |
| 0.40-0.59 | High (~35% chance) |
| 0.25-0.39 | Moderate (~20% chance) |
| Below 0.25 | Low (~10% chance) |

## Converting Predictions to Risk Play Selections

### Forebet Probability → Claim Mapping

This is the most direct and valuable mapping:

| Forebet Prediction | Probability | Best Risk Claim | Claim Level |
|--------------------|-------------|-----------------|-------------|
| Over 2.5 goals | > 70% | `match_over_2_5_goals` | Yellow (25%) |
| Over 2.5 goals | > 55% | `match_2plus_goals` | Green (15%) |
| BTTS Yes | > 65% | `both_teams_score` | Yellow (25%) |
| Home win | > 70% | `team_scores_first` (home team) | Yellow (25%) |
| Away win | > 70% | `team_scores_first` (away team) | Yellow (25%) |
| Predicted score 3-0, 4-0, etc. | High confidence | `team_wins_by_3plus` | Red (35%) |
| Predicted exact score | Very high confidence | `exact_score` | Red (35%) |

### Expert Consensus → Confidence Level

| Number of Sources Agreeing | Confidence | Action |
|---------------------------|------------|--------|
| 4+ sites agree | Very High | Take the corresponding claim, even Yellow/Red |
| 3 sites agree | High | Take Green or Yellow claim |
| 2 sites agree | Medium | Take Green claim only |
| Sites disagree | Low | Take safest Green or skip |

### Scoreline Predictions → exact_score Claim

Only use `exact_score` (Red, 35% stake) when:
1. Forebet AND at least 2 other sites predict the same scoreline
2. The predicted score is common (1-0, 2-0, 2-1, 1-1)
3. Your team is in the bottom 25% of standings (you need high-variance plays)
4. The match involves a heavy favorite vs a very weak team (more predictable)

### Card Predictions → Card Claims

- Search for `"[Team A] vs [Team B] cards prediction"` or check Forebet card predictions
- World Cup group stage matches average 3.8 cards
- Physical matches between rivals: expect 4-5+ cards
- First matchday of group stage: referees often lenient, fewer cards
- Must-win games: more fouls, more cards
- Teams from South America, Africa, Southern Europe tend toward more cards historically

## Combining Multiple Prediction Sources

### The Consensus Method

For each potential pick (player or risk claim), score it:
- +3 if ESPN recommends/predicts it
- +3 if BBC recommends/predicts it
- +3 if Forebet mathematical model supports it
- +2 if Sportskeeda predicts it
- +2 if Sky Sports predicts it
- +1 if WhoScored data supports it
- +1 if historical Wikipedia data supports it

Total consensus score:
- 10+: Very strong pick, high confidence
- 7-9: Strong pick
- 4-6: Moderate pick
- Below 4: Weak pick, look for alternatives

### Handling Conflicting Predictions

When sources disagree:
1. Weight mathematical models (Forebet, xG data) over pundit opinions
2. Weight sites with lineup info (ESPN, BBC) over those without
3. Weight recent information (today's articles) over older predictions
4. When truly uncertain, default to the conservative option (pick confirmed starters, take Green claims)

## Time-Critical Lineup Information

In the hours before kickoff, lineup predictions become much more accurate:
- 48+ hours before: Predictions are speculative, ~60% accuracy
- 24 hours before: Team news narrows options, ~70% accuracy
- 1-2 hours before: Confirmed lineups often leak, ~90% accuracy
- Official announcement: 100% accurate (usually 1 hour before kickoff)

Since the skill runs before the daily cutoff (9 PM Mountain Time), you may be researching before official lineups are announced. Focus on:
- Manager press conference quotes about team selection
- Training reports (who trained, who was absent)
- Beat reporter predictions (local journalists often know best)
