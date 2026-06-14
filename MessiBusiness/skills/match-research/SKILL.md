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

**First, scan `game-board/players.json` for global superstars in today's eligible pool**, using the web-verified Global Superstar Shortlist and all-48-teams star table in `../pick-fantasy-xi/references/world-cup-2026-knowledge.md`. The pool is ground truth: any superstar listed as eligible IS at the tournament and pickable — never write one off from memory or pre-tournament narratives. Flag every eligible superstar prominently in the research summary so the XI skill cannot miss them (we missed an eligible Messi in the warmup and paid for it).

**Then verify each flagged superstar's fitness** — the Injury Watchlist in that same knowledge file is the starting point (snapshot 2026-06-10), but always run fresh searches and let fresh news win:
- Search: `"[Player name] injury"` and `"[Team name] team news World Cup 2026"`
- Record one status per superstar: **OUT** (stated to miss this match / not in squad / suspended — exclude from all picks), **DOUBTFUL** (fitness race, game-time decision, or on the Watchlist), or **FIT** (auto-pick candidate)
- **DOUBTFUL is fail-closed.** Flag it as "EXCLUDE unless a fresh predicted/confirmed starting XI names him in the starting 11." Pool eligibility does NOT upgrade a DOUBTFUL to FIT — a player is listed as eligible whether or not he actually starts. If you cannot find a fresh lineup, or your search failed / returned the wrong fixture, leave the status as DOUBTFUL→EXCLUDE and tell the XI skill to use the next healthy player. Never resolve an injury doubt by falling back to "he's in the pool."
- Known watch items as of 2026-06-10: **Neymar (`player_id` 276 — calf, uncertain for June 13 vs Morocco; we were advised against him, treat as HARD EXCLUDE unless a fresh XI confirms a start)**, Yamal (targeted ~June 15), Romero/Molina (Argentina), Kudus (Ghana), Merino (Spain), Jose Gimenez (Uruguay)

For the star players on each team:
- Search: `"[Player name] World Cup 2026 form"` for current fitness and form
- Search: `"[Player name] goals assists 2025-2026 season"` for recent stats
- Check if the player is their team's penalty taker or set-piece specialist
- Check whether the player starts and typically plays the full 90 — flag likely early-sub candidates (veteran CBs/holding mids on heavy favorites), since a sub before 60' forfeits the minutes AND clean-sheet bonuses

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
- Eligible superstars in today's pool: [names + player_ids — auto-pick candidates, never omit]
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

## When Research Is Unavailable — Never Fabricate

If a search fails, a domain is unreachable, results cover the wrong fixture, or there is no network at all, **do not invent the missing intelligence**. Mark each field you could not verify as `UNKNOWN — no source` in the research summary rather than guessing a lineup, probability, or scoreline, and lower that match's confidence to "no external data."

When external research is wholly unavailable, fall back to the provided ground-truth files — this is allowed and is not fabrication:
- `players.json` eligibility and `prior_world_cup_record` for who is in the pool and their real prior stats.
- The star tiers and Injury Watchlist in `../pick-fantasy-xi/references/world-cup-2026-knowledge.md` — apply the injury gate **fail-closed** (no confirmation of a doubtful player's start = exclude; never resolve a doubt with "he's in the pool").
- `standings-before.json`, `teams.json`, `matches.json` for context and valid IDs.

Pass these board-derived findings forward with explicit low confidence so the XI and risk skills lean conservative. A correct "we could not confirm X" beats a fabricated lineup.

## Step 5: Pass Findings Forward

Use the compiled research summaries to inform the `pick-fantasy-xi` and `choose-risk-play` skills. The research should directly feed into:
- Which players to pick (confirmed starters from research)
- Which formation to use (based on which teams have the best pickable players)
- Which risk claim to select (based on prediction probabilities)
- Which match to target for risk play (the one with highest-confidence predictions)
