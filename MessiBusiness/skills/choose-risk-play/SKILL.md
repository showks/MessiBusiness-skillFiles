---
name: choose-risk-play
description: Expected-value risk play selection — uses prediction-market and Forebet probabilities to pick the highest-EV claim, which is almost always a high-confidence Green.
---

# Risk Play Selection Skill

Select the risk play claim with the highest expected value. The stake is a percentage of your same-day Fantasy XI score, so your standings rank does not affect the choice.

## Step 1: How the Stake Works

The Risk Play stake is a percentage of your **same-day Fantasy XI score**:

| Risk type | Stake | If correct | If incorrect |
|-----------|-------|-----------|--------------|
| Green  | 15% of same-day XI | +15% | −15% |
| Yellow | 25% of same-day XI | +25% | −25% |
| Red    | 35% of same-day XI | +35% | −35% |
| Invalid / skipped | 0 | 0 | 0 |

An XI that scores 0 makes the stake 0. The stake comes from today's XI, not your tournament total, so **pick by probability, not by rank.**

### Pick the highest expected value

Expected value of a claim = `stake% × (2P − 1)`, where P is its hit probability. The higher-stake tiers (Yellow, Red) are also the lower-probability claims, so a bigger stake rarely beats a high-probability Green. Therefore:

1. **Default: Green `match_2plus_goals` on the strongest favorite** (~85% when a heavy favorite plays). This is the pick on a normal day, at every rank.
2. **Yellow `match_2plus_yellow_cards` only** when read evidence puts a physical match at ~70%+.
3. **Never take a Red claim** — every Red claim is below 50% probability, which is negative expected value.
4. **Stake on the same match as your strongest Fantasy XI picks.**

## Step 2: Research Match Probabilities from Prediction Sites

For EACH match today, search prediction websites for specific probabilities:

### Check the prediction markets (ground truth for odds — do this first):
- Search: `"[Team A] [Team B] Kalshi"` and `"[Team A] [Team B] Polymarket"`, or the World Cup market pages on kalshi.com / polymarket.com
- A market share price IS the implied probability (a "Yes" at $0.62 ≈ 62%). Read off each side's win probability and any total-goals markets.
- This is the most reliable read on which match has the strongest favorite — exactly what the default Green claim targets.

### Search Forebet (mathematical model — highest-priority non-market source):
- Search: `"[Team A] vs [Team B]" site:forebet.com`
- Extract: 1X2 percentages, over/under 2.5 probability, BTTS probability, predicted score
- Forebet uses a mathematical model with historical data — treat these as reliable, and cross-check against the market price above; if they diverge sharply, lower confidence.

### One expert read (core preview):
- Take the predicted scoreline and key match narrative from The Guardian's preview (`"[Team A] v [Team B] World Cup 2026" site:theguardian.com`).
- Backup ONLY if The Guardian is unreachable: CBS Sports or Fox Sports.

### Search for card predictions:
- Search: `"[Team A] vs [Team B] cards prediction"` on available prediction sites
- Check if Forebet provides card predictions for the match

### Cross-reference predictions (three independent core signals: market, Forebet, Guardian):
- Market + Forebet + The Guardian all point the same way → **High confidence** (80%+)
- Market and Forebet agree (preview silent or unavailable) → **Moderate-High confidence** (~70%)
- Only one signal available, or market and Forebet disagree → **Low confidence** (below 60%) — lean Green or skip

## Step 3: Map Predictions to Claims

### GREEN CLAIMS (15% of same-day XI) — Target 75%+ win probability — your default tier

| Claim | When Prediction Sites Say... | Base Probability |
|-------|------------------------------|-----------------|
| `match_2plus_goals` | Default — ~75% of WC matches have 2+ goals; near-lock when a strong favorite plays (Forebet over 1.5 > 70%) | ~78% (~85%+ with a heavy favorite) |
| `goal_before_halftime` | Forebet over 0.5 first half > 65%, attacking match or heavy favorite | ~75% |
| `match_2plus_cards` | Physical teams, competitive match, or Forebet cards prediction | ~78% |
| `no_goal_first_10` | ONLY in even, cagey matchups. **NEVER with a heavy favorite** — favorites press from kickoff | ~75% even match; ~60-65% with heavy favorite |
| `no_goal_stoppage_time` | Cagey/defensive matches only — modern stoppage time is long, making this riskier than it looks | ~75% |

**Default GREEN pick**: `match_2plus_goals` on the match with the strongest favorite.

### YELLOW CLAIMS (25% of same-day XI) — only `match_2plus_yellow_cards` at ~70%+ is worth it

| Claim | When Prediction Sites Say... | Expected Probability |
|-------|------------------------------|---------------------|
| `match_2plus_yellow_cards` | Forebet/experts predict physical match, 3+ cards expected | ~70% |
| `team_scores_first` | One team is 70%+ favorite AND predicted to score first | ~60% |
| `match_over_2_5_goals` | Forebet over 2.5 > 60%, multiple experts predict 3+ goals | ~55% |
| `both_teams_score` | Forebet BTTS > 60%, two attacking teams facing each other | ~55% |
| `player_scores` | Star forward on a dominant team, confirmed starter | ~40% |

**Best YELLOW pick**: `match_2plus_yellow_cards` — highest probability Yellow claim.
**Best upside YELLOW**: `team_scores_first` on a heavy favorite backed by Forebet data.

### RED CLAIMS (35% of same-day XI) — AVOID under the current rule

> Every Red claim below is below 50% probability, so it is **negative expected value**. Do not take a Red claim on a normal group-stage day.

| Claim | When Prediction Sites Say... | Expected Probability |
|-------|------------------------------|---------------------|
| `team_wins_by_3plus` | Massive mismatch (e.g., top 5 team vs debutant), 3+ predictions agree | ~18% |
| `exact_score` | Forebet + 2 experts predict same scoreline, common score (2-0, 1-0) | ~10% |
| `player_scores_2plus` | Elite striker vs very weak defense, confirmed starter, penalty taker | ~12% |
| `red_card_shown` | Heated rivalry, aggressive teams, history of red cards | ~10% |
| `team_comeback_win` | **AVOID** — too unpredictable even with research | ~6% |
| `match_goes_to_extra_time` | Knockout only, very even teams, multiple sources say close match | ~22% |
| `match_goes_to_penalties` | Knockout only, defensive teams, experts predict extra time | ~13% |

**Best RED pick (group stage)**: `team_wins_by_3plus` on a massive mismatch.
**Best RED pick (knockout)**: `match_goes_to_extra_time` on an even knockout match.

## Step 4: Decision Algorithm (EV-first — same at every rank)

```
1. From research, find the match with the strongest favorite
   (highest market win prob / Forebet 1X2) and its goal-line numbers
   (Forebet over 1.5 / over 2.5). This is your target match — and it
   should be the same fixture your best Fantasy XI picks come from.

2. DEFAULT: Green match_2plus_goals on that match.
   Confirm it clears ~80% (heavy favorite, or Forebet over-1.5 > 70%,
   or two attacking sides). On a normal day this is the pick — stop here.

3. YELLOW UPGRADE TEST (only fires with real evidence):
   Is there a match with read evidence of a physical/card-heavy game
   (derby/grudge, or a Forebet cards line) at ~70%+ for 2+ yellow cards?
   YES and the evidence is genuinely that strong → match_2plus_yellow_cards.
   Otherwise → stay on the Green from step 2.

4. RED: do not take any Red claim — all are negative-EV. If nothing
   clears the Green bar with confidence, go to Step 7 (skip, or blind
   Green on the clearest board mismatch).
```

Your rank does not change this algorithm — leader or last place, the highest-EV claim is the same high-confidence Green.

## Step 5: Select Match and Construct the Claim

1. Choose the match with the highest-confidence prediction data.
2. Build the risk play object with ALL required fields from `claim-catalog.json`:
   - `claim_id`: The selected claim
   - `match_id`: From `matches.json` — must be a valid match for today
   - `team_id` (if required): Must be home or away team in the match
   - `player_id` (if required): Must be in `players.json` AND belong to a team in the match — AND injury-cleared: never stake a player-based claim (`player_scores`, `player_scores_2plus`) on anyone whose start is unconfirmed. Apply the Injury & Availability Protocol in `../pick-fantasy-xi/references/world-cup-2026-knowledge.md` plus a fresh `"[player] injury"` search; he must be named in the fresh confirmed/predicted starting lineup
   - `home_score` / `away_score` (if exact_score): Integers matching the predicted scoreline

3. **NEVER include**: `bet_points`, `stake`, `stake_percent` — the tournament calculates these.

## Step 6: Validate

Before finalizing:
- [ ] `claim_id` exists in `claim-catalog.json`
- [ ] All required fields for that claim are present
- [ ] `match_id` is from today's `matches.json`
- [ ] `match_id` has its own research block in the `match-research` summary (Match Coverage Contract — never stake the Risk Play on a fixture you did not research; if the strongest favorite is in an under-researched match, go research it first)
- [ ] `team_id` (if used) is home_team_id or away_team_id in that match
- [ ] `player_id` (if used) is in `players.json` and belongs to a team in that match
- [ ] No prohibited fields (bet_points, stake, stake_percent) included

If validation fails, fall back to: `{ "claim_id": "match_2plus_goals", "match_id": "[the match with the strongest favorite]" }`

## Step 7: Safety Net

If ALL of the following are true:
- Web research returned no useful prediction data
- You cannot confidently assess any claim's probability
- You have no prediction site data to work with

Then choose ONE of:
- `risk_play: null` (skip entirely — no risk, no reward)
- `{ "claim_id": "match_2plus_goals", "match_id": "[the match whose teams look most mismatched on the board data]" }` (safest blind Green — ~75% baseline, higher in mismatches; never blind-pick no_goal_first_10)

A wrong claim loses points. A skipped claim loses nothing. When truly blind, skipping is better than guessing on Yellow/Red.

**Never fabricate a probability.** Do not invent a Forebet figure, a Kalshi/Polymarket market price, or any percentage to justify a claim — if you did not read it from a reachable source, you do not have it. Base the blind Green only on what the board data plainly shows (e.g., a top-tier team vs a debutant). A Yellow or Red claim requires real read evidence; absent it, take the blind Green or skip. This is the choose-risk-play half of the package-wide No-Fabrication Policy in the team `README.md`.
