# Google Ranking Signals — Claude Code Plugin

A Claude Code plugin that provides SEO analysis grounded in the 2024 Google Content Warehouse API leak (2,596 modules, 14,014 attributes).

## What it does

This plugin gives Claude Code deep knowledge of Google's actual ranking signals — not what Google says publicly, but what the leaked internal documentation reveals. Use it to:

- **Audit a page** against confirmed ranking signals
- **Optimize content** using signal-by-signal checklists
- **Analyze ranking factors** with specific attribute citations
- **Compare competitors** using signal gap analysis
- **Answer SEO questions** grounded in leaked documentation

## Signal categories covered

| Category | What's included |
|---|---|
| NavBoost (Clicks) | Click quality signals, pogo-sticking, 13-month data window |
| Site Authority | Domain trust, age, sandbox detection, PageRank |
| Content Quality | Freshness, token analysis, title matching, version history |
| Author & Entity | E-E-A-T signals, author scoring, entity recognition |
| Link Signals | Backlink quality tiers, diversity, anchor text analysis |
| Chrome & UX | Engagement metrics, session data, scroll depth |
| Special Treatments | Whitelists, YMYL treatments, penalty signals |
| Ranking Architecture | Ascorer, Twiddlers, re-ranking pipeline |

## Installation

```bash
claude plugin add schoberg/google-ranking-signals-plugin
```

## Usage

Once installed, Claude Code will automatically use this skill when you ask things like:

- "Audit this page for SEO"
- "Analyze the ranking signals for example.com"
- "Check NavBoost signals"
- "How does Google actually rank pages?"
- "Optimize this content for Google"
- "Evaluate site authority for this domain"

## License

MIT
