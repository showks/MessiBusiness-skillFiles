# Player Selection Intelligence Guide

## Points Scoring Reference

| Event | Points | Frequency | Strategy Implication |
|-------|--------|-----------|---------------------|
| Player starts | +2 | Every starter | Pick confirmed starters above all |
| Plays 60+ min | +2 | Most starters | Starters who play full 90 = guaranteed +4 |
| Goal scored | +6 | ~2.5 per match average | Target prolific forwards on strong teams |
| Assist recorded | +4 | ~1.5 per match average | Target creative midfielders and playmakers |
| Clean sheet (DEF/GK, requires 60+ min played) | +4 | ~30% of teams per match | Target FULL-90 defenders from clear favorites — defenders subbed before 60' earn nothing from a shutout (verified in warmup: Otamendi/L. Martinez got +2 despite Argentina's 3-0 clean sheet) |
| GK 3+ saves | +2 | ~50% of GKs per match | GKs facing moderate shot volume are ideal |
| Yellow card | -1 | ~3-4 per match average | Avoid defensive midfielders in tense matches |
| Red card | -3 | ~2% chance per player | Rare but devastating; avoid reckless players |
| Own goal | -3 | Very rare | Not worth worrying about |

## Key Patterns in World Cup Football

### Goal Distribution
- Average World Cup match: 2.5-2.7 goals
- Group stage matches with clear favorites: 2.8-3.2 goals
- Matches between evenly-matched teams: 2.0-2.3 goals
- Matches involving debutant/weaker teams: often 3+ goals (one-sided)

### Clean Sheet Likelihood by Match Type
- Heavy favorite vs weak team: ~50-60% clean sheet for favorite
- Moderate favorite: ~35-40% clean sheet
- Even match: ~25-30% for either side
- Weak team vs strong team: ~10-15% clean sheet for weak team

### Starter vs Substitute Value
- Starter who plays 90 min and scores: +10 points minimum
- Starter who plays 90 min, no events: +4 points
- Substitute who plays 30 min: +2 points (start bonus only, unlikely 60+ min)
- Player who doesn't play: 0 points

### Card Risk Factors
- Defensive midfielders in high-stakes matches: highest yellow risk
- Center-backs in physical matches: elevated yellow risk
- First match of group stage: referees often lenient (fewer cards)
- Must-win group matches: more cards
- Rivalry matches: significantly more cards

### Substitution Risk (learned in the 2026-06-10 warmup)
- A start is only worth +2 if the player is subbed before 60 minutes — and an early sub ALSO forfeits the clean-sheet bonus.
- Highest sub risk: veteran (30+) center-backs and holding midfielders on dominant teams — once the game is safe, managers rest legs. In the warmup, Otamendi, Lisandro Martinez, Lo Celso, and Palacios all started vs Iceland and were withdrawn before 60' (+2 each instead of +8).
- Lowest sub risk: goalkeepers (effectively never subbed), first-choice fullbacks, the main striker, and the team's talisman.
- Deduct **-1.5 expected points** from veteran rotation candidates in matches their team should win comfortably; prefer the full-90 profiles above for clean-sheet plays.

## Player Tier System

### Tier 0: Automatic Picks (check BEFORE any scoring)
- Scan today's eligible pool in `players.json` for global superstars using the web-verified Global Superstar Shortlist and the all-48-teams star table in `world-cup-2026-knowledge.md`. The pool is ground truth: if a superstar is eligible, they are playing in this tournament — never assume retirement or absence based on narratives.
- Any such superstar expected to start is an automatic pick. Leaving an eligible Messi out of the warmup XI was our costliest error of the entire matchday.
- **Injury gate (fail-closed)**: before auto-picking, apply the Injury & Availability Protocol in `world-cup-2026-knowledge.md` AND run a fresh `"[player] injury"` search. Stated to MISS the match → drop from the XI entirely (an absent superstar scores 0). Any fitness or rotation doubt → **EXCLUDE by default**; pick ONLY if a fresh predicted/confirmed starting XI names him in the starting 11. Pool eligibility does not clear a doubt, and failed/wrong-fixture research counts as no confirmation → exclude and use the next confirmed starter in the same pool. Reputation is never confirmation — big names (Neymar included) have scored 0 picked on fame while benched/injured.

### Tier 1: Must-Pick (Expected 6+ points)
- Star forward for heavy favorite, confirmed starter
- Defender on team expected to win 3-0 or better (clean sheet + start)
- Penalty taker who is also the main striker

### Tier 2: Strong Pick (Expected 4-5 points)
- Confirmed starter on any team (guaranteed +4 from minutes)
- Attacking midfielder on strong team (start + assist/goal chance)
- GK of heavy favorite (start + clean sheet likely)

### Tier 3: Moderate Pick (Expected 3-4 points)
- Likely starter but not confirmed
- Defender on moderately-favored team
- GK facing enough shots for save bonus

### Tier 4: Risky Pick (Expected 1-3 points)
- Rotation risk players
- Players on evenly-matched team (lower clean sheet odds)

### Tier 5: Avoid (Expected 0-2 points)
- Players unlikely to start
- Players with no prior World Cup data and no web evidence of starting
- Yellow-card-prone players in high-intensity matches

## Prior World Cup Stats Interpretation

When `prior_world_cup_record` is available:
- `starts` vs `appearances` — if starts = appearances, they were a nailed-on starter
- `minutes` / `appearances` — average minutes per game, closer to 90 = reliable starter
- `goals` / `appearances` — goal rate, anything above 0.3 is elite
- `assists` / `appearances` — assist rate
- `yellow_cards` / `appearances` — card rate, above 0.5 = card-prone
- `saves` (GK only) — saves / appearances, above 3 per game = shot-stopper

## Multi-Match Diversification

When 3-4 matches are on the same day:
- Spread picks across matches to reduce variance
- Diversify ONLY with players from the FAVORED side of each match. An underdog starter caps out around +4 (start + minutes) with near-zero goal/clean-sheet upside — in the warmup, our 2 Iceland + 1 Congo DR picks returned 10 points from 3 slots while the favorite's productive players returned 7-8 each.
- Never roster a player from a team expected to lose just to "represent" that match. If a match has no clearly favored side, take its best attackers; otherwise skip the match entirely.
- Concentration in one match is acceptable only when that match has an overwhelming favorite (e.g., Brazil vs a very weak team)
- Aim for at least 2 different matches represented in your XI
- The GK pick naturally forces diversification since you can only pick one
