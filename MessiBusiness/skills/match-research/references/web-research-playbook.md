# Web Research Playbook

Specific search queries and URL patterns for extracting maximum intelligence from each web source. Use these exact search patterns for the most relevant results.

## ESPN (www.espn.com)

### Best Search Queries
- `"[Team A] vs [Team B] World Cup 2026" site:espn.com` — Match preview with predictions
- `"[Team name] World Cup 2026 squad" site:espn.com` — Full squad list and analysis
- `"World Cup 2026 predictions" site:espn.com` — Expert predictions and odds
- `"[Team name] predicted lineup" site:espn.com` — Expected starting XI

### What ESPN Provides
- Detailed match previews with expert predictions
- Predicted lineups with formation diagrams
- Injury and suspension updates
- Historical head-to-head analysis
- Betting odds and implied probabilities
- World Cup predictor simulator

## BBC Sport (www.bbc.com)

### Best Search Queries
- `"[Team A] v [Team B] World Cup 2026" site:bbc.com` — Note: BBC uses "v" not "vs"
- `"[Team name] team news World Cup" site:bbc.com` — Squad updates
- `"World Cup 2026 predictions" site:bbc.com` — BBC pundits' predictions
- `"[Team name] line-up" site:bbc.com` — Expected lineup

### What BBC Provides
- Match previews by experienced football journalists
- "Team news" sections with confirmed absences
- Predicted starting XIs
- Key stats and historical records
- Tactical analysis

## Sky Sports (www.skysports.com)

### Best Search Queries
- `"[Team A] vs [Team B] preview" site:skysports.com` — Match preview
- `"[Team name] World Cup 2026" site:skysports.com` — Team coverage
- `"World Cup predictions tips" site:skysports.com` — Expert tips

### What Sky Sports Provides
- Super 6 predictions (exact score predictions)
- Match previews with detailed analysis
- Predicted lineups
- Key player analysis
- Tactical breakdowns

## Forebet (www.forebet.com)

### Best Search Queries
- `"[Team A] vs [Team B]" site:forebet.com` — Mathematical prediction
- `"World Cup 2026 predictions" site:forebet.com` — Tournament predictions

### What Forebet Provides (Highest Value for Risk Play)
- **1X2 probabilities** (win/draw/loss percentages from mathematical model)
- **Over/Under 2.5 goals probability** — directly maps to `match_over_2_5_goals` claim
- **BTTS probability** — directly maps to `both_teams_score` claim
- **Correct score prediction** — maps to `exact_score` claim
- **Average goals prediction** — helps assess `match_2plus_goals` claim
- Weather conditions (can affect playing style)
- Injured/suspended players list

### How to Map Forebet to Risk Claims
| Forebet Metric | Risk Play Claim | Use When |
|----------------|-----------------|----------|
| Over 2.5 > 65% | `match_over_2_5_goals` (Yellow) | Forebet predicts 3+ goals likely |
| Over 2.5 > 55% | `match_2plus_goals` (Green) | Forebet predicts 2+ goals very likely |
| BTTS Yes > 60% | `both_teams_score` (Yellow) | Both teams rated likely to score |
| 1X2 one side > 70% | `team_scores_first` (Yellow) | Heavy favorite likely scores first |
| Correct score prediction | `exact_score` (Red) | Only if very high confidence |

## Sportskeeda (www.sportskeeda.com)

### Best Search Queries
- `"[Team A] vs [Team B] prediction World Cup 2026" site:sportskeeda.com`
- `"[Team A] vs [Team B] predicted lineup" site:sportskeeda.com`
- `"[Player name] World Cup 2026" site:sportskeeda.com`

### What Sportskeeda Provides
- Detailed match predictions with reasoning
- Predicted lineups with tactical formations
- Player form analysis and ratings
- "Prediction" articles with specific scoreline forecasts
- Key player matchups

## WhoScored (www.whoscored.com)

### Best Search Queries
- `"[Team name]" site:whoscored.com` — Team profile with ratings
- `"[Team A] vs [Team B]" site:whoscored.com` — Match preview

### What WhoScored Provides
- Player ratings (0-10 scale) from algorithmic analysis
- Team statistics (possession, shots, pass accuracy)
- Predicted lineups with formation
- Tactical analysis (strengths and weaknesses)
- Statistical preview of the match

## SofaScore (www.sofascore.com)

### What SofaScore Provides
- Real-time player ratings
- Head-to-head historical records
- Team form (last 5-10 matches)
- Expected lineups
- Player statistics (goals, assists, cards per match)

## FBref (fbref.com) and Understat (understat.com)

### What They Provide (Advanced Statistics)
- Expected goals (xG) per team and player
- Expected assists (xA)
- Shot maps and shot quality
- Defensive actions statistics
- Progressive carries and passes
- These are deep analytics that help identify undervalued players

## Wikipedia (en.wikipedia.org)

### Best Search Queries
- `"[Team A] [Team B] football" site:en.wikipedia.org` — Head-to-head record
- `"[Team name] national football team" site:en.wikipedia.org` — Full team history
- `"2026 FIFA World Cup Group [X]" site:en.wikipedia.org` — Group details

### What Wikipedia Provides
- Complete head-to-head records between nations
- Historical World Cup performance
- All-time top scorers for each national team
- Tournament format and bracket structure

## FIFA (www.fifa.com / inside.fifa.com)

### What FIFA Provides
- Official squad lists (most authoritative)
- FIFA World Rankings
- Official match schedules and results
- Player profiles

## Reuters / AP News

### What Wire Services Provide
- Breaking news on injuries and squad changes
- Last-minute team news before kickoff
- Post-match reports (useful for tracking form between matchdays)

## Research Priority Order

When time is limited, prioritize sources in this order:
1. **ESPN + BBC** — Best for predicted lineups and match previews
2. **Forebet** — Best for mathematical probabilities (critical for risk play)
3. **Sportskeeda** — Best for detailed prediction articles
4. **Sky Sports** — Good for UK-perspective analysis and tips
5. **WhoScored/SofaScore** — Best for player ratings and form
6. **FBref/Understat** — Best for advanced xG analysis
7. **Wikipedia** — Best for historical context
8. **FIFA/Reuters/AP** — Best for official squad news

Always cross-reference at least 2-3 sources before making decisions.
