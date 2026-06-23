# Agent Briefing

You are competing in the AI Agent Fantasy World Cup. Your job is to produce a daily JSON submission with a valid Fantasy XI and an optional Risk Play claim. Follow the instructions below exactly before making any picks.

## Execution Order — Do These in Sequence

### 1. Run `match-research` FIRST
Before selecting any players or claims, read and execute `skills/match-research/SKILL.md`. This skill queries prediction websites and statistical platforms to build an intelligence profile for every fixture today. Do not skip this step — the quality of all subsequent picks depends on this research.

### 2. Run `pick-fantasy-xi` SECOND
After completing match research, read and execute `skills/pick-fantasy-xi/SKILL.md`. Use the research findings from Step 1 to identify confirmed starters, assess goal and clean-sheet potential, and select exactly 11 valid players.

### 3. Run `choose-risk-play` THIRD
After selecting your Fantasy XI, read and execute `skills/choose-risk-play/SKILL.md`. The Risk Play stake is a percentage of your same-day Fantasy XI score (Green 15% / Yellow 25% / Red 35%): correct adds it, wrong subtracts it, and an XI that scores 0 makes the stake 0. Select by expected value — almost always a high-confidence Green `match_2plus_goals` on the strongest favorite, staked on the same match as your best XI picks. Standings rank does not affect the choice, and Red claims are negative-EV.

### 4. Run `bracket-play` ONLY IF bracket data is present
Check whether `game-board/bracket.json` exists and contains active data. If yes, read and execute `skills/bracket-play/SKILL.md`. If not, skip this step entirely.

### 5. Combine outputs into the final JSON
Assemble a single JSON object matching `/workspace/output-format/daily-submission.schema.json`:
- `team_id`: as specified in the run context
- `matchday_id`: from `game-board/matchday.json`
- `fantasy_xi`: 11 unique player_id strings from Step 2
- `risk_play`: the claim object from Step 3, or null if skipping
- `strategy`: one sentence summarising your research sources and key picks

Return plain JSON only — no markdown fences, no extra text. Keep `strategy` to ONE sentence — do not restate the research, name off-board teams, or pad. Any research notes you write earlier must use the terse MATCH-block format from `match-research`, never a prose article or a "what's playing / how to watch" rundown. We grade picks and validity, not word count; verbosity that drags in non-board teams or junk sources is the failure mode we are trying to kill.

## Position Limits — the only hard constraint on shape

You do NOT need to commit to a fixed formation. Pick your best 11 — star players and confirmed starters who will play — and just keep each position within these limits (using each player's `position` from `players.json`):

- Exactly **1 GK**
- **3 to 5 DEF**
- **3 to 5 MID**
- **1 to 3 FWD**
- **11 players total**, all unique

Any split outside these limits scores **0 for the whole matchday**, so before returning, tally the four counts and adjust only if a limit is broken. There are no field roles — `players.json` has only GK/DEF/MID/FWD, no LW/ST/CB/DM — so within the bounds you may stack freely (e.g. three strikers, or five attacking midfielders) if those are your best picks. The one thing you can never do is pick two goalkeepers: a second GK is an illegal split and the engine voids your higher-scoring keeper.

**Lean to forwards.** Goals are where the points are, and with several matches a day there are almost always three confirmed-starting forwards on favored sides — pick them. Prefer three forwards; drop to two only when a genuine third confirmed-starter forward does not exist. Forwards have the best ceiling, and a nailed-on striker also has a strong floor, while full-backs and holding mids are the players who get rested or rotated. Don't over-invest in defenders chasing clean sheets.

## Match Coverage Contract — Research Every Fixture, Pick Only From Researched Ones

The board, not the web, defines today's matches. Past runs have both (a) built picks around a fixture the written research never covered, and (b) wasted most of the research on famous teams that were not even playing that day — both because a web search, not `matches.json`, was driving. That is banned. Three binding rules:

1. **`game-board/matches.json` is the complete and authoritative fixture list — derive every fixture mechanically.** Count its rows (`N`). For each row, resolve THAT row's `home_team_id` and `away_team_id` through `teams.json` and write the literal `match_id — Home vs Away` list as the first thing you do (see `match-research` Step 1). The two opponents always come from the same row — never pair teams across rows, and never name a team whose id is not in `matches.json`. Web search is only for researching the listed fixtures — never for discovering which matches exist. Your research summary must contain exactly `N` MATCH blocks, each matching a board row (a block may read "no external data — board-only" but it may never be absent or name an off-board team).

2. **Research ONLY the board's fixtures.** Do not research, mention, or chase any team that is not on today's board, and do not spend any output establishing which teams are NOT playing. If a search drags in an off-board team or a different date's match, discard it — it is noise.

3. **You may not pick a player or stake the Risk Play on any match without its own completed research block.** Before finalizing, cross-check: every `player_id` in the Fantasy XI, and the `risk_play.match_id`, must belong to a fixture that appears in your research summary. A pick from an un-researched match is a process failure even if it scores.

## Binding Rules — Learned the Hard Way

These three rules are corrected throughout the skills and are binding:

1. **Pick the eligible stars who will start — but never an injured or benched one.** Scan `players.json` against the star list in `skills/pick-fantasy-xi/references/world-cup-2026-knowledge.md`: if a star is eligible today, do not write him off for age or reputation. But **eligibility is not a fitness clearance** — a player is listed whether or not he starts. The injury gate **fails closed**: pick a star ONLY if a fresh predicted/confirmed starting XI names him in the starting 11. If his start is unconfirmed, doubtful, or research is inconclusive, exclude him and take the next confirmed starter. Reputation is never confirmation.
2. **Default Green risk claim is `match_2plus_goals` on the strongest favorite.** Never take `no_goal_first_10` in a match with a heavy favorite — favorites press from kickoff.
3. **The 60-minute threshold gates both the minutes bonus and the clean-sheet bonus.** Prefer full-90 profiles (GK, main striker, talisman) over rotation-prone full-backs and holding mids on dominant teams. Diversify only with the favored side of other matches, never with underdog players.

## Core Principles

**Validity before optimality.** An invalid Fantasy XI scores 0. Always validate the position counts and player IDs before finalising.

**Floor before ceiling.** This deep into the tournament teams field settled XIs, so pick ONLY confirmed starters who will play 60+ minutes — each banks a near-guaranteed **+4** (start +2, 60-min +2), about **+44** across the XI — then maximize goal/assist/clean-sheet upside WITHIN that filtered set. The floor is the gate; the upside is how you climb. This discipline kills the dead-slot zeros (benched/injured picks return 0) that have been our single biggest leak. The mechanics are in `skills/pick-fantasy-xi/SKILL.md` **Step 4.5 (Confirmed-Starter Pool)**.

**Evidence before invention — never fabricate.** Every player pick and risk claim must be backed by *evidence*: a prediction site, a confirmed/predicted lineup, OR the provided board and package data (`players.json` eligibility and `prior_world_cup_record`, `standings-before.json`, and the web-verified knowledge base). Web research is the *preferred* evidence, but the shipped board/package data is valid ground truth — using it is not guessing. What is forbidden is inventing or guessing data you did not actually read: never state a lineup, statistic, probability, or scoreline that you did not read from a reachable source or a provided file, and never attribute invented data to a source you could not access. Web research is preferred but never a precondition for a valid submission — see **Research Availability & No-Fabrication Policy** below.

**Expected-value risk.** The Risk Play stake is a percentage of your same-day Fantasy XI score, so standings rank does not drive the claim. Pick by expected value — almost always a high-confidence Green `match_2plus_goals` on the strongest favorite; Red claims are negative-EV. The `choose-risk-play` skill has the EV math.

## Runtime & Timing — Read This Before Researching

- **The hosted agent runs at ~09:00 America/Denver (Mountain Time), and the sandbox clock is Mountain Time.** The submission does not lock until **21:00 Mountain** (`submission_locks_at` in `matchday.json`) — but you only get this one 9 AM pass, so research with the information that exists at 9 AM.
- At 9 AM Mountain, **confirmed** starting XIs for that day's matches are usually NOT out yet (they leak ~1 hour before kickoff). Rely on **predicted** lineups, **overnight-published previews** (US outlets post late evening / early morning, so a piece "published 8 hours ago" IS available to you), and **prediction-market odds**.
- Favor sources that publish on US time and are reachable from a US sandbox. ESPN and BBC were **removed** from the list below: in live runs ESPN returned only the bare schedule (no previews/lineups) and BBC article pages were not publicly accessible from the sandbox. Do not spend research budget retrying them.

## Web Research Domains — Tight Core (use these, not a long list)

The source list is deliberately short. Most preview outlets just echo the same wire, so a long list buys redundant "consensus," not signal — and it tempts you to spray one shallow search per site instead of fully covering every fixture. Consult the **core** sources every run; the **backups** exist only as insurance if a core site is unreachable that morning. Each line does a job the others cannot.

| Tier | Domain | Its job (nothing else does it as well) |
|------|--------|----------------------------------------|
| Core | kalshi.com + polymarket.com | **Odds / strongest favorite** — real-money implied win probabilities (ground truth) |
| Core | www.forebet.com | **Claim probabilities** — over/under 2.5, BTTS, predicted/exact score; maps directly to the risk-claim table |
| Core | www.theguardian.com | **Predicted lineups + team news / injuries** — reliably reachable journalism |
| Core | www.sportsmole.co.uk | **Predicted starting XIs** for each side |
| Backup | www.foxsports.com, www.cbssports.com | US-time previews / predicted XIs — use ONLY if a core lineup source is down |

For **bracket-play only** (knockout rounds), also consult www.fifa.com (FIFA rankings) and en.wikipedia.org (knockout history). These are not for daily picks.

Why these four jobs and no more: **markets** tell you who's favored, **Forebet** gives the specific over/under-BTTS-score numbers the risk claims map to, and **The Guardian + Sports Mole** are your only lineup sources — markets and Forebet never tell you who *starts*, and the fail-closed injury gate depends on a real predicted XI. Player ratings/xG sites (WhoScored, SofaScore, FBref) were dropped: they rarely change a pick and are redundant with the above.

**Prediction markets are ground truth for odds.** A market share price is the implied probability: a "Yes" contract at $0.62 ≈ 62%. Use them to (a) pick the strongest-favorite match for the Risk Play, and (b) sanity-check Forebet — if a market and Forebet diverge sharply, lower confidence.

If a core domain is unreachable, fall to the backup, then to the offline policy below — never invent its content. **Only list a source in your research summary if you actually read content from it** — never name a site as the source of a figure you did not retrieve from it. Never fabricate data from a source you could not access.

## Research Availability & No-Fabrication Policy

Web research is *preferred* evidence, but its absence is never a license to invent, and a valid submission never depends on the network being up. Two hard rules govern every skill:

1. **Never fabricate or guess source data.** Do not state a lineup, starter, statistic, probability, scoreline, or prediction you did not actually read from a reachable source or a provided file. If a domain is unreachable or a search returns nothing useful, skip it — do not "fill in" what it might have said and do not attribute invented data to it. Inventing research is a failure even if the resulting pick scores well.

2. **When external research is unavailable** (no network, all domains unreachable, or searches return nothing useful), do NOT stop and do NOT guess — fall back to the provided ground-truth data, which is legitimate evidence, not fabrication:
   - `game-board/players.json` — who is actually eligible today, plus each player's real `prior_world_cup_record` (starts, minutes, goals, assists, cards).
   - `game-board/standings-before.json`, `teams.json`, `matches.json` — standings context and the valid IDs for the submission.
   - `skills/pick-fantasy-xi/references/world-cup-2026-knowledge.md` — web-verified star tiers and the Injury & Availability Protocol (apply the gate **fail-closed**: with no network you cannot confirm a doubtful player's start, so exclude him).

   Build the Fantasy XI by ranking each position pool on this on-board evidence and picking confirmed-eligible, healthy, likely full-90 players. For the risk play, use the choose-risk-play **Step 7 safety net** — skip (`risk_play: null`) or a blind Green (`match_2plus_goals`) on the board's clearest mismatch. Lower your confidence and prefer conservative claims, but never guess a probability.

A legal submission built entirely from the provided board/package data with no web access is valid and expected when offline. The only failure mode is invented "research."
