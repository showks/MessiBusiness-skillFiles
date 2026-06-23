# Historical World Cup Statistical Patterns

Data-driven baselines from past World Cups to calibrate predictions when web research is incomplete.

## Goal Statistics Across Recent World Cups

| Tournament | Total Goals | Goals/Match | 2+ Goals % | 3+ Goals % | 0-0 Draws |
|-----------|-------------|-------------|------------|------------|-----------|
| 2022 Qatar | 172 | 2.55 | 75% | 52% | 4 (6.3%) |
| 2018 Russia | 169 | 2.64 | 73% | 55% | 1 (1.6%) |
| 2014 Brazil | 171 | 2.67 | 78% | 56% | 1 (1.6%) |
| 2010 S. Africa | 145 | 2.27 | 67% | 44% | 7 (10.9%) |
| Average | — | 2.53 | 73% | 52% | ~5% |

### Goal Timing Distribution (2022 World Cup)

**WARNING — read the math correctly.** "% of total goals" is NOT "% of matches". To estimate a claim probability, divide the goal count by 64 matches to get goals-per-match in that window, then convert to the chance of at least one goal. Confusing the two overstates `no_goal_first_10` badly (it is ~75-80% at best, not ~91%).

| Period | Goals Scored | % of Total | Per-Match Rate | Claim Relevance |
|--------|-------------|------------|----------------|-----------------|
| 0-10 min | 15 | 8.7% | ~0.23/match | A first-10 goal occurs in ~20-25% of matches (more vs heavy favorites) → `no_goal_first_10` hits only ~75-80%, and ~60-65% in blowouts |
| 11-45 min | 62 | 36.0% | ~0.97/match | `goal_before_halftime` hits ~75% |
| 46-80 min | 67 | 39.0% | ~1.05/match | Most goals come mid-second half |
| 81-90 min | 11 | 6.4% | ~0.17/match | Late drama but not common |
| Stoppage time | 17 | 9.9% | ~0.27/match | A stoppage-time goal occurs in ~22-25% of matches (long modern added time) → `no_goal_stoppage_time` hits ~75%, not 80%+ |

## Card Statistics

| Tournament | Yellow Cards | Red Cards | Cards/Match | 2+ Cards % |
|-----------|-------------|-----------|-------------|------------|
| 2022 Qatar | 219 | 4 | 3.48 | 82% |
| 2018 Russia | 219 | 4 | 3.42 | 80% |
| 2014 Brazil | 187 | 10 | 3.08 | 78% |
| Average | — | 3.33 | ~6% red | ~80% |

### Card Distribution by Match Type
| Match Type | Avg Cards | 2+ Yellow % | Red Card % |
|-----------|-----------|-------------|------------|
| Group Stage | 3.2 | 72% | 5% |
| Knockout Stage | 3.8 | 82% | 8% |
| Semifinal/Final | 4.1 | 88% | 10% |
| Rivalry matches | 4.5+ | 90%+ | 12% |

## Clean Sheet Patterns

| Tournament | Clean Sheets | CS % (per team per match) |
|-----------|-------------|--------------------------|
| 2022 Qatar | 41 | 32% |
| 2018 Russia | 39 | 30% |
| 2014 Brazil | 33 | 26% |
| Average | — | ~29% |

### Clean Sheet by Team Strength
| Team Category | CS % in Group Stage | CS % in Knockouts |
|--------------|--------------------|--------------------|
| Top 8 seed | 42% | 35% |
| Mid seed (9-24) | 28% | 22% |
| Low seed (25-48) | 18% | 12% |

## Both Teams to Score (BTTS) Patterns

| Tournament | BTTS Yes % | BTTS No % |
|-----------|-----------|-----------|
| 2022 Qatar | 48% | 52% |
| 2018 Russia | 52% | 48% |
| 2014 Brazil | 53% | 47% |
| Average | ~51% | ~49% |

### BTTS by Match Archetype
| Archetype | BTTS Yes % |
|-----------|-----------|
| Two strong attacking teams | 62% |
| Clear favorite vs moderate team | 48% |
| Heavy favorite vs weak team | 38% |
| Two defensive teams | 35% |

## Exact Score Distribution (2022 World Cup)

| Scoreline | Occurrences | % of Matches |
|-----------|-------------|-------------|
| 1-0 | 11 | 17.2% |
| 2-1 | 9 | 14.1% |
| 0-0 | 4 | 6.3% |
| 2-0 | 7 | 10.9% |
| 1-1 | 6 | 9.4% |
| 3-1 | 4 | 6.3% |
| 0-1 | 4 | 6.3% |
| 3-0 | 3 | 4.7% |
| Other | 16 | 25.0% |

Most common scores for `exact_score` claim: 1-0, 2-1, 2-0 cover 42% of all matches.

## Knockout Stage Specific Patterns

| Event | 2022 % | 2018 % | Historical Avg |
|-------|--------|--------|---------------|
| Match goes to extra time | 25% | 25% | 24% |
| Match goes to penalties | 12.5% | 12.5% | 14% |
| Comeback win (concede first, win) | 11% | 14% | 12% |
| Team wins by 3+ goals | 6% | 8% | 8% |

## Team-Specific Historical Patterns

### Teams That Score Early (minutes 1-15)
- France, Brazil, Germany, Netherlands historically score early
- England, Spain tend to build slowly

### Teams Prone to Cards
- South American teams: historically higher card count
- African teams: often physical but varies by team
- Asian teams: generally fewer cards
- European teams: varies widely

### Teams With World Cup Pedigree (Deep Run History)
- Brazil: 5 titles, consistent deep runs
- Germany: 4 titles, rarely eliminated before QF
- Argentina: 3 titles, strong knockout record
- France: 2 titles, dominant when clicking
- Spain: 1 title, but inconsistent in knockouts
- England: 1 title, historically underperform vs expectations

### Dark Horse Patterns
In each of the last 4 World Cups, at least one team from outside the top 8 seeds reached the semifinals:
- 2022: Morocco (reached SF)
- 2018: Croatia (reached Final)
- 2014: Costa Rica (reached QF), Colombia (QF)
- 2010: Uruguay (reached SF), Ghana (QF)

## 2026 Format Considerations

The expanded 48-team format introduces new dynamics:
- More mismatches in group stage (Pot 1 vs Pot 4 gap is wider)
- Round of 32 is new — expect some upsets as lower seeds play more games
- Best third-place advancement means defensive play could be rewarded (teams trying not to lose rather than to win)
- 8 best third-place teams advance — this rewards goal difference, so teams may try to win big
