---
name: pick-fantasy-xi
description: Research-driven Fantasy XI selection — builds a confirmed-starter floor pool from fresh predicted lineups (locking the +4 start/60-min floor), then maximizes goal/assist/clean-sheet upside within it to pick 11 valid players.
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

### Step 2.0: Superstar pool scan (do this FIRST, before any web search)

Scan the eligible players from Step 1 for global superstars using the **Global Superstar Shortlist** and the **One Verified Star Per Team** table in `references/world-cup-2026-knowledge.md` (web-verified list covering all 48 teams). The pool is ground truth **for narrative write-offs only**: **if a superstar is in `players.json` and eligible today, they are playing in this tournament** — never write them off based on prior narratives, age, or assumed retirement. In the warmup we left an eligible Messi out of the XI; it was our costliest error and we finished rank 21.

**Pool eligibility is NOT a fitness clearance.** Eligibility rebuts *narrative* doubts (age, retirement, "successor generation"). It does NOT rebut a *specific, current injury or fitness report*. A name flagged by the Injury & Availability Protocol, or any name with a fresh "injury" / "team news" hit, stays gated by the check below no matter how eligible the pool says they are. Players appear in `players.json` before kickoff regardless of whether they will actually start, so an injured star can be both "eligible" and a guaranteed 0.

**Mandatory injury check for every eligible superstar — FAIL CLOSED (default = exclude):**
1. Follow the **Injury & Availability Protocol** in `references/world-cup-2026-knowledge.md`: there is no current static status list — run a FRESH search `"[Player name] injury"` / `"[Team name] team news"` and decide from a **fresh predicted XI**, not any dated snapshot. The only stable list is the season-ending hard-excludes in that Protocol.
2. **Reports state he will MISS this match** (ruled out, not in squad, suspended) → remove him from auto-pick AND from XI consideration entirely. An injured superstar scores 0.
3. **Doubtful / game-time decision / "racing to be fit" / any unconfirmed start** → **EXCLUDE by default.** Pick him ONLY if a fresh **predicted or confirmed starting XI explicitly names him in the starting 11** (a generic "he's eligible" or "could feature" is not enough). One such source is the bar; if sources conflict, exclude.
4. **Research is missing, inconclusive, failed, returned the wrong fixture, or you simply could not confirm a start** → treat the flag as UNRESOLVED → **EXCLUDE.** Do not fall back to "he's in the pool so pick him." Skip to the next-best **healthy** player in the same pool. There is never a reason to gamble a slot on an unconfirmed injury doubt: the board has 49 FWDs, 67 MIDs, 70 DEFs, 25 GKs — a fit replacement always exists, and skipping one doubtful star costs nothing.
5. **Named as a starter in a fresh predicted/confirmed XI for THIS match** → locked-in pick. A dated "fit/returning" note is NOT confirmation; only a fresh XI is.

> **This applies to the famous names too — fame is not a team sheet.** The season-ending hard-excludes in the Protocol (Rodrygo, Militao, Estevao, de Ligt, Simons, Mitoma, …) never play. Beyond those, stars including Neymar have been picked on reputation while not starting and returned 0 — every superstar clears the same fresh-XI check as anyone else. When a big name's start cannot be confirmed, take a confirmed-starting attacker from the same favored side instead.

Confirm each surviving superstar's starting status in the lineup research below and treat confirmed/likely starters among them as locked-in picks.

For EACH match today, search multiple prediction sites for expected starting XIs:

### Core lineup sources (use these two — fall to a backup only if one is down):
- The Guardian: `"[Team A] v [Team B] team news World Cup 2026"` on www.theguardian.com
- Sports Mole: `"[Team A] vs [Team B] predicted lineup"` on www.sportsmole.co.uk
- Backup ONLY if a core source is unreachable: Fox Sports (www.foxsports.com) or CBS Sports (www.cbssports.com)

Note the 9 AM Mountain runtime: confirmed XIs are often not out yet, so "predicted lineup" articles (including ones published overnight) are your main evidence — see README "Runtime & Timing."

### What to extract:
- Which players are predicted to start (cross-reference across sources)
- The likely formation each team will use
- Any injuries, suspensions, or rotation expected
- Who takes penalties and free kicks for each team

### Starter confidence levels (calibrated to the two core lineup sources):
- In BOTH The Guardian's and Sports Mole's predicted XI = **Confirmed starter** — must-pick candidate
- In ONE core source, not contradicted by the other = **Likely starter** — strong pick
- Only mentioned as a doubt / rotation risk = **Possible starter** — only pick if high upside
- Not named as a starter in either = **Bench player** — avoid entirely

With just two lineup sources, "both agree" is your confirmed bar. If one core source is down, a backup outlet's XI can stand in for it.

### If web research is unavailable (no network / all sources unreachable / no useful results)

Do NOT fabricate lineups or starters, and do NOT abandon the task — fall back to the provided ground-truth data, which is valid evidence, not a guess:
- Rank players within each position pool using `players.json` eligibility and `prior_world_cup_record` (starts, minutes, goals, assists per prior World Cup). High prior-WC minutes ≈ a reliable starter; a strong goal/assist rate ≈ attacking upside.
- Use the web-verified star tiers and the Injury & Availability Protocol in `references/world-cup-2026-knowledge.md`. Keep the gate **FAIL-CLOSED**: with no network you cannot confirm anyone's start, so exclude every season-ending hard-exclude and any name whose start you cannot positively establish (Neymar included), and lean on prior-WC starter minutes to choose among the rest.
- Only call a player a "starter" on real evidence (strong prior-WC minutes or knowledge-base star status) — never assert a start you did not read. Prefer healthy, eligible, full-90 profiles and lower your confidence accordingly.

A legal XI built from board data is valid and expected when offline; an XI built from invented research is a failure even if it scores. Then continue to Step 3 (use board/knowledge-base context in place of the missing web data).

## Step 3: Research Match Predictions and Statistics

For each match, gather probability data from prediction platforms:

### Check the prediction markets (cleanest read on the favorite):
- Search `"[Team A] [Team B] Kalshi"` / `"[Team A] [Team B] Polymarket"` — each side's implied win probability and any total-goals market.

### Search Forebet for mathematical predictions:
- Search: `"[Team A] vs [Team B]" site:forebet.com`
- Extract: 1X2 probabilities, over/under 2.5, BTTS probability, predicted score

### One expert read (from the core preview):
- Take the predicted scoreline and favorite from The Guardian's preview. Skip footballpredictions, xG, and ratings sites — out of the core list and rarely decisive.

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
The clean-sheet bonus only pays if the player stays on the pitch 60+ minutes — combine this with Substitution Risk below.
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

### Substitution Risk (subtract from expected value)
An early sub forfeits both the 60-minute bonus AND the clean-sheet bonus (warmup evidence: 4 of our Argentina starters were withdrawn before 60' and returned +2 each despite a 3-0 clean sheet).
- Veteran (30+) center-back or holding midfielder on a heavy favorite: **-1.5** (rested once the game is safe)
- Squad player likely to be a planned half-time/tactical sub: **-1**
- Goalkeepers, first-choice fullbacks, the main striker, and the team's talisman: **no deduction** (full-90 profiles)

## Step 4.5: Build the Confirmed-Starter Pool — the Floor Filter (do this BEFORE position pools)

This is the highest-value habit at this stage of the tournament. By matchday 2–3 most teams have a settled first XI, and any player who **starts and completes 60'** banks a **+4 floor every time** — start `+2` plus 60-minute `+2`, before any goal/assist/clean-sheet upside. Eleven such players floor the whole XI near **+44** and, more importantly, **eliminate the dead-slot zeros that have cost us most** (a benched or injured pick returns 0 and wastes a slot). So you will pick ONLY from players who belong in this pool.

**The pool is the FILTER, not the objective.** It locks the floor; you still maximize *ceiling* within it (Steps 5–6). Do not pick the 11 "safest" names — pick the 11 highest-upside players who are ALSO in this pool. Floor is the gate; goals/assists/clean sheets are how you actually climb. (Note the floor is high-probability, not a true guarantee — an early red, a first-half injury, or a tactical hook before 60' can still break it; that is exactly what 4.5b guards against.)

### 4.5a — Admission test (a player joins the pool only if he passes ALL three)
1. **Confirmed/predicted STARTER** — named in a fresh predicted XI (Guardian/Sports Mole) for *today's* fixture; offline, a clear regular starter by `prior_world_cup_record` minutes. Reputation is not admission. (Same hard gate as Step 7 — apply it now so the entire pool is clean before you pick.)
2. **Likely to complete 60'** — pass the 90-minute reliability tag in 4.5b; keep early-hook profiles out of your floor slots.
3. **On a motivated team** — pass the dead-rubber check in 4.5c.

Rebuild this pool FRESH every matchday. It is never a saved whitelist — rest, knocks, and yellow-card suspensions churn it daily. (This is what makes it different from the old static injury list that burned us.)

### 4.5b — Tag 60-minute reliability (the floor's weak link)
The 60' gate fails on exactly the players that look safest: veteran centre-backs and holding mids on heavy favorites get **hooked once the game is won**. Evidence: Otamendi and Molina *started* Argentina's 3-0 and scored **0** — subbed before 60', forfeiting both the minutes and the clean-sheet bonus. So tag every pool member:
- **Reliable-90 anchors (prefer for the floor):** the favorite's GK, first-choice fullbacks who play 90, the talisman/main creator, the main #9 striker. A favorite's GK is the safest +4 on the board — keepers almost never come off.
- **Early-hook risks (discount, especially on blowout favorites):** 30+ veteran CBs and holding mids who get rested at 3-0. Their clean-sheet upside is real but their 60' completion is not — do NOT treat them as floor-safe.

### 4.5c — Dead-rubber / rotation check
A starter pool is only "constant" while the match still matters:
- **Team already qualified, or already eliminated, going into this match** → expect heavy rotation or an experimental XI (most acute on the final group matchday, MD3 — read the `round` field and the preview). Drop confidence hard; prefer the motivated side, or pick from a different match.
- **Must-win / still-alive match** → first XI very likely; the pool holds.

Never assume an earlier matchday's lineup simply repeats — confirm against today's predicted XI and the stakes. The confirmed-starter pool you build here is the ONLY set Step 5 may draw from.

## Step 5: Build Position Pools (DO THIS BEFORE PICKING)

Formation errors score 0. To make an illegal formation **structurally impossible**, you must select by filling position quotas, never by picking "best players" and hoping the counts work out.

> **There are only four buckets — no field roles.** `players.json` classifies every player as exactly one of `GK`, `DEF`, `MID`, or `FWD`. There is NO sub-role (no LW / ST / RW / CB / DM / "second striker"), and the rules never ask for one. The ONLY roster constraints are: exactly **1 GK**, **3–5 DEF**, **3–5 MID**, **1–3 FWD**, total 11. So:
> - **GK is the exception to stacking — exactly ONE goalkeeper, EVER.** Never pick two GKs, not even to chase two clean sheets in two matches. Two GKs is an illegal split: the scoring engine applies a position penalty that EXCLUDES your higher-scoring keeper (we did this once and forfeited a +10 GK, keeping the +8 one — a pure self-inflicted loss). If two keepers both look great, pick the better one and spend the other slot on a DEF/MID/FWD. The "stack freely" rule below is for DEF/MID/FWD ONLY.
> - Within the outfield buckets, rank purely by expected points and take the top N. **You may and often should stack same-archetype players** — three out-and-out strikers in the FWD slots, two No. 9s from different matches, five attacking/creative midfielders with zero holding mids. Whatever scores the most points.
> - Do **not** reserve a slot for "variety" (e.g. don't keep a winger out to fit a target-man, or a goalscorer out to fit a playmaker). Two great strikers beat one great striker plus one mediocre winger.
> - Pick the bucket split (the formation) that **maximizes total expected points** subject only to the 1 / 3–5 / 3–5 / 1–3 limits. If your best XI is six attackers and three defenders, use 1-3-4-3 or 1-3-5-2; if defenders are the value, load DEF. Let the points decide the shape, not a preferred shape decide the points.

First, sort the **confirmed-starter pool from Step 4.5** into exactly one of four position pools, using the `position` field from `players.json` as the only authority. You are sorting the floor-filtered set, NOT all eligible players — that restriction is what locks the +4 floor. Do not infer position from web research — a player's pool is whatever `players.json` says (`GK`, `DEF`, `MID`, or `FWD`).

```
GK pool  = [pool members where position == "GK"],  sorted by expected points
DEF pool = [pool members where position == "DEF"], sorted by expected points
MID pool = [pool members where position == "MID"], sorted by expected points
FWD pool = [pool members where position == "FWD"], sorted by expected points
```

Confirm each position pool has enough confirmed starters to fill a legal formation: at least 1 GK, 3 DEF, 3 MID, 1 FWD. With several matches' worth of starters this is normally easy. If the floor-filtered pool is genuinely too thin in one position (rare — e.g. a one- or two-match day), backfill ONLY that position with the next-best player by `prior_world_cup_record` starter minutes — never with someone you expect to be benched. A thin pool is a reason to reach for a proven starter, never a reason to drop the floor filter entirely.

## Step 6: Commit to a Formation, Then Fill Exact Quotas

### 6a. Write down ONE formation as four explicit numbers

Pick exactly one row. Write its quotas down as `GK / DEF / MID / FWD` and treat them as a hard contract. Every legal formation sums to 11 with 1 GK, 3-5 DEF, 3-5 MID, 1-3 FWD:

| Formation | GK | DEF | MID | FWD | When to Use |
|-----------|----|----|-----|-----|-------------|
| 1-3-5-3 | 1 | 3 | 5 | 3 | Many strong midfielders + 3 elite forwards |
| 1-3-4-3 | 1 | 3 | 4 | 3 | 3 elite forwards, balanced midfield |
| 1-4-3-3 | 1 | 4 | 3 | 3 | Clean-sheet team + 3 elite forwards |
| 1-4-4-2 | 1 | 4 | 4 | 2 | Clean-sheet team + good mids, only 2 standout forwards |
| 1-4-5-1 | 1 | 4 | 5 | 1 | Midfield-heavy, only 1 elite forward |
| 1-5-3-2 | 1 | 5 | 3 | 2 | Multiple clean-sheet opportunities |
| 1-5-4-1 | 1 | 5 | 4 | 1 | Extreme clean-sheet play, 1 elite forward |

Default to **1-4-4-2** or **1-4-3-3** if unsure — both are always legal and balanced.

### 6b. Fill each quota from ONLY its own pool

Take exactly the quota number from the top of each pool. Because you draw GKs only from the GK pool, you cannot end up with 2 GKs.

```
selected_GK  = top 1 from GK pool          (always exactly 1)
selected_DEF = top [DEF quota] from DEF pool
selected_MID = top [MID quota] from MID pool
selected_FWD = top [FWD quota] from FWD pool
fantasy_xi   = selected_GK + selected_DEF + selected_MID + selected_FWD
```

You may swap a default top pick for a lower one in the SAME pool for strategic reasons (e.g., a confirmed starter over a benched star, or diversifying across matches). Swapping within a pool never changes the position counts, so the formation stays legal.

- **Ceiling within the floor**: every player in these pools already banks the ~+4 floor (Step 4.5), so this step is pure upside — within each position take the highest goal/assist/clean-sheet potential (penalty takers and attackers on favorites, full-90 clean-sheet defenders), never the merely "safest" name. The floor is already locked; now chase points.
- **Diversification**: Prefer spreading picks across 2-3 matches, but ONLY with players from the favored side of each match. Never roster an underdog's player just to cover a match — underdog starters cap at ~+4 with no upside (our Iceland/Congo picks in the warmup wasted 3 slots). Swap within a pool to achieve this — never by changing a position quota.
- **Archetypes to favor within pools**: eligible superstars from Step 2.0 (always first), penalty takers on favored teams (+6 goal upside), set-piece specialists, attacking full-backs (90 min + clean sheet + assist upside), the main #9 striker.

## Step 7: Mandatory Recount and Repair Loop

Do NOT output until this passes. Re-read the `position` field in `players.json` for each of your 11 chosen `player_id` values and tally them fresh (do not trust your memory of which pool you used):

```
count_GK  = number of chosen players whose players.json position == "GK"
count_DEF = number of chosen players whose players.json position == "DEF"
count_MID = number of chosen players whose players.json position == "MID"
count_FWD = number of chosen players whose players.json position == "FWD"
total     = count_GK + count_DEF + count_MID + count_FWD
```

Check ALL of these. Every one must be true:
- **`count_GK == 1`** — exactly one goalkeeper, NOT zero and NOT two. This is the most-failed check (we shipped two GKs once and the engine voided our higher keeper). Verify it FIRST, every time.
- `total == 11`
- `3 <= count_DEF <= 5`
- `3 <= count_MID <= 5`
- `1 <= count_FWD <= 3`
- All 11 `player_id` values are unique (no duplicates)
- Every `player_id` exists in `players.json` and lists today's `matchday_id` in `eligible_matchday_ids`
- Every `player_id` belongs to a team in a fixture that has its own research block from `match-research` (Match Coverage Contract). If you want a player from a fixture you did not research, go research that fixture first — do not pick from a blank.
- **Every `player_id` is a confirmed/predicted STARTER for his match** — named in a fresh predicted XI (Guardian/Sports Mole), or offline a clear regular starter by prior-WC minutes. Reputation is NOT confirmation. A benched, rotated, or injured pick scores 0 and wastes a slot — we have leaked points this way on nearly every matchday. This re-verifies the Step 4.5 admission test: if any name slipped in without confirmation, drop it now and replace with a confirmed starter in the same pool.

### If ANY check fails, repair and recount:
- **count_GK == 2** → remove the weaker GK; add the best unused player from whichever outfield pool is below its minimum.
- **count_GK == 0** → remove your weakest outfield player; add the best GK.
- **count_DEF > 5** → remove the weakest DEF; add the best unused MID or FWD that is under quota.
- **count_DEF < 3** → remove the weakest over-quota outfielder; add the best unused DEF.
- **count_MID > 5 or < 3** → same logic: trim the surplus position, top up the deficient one.
- **count_FWD > 3** → remove weakest FWD; add best unused DEF/MID under quota.
- **count_FWD == 0** → remove weakest over-quota outfielder; add the best FWD.
- **duplicate / ineligible id** → drop it and add the best unused, eligible player of the SAME position.
- **unconfirmed starter** (you cannot find the player in a fresh predicted XI) → drop him; add the best player in the SAME pool whose start you CAN confirm. Never keep a name just because he is famous.

After ANY change, run the entire tally and checklist again from the top. Repeat until every check passes. Only then proceed to output. A valid XI that is slightly suboptimal beats an invalid XI that scores 0 every time.

## Step 8: Output

Return the 11 `player_id` values as a JSON array of strings for `fantasy_xi`, ordered GK → DEF → MID → FWD so the formation is easy to verify. Write a short `strategy` note stating the formation you used (e.g., "1-4-4-2"), the prediction sources consulted, and your top expected scorers.
