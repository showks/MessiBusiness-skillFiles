# Prediction Site Strategy: Extracting Maximum Edge

This guide explains how to convert raw prediction-site data into actionable fantasy picks and risk play selections.

## Converting Predictions to Fantasy XI Picks

### From Predicted Lineups → Starter Identification

With two core lineup sources (The Guardian + Sports Mole):
- In BOTH predicted XIs = **confirmed starter** (very high confidence)
- In ONE, not contradicted by the other = **likely starter** (high confidence)
- Only a doubt / rotation mention = **possible starter** (moderate confidence)
- In neither predicted XI = **unlikely starter** (avoid). If one core source is down, a backup outlet's XI can stand in for it.

### From Match Predictions → Player Targeting

| Prediction Site Says | Fantasy XI Action |
|---------------------|-------------------|
| Team A heavily favored (70%+) | Pick Team A's attackers for goals, Team A's defenders for clean sheet |
| Close match (40-60% either side) | Avoid defenders (no clean sheet), pick attackers from both sides |
| High-scoring predicted (3+ goals) | Maximize attackers from both teams, skip clean sheet strategy |
| Low-scoring predicted (under 2 goals) | Favor defenders and GKs for clean sheet bonus |
| BTTS likely (60%+) | Don't rely on clean sheets, focus on goal/assist potential |
| BTTS unlikely (below 40%) | Heavy clean sheet play on the favored team's defense |

> Player-ratings and xG tables were removed along with the WhoScored/SofaScore/FBref sources. With the tight-core list, judge individual attackers by: is he a **confirmed starter** (above), is he his team's **penalty/set-piece taker**, and does the **market/Forebet** rate his team a strong favorite (more team goals → more chances for him). That ordering captures almost all of the fantasy signal those stat sites provided.

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

### Prediction Market (Kalshi / Polymarket) → Claim Mapping

Market prices are real-money implied probabilities — treat them as the single most trustworthy odds source, and as the tie-breaker when Forebet and pundits disagree.

| Market reads | Implied | Best Risk Claim | Claim Level |
|--------------|---------|-----------------|-------------|
| One side's win contract > ~75% | Heavy favorite | `match_2plus_goals` on that match (default Green) | Green (15%) |
| One side's win contract > ~70% | Strong favorite | `team_scores_first` (that team) | Yellow (25%) |
| "Over 2.5 goals" / total-goals market priced high | Goals expected | `match_over_2_5_goals` | Yellow (25%) |
| Lopsided market (favorite > ~85%, e.g. top team vs debutant) | Mismatch | `team_wins_by_3plus` | Red (35%) |

To find the strongest-favorite match for the default Green claim, rank today's fixtures by the favorite's market win price and take the highest. If markets are unavailable for a fixture, fall back to Forebet's 1X2.

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
- +4 if the Kalshi/Polymarket market price supports it (real-money odds — the strongest single signal)
- +3 if Forebet's model supports it
- +2 if The Guardian predicts/supports it
- +2 if Sports Mole predicts/supports it
- +1 if a backup outlet (Fox Sports / CBS Sports) supports it

Total consensus score (max 12):
- 8+: Very strong pick, high confidence
- 5-7: Strong pick
- 3-4: Moderate pick
- Below 3: Weak pick, look for alternatives

### Handling Conflicting Predictions

When sources disagree:
1. Weight prediction-market odds (Kalshi/Polymarket) and the Forebet model over pundit opinions
2. Weight sites with lineup info (The Guardian, Sports Mole) over those without
3. Weight recent information (today's articles, including overnight US posts) over older predictions
4. When truly uncertain, default to the conservative option (pick confirmed starters, take Green claims)

## Time-Critical Lineup Information

In the hours before kickoff, lineup predictions become much more accurate:
- 48+ hours before: Predictions are speculative, ~60% accuracy
- 24 hours before: Team news narrows options, ~70% accuracy
- 1-2 hours before: Confirmed lineups often leak, ~90% accuracy
- Official announcement: 100% accurate (usually 1 hour before kickoff)

**The agent runs at ~09:00 Mountain Time** (the submission does not lock until 21:00 Mountain, but you only get the one 9 AM pass). At 9 AM, official lineups for that day's matches are almost never out yet — they leak ~1 hour before kickoff. So you are researching *predicted*, not confirmed, lineups. Focus on:
- Prediction-market odds (Kalshi/Polymarket) and Forebet probabilities — these are available around the clock
- Overnight-published previews from US outlets (a piece posted "8 hours ago" the night before IS available at 9 AM)
- Manager press conference quotes about team selection
- Training reports (who trained, who was absent)
- Beat reporter predictions (local journalists often know best)
