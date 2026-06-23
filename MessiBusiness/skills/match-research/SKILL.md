---
name: match-research
description: Deep pre-match research skill that queries prediction websites, statistics platforms, and sports news to build a comprehensive intelligence profile for every fixture on the matchday.
---

# Match Research Skill

Before making any picks, research every match on today's board using prediction websites and statistical platforms. This research informs both the Fantasy XI selection and the Risk Play decision.

## Step 1: List Today's Fixtures From the Board (before any web search)

**A fixture is exactly one row of `matches.json` — never two team names you recall.** Build the fixture list mechanically:

1. Read `game-board/matches.json`. This is the complete and authoritative schedule. Count its rows — call it `N`. You research exactly `N` fixtures and your summary ends with exactly `N` MATCH blocks.
2. Read `game-board/teams.json` into a `team_id → country name` map.
3. For each row, resolve **that row's own two IDs**: `home = teams[row.home_team_id]`, `away = teams[row.away_team_id]`, plus `row.match_id` and `kickoff_at`. The two opponents always come from the same row — never pair a team from one row with a team from another, and never introduce a team whose id is not in `matches.json`.
4. Write the numbered fixture list before searching anything:
   ```
   FIXTURES (N = <count>):
   1. match_id <id> — <Home> (team_id <h>) vs <Away> (team_id <a>), kickoff <time>
   2. ...
   ```
5. **This list is the only set of fixtures and team names you may search, mention, or pick from.** Every search query must name a pairing from the list (`"<Home> vs <Away> World Cup 2026"`). Do not search a pairing that is not a row in the list, do not search a team that is not in the list, do not ask the web which matches are on today, and do not write about which teams are NOT playing.

Remember the clock: the agent runs at ~09:00 Mountain Time, so confirmed XIs for today's matches are usually not out yet. Lean on predicted lineups, overnight-published US previews, and prediction-market odds (see README "Runtime & Timing").

**Sources: core list only.** Use only the core sources (Kalshi/Polymarket, Forebet, The Guardian, Sports Mole; Fox/CBS as backup). Do NOT cite or rely on "how to watch / live stream / TV schedule" listicles, random AI-predictor blogs, or Reddit — they have handed us wrong dates and noise. If a core source has nothing for a fixture, mark it board-only; never substitute a junk source or invent coverage.

## Step 2: Research Each Match (Do ALL of these for EACH match)

### 2a. Search for Predicted Lineups

This is the highest-value research. Get predicted starting XIs from the **two core lineup sources** — The Guardian and Sports Mole (fall to a backup only if one is down):

- Search: `"[Team A] vs [Team B] predicted lineup World Cup 2026"` on The Guardian AND Sports Mole
- Search: `"[Team A] vs [Team B] team news World Cup 2026"` for injury/suspension updates (The Guardian's "team news" sections are the best for this)
- Backup ONLY if The Guardian or Sports Mole is unreachable that morning: same searches on Fox Sports or CBS Sports

For each team, try to identify:
- The expected starting goalkeeper
- The expected back line (and formation: 4 or 3 at the back?)
- The expected midfield
- The expected forwards/attackers
- Any confirmed absences (injury, suspension, rest)

### 2b. Search for Match Predictions and Probabilities

Query prediction platforms AND prediction markets for outcome probabilities:

- **Check the prediction markets first** (ground truth for odds): search `"[Team A] [Team B] Kalshi"` and `"[Team A] [Team B] Polymarket"`, or the World Cup market pages on kalshi.com / polymarket.com. A market share price IS the implied probability (a "Yes" at $0.62 ≈ 62%). Read off each team's win probability and any total-goals markets. This is the most reliable read on which side is the strongest favorite.
- Search: `"[Team A] vs [Team B] prediction"` on Forebet — 1X2 percentages, over/under 2.5 goals probability, BTTS probability, predicted score. Cross-check against the market price above; if they diverge sharply, lower confidence.

Record for each match:
- Win/draw/loss probabilities for each team (cross-check Forebet's model against the Kalshi/Polymarket market price — if they disagree sharply, lower confidence)
- Over/under 2.5 goals probability
- Both teams to score (BTTS) probability
- Predicted/most likely scoreline
- Any consensus among multiple sources

### 2c. Note Team Form (lightweight — don't over-invest)

Form is a tie-breaker, not a primary signal — the markets and Forebet already price it in. From The Guardian preview (and Forebet's match notes) jot down only:
- Recent results / momentum heading into the match
- Defensive solidity (are they conceding goals?) — informs clean-sheet picks

Skip dedicated ratings/xG sites (WhoScored, SofaScore, FBref); they rarely change a pick and are not in the core list.

### 2d. Search for Key Player Intel

**First, scan `game-board/players.json` for global superstars in today's eligible pool**, using the Global Superstar Shortlist and all-48-teams star table in `../pick-fantasy-xi/references/world-cup-2026-knowledge.md`. The pool is ground truth: any superstar listed as eligible IS at the tournament and pickable — never write one off from memory or pre-tournament narratives. Flag every eligible superstar prominently in the research summary so the XI skill cannot miss them.

**Then verify each flagged superstar's fitness** via the **Injury & Availability Protocol** in that same knowledge file. There is no current static status list — run fresh searches and decide from a fresh predicted XI:
- Search: `"[Player name] injury"` and `"[Team name] team news World Cup 2026"`
- Record one status per superstar: **OUT** (stated to miss this match / not in squad / suspended, or a season-ending hard-exclude — drop from all picks), **DOUBTFUL** (any fitness or rotation doubt), or **STARTING** (named in a fresh predicted XI for this match — only then an auto-pick).
- **OUT and DOUBTFUL are both fail-closed: pick ONLY if a fresh predicted/confirmed XI names him in the starting 11.** Pool eligibility never upgrades a player to STARTING — he is listed whether or not he plays. If you cannot find a fresh lineup, or research failed / returned the wrong fixture, leave him EXCLUDED and tell the XI skill to use the next confirmed starter. Never resolve a doubt with "he's in the pool."
- Apply this to every big name, no exceptions: reputation is not a team sheet. Only a fresh XI naming the player a starter clears anyone. Do not rely on a remembered injury list; verify per match.

For the star players on each team:
- Search: `"[Player name] World Cup 2026 form"` for current fitness and form
- Search: `"[Player name] goals assists 2025-2026 season"` for recent stats
- Check if the player is their team's penalty taker or set-piece specialist
- Check whether the player starts and typically plays the full 90 — flag likely early-sub candidates (veteran CBs/holding mids on heavy favorites), since a sub before 60' forfeits the minutes AND clean-sheet bonuses

## Step 3: Compile a Research Summary — One Block Per Board Fixture

Produce **exactly `N` MATCH blocks — one for every fixture in `matches.json`, none skipped.** Even a match you could find no external data on gets a block (fill the fields with `UNKNOWN — no source` and mark it board-only). Do not let a fixture fall out of the summary just because research was thin; that is how a fixture once got dropped from the research while still being picked from.

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
- The star tiers and Injury & Availability Protocol in `../pick-fantasy-xi/references/world-cup-2026-knowledge.md` — apply the gate **fail-closed** (no confirmation of a player's start = exclude; never resolve a doubt with "he's in the pool").
- `standings-before.json`, `teams.json`, `matches.json` for context and valid IDs.

Pass these board-derived findings forward with explicit low confidence so the XI and risk skills lean conservative. A correct "we could not confirm X" beats a fabricated lineup.

## Step 5: Coverage & Consistency Gate (do this before passing anything forward)

Before handing findings to the pick skills, verify:

- [ ] The number of MATCH blocks equals `N`, the count of fixtures in `matches.json`. Every `match_id` on the board appears in the summary exactly once.
- [ ] **Reconcile each block to the fixture list:** for every MATCH block, its `match_id` is a real row in `matches.json` and its two team names equal `teams[home_team_id]` and `teams[away_team_id]` for that row. If a block names a team not in the list, or a pairing that is not a single board row, delete it and research the row it should have been.
- [ ] No fixture was dropped because research was thin — thin matches are present as board-only blocks, not missing.
- [ ] Every source you named in the summary is one you actually read content from (no citing ESPN/BBC/Forebet/a market for a figure you never retrieved).

This is the hand-off promise to the downstream skills: **a player or risk claim may only be chosen from a match that has a block here.** If `pick-fantasy-xi` or `choose-risk-play` later wants a player or match with no block, the fix is to come back and research that fixture — never to pick from a blank.

## Step 6: Pass Findings Forward

Use the compiled research summaries to inform the `pick-fantasy-xi` and `choose-risk-play` skills. The research should directly feed into:
- Which players to pick (confirmed starters from research)
- Which formation to use (based on which teams have the best pickable players)
- Which risk claim to select (based on prediction probabilities and prediction-market odds)
- Which match to target for risk play (the one with highest-confidence predictions)
