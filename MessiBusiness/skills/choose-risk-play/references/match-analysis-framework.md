# Match Analysis Framework

A structured approach to analyzing each match on the board for Risk Play claim selection.

## Step-by-Step Match Analysis

### Phase 1: Classify the Match

Determine the match archetype from prediction site data:

| Archetype | Characteristics | Best For |
|-----------|----------------|----------|
| **Blowout** | One team 75%+ favorite, 3+ goal margin expected | `team_wins_by_3plus`, `team_scores_first`, `match_2plus_goals` |
| **Clear Favorite** | One team 60-75% favorite, 1-2 goal margin | `team_scores_first`, `match_2plus_goals`, `match_2plus_yellow_cards` |
| **Competitive** | Neither team above 55% | `match_2plus_yellow_cards`, `no_goal_first_10`, `both_teams_score` |
| **Defensive Grind** | Under 2.0 expected goals, low-scoring prediction | `no_goal_first_10`, `no_goal_stoppage_time` |
| **Shootout** | Over 3.0 expected goals, attacking teams | `match_over_2_5_goals`, `both_teams_score`, `match_2plus_goals` |

### Phase 2: Check for Special Factors

These factors can shift predictions significantly:

**Altitude and Climate**
- Mexico City venues: altitude affects player stamina (more late goals, more subs)
- Summer heat in southern US cities (Houston, Dallas, Miami): European teams may tire
- Indoor/retractable roof stadiums: weather neutral

**Tournament Context**
- Matchday 1 (opening group match): teams cautious, fewer goals, fewer cards
- Matchday 3 (final group match): desperate teams take risks, more goals and cards
- Dead rubber (both teams qualified): rotated squads, experimental lineups

**Head-to-Head History**
- Some matchups are historically high-scoring (e.g., Brazil vs Argentina)
- Some matchups are historically cagey (e.g., France vs Germany)
- Check Wikipedia for the last 5 head-to-head results

**Referee Tendencies**
- If the referee is announced, search for their card statistics
- Some referees average 5+ cards per match, others under 3
- This directly affects `match_2plus_cards` and `match_2plus_yellow_cards` claims

### Phase 3: Calculate Claim Probabilities

For the match's archetype, estimate each claim's probability:

#### Blowout Match (e.g., Brazil vs Haiti)
| Claim | Estimated Probability | Reasoning |
|-------|----------------------|-----------|
| `match_2plus_goals` | 90% | Favorite will score multiple |
| `no_goal_first_10` | 80% | Even favorites rarely score in first 10 |
| `goal_before_halftime` | 85% | Favorite usually scores before HT |
| `match_2plus_cards` | 70% | Fewer fouls when one team dominates |
| `team_scores_first` (favorite) | 80% | Heavy favorite usually opens scoring |
| `match_over_2_5_goals` | 70% | Favorites often score 3+ |
| `team_wins_by_3plus` (favorite) | 25% | Only if truly massive quality gap |

#### Clear Favorite Match (e.g., England vs Panama)
| Claim | Estimated Probability | Reasoning |
|-------|----------------------|-----------|
| `match_2plus_goals` | 80% | Likely 2-3 goals |
| `no_goal_first_10` | 85% | Standard early-match pattern |
| `team_scores_first` (favorite) | 65% | Usually scores first but not always |
| `match_2plus_yellow_cards` | 75% | Moderate physicality |
| `match_over_2_5_goals` | 55% | Possible but not certain |

#### Competitive Match (e.g., Netherlands vs Japan)
| Claim | Estimated Probability | Reasoning |
|-------|----------------------|-----------|
| `no_goal_first_10` | 88% | Both teams cautious early |
| `match_2plus_cards` | 85% | More tackling in even matches |
| `match_2plus_yellow_cards` | 75% | Physical contest |
| `match_2plus_goals` | 70% | Usually some goals but could be cagey |
| `both_teams_score` | 55% | Possible but uncertain |

### Phase 4: Risk-Reward Assessment

For each viable claim, calculate expected value:

```
Expected Value = probability * stake - (1 - probability) * stake
               = stake * (2 * probability - 1)
```

Example with 200 tournament points:
- Green (30 pts stake): 85% probability → EV = 30 * (2*0.85 - 1) = +21 pts
- Yellow (50 pts stake): 65% probability → EV = 50 * (2*0.65 - 1) = +15 pts
- Red (70 pts stake): 25% probability → EV = 70 * (2*0.25 - 1) = -35 pts

The math clearly shows: **Green claims with high probability dominate Red claims with low probability on expected value**. Only take Red when:
1. You are behind and need variance (not expected value)
2. You have unusually high confidence (40%+ on a Red claim)
3. Your point total is so low that the stake is trivial

## Common Mistakes to Avoid

1. **Recency bias**: One match result doesn't change underlying probabilities
2. **Favorite bias**: Even 80% favorites lose 20% of the time — that's 1 in 5 matches
3. **Narrative bias**: "This team is due for an upset" is not a probability-based argument
4. **Sunk cost**: Past losses don't affect today's optimal claim — always pick based on today's probabilities
5. **Gambling fallacy**: "I've been conservative, time to go big" — your strategy should be standings-based, not feelings-based
