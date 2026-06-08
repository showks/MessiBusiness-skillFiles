---
name: pick-fantasy-xi
description: Research-driven Fantasy XI selection using prediction websites, expected lineup data, xG statistics, and scoring optimization to pick 11 players that maximize expected fantasy points.
---

# Fantasy XI Selection Skill

Select exactly 11 players from `game-board/players.json` to maximize expected fantasy points. Every pick must be justified by web research from prediction sites and statistical platforms.

## Step 1: Load the Board

1. Read `game-board/matchday.json` for the current `matchday_id`.
2. Read `game-board/matches.json` for all fixtures (match_id, home/away teams, kickoff times).
3. Read `game-board/teams.json` to map team IDs to country names.
4. Read `game-board/players.json` — filter to players whose `eligible_matchday_ids` includes today's matchday.
5. Read `game-board/standings-before.json` to understand the competitive context.

## Step 2: Research Predicted Lineups (MOST IMPORTANT STEP)

For EACH match today, search multiple prediction sites for expected starting XIs:

### Primary lineup sources (search all of these):
- ESPN: `"[Team A] vs [Team B] predicted lineup World Cup 2026"` on www.espn.com
- BBC: `"[Team A] v [Team B] team news World Cup 2026"` on www.bbc.com
- Sportskeeda: `"[Team A] vs [Team B] predicted lineup"` on www.sportskeeda.com
- Sky Sports: `"[Team A] vs [Team B] starting XI"` on www.skysports.com
- WhoScored: `"[Team name] predicted lineup"` on www.whoscored.com

### What to extract:
- Which players are predicted to start (cross-reference across sources)
- The likely formation each team will use
- Any injuries, suspensions, or rotation expected
- Who takes penalties and free kicks for each team

### Starter confidence levels:
- Named in 3+ sources = **Confirmed starter** — must-pick candidate
- Named in 2 sources = **Likely starter** — strong pick
- Named in 1 source = **Possible starter** — only pick if high upside
- Not named anywhere = **Bench player** — avoid entirely

## Step 3: Research Match Predictions and Statistics

For each match, gather probability data from prediction platforms:

### Search Forebet for mathematical predictions:
- Search: `"[Team A] vs [Team B]" site:forebet.com`
- Extract: 1X2 probabilities, over/under 2.5, BTTS probability, predicted score

### Search for xG and advanced stats:
- Search: `"[Team name] xG"` on fbref.com or understat.com
- Extract: Team xG per game, defensive xGA, key player xG

### Search for expert predictions:
- Search: `"[Team A] vs [Team B] prediction"` on footballpredictions.com
- Search: `"[Team A] vs [Team B] preview"` on ESPN, BBC, Sportskeeda

### Synthesize into match profiles:
For each match, determine:
- **Dominant favorite** (70%+ win probability) — target their attackers AND defenders
- **Moderate favorite** (55-70%) — target their attackers, cautious on defenders
- **Even match** (45-55% either side) — target attackers from both sides, skip clean sheets
- **Expected goal count** — high (3+), medium (2-3), low (0-2)

## Step 4: Score Every Eligible Player

Using research findings, estimate expected fantasy points for each player:

### Guaranteed Points (Starter Bonus)
- Confirmed starter who plays 60+ min: **+4 base points** (virtually guaranteed)
- Likely starter: **+3.5 expected** (small chance of rotation)
- Bench player: **+0.5 expected** (may get 20-30 min cameo)

### Goal Potential (by position and match context)
- Star forward on dominant team, confirmed starter: **+3 to +5 expected**
  - World Cup forwards average ~0.45 goals per 90 for strong teams
- Attacking midfielder on strong team: **+2 to +3 expected**
- Penalty taker: **+1 additional expected value** (penalties occur in ~25% of WC matches)
- Forward on weaker team: **+1 expected**

### Assist Potential
- Creative playmaker / set-piece taker: **+1.5 to +2.5 expected**
- Wing-back on dominant team (crosses into box): **+1 expected**

### Clean Sheet Potential (DEF and GK only)
- DEF on team with 70%+ win probability AND under 1.5 goals conceded expected: **+3 expected**
- DEF on moderate favorite: **+1.5 expected**
- DEF on underdog or in close match: **+0 expected** (don't count on it)

### GK Save Bonus
- GK on slight underdog facing moderate attack: **+1.5 expected** (3+ saves likely)
- GK on heavy favorite: **+0.5 expected** (may not face enough shots for bonus)
- GK on team facing elite attack: **+1 expected** (lots of saves but no clean sheet)

### Card Risk (subtract from expected value)
- Player with 0.3+ yellow cards per game in prior stats: **-0.5**
- Defensive midfielder in a high-stakes match: **-0.3**
- Player from a team known for physical play: **-0.2**

## Step 5: Optimize Formation and Selection

### Formation Selection
Choose the formation that maximizes total expected points across all 11 picks:

| Formation | When to Use |
|-----------|-------------|
| 1-3-5-3 | Many strong midfielders, 3 elite forwards available |
| 1-3-4-3 | 3 elite forwards, balanced midfield options |
| 1-4-3-3 | Strong clean sheet opportunity + 3 elite forwards |
| 1-4-4-2 | Strong clean sheet opportunity + good midfielders but only 2 standout forwards |
| 1-5-3-2 | Multiple clean sheet opportunities across teams |
| 1-5-4-1 | Extreme clean sheet play, only 1 elite forward |

### Selection Algorithm
1. Rank all eligible players by expected fantasy points.
2. Pick the top GK (considering both clean sheet potential AND save bonus).
3. Pick the top 3 forwards by expected points.
4. Determine whether to add DEF or MID slots based on which remaining players score highest.
5. Fill remaining slots to reach exactly 11 while respecting formation constraints.
6. **Diversification**: Spread picks across at least 2-3 different matches when possible. Don't put all 11 from one match unless one fixture is overwhelmingly dominant.

### Key Player Archetypes to Target
- **The penalty taker on the favored team** — penalties are +6 points and occur in ~25% of matches
- **The set-piece specialist** — free kicks and corners create both goal and assist opportunities
- **Full-backs on dominant teams** — they play 90 min (start bonus), get clean sheets, AND occasionally assist from crosses
- **The target striker** — the #9 who takes the most shots for a strong team

## Step 6: Final Validation

Before outputting, verify ALL of these:
- [ ] Exactly 11 unique `player_id` values
- [ ] Exactly 1 GK
- [ ] 3 to 5 DEF
- [ ] 3 to 5 MID
- [ ] 1 to 3 FWD
- [ ] Total = 11
- [ ] Every `player_id` exists in `players.json`
- [ ] Every player's `eligible_matchday_ids` includes today's matchday
- [ ] No duplicate player IDs

If ANY check fails, fix it immediately. An invalid XI scores 0 — this is the single worst outcome.

## Step 7: Output

Return the 11 `player_id` values as a JSON array of strings for `fantasy_xi`. Write a short `strategy` note explaining: which prediction sources you used, why you chose this formation, and which players you expect to score highest.
