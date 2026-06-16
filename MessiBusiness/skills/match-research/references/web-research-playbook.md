# Web Research Playbook

Search queries and what to extract from each source in the **tight-core list**. Go deep on these few — do not spray shallow searches across many outlets that just echo the same wire. Cover every fixture on the board (Match Coverage Contract).

> The list was deliberately trimmed to one or two best-in-class sources per job: **odds** (prediction markets), **claim probabilities** (Forebet), and **predicted lineups** (The Guardian + Sports Mole). ESPN, BBC, Sky Sports, Sportskeeda, WhoScored, SofaScore, FBref, Understat, Goal.com, footballpredictions, Reuters and AP were removed — either unreachable from the sandbox or redundant with the core. Fox Sports / CBS Sports are kept only as a backup if a core lineup source is down.

## Prediction Markets — Kalshi (kalshi.com) & Polymarket (polymarket.com)  · CORE: odds / favorite

### Best Search Queries
- `"[Team A] [Team B] Kalshi"` / `"[Team A] [Team B] Polymarket"` — the match win/outcome market
- `"World Cup 2026 Kalshi"` / `"World Cup 2026 winner Polymarket"` — tournament and per-match markets
- Or open the World Cup market pages directly on kalshi.com / polymarket.com

### What the Markets Provide (Ground Truth for Odds)
- **Real-money implied probabilities** — a "Yes" share at $0.62 ≈ 62% chance. The single most trustworthy odds source.
- Per-match win probability for each side → identifies the strongest favorite (the default Green Risk Play target)
- Sometimes total-goals and other outcome markets
- Tournament-winner odds (useful for bracket play)
- Use as the tie-breaker when Forebet and pundits disagree; if a market and Forebet diverge sharply, lower confidence.

## Forebet (www.forebet.com)  · CORE: claim probabilities

### Best Search Queries
- `"[Team A] vs [Team B]" site:forebet.com` — Mathematical prediction
- `"World Cup 2026 predictions" site:forebet.com` — Tournament predictions

### What Forebet Provides (the numbers the risk claims map to)
- **1X2 probabilities** (win/draw/loss percentages from the model)
- **Over/Under 2.5 goals probability** → `match_over_2_5_goals` / `match_2plus_goals`
- **BTTS probability** → `both_teams_score`
- **Correct-score prediction** → `exact_score`
- **Average-goals prediction** — helps assess `match_2plus_goals`
- Injured/suspended players list; weather

## The Guardian (www.theguardian.com)  · CORE: predicted lineups, team news, injuries

### Best Search Queries
- `"[Team A] v [Team B] World Cup 2026" site:theguardian.com` — Note: Guardian often uses "v" not "vs"
- `"[Team name] team news World Cup" site:theguardian.com` — Squad updates / injuries
- `"[Team name] line-up" site:theguardian.com` — Expected lineup

### What The Guardian Provides
- Match previews by experienced football journalists (reliably reachable)
- "Team news" sections with confirmed absences — feeds the fail-closed injury gate
- Predicted starting XIs, key stats, tactical analysis
- A predicted scoreline / favorite for the Risk Play narrative

## Sports Mole (www.sportsmole.co.uk)  · CORE: predicted starting XIs

### Best Search Queries
- `"[Team A] vs [Team B] prediction team news lineups" site:sportsmole.co.uk`
- `"[Team A] vs [Team B] World Cup 2026 preview" site:sportsmole.co.uk`

### What Sports Mole Provides
- A dedicated **predicted starting XI** for each side (the second lineup vote alongside The Guardian)
- Team news and likely formation
- A predicted scoreline

## Fox Sports / CBS Sports  · BACKUP ONLY (use if a core lineup source is down)

### Best Search Queries
- `"[Team A] vs [Team B] World Cup 2026" site:foxsports.com` — US-broadcaster preview, predicted XI
- `"[Team A] vs [Team B] prediction" site:cbssports.com` — US soccer desk preview, predicted XI

### What They Provide
- US-time previews with predicted lineups and team news — a stand-in second source if The Guardian or Sports Mole is unreachable that morning. Do not consult them when the two core lineup sources already agree.

## FIFA (www.fifa.com) & Wikipedia (en.wikipedia.org)  · BRACKET-PLAY ONLY

Not for daily picks. The bracket-play skill uses these for knockout reasoning:
- FIFA — official FIFA World Rankings and confirmed group-stage results
- Wikipedia — head-to-head records and historical knockout-round performance (`"[Team A] [Team B] football" site:en.wikipedia.org`, `"2026 FIFA World Cup Group [X]" site:en.wikipedia.org`)

## Research Priority Order

For daily picks, hit the core in this order and stop once you have the fixture covered:
1. **Kalshi / Polymarket** — odds, strongest-favorite identification (for the Risk Play)
2. **Forebet** — over/under, BTTS, predicted/exact score (claim probabilities)
3. **The Guardian + Sports Mole** — predicted XIs and team news (the two lineup votes)
4. **Fox Sports / CBS Sports** — only if a core lineup source is unreachable

Always cross-reference the two lineup sources before locking a starter, and cross-check the market price against Forebet before staking the Risk Play.
