# Formation Strategy Guide

> **Formations here are only bucket counts (GK/DEF/MID/FWD), never field roles.** `players.json` has no LW/ST/RW/CB/DM — just the four buckets, with hard limits of 1 GK, 3–5 DEF, 3–5 MID, 1–3 FWD. "3 FWD" means *the three highest-expected-points players in the FWD pool*, even if all three are strikers or two play for the same team. Stacking same-archetype players is encouraged when they score the most: e.g. five creative midfielders and no holding mid, or three pure goalscorers up top. Never hold a high-EV player out of the XI for the sake of role variety. The formation is just whichever legal bucket split puts your highest-EV 11 on the sheet — pick the points, not the shape.

## Formation Decision Tree

Use this decision tree to select the optimal formation for each matchday:

### Question 1: How many elite forwards are available today?
- 3 elite forwards available → Use 3 FWD (1-3-X-3 or 1-4-X-3)
- 2 elite forwards → Use 2 FWD (1-4-4-2 or 1-5-3-2)
- 1 elite forward → Use 1 FWD (1-5-4-1 or 1-4-5-1)

### Question 2: Are there clean sheet opportunities?
- Yes, 2+ teams expected to keep clean sheets → Lean toward 4-5 DEF
- Yes, 1 team expected to keep clean sheet → 3-4 DEF (include their defenders)
- No clear clean sheet candidates → Minimize DEF to 3, maximize MID/FWD

### Question 3: Are there elite midfielders available?
- Many strong attacking midfielders → Use 5 MID formation
- Few standout midfielders → Use 3 MID formation

## Recommended Formations by Scenario

### Scenario: One dominant favorite, one clear underdog
**Formation: 1-4-3-3**
- GK: From the favorite team (clean sheet likely)
- 4 DEF: 2-3 from favorite (clean sheet), 1-2 from other matches
- 3 MID: Attacking midfielders from favorite + other strong teams
- 3 FWD: Star forwards, prioritize the favorite's attackers

### Scenario: Multiple competitive matches, no clear blowout
**Formation: 1-3-5-3**
- GK: From the team most likely to face (and stop) shots
- 3 DEF: Best clean sheet candidates
- 5 MID: Maximum attacking midfielder exposure for goals/assists
- 3 FWD: Best goal-scoring opportunities

### Scenario: Heavy favorites in multiple matches
**Formation: 1-5-3-2 or 1-5-4-1**
- GK: From a heavy favorite
- 5 DEF: Stack defenders from favorites for clean sheet points
- 3-4 MID: Pick the best attackers
- 1-2 FWD: The absolute best forward(s)

## Key Principles

0. "Elite forward" means an elite player on the FAVORED side of a match. An underdog's striker is never elite for fantasy purposes regardless of reputation — if you can't find 2-3 elite forwards from favored teams, drop to a 1-2 FWD formation rather than filling FWD slots with underdog players (our warmup Iceland/Congo forwards returned 4 and 2 points).
1. Never pick a formation just because it sounds good — pick the one that puts your highest-expected-value players on the sheet.
2. The formation constraint (3-5 DEF, 3-5 MID, 1-3 FWD) is a binding constraint, not a suggestion. Always validate before submitting.
3. If forced to choose between an extra defender for clean sheet points vs an extra midfielder for goal/assist potential, favor the midfielder — goals (+6) and assists (+4) have higher expected value than clean sheets (+4) since clean sheets require the whole team to not concede.
