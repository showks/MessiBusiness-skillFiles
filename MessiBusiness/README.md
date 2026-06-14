# Agent Briefing

You are competing in the AI Agent Fantasy World Cup. Your job is to produce a daily JSON submission with a valid Fantasy XI and an optional Risk Play claim. Follow the instructions below exactly before making any picks.

## Execution Order — Do These in Sequence

### 1. Run `match-research` FIRST
Before selecting any players or claims, read and execute `skills/match-research/SKILL.md`. This skill queries prediction websites and statistical platforms to build an intelligence profile for every fixture today. Do not skip this step — the quality of all subsequent picks depends on this research.

### 2. Run `pick-fantasy-xi` SECOND
After completing match research, read and execute `skills/pick-fantasy-xi/SKILL.md`. Use the research findings from Step 1 to identify confirmed starters, assess goal and clean-sheet potential, and select exactly 11 valid players.

### 3. Run `choose-risk-play` THIRD
After selecting your Fantasy XI, read and execute `skills/choose-risk-play/SKILL.md`. Use your current standings position from `game-board/standings-before.json` and the prediction probabilities from Step 1 to select the optimal risk claim.

### 4. Run `bracket-play` ONLY IF bracket data is present
Check whether `game-board/bracket.json` exists and contains active data. If yes, read and execute `skills/bracket-play/SKILL.md`. If not, skip this step entirely.

### 5. Combine outputs into the final JSON
Assemble a single JSON object matching `/workspace/output-format/daily-submission.schema.json`:
- `team_id`: as specified in the run context
- `matchday_id`: from `game-board/matchday.json`
- `fantasy_xi`: 11 unique player_id strings from Step 2
- `risk_play`: the claim object from Step 3, or null if skipping
- `strategy`: one sentence summarising your research sources and key picks

Return plain JSON only — no markdown fences, no extra text.

## Hard Formation Contract — Non-Negotiable

The Fantasy XI MUST contain exactly these position counts, where each player's position is whatever `players.json` says (`GK`/`DEF`/`MID`/`FWD`):

- Exactly **1 GK** — never 0, never 2
- **3 to 5 DEF**
- **3 to 5 MID**
- **1 to 3 FWD**
- **Exactly 11 players total**, all unique

Any other split scores **0 for the entire matchday**. Build the XI by filling position quotas (see `pick-fantasy-xi/SKILL.md`), never by picking 11 "best players" and hoping the counts are legal. Before returning, re-read each chosen player's `position` from `players.json`, tally the four counts, and repair until every bound above is satisfied.

**These four buckets are the ONLY position rule — there are no field roles.** `players.json` has no LW/ST/RW/CB/DM; do not invent role slots. You need exactly 1 GK and any legal count of DEF/MID/FWD within the bounds above. Within a bucket, take the highest-expected-points players and **stack freely** — three pure strikers, two No. 9s from different matches, five attacking midfielders with no holding mid are all fine if they score the most. Never bench a high-value player for "positional variety." Choose the bucket split (formation) that maximizes total expected points; let the points pick the shape.

## Warmup Post-Mortem — Binding Corrections (from 2026-06-10, rank 21)

These three errors cost us the warmup. They are corrected throughout the skills and are binding:

1. **Never leave an eligible superstar out of the XI — but never gamble on an injured one either.** Messi was in the player pool and we did not pick him. `players.json` is ground truth **against narrative write-offs** (retirement, age, "he's past it") — scan it BEFORE any research against the web-verified Global Superstar Shortlist and all-48-teams star table in `skills/pick-fantasy-xi/references/world-cup-2026-knowledge.md`, and auto-pick those expected to start. **Eligibility is NOT a fitness clearance** — a player is listed before kickoff whether or not he actually starts. Injuries are gated separately and **fail closed**: check the Injury Watchlist in that file plus a fresh `"[player] injury"` search; a superstar stated to MISS the match is excluded entirely, and a DOUBTFUL one (or anyone on the Watchlist) is **excluded by default** — pick him ONLY if a fresh predicted/confirmed starting XI names him in the starting 11. **If research fails, is inconclusive, or returns the wrong fixture, that is NOT confirmation → exclude and use the next healthy player.** We were advised against Neymar and the agent picked him anyway on eligibility alone — that is the exact mistake this rule prevents. As of 2026-06-10 Neymar (`player_id` 276) is a **hard exclude** for June 13 vs Morocco unless his start is freshly confirmed.
2. **Default Green risk claim is `match_2plus_goals` on the match with the strongest favorite** — NOT `no_goal_first_10`, which lost an 8-point stake when Argentina scored inside 10 minutes. Never take `no_goal_first_10` in a match involving a heavy favorite; favorites press from kickoff.
3. **The 60-minute threshold gates both the minutes bonus AND the clean-sheet bonus.** Four of our starters (Otamendi, L. Martinez, Lo Celso, Palacios) were subbed before 60' and returned +2 each despite Argentina's 3-0 clean sheet. Prefer full-90 profiles (GK, first-choice fullbacks, main striker, talisman); discount veteran rotation candidates on dominant teams. And never fill slots with underdog players for "diversification" — our Iceland/Congo picks returned 10 points from 3 slots. Diversify only with the favored side of other matches.

## Core Principles

**Validity before optimality.** An invalid Fantasy XI scores 0. Always validate formation and player IDs before finalising.

**Evidence before invention — never fabricate.** Every player pick and risk claim must be backed by *evidence*: a prediction site, a confirmed/predicted lineup, OR the provided board and package data (`players.json` eligibility and `prior_world_cup_record`, `standings-before.json`, and the web-verified knowledge base). Web research is the *preferred* evidence, but the shipped board/package data is valid ground truth — using it is not guessing. What is forbidden is inventing or guessing data you did not actually read: never state a lineup, statistic, probability, or scoreline that you did not read from a reachable source or a provided file, and never attribute invented data to a source you could not access. Web research is preferred but never a precondition for a valid submission — see **Research Availability & No-Fabrication Policy** below.

**Adaptive risk.** Adjust claim aggressiveness based on standings rank — conservative when ahead, aggressive when behind. The skills explain exactly how.

## Web Research Domains

Use these domains when the skills instruct you to search the web:

| Priority | Domain | Best For |
|----------|--------|----------|
| High | www.espn.com | Predicted lineups, match previews, odds |
| High | www.bbc.com | Team news, confirmed absences, previews |
| High | www.forebet.com | Mathematical probabilities (1X2, over/under, BTTS) |
| High | www.sportskeeda.com | Predicted lineups, detailed match predictions |
| Medium | www.skysports.com | Expert tips, predicted XIs |
| Medium | www.whoscored.com | Player ratings, tactical analysis |
| Medium | www.sofascore.com | Player form, head-to-head stats |
| Medium | fbref.com | xG data, advanced player statistics |
| Medium | understat.com | Expected goals and shot quality |
| Medium | footballpredictions.com | World Cup match tips |
| Low | en.wikipedia.org | Head-to-head history, tournament records |
| Low | www.fifa.com | Official squad lists, FIFA rankings |
| Low | www.reuters.com | Breaking injury and squad news |
| Low | apnews.com | AP match previews |
| Low | inside.fifa.com | Official tournament data |

If a domain is unreachable, skip it and use whatever is available. Never fabricate data from a source you could not access.

## Research Availability & No-Fabrication Policy

Web research is *preferred* evidence, but its absence is never a license to invent, and a valid submission never depends on the network being up. Two hard rules govern every skill:

1. **Never fabricate or guess source data.** Do not state a lineup, starter, statistic, probability, scoreline, or prediction you did not actually read from a reachable source or a provided file. If a domain is unreachable or a search returns nothing useful, skip it — do not "fill in" what it might have said and do not attribute invented data to it. Inventing research is a failure even if the resulting pick scores well.

2. **When external research is unavailable** (no network, all domains unreachable, or searches return nothing useful), do NOT stop and do NOT guess — fall back to the provided ground-truth data, which is legitimate evidence, not fabrication:
   - `game-board/players.json` — who is actually eligible today, plus each player's real `prior_world_cup_record` (starts, minutes, goals, assists, cards).
   - `game-board/standings-before.json`, `teams.json`, `matches.json` — standings context and the valid IDs for the submission.
   - `skills/pick-fantasy-xi/references/world-cup-2026-knowledge.md` — web-verified star tiers and the Injury Watchlist (apply the injury gate **fail-closed**: with no network you cannot confirm a doubtful player's start, so exclude him).

   Build the Fantasy XI by ranking each position pool on this on-board evidence and picking confirmed-eligible, healthy, likely full-90 players. For the risk play, use the choose-risk-play **Step 7 safety net** — skip (`risk_play: null`) or a blind Green (`match_2plus_goals`) on the board's clearest mismatch. Lower your confidence and prefer conservative claims, but never guess a probability.

A legal submission built entirely from the provided board/package data with no web access is valid and expected when offline. The only failure mode is invented "research."
