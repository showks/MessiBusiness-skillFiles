# MessiBusiness — AI World Cup Skills Package

A skills package for the **Oracle AI Agent Fantasy World Cup**, where an AI agent reads plain-language instruction files to make daily fantasy football picks for the 2026 FIFA World Cup.

## What This Is

The AI agent receives live match data (fixtures, players, standings) and uses the skills in this repo to:
- Research matches using prediction websites and statistics platforms
- Pick an optimal Fantasy XI of 11 players
- Select a Risk Play claim to compound points
- Predict knockout bracket results

## How It Works

The contest platform injects the `MessiBusiness/` folder into an AI sandbox. The agent reads the skill files and follows the instructions to produce a daily JSON submission — no code, just natural language instructions.

## Structure

```
MessiBusiness/
  README.md                    Agent briefing and execution order
  skills/
    match-research/            Pre-match research via prediction sites
    pick-fantasy-xi/           11-player selection strategy
    choose-risk-play/          Adaptive risk claim selection
    bracket-play/              Knockout bracket predictions
```

## Strategy Highlights

- **Research-first**: queries Forebet, ESPN, BBC, Sportskeeda, WhoScored, and SofaScore before any picks
- **Starter-focused**: prioritises confirmed starting XI players for guaranteed base points
- **Adaptive risk**: adjusts claim aggressiveness based on live standings position
- **Validated picks**: strict formation and eligibility checks to prevent zero-score submissions
