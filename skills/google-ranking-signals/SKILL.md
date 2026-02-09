---
name: Google Ranking Signals
description: >
  This skill should be used when the user asks to "audit a page for SEO",
  "analyze ranking signals", "check Google ranking factors", "optimize content for Google",
  "explain how Google ranks pages", "analyze the Google leak", "check NavBoost signals",
  "evaluate site authority", "improve E-E-A-T", "understand why a page ranks",
  or requests any SEO analysis grounded in Google's internal ranking systems.
  Provides comprehensive expertise based on the 2024 Google Content Warehouse API leak
  (2,596 modules, 14,014 attributes).
version: 0.1.0
---

# Google Ranking Signals — 2024 Leak Intelligence

## Overview

In May 2024, internal documentation from Google's Content Warehouse API was publicly leaked, revealing 2,596 modules and 14,014 attributes that describe how Google evaluates, scores, and ranks web pages. This was the largest confirmed leak of Google's ranking system internals in the company's history.

The leak contradicted several of Google's long-standing public statements:

| Google's Public Claim | What the Leak Revealed |
|---|---|
| "We don't use domain authority" | `siteAuthority` attribute exists and is used |
| "Chrome data isn't used for ranking" | NavBoost system ingests Chrome click data extensively |
| "Click data is too noisy to use" | NavBoost is one of the most powerful re-ranking signals |
| "We don't sandbox new sites" | `hostAge` attribute confirms new-domain sandboxing |
| "PageRank is outdated" | PageRank is still computed for every indexed document |

**Important caveats:** The leak shows what attributes exist, not how heavily each is weighted. Some features may be experimental or deprecated. Treat signals as confirmed factors, but impact levels as informed estimates.

## Core Signal Categories

| Category | Primary Modules | Key Fields | Reference |
|---|---|---|---|
| NavBoost (Clicks) | `QualityNavboostCrapsCrapsClickSignals` (10), `QualityNavboostCrapsCrapsData` (15+) | goodClicks, badClicks, lastLongestClicks, unicornClicks | `references/navboost-signals.md` |
| Site Authority | `QualityNsrNsrData` (58), `PerDocData` (230+) | siteAuthority, nsr, pagerankNs, hostAge, chromeInTotal | `references/site-authority-signals.md` |
| Content Quality | `QualityNsrPQData` (19), `QualityTimebasedSyntacticDate` (14) | contentEffort, chard, tofu, OriginalContentScore, bylineDate | `references/content-quality-signals.md` |
| Author & Entity | `RepositoryWebrefEntityJoin` (12), `PerDocData` | annotatedEntityId, authorObfuscatedGaiaStr, topicalityScore | `references/author-entity-signals.md` |
| Link Signals | `IndexingDocjoinerAnchorStatistics` (53), `AnchorsAnchor` (38), `AnchorsAnchorSource` (25) | penguinPenalty, uniqueDomainCount, homePageInfo, pagerankWeight | `references/link-signals.md` |
| Chrome & UX | `QualityNsrNsrData`, NavBoost integration | chromeInTotal, directFrac, pnav, voterTokenCount | `references/chrome-ux-signals.md` |
| Special Treatments | `CompressedQualitySignals` (38) | pandaDemotion, scamness, unauthoritativeScore, productReview* | `references/special-treatments.md` |
| Ranking Architecture | `CompositeDoc` (44), `CompressedQualitySignals`, `GDocumentBase` (32) | scaledSelectionTierRank, crapsNew*Signals | `references/ranking-architecture.md` |

For attribute-level lookup, consult `references/signal-catalog.md` — an alphabetical index of 150+ confirmed attributes with module paths, data types, and impact levels.

## Workflow: Page Audit

To audit a page against the leaked ranking signals:

1. **Gather page data** — Fetch the target URL. Read its HTML, content, metadata, and structure. Note the title tag, headings, author byline, publish date, word count, and internal/external links.

2. **Evaluate by signal category** — Walk through each category in the table above. For each, consult the corresponding reference file and check the page against the documented attributes. Key areas to assess:
   - Click-worthiness (title, meta description, SERP appearance)
   - Content depth and freshness (token count, unique vocabulary, dates)
   - Author/entity signals (byline, credentials, brand mentions)
   - Link profile (diversity, quality, freshness)
   - UX indicators (page speed, layout, engagement potential)

3. **Identify signal gaps** — Flag categories where the page is weak or missing signals entirely. Prioritize gaps in high-impact categories (NavBoost, Content Quality, Links).

4. **Prioritize improvements** — Rank recommendations by estimated impact and implementation effort. Quick wins first, structural changes second.

5. **Deliver actionable recommendations** — Cite specific attributes from the leak to ground each recommendation. Reference the detailed guide in `examples/page-audit-workflow.md`.

## Workflow: Content Optimization

To optimize content based on leak insights:

1. **Analyze content signals** — Check token count, vocabulary diversity (unique tokens vs total), title-query alignment (titlematchScore), and freshness indicators (BylineDate, SyntacticDate).

2. **Evaluate author/entity presence** — Verify author byline exists, credentials are discoverable, and entity associations are clear. Check for brand mentions across the web.

3. **Assess freshness** — Determine if the query triggers QDF. Review publish dates, last-modified signals, and content update history.

4. **Optimize for engagement** — Improve elements that drive goodClicks and lastLongestClicks: compelling titles, strong introductions, comprehensive coverage that satisfies search intent.

5. **Apply the structured checklist** in `examples/content-optimization-checklist.md` for a signal-by-signal walkthrough.

## Workflow: Answering Ranking Questions

To answer specific questions about how Google's ranking system works:

1. **Search the signal catalog** — Consult `references/signal-catalog.md` for the relevant attribute(s). Each entry includes the module path, data type, impact level, and cross-references.

2. **Read the category reference** — For deeper context, read the corresponding category file from `references/`.

3. **Cite specific attributes** — Ground every answer in named attributes and modules from the leak. Distinguish between confirmed attributes and inferred behavior.

4. **Note contradictions** — When the answer contradicts Google's public statements, call this out explicitly with the evidence from the leak.

## Additional Resources

### Reference Files

Detailed technical documentation organized by signal category:

- **`references/navboost-signals.md`** — Click data, pogo-sticking, 13-month window, Chrome integration
- **`references/site-authority-signals.md`** — Domain trust, age, sandbox, PageRank
- **`references/content-quality-signals.md`** — Freshness, tokens, title matching, version history
- **`references/author-entity-signals.md`** — E-E-A-T, author scoring, entity recognition
- **`references/link-signals.md`** — Backlink quality tiers, diversity, freshness, anchor text
- **`references/chrome-ux-signals.md`** — Engagement metrics, session data, scroll depth
- **`references/special-treatments.md`** — Whitelists, authority flags, YMYL treatments
- **`references/ranking-architecture.md`** — Ascorer, Twiddlers, re-ranking pipeline
- **`references/signal-catalog.md`** — Alphabetical index of 150+ attributes (use for quick lookup)

### Example Files

Practical workflows and templates:

- **`examples/page-audit-workflow.md`** — Complete audit walkthrough with sample report format
- **`examples/content-optimization-checklist.md`** — Signal-by-signal optimization checklist
- **`examples/signal-gap-analysis.md`** — Competitive signal comparison framework
