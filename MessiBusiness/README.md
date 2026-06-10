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

## Warmup Post-Mortem — Binding Corrections (from 2026-06-10, rank 21)

These three errors cost us the warmup. They are corrected throughout the skills and are binding:

1. **Never leave an eligible superstar out of the XI.** Messi was in the player pool and we did not pick him. `players.json` is ground truth for availability — scan it BEFORE any research against the web-verified Global Superstar Shortlist and all-48-teams star table in `skills/pick-fantasy-xi/references/world-cup-2026-knowledge.md`, and auto-pick those expected to start. Never assume a star is retired or absent based on narratives. **One exception — injuries**: check the Injury Watchlist in that file plus a fresh `"[player] injury"` search; a superstar stated to MISS the match is excluded entirely, and a doubtful one is picked only if fresh predicted lineups confirm he starts (e.g., as of 2026-06-10 Neymar is doubtful for June 13 vs Morocco).
2. **Default Green risk claim is `match_2plus_goals` on the match with the strongest favorite** — NOT `no_goal_first_10`, which lost an 8-point stake when Argentina scored inside 10 minutes. Never take `no_goal_first_10` in a match involving a heavy favorite; favorites press from kickoff.
3. **The 60-minute threshold gates both the minutes bonus AND the clean-sheet bonus.** Four of our starters (Otamendi, L. Martinez, Lo Celso, Palacios) were subbed before 60' and returned +2 each despite Argentina's 3-0 clean sheet. Prefer full-90 profiles (GK, first-choice fullbacks, main striker, talisman); discount veteran rotation candidates on dominant teams. And never fill slots with underdog players for "diversification" — our Iceland/Congo picks returned 10 points from 3 slots. Diversify only with the favored side of other matches.

## Core Principles

**Validity before optimality.** An invalid Fantasy XI scores 0. Always validate formation and player IDs before finalising.

**Research before intuition.** Every player pick and risk claim must be backed by at least one prediction site or confirmed lineup source. Do not guess.

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
