# Content Quality Signals — Page Quality, Freshness, Tokens & Dates

Content quality in Google's ranking system is evaluated through multiple dedicated modules. The `QualityNsrPQData` module (19 fields) provides page-level quality scores including Google's internal code-named predictors. `QualityTimebasedSyntacticDate` (14 fields) handles date extraction and freshness. `PerDocData` contributes content-level signals like token analysis, originality, and spam detection.

**Source module documentation:** [HexDocs](https://hexdocs.pm/google_api_content_warehouse/0.4.0/api-reference.html)

---

## QualityNsrPQData — Page-Level Quality Scores

The dedicated page quality module. Contains Google's internal quality predictors, many named with food code names.

| Field | Type | Description |
|---|---|---|
| `chard` | number() | **Page quality predictor** — internal code name, likely content depth/quality |
| `contentEffort` | number() | **Content effort score** — measures investment in content creation |
| `deltaAutopilotScore` | number() | Page deviation from site-level autopilot score |
| `deltaPageQuality` | number() | Page deviation from site-level page quality |
| `keto` | number() | **Page quality predictor** — internal code name |
| `linkIncoming` | number() | Incoming link quality signal for the page |
| `linkOutgoing` | number() | Outgoing link quality signal for the page |
| `numOffdomainAnchors` | integer() | Count of off-domain anchors pointing to the page |
| `page2vecLq` | number() | Page2Vec low-quality score (embedding-based quality) |
| `rhubarb` | number() | **Page quality predictor** — internal code name |
| `tofu` | number() | **Page quality predictor** — internal code name, used in both PQData and NSR |
| `urlAutopilotScore` | number() | URL-level autopilot quality score |
| `vlq` | number() | **Very Low Quality** score at page level |
| `subchunkData` | list(nested) | Quality data broken down by page sub-sections |

### Internal Code Names — CHARD, KETO, RHUBARB, TOFU

Google uses food-themed code names for quality prediction models:

- **CHARD**: Appears to correlate with content depth and substantive quality. Pages with long-form, well-structured original content score higher.
- **KETO**: Likely a quality dimension related to content leanness/efficiency — how much value per unit of content.
- **RHUBARB**: Another quality predictor; exact dimension unknown but present in the scoring pipeline.
- **TOFU**: Present in both `QualityNsrPQData.tofu` (page-level) and `QualityNsrNsrData.tofu` (site-level). A cross-level quality signal that may assess content substance.

These predictors feed into the overall page quality assessment. Their exact formulas are unknown, but they represent different dimensions of content quality that Google evaluates independently.

### contentEffort — Editorial Investment

Measures the apparent effort invested in creating the content. Higher scores indicate:
- Original research, reporting, or analysis
- Detailed explanations with supporting evidence
- Custom illustrations, charts, or data visualizations
- Structured, well-organized presentation
- Evidence of expert review or editorial process

Lower scores indicate:
- Thin, auto-generated, or template-based content
- Aggregated/scraped content without original value
- Minimal formatting or structure
- Stock content with no unique perspective

### vlq — Very Low Quality Flag

A page-level quality floor. High `vlq` scores mark content as very low quality, which can:
- Prevent the page from ranking for competitive queries
- Trigger additional quality filters in the ranking pipeline
- Be influenced by site-level `vlqNsr` from `QualityNsrNsrData`

### page2vecLq — Embedding-Based Quality

Uses Page2Vec embeddings (vector representations of page content) to assess quality. This is a machine-learned signal that compares the page's content embedding against known low-quality patterns.

---

## QualityTimebasedSyntacticDate — Date & Freshness Extraction

Google's date extraction module with 14 fields for parsing, validating, and scoring content dates.

| Field | Type | Description |
|---|---|---|
| `bylineDate` | String.t | Date extracted from the author byline |
| `syntacticDate` | String.t | Date parsed from page syntax (URL, meta, structured data) |
| `useAsBylineDate` | boolean() | Whether the syntactic date should be treated as byline date |
| `trustSyntacticDateInRanking` | boolean() | **Whether the extracted date is trusted for ranking** |
| `bylineDateConfidence` | number() | Confidence score for byline date extraction |
| `syntacticDateConfidence` | number() | Confidence score for syntactic date extraction |
| `contentAge` | integer() | Computed content age in days |
| `lastSignificantUpdate` | String.t | Date of last substantive content change |
| `firstSeen` | String.t | Date page was first indexed |
| `dateReliability` | number() | Overall date reliability score (0-1) |
| `dateSource` | integer() | Which source the date was extracted from |
| `semanticDate` | String.t | Date derived from content semantics |
| `timeSensitivity` | number() | How time-sensitive the content is |
| `isHotdoc` | boolean() | Trending/hot document flag |

### trustSyntacticDateInRanking — Date Trust

Critical boolean that determines whether Google uses the detected date for freshness ranking. When `false`, the page receives no freshness benefit regardless of its stated date.

Trust drops when:
- Multiple conflicting dates appear on the page
- URL date doesn't match structured data date
- Date appears to have been changed without content changes
- Historical crawl data contradicts the stated date
- The date format is ambiguous or unparseable

### lastSignificantUpdate — Meaningful Changes

Google distinguishes between trivial and significant content updates. Only substantial changes update this field. Cosmetic changes (updating a copyright year, changing a sidebar widget) don't qualify.

Significant updates include:
- Adding or rewriting substantial content sections
- Updating facts, statistics, or outdated information
- Adding new research, examples, or analysis
- Restructuring content for improved coverage

### isHotdoc — Trending Content

Boolean flag for content that's currently trending. When true, the document may receive temporary ranking boosts through the RealTimeBoost Twiddler. Related to QDF (Query Deserves Freshness) activation at the query level.

### dateSource — Where Google Finds Dates

Google checks dates from multiple sources and cross-validates:
1. URL path patterns (e.g., `/2024/05/article-title`)
2. JSON-LD structured data (`datePublished`, `dateModified`)
3. Open Graph meta tags (`article:published_time`)
4. Visible byline date on the page
5. HTTP headers (Last-Modified)
6. Sitemap `<lastmod>` values

**Critical:** Conflicting dates across sources reduce `dateReliability`, which can negate freshness benefits entirely.

---

## PerDocData — Content-Level Fields

The master per-document module contributes content quality signals:

| Field | Type | Description |
|---|---|---|
| `bodyWordsToTokensRatio` | number() | **Vocabulary diversity** — ratio of unique words to total tokens |
| `titleHardTokenCount` | integer() | Token count in the title |
| `OriginalContentScore` | number() | **Content originality** — unique vs. duplicated/scraped |
| `KeywordStuffingScore` | integer() | Keyword over-optimization detection |
| `GibberishScore` | integer() | Non-sensical/auto-generated content detection |
| `commercialScore` | number() | Commercial intent level of the content |
| `ymylHealthScore` | number() | YMYL health topic classification |
| `ymylNewsScore` | number() | YMYL news topic classification |
| `TagPageScore` | number() | Tag/category page detection (often thin content) |
| `shingleInfo` | nested | Content fingerprint for near-duplicate detection |
| `contentLength` | integer() | Raw document content length |
| `numUrls` | integer() | Number of URLs/links in the document |

### bodyWordsToTokensRatio — Vocabulary Diversity

One of the most actionable content signals. Measures the ratio of unique words to total tokens in the page body.

- **High ratio** (diverse vocabulary): Indicates substantive, well-written content with varied expression
- **Low ratio** (repetitive vocabulary): May indicate keyword stuffing, shallow content, or templated pages
- **Practical target**: Natural, well-written content on a topic typically produces a healthy ratio without deliberate optimization

### OriginalContentScore — Content Uniqueness

Measures how much of the page content is original vs. duplicated from other sources. Evaluated through:
- **Shingle comparison**: Content fingerprints (`shingleInfo`) compared against the index
- **Near-duplicate detection**: Pages with substantially similar content clusters
- **Syndication detection**: Identifying republished content from other sources

Boosting originality:
- Conduct and publish original research or data
- Provide first-person experience and unique perspectives
- Create original visualizations, charts, and media
- Offer novel analysis rather than restating known information

### KeywordStuffingScore — Over-Optimization Detection

Algorithmic detection of unnatural keyword repetition. Modern detection goes beyond simple density:
- Measures unnatural repetition patterns across the document
- Evaluates keyword distribution (clustered vs. spread)
- Detects synonym-stuffing and entity-stuffing variants
- Identifies artificially inserted keyword variations in headings, alt text, and metadata

### GibberishScore — Content Coherence

Detects non-sensical or auto-generated content. Triggered by:
- Machine-generated text without human editing (pre-ChatGPT era detection)
- Spun/article-spinner content
- Markov chain or template-based content generation
- Content in incorrect or mixed languages

---

## QualityNsrNsrData — Site-Level Content Fields

Several `QualityNsrNsrData` fields relate to content quality at the site level:

| Field | Type | Description |
|---|---|---|
| `articleScore` | number() | Article content quality score |
| `articleScoreV2` | number() | Updated article quality scoring model |
| `titlematchScore` | number() | Site-level title-query matching quality |
| `videoScore` | number() | Video content quality assessment |
| `shoppingScore` | number() | Shopping/e-commerce content quality |
| `ugcScore` | number() | User-generated content quality |
| `clutterScore` | number() | Page clutter/ad density assessment |
| `tofu` | number() | Cross-level quality predictor (see PQData section) |
| `vlqNsr` | number() | Site-level Very Low Quality score |

### articleScore / articleScoreV2 — Article Quality

Site-level assessment of article content quality. V2 is the updated model. These influence how the site's content is perceived across all pages:
- Sites with consistently high article scores get a quality floor boost
- Sites with low scores face higher hurdles for individual pages to rank

### clutterScore — Ad and Clutter Density

Measures how cluttered the page/site is with ads, pop-ups, and non-content elements. High clutter scores lead to:
- Lower content quality assessments
- Higher badClicks rates (users bounce from cluttered pages)
- Potential QualityBoost demotion

---

## Content Freshness & QDF

### Query Deserves Freshness (QDF)

QDF is a query-level classifier, not a page attribute. When activated:
- Recently published/updated content receives temporary ranking boosts via RealTimeBoost Twiddler
- The `timeSensitivity` field in date signals correlates with QDF activation
- `isHotdoc` pages benefit most when QDF is active

QDF triggers for:
- Breaking news events
- Trending topics and viral content
- Recurring events (elections, sports, annual events)
- Product launches and updates
- Rapidly evolving topics

QDF does NOT trigger for:
- Evergreen informational queries
- Historical queries
- Stable navigational queries

### freshboxArticleScores — Freshness Box

From `PerDocData`, these scores determine eligibility for freshness-related SERP features (Top Stories, fresh results carousel). Content must demonstrate both recency and quality.

---

## CompressedQualitySignals — Content Quality in Scoring

Content quality signals compressed for rapid access during ranking:

| Field | Type | Description |
|---|---|---|
| `lowQuality` | integer() | S2V low quality score from NSR |
| `ugcDiscussionEffortScore` | integer() | Quality assessment for UGC/forum content |
| `productReviewPPromotePage` | integer() | Product review quality promotion (page) |
| `productReviewPDemotePage` | integer() | Product review quality demotion (page) |
| `productReviewPUhqPage` | integer() | Ultra-high-quality product review page flag |
| `productReviewPReviewPage` | integer() | Product review page classification |

### Product Review Signals

Google has dedicated signals for product review content quality:
- **productReviewPPromotePage**: Positive boost for high-quality reviews with evidence of genuine product experience
- **productReviewPDemotePage**: Demotion for thin affiliate reviews without original value
- **productReviewPUhqPage**: Flag for ultra-high-quality reviews (original photos, detailed testing, comparative analysis)

Quality product reviews require:
- Evidence of actual product use (original photos, screenshots)
- Quantitative measurements and testing data
- Comparative context against alternatives
- Both pros and cons with specific details
- Author expertise in the product category

---

## Version History

### PerDocData Version Tracking

| Field | Type | Description |
|---|---|---|
| `urlChangesCount` | integer() | Number of content changes tracked |
| `lastUrlChange` | String.t | Timestamp of most recent content change |

Google stores every version of every indexed page but only uses the **last 20 changes** for link analysis and freshness evaluation.

Implications:
- Regular, meaningful updates are positive signals
- Cosmetic-only updates (changing dates without content changes) are detectable and can backfire
- Each update is compared against the previous version; substantial changes carry more weight
- Link analysis uses version history to detect manipulative link insertion after page aging

---

## Audit Checklist for Content Quality

- [ ] **contentEffort**: Content demonstrates editorial investment (original research, detailed analysis, custom media)
- [ ] **OriginalContentScore**: Majority of content is unique, not scraped or syndicated
- [ ] **bodyWordsToTokensRatio**: Diverse vocabulary, not repetitive or keyword-stuffed
- [ ] **KeywordStuffingScore**: No unnatural keyword repetition patterns
- [ ] **titlematchScore**: Title accurately reflects content and target queries
- [ ] **Date signals**: Publish date visible, consistent across all sources (URL, meta, byline, JSON-LD)
- [ ] **trustSyntacticDateInRanking**: No conflicting date signals that would break trust
- [ ] **lastSignificantUpdate**: Content has been meaningfully updated (not just cosmetic changes)
- [ ] **clutterScore**: Minimal ads, pop-ups, and non-content elements
- [ ] **Product reviews**: If review content, has original photos, testing data, genuine experience evidence
- [ ] **vlq check**: No Very Low Quality flags at page or site level
- [ ] **GibberishScore**: Content is coherent, well-structured, human-quality writing
