# Team Strategy Overview

This package guides the AI agent through a structured, research-driven approach to the Fantasy World Cup. Every decision is grounded in live web research from prediction sites, statistical analysis, and adaptive risk management.

## Philosophy

1. **Research First** — Before picking a single player, research every match using prediction websites, statistical platforms, and sports news. The best picks come from confirmed lineup news and expert predictions, not guesswork.

2. **Starters First** — The single biggest source of reliable points is picking players who actually start and play 60+ minutes (+4 guaranteed points each). We prioritize confirmed/expected starters above all else.

3. **Asymmetric Upside** — After locking in starters, tilt toward attacking players from dominant teams (goals +6, assists +4) and defenders from heavy favorites (clean sheets +4).

4. **Adaptive Risk** — Risk play strategy adapts to standings position. Protect leads with Green claims. Catch up with Yellow/Red claims backed by prediction site data.

## Web Research Domains

The agent should search these sources for match intelligence. Use whichever domains are accessible:

### Tier 1: Sports News with Predictions (High Priority)
- **www.espn.com** — Match previews, predicted lineups, expert analysis, World Cup predictor
- **www.bbc.com** — BBC Sport match previews, team news, tactical analysis
- **www.skysports.com** — Sky Sports match previews, predicted lineups, expert tips

### Tier 2: Prediction & Statistics Platforms
- **www.forebet.com** — Mathematical predictions with probabilities for 1X2, over/under, BTTS, correct score
- **www.sportskeeda.com** — Match predictions, predicted lineups, player form analysis
- **www.whoscored.com** — Player ratings, team statistics, tactical analysis, predicted lineups
- **www.sofascore.com** — Real-time ratings, form, lineups, head-to-head stats
- **fbref.com** — Deep statistics powered by StatsBomb, xG data, player performance
- **understat.com** — Expected goals (xG), shot maps, team xG trends
- **footballpredictions.com** — World Cup-specific match predictions and tips

### Tier 3: News and Wire Services
- **www.reuters.com** — Breaking team news, injury updates
- **apnews.com** — AP match previews and team news
- **www.fifa.com** — Official match info, squad announcements
- **inside.fifa.com** — Official FIFA tournament data

### Tier 4: Encyclopedic Reference
- **en.wikipedia.org** — Historical head-to-head records, tournament history, team profiles

When a domain is unreachable, skip it and rely on available sources. The skills are designed to work with any subset of these domains.

## Skills

| Skill | Purpose | Priority |
|-------|---------|----------|
| `match-research` | Deep research on every match using prediction sites and stats platforms | Run first |
| `pick-fantasy-xi` | Select 11 players using research findings, lineup data, and scoring optimization | Run second |
| `choose-risk-play` | Select optimal risk claim using prediction data and standings-based risk management | Run third |
| `bracket-play` | Handle knockout bracket predictions when bracket play opens | Run when available |

## Execution Order

1. Run `match-research` first — gather prediction data, expected lineups, and match analysis for every fixture.
2. Run `pick-fantasy-xi` second — use the research to select 11 optimal players.
3. Run `choose-risk-play` third — use prediction site probabilities and standings to select a risk claim.
4. Run `bracket-play` only when bracket data is present in the workspace.
5. Combine all outputs into the final JSON submission matching the schema.
