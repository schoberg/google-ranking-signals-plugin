# Ranking Architecture — CompositeDoc, Ascorer, Twiddlers & Index Tiers

The leak reveals Google's multi-stage ranking architecture through its data structures. `CompositeDoc` (44 fields) is the master container that aggregates all signals for a document. `CompressedQualitySignals` (38 fields) serves as a pre-computed "cheat sheet" for rapid scoring. The pipeline flows from indexing through Ascorer (primary ranking) to Twiddlers (re-ranking) to diversity constraints.

**Source module documentation:** [HexDocs](https://hexdocs.pm/google_api_content_warehouse/0.4.0/api-reference.html)

---

## CompositeDoc — The Master Document Container

`CompositeDoc` (44 fields) is the central data structure that combines all signals for a single document. Every other module feeds into it.

| Field | Type | Description |
|---|---|---|
| `doc` | nested | Base document data (`GDocumentBase`) |
| `perDocData` | nested | **`PerDocData`** — 230+ per-document signals |
| `qualitySignals` | nested | **`CompressedQualitySignals`** — 38 compressed quality signals |
| `anchorStats` | nested | **`IndexingDocjoinerAnchorStatistics`** — 53 link profile signals |
| `contentAnnotations` | nested | Content analysis and entity annotations |
| `entityAnnotations` | nested | **`RepositoryWebrefEntityJoin`** — Knowledge Graph entities |
| `dateAnnotations` | nested | **`QualityTimebasedSyntacticDate`** — date/freshness signals |
| `navboostData` | nested | NavBoost click signals |
| `nsrData` | nested | **`QualityNsrNsrData`** — 58 site quality signals |
| `pqData` | nested | **`QualityNsrPQData`** — 19 page quality signals |
| `richsnippet` | nested | Structured data and rich snippet eligibility |
| `spamBrainData` | nested | AI spam detection results |
| `urlHistory` | nested | URL version tracking data |
| `robotsInfo` | nested | Robots directives and crawl permissions |
| `language` | String.t | Document language |
| `country` | String.t | Document geographic association |

### How CompositeDoc Reveals the Architecture

CompositeDoc shows exactly which signal modules are combined for each document:
1. **GDocumentBase** — URL, DocId, canonical, robot directives
2. **PerDocData** — The 230+ field master signal module (PageRank, spam, freshness, content)
3. **CompressedQualitySignals** — Pre-computed quality "cheat sheet" (38 fields)
4. **IndexingDocjoinerAnchorStatistics** — Complete link profile (53 fields)
5. **QualityNsrNsrData** — Site-level quality (58 fields)
6. **QualityNsrPQData** — Page-level quality (19 fields)
7. **Entity/Date/NavBoost/Spam** — Specialized signal modules

This reveals the full signal stack available to the ranking algorithm for every document.

---

## GDocumentBase — Foundation Document Fields

The base document data (32 fields) that anchors every indexed page:

| Field | Type | Description |
|---|---|---|
| `URL` | String.t | The indexed URL |
| `DocId` | integer() | Internal document identifier |
| `canonicalUrl` | String.t | The canonical URL (if different from indexed URL) |
| `pagerank` | number() | Base PageRank value |
| `crawlTime` | integer() | Last crawl timestamp |
| `contentType` | String.t | Document content type (HTML, PDF, etc.) |
| `robotsMeta` | integer() | Robots meta tag directives |
| `noindexReason` | integer() | Why the page is noindexed (if applicable) |
| `redirectInfo` | nested | Redirect chain information |
| `displayUrl` | String.t | URL shown in search results |

---

## CompressedQualitySignals — The Scoring Cheat Sheet

The `CompressedQualitySignals` module (38 fields) is a critical architectural revelation. It contains **pre-computed, compressed** versions of quality signals from multiple source modules, stored for rapid access during ranking.

### Why Compression Matters

During ranking, Google evaluates millions of candidate pages per query. Reading full signal modules (230+ fields in PerDocData alone) for every candidate would be too slow. Instead:

1. **At index time**: Full signals are computed and stored in their source modules
2. **Compression**: Key signals are compressed into `CompressedQualitySignals` — floats become 10-bit integers, complex scores become simple flags
3. **At query time**: The compressed signals are used for rapid preliminary scoring in the Mustang system
4. **Full evaluation**: Only top candidates get full signal evaluation

### Signal Compression Examples

| Original Signal | Compressed Version | Compression |
|---|---|---|
| `QualityNsrNsrData.siteAuthority` (number) | `CompressedQualitySignals.siteAuthority` (integer) | Float → integer |
| NavBoost click data (multiple floats) | `crapsNewUrlSignals` (String.t) | Multiple fields → compressed string |
| Quality demotion scores | `pandaDemotion`, `babyPandaDemotion`, etc. (integers) | Pre-computed flags |
| Spam scores (0-1 floats) | `scamness`, `unauthoritativeScore` (0-1023 integers) | Float → 10-bit int |

### Complete CompressedQualitySignals Field List

Organized by function:

**Authority & Quality:**
- `siteAuthority` (integer) — Compressed site authority
- `authorityPromotion` (integer) — Positive authority boost
- `unauthoritativeScore` (integer, 0-1023) — Authority deficit penalty
- `lowQuality` (integer) — Low quality flag
- `nsrConfidence` (integer) — NSR calculation confidence

**Demotions:**
- `pandaDemotion` (integer) — Panda content quality penalty
- `babyPandaDemotion` (integer) — Lighter Panda penalty
- `babyPandaV2Demotion` (integer) — Updated lighter Panda
- `exactMatchDomainDemotion` (integer) — EMD penalty
- `anchorMismatchDemotion` (integer) — Anchor-content mismatch
- `navDemotion` (integer) — Doorway/navigation page penalty
- `serpDemotion` (integer) — SERP behavioral demotion
- `scamness` (integer, 0-1023) — Scam likelihood

**NavBoost (CRAPS):**
- `crapsNewUrlSignals` (String.t) — Compressed URL-level click data
- `crapsNewHostSignals` (String.t) — Compressed host-level click data
- `crapsNewPatternSignals` (String.t) — Compressed pattern-level click data
- `crapsAbsoluteHostSignals` (integer) — Absolute host-level signals
- `crapsUnscaledIpPriorBadFraction` (integer) — IP-based spam fraction

**Product Reviews:**
- `productReviewPPromotePage` (integer) — Review page promotion
- `productReviewPDemotePage` (integer) — Review page demotion
- `productReviewPPromoteSite` (integer) — Review site promotion
- `productReviewPDemoteSite` (integer) — Review site demotion
- `productReviewPUhqPage` (integer) — Ultra-high-quality review
- `productReviewPReviewPage` (integer) — Review page classification

**Content Quality:**
- `ugcDiscussionEffortScore` (integer) — UGC/forum content quality

---

## The Ranking Pipeline

```
Crawl → Index → [CompositeDoc assembled] → Retrieval → Ascorer → Twiddlers → Diversity → Final SERP
```

### Stage 1: Crawl & Index

- Googlebot crawls pages and stores content
- Content is parsed; attributes are extracted into module fields
- Entity recognition, date extraction, quality flags are computed
- Links are processed; link graph is updated
- `CompositeDoc` is assembled with all signal modules
- `CompressedQualitySignals` is generated from source modules
- Version history is updated (last 20 changes tracked)

### Stage 2: Retrieval

- For a given query, candidate pages are retrieved from the index
- `scaledSelectionTierRank` determines which index tier the page is in
- Hundreds to thousands of candidates are selected
- Initial filtering removes obviously irrelevant results

### Stage 3: Ascorer (Primary Ranking)

The main ranking algorithm. Evaluates pages using signals from `CompositeDoc`:

**Per-document signals** (from CompositeDoc, computed at index time):
- PageRank variants from `PerDocData`
- Content quality from `QualityNsrPQData`
- Site quality from `QualityNsrNsrData`
- Entity signals from `RepositoryWebrefEntityJoin`
- Link profile from `IndexingDocjoinerAnchorStatistics`

**Per-query signals** (computed at query time):
- `titlematchScore` — title-query alignment
- Relevance scores — content-query matching
- QDF activation — freshness boost eligibility
- Query classification — informational/navigational/transactional

### Stage 4: Twiddlers (Re-ranking)

Post-Ascorer functions that re-order results. They use `CompressedQualitySignals` for rapid access:

**NavBoost Twiddler** — Most powerful re-ranker
- Uses `crapsNewUrlSignals`, `crapsNewHostSignals`, `crapsNewPatternSignals`
- Aggregated click data over 13-month rolling window
- Can significantly override Ascorer rankings
- Details: See `navboost-signals.md`

**QualityBoost Twiddler** — Content quality adjustments
- Uses `pandaDemotion`, `babyPandaDemotion`, `lowQuality`
- Uses `authorityPromotion`, `unauthoritativeScore`
- Can demote low-quality sites even with relevant content
- Can boost high-quality sites for competitive queries

**RealTimeBoost Twiddler** — Freshness adjustments
- Activated when query triggers QDF
- Uses `isHotdoc` and date signals
- Provides temporary ranking boost for fresh content
- Boost decays as content ages

**Other Twiddlers:**
- **LocalBoost**: Location-specific query adjustments
- **PersonalizationTwiddler**: Limited user-specific adjustments
- **SafeSearchTwiddler**: Content safety filtering
- **HostDiversityTwiddler**: Limits results from a single domain

### Stage 5: Diversity Constraints

Final adjustments to ensure result variety:
- **Host diversity**: Limits results from a single domain (typically 2-3 per SERP page)
- **Content type diversity**: Mixes articles, videos, images, tools
- **Source diversity**: Includes different source types (news, blogs, forums)
- **Intent diversity**: For ambiguous queries, serves results for multiple intents

### Stage 6: SERP Generation

Results are formatted with SERP features based on eligibility signals from `CompositeDoc`.

---

## Index Tiers — scaledSelectionTierRank

The leak reveals Google maintains multiple index tiers:

| Tier | Name | Description |
|---|---|---|
| Tier 0 | **Base** | Standard index — most pages |
| Tier 1 | **Zeppelins** | High-quality/high-authority pages (priority retrieval) |
| Tier 2 | **Landfills** | Low-quality pages (deprioritized retrieval) |

### How Tiers Work

- `scaledSelectionTierRank` in `PerDocData` determines the page's tier
- **Zeppelins** tier pages are retrieved first and given priority in candidate selection
- **Landfills** tier pages are retrieved last and may not be considered for competitive queries
- Tier assignment is based on a combination of quality signals, authority, and content assessment

### Implications

A page in the Landfills tier faces multiple disadvantages:
1. May not be retrieved as a candidate for competitive queries
2. Even if retrieved, starts with a quality disadvantage
3. Must overcome both retrieval bias and ranking signal deficits

Moving from Landfills to Base (or Base to Zeppelins) requires sustained quality improvement across multiple signal categories.

---

## Query Classification

Google classifies queries to activate different signal weights and Twiddlers:

| Query Type | Primary Signals Weighted | Twiddlers Activated |
|---|---|---|
| Informational | Content quality, E-E-A-T, comprehensiveness | QualityBoost |
| Navigational | Brand/entity, exact match, direct traffic | NavBoost (heavily) |
| Transactional | Product signals, commercial intent, reviews | QualityBoost + product review signals |
| Local | Location, GMB, local authority | LocalBoost |
| YMYL | Trust, authority, credentials (amplified) | QualityBoost (strict mode) |
| Time-sensitive | Freshness, recency, isHotdoc | RealTimeBoost |

---

## Why Rankings Fluctuate

The multi-stage architecture explains common fluctuations:

- **Twiddler weight updates**: Google adjusts Twiddler thresholds regularly — NavBoost or QualityBoost recalibration shifts rankings without any SEO changes
- **NavBoost data refresh**: 13-month rolling window means click patterns continuously update
- **Index tier changes**: Quality reassessment can move pages between tiers
- **Query reclassification**: Google's understanding of query intent may shift, changing which signals are weighted
- **Diversity changes**: New content entering the SERP displaces existing results
- **CompressedQualitySignals recalculation**: Periodic compression updates change the "cheat sheet"

---

## Diagnosing Ranking Issues

Using the architecture:

| Symptom | Likely Cause | Check |
|---|---|---|
| Good content, poor rankings | NavBoost signals weak | Click metrics, engagement data |
| Good links, poor rankings | Content quality / QualityBoost | Panda demotions, NSR, site quality |
| Good everything, volatile | Twiddler updates or NavBoost shifts | Monitor over weeks, not days |
| Good everything, stuck | Index tier (Landfills) | Site-wide quality, authority signals |
| Rankings for some queries only | Query classification mismatch | Content format vs. query intent |
| Sudden drop, no changes | Demotion triggered | Check all CompressedQualitySignals demotions |

---

## Audit Checklist for Ranking Architecture

- [ ] **CompositeDoc completeness**: Page has signals present across all major modules
- [ ] **No demotion flags**: Clean CompressedQualitySignals (no Panda, EMD, nav, anchor demotions)
- [ ] **authorityPromotion active**: Positive authority boost present
- [ ] **Index tier**: Not in Landfills (check for site-wide quality issues)
- [ ] **NavBoost health**: Sufficient click data, positive goodClicks/badClicks ratio
- [ ] **Multi-stage optimization**: Strong signals at Ascorer level AND Twiddler level
- [ ] **Query-signal alignment**: Content format matches the target query type
- [ ] **Diversity surviving**: Unique value proposition that differentiates from same-domain and competitor results
- [ ] **CompressedQualitySignals**: Favorable compressed signals for rapid scoring
