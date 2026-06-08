---
name: match-research
description: Deep pre-match research skill that queries prediction websites, statistics platforms, and sports news to build a comprehensive intelligence profile for every fixture on the matchday.
---

# Match Research Skill

Before making any picks, research every match on today's board using prediction websites and statistical platforms. This research informs both the Fantasy XI selection and the Risk Play decision.

## Step 1: Identify Today's Matches

1. Read `game-board/matches.json` to list all fixtures.
2. Read `game-board/teams.json` to get team names and countries.
3. For each match, note: `match_id`, home team name, away team name, kickoff time.

## Step 2: Research Each Match (Do ALL of these for EACH match)

### 2a. Search for Predicted Lineups

This is the highest-value research. Search for confirmed or predicted starting XIs:

- Search: `"[Team A] vs [Team B] predicted lineup World Cup 2026"` on ESPN, BBC, Sky Sports
- Search: `"[Team A] starting XI World Cup 2026"` on ESPN or BBC
- Search: `"[Team A] vs [Team B] team news World Cup 2026"` for injury/suspension updates
- Search: `"[Team A] predicted lineup"` on Sportskeeda or WhoScored if accessible

For each team, try to identify:
- The expected starting goalkeeper
- The expected back line (and formation: 4 or 3 at the back?)
- The expected midfield
- The expected forwards/attackers
- Any confirmed absences (injury, suspension, rest)

### 2b. Search for Match Predictions and Probabilities

Query prediction platforms for outcome probabilities:

- Search: `"[Team A] vs [Team B] prediction"` on Forebet — look for 1X2 percentages, over/under 2.5 goals probability, BTTS probability, predicted score
- Search: `"[Team A] vs [Team B] World Cup 2026 prediction tips"` on ESPN, BBC, Sky Sports
- Search: `"[Team A] vs [Team B] preview prediction"` on footballpredictions.com
- Search: `"[Team A] vs [Team B] odds World Cup"` for bookmaker-implied probabilities

Record for each match:
- Win/draw/loss probabilities for each team
- Over/under 2.5 goals probability
- Both teams to score (BTTS) probability
- Predicted/most likely scoreline
- Any consensus among multiple sources

### 2c. Search for Team Form and Statistics

- Search: `"[Team name] World Cup 2026 squad form"` on ESPN or BBC
- Search: `"[Team name] recent results 2026"` for latest form heading into the tournament
- Search: `"[Team A] vs [Team B] head to head"` on Wikipedia for historical record
- If accessible, check WhoScored, SofaScore, or FBref for:
  - Team xG (expected goals) averages
  - Defensive record (goals conceded per game)
  - Key player ratings

### 2d. Search for Key Player Intel

For the star players on each team:
- Search: `"[Player name] World Cup 2026 form"` for current fitness and form
- Search: `"[Player name] goals assists 2025-2026 season"` for recent stats
- Check if the player is their team's penalty taker or set-piece specialist

## Step 3: Compile a Research Summary

For each match, compile these findings into a structured assessment:

```
MATCH: [Team A] vs [Team B] (match_id: XXXXX)

PREDICTED LINEUPS:
- [Team A] likely XI: [GK, DEF, DEF, DEF, DEF, MID, MID, MID, FWD, FWD, FWD]
- [Team B] likely XI: [GK, DEF, DEF, DEF, DEF, MID, MID, MID, FWD, FWD, FWD]
- Key absences: [any injured/suspended players]

MATCH PREDICTION:
- Favored team: [Team A / Team B / Draw]
- Win probabilities: [Team A]% / Draw% / [Team B]%
- Expected goals: Over 2.5 probability = X%
- BTTS probability: X%
- Predicted score: X-X
- Confidence level: [High/Medium/Low]

KEY PLAYERS TO TARGET:
- Top goal threat: [Player name] (player_id: XXX) — [reason]
- Top assist threat: [Player name] (player_id: XXX) — [reason]
- Clean sheet candidates: [Team name] defenders if they are favored
- Save bonus candidate: [GK name] if facing moderate shot volume

RISK PLAY IMPLICATIONS:
- Green claims likely to hit: [list with reasoning]
- Yellow claims worth considering: [list with reasoning]  
- Red claims with upside: [list with reasoning, if any]
```

## Step 4: Cross-Reference Predictions

When multiple prediction sources are available:
- If 3+ sources agree on an outcome, treat it as high confidence
- If sources disagree, treat as medium confidence and lean toward the source with the best track record (Forebet mathematical model > pundit opinion)
- Never treat any single prediction as certain — even heavy favorites lose in World Cups

## Step 5: Pass Findings Forward

Use the compiled research summaries to inform the `pick-fantasy-xi` and `choose-risk-play` skills. The research should directly feed into:
- Which players to pick (confirmed starters from research)
- Which formation to use (based on which teams have the best pickable players)
- Which risk claim to select (based on prediction probabilities)
- Which match to target for risk play (the one with highest-confidence predictions)
