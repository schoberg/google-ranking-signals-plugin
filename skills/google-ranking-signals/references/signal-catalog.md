# Signal Catalog — Alphabetical Index of Confirmed Attributes

Comprehensive index of confirmed attributes from the 2024 Google Content Warehouse API leak. Every attribute, module path, and data type below comes from the actual leaked API documentation, mirrored at [HexDocs](https://hexdocs.pm/google_api_content_warehouse/0.4.0/api-reference.html).

**Impact levels:** Critical (top-tier factor), High (significant), Medium (meaningful), Low (minor/situational).

**Caveat:** The leak confirms these attributes exist. Exact weighting is unknown. Some may be experimental or deprecated.

---

## A

### absoluteImpressions
- **Module:** QualityNavboostCrapsCrapsClickSignals
- **Type:** float()
- **Category:** NavBoost
- **Impact:** Medium
- **Description:** Host-level unsquashed impression count. Aggregated across all queries for the host.
- **Optimization:** Build overall site visibility and brand recognition to increase host-level impressions.

### anchorCount
- **Module:** IndexingDocjoinerAnchorStatistics
- **Type:** integer()
- **Category:** Links
- **Impact:** High
- **Description:** Total number of anchors (links) pointing to the document.
- **Optimization:** Build diverse, natural backlinks. See `link-signals.md`.

### anchorMismatchDemotion
- **Module:** CompressedQualitySignals
- **Type:** integer()
- **Category:** Demotions
- **Impact:** High
- **Description:** Demotion from QualityBoost.mismatched.boost — triggered when inbound anchor text doesn't match page content.
- **Optimization:** Ensure inbound links use relevant, topically aligned anchor text.

### anchorSpamInfo
- **Module:** IndexingDocjoinerAnchorStatistics
- **Type:** IndexingDocjoinerAnchorSpamInfo.t
- **Category:** Spam
- **Impact:** High
- **Description:** Detailed anchor spam analysis data. Nested object with spam classification details.
- **Optimization:** Audit and disavow manipulative inbound links.

### articleScore / articleScoreV2
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Quality
- **Impact:** Medium
- **Description:** Article quality classification score at site level. V2 is the updated version.
- **Optimization:** Maintain consistent, high-quality article content across the entire site.

### authorityPromotion
- **Module:** CompressedQualitySignals
- **Type:** integer()
- **Category:** Quality
- **Impact:** High
- **Description:** Positive authority modifier from QualityBoost.authority.boost. Applied in Qstar scoring.
- **Optimization:** Build site authority through quality content, authoritative backlinks, and brand recognition.

### authorObfuscatedGaiaStr
- **Module:** PerDocData
- **Type:** list(String.t)
- **Category:** Author & Entity
- **Impact:** Medium
- **Description:** Obfuscated Google Account IDs linked to the page author. Used for E-E-A-T author identity matching.
- **Optimization:** Consistent author identity across publications strengthens author signals.

## B

### babyPandaDemotion / babyPandaV2Demotion
- **Module:** CompressedQualitySignals
- **Type:** integer()
- **Category:** Demotions
- **Impact:** High
- **Description:** Iterations on the Panda algorithm targeting low-quality content. V2 replaced the original.
- **Optimization:** Remove or substantially improve thin, low-value content site-wide.

### badbacklinksPenalized
- **Module:** IndexingDocjoinerAnchorStatistics
- **Type:** boolean()
- **Category:** Links / Spam
- **Impact:** High
- **Description:** Flag indicating the page has been penalized for bad backlinks.
- **Optimization:** Audit backlinks and disavow toxic links. See `link-signals.md`.

### badClicks
- **Module:** QualityNavboostCrapsCrapsClickSignals
- **Type:** float()
- **Category:** NavBoost
- **Impact:** Critical
- **Description:** Low-quality clicks (pogo-sticking, quick returns to SERP). A high ratio actively demotes.
- **Optimization:** Match content to search intent, improve page speed, deliver value above the fold. See `navboost-signals.md`.

### bodyWordsToTokensRatio
- **Module:** PerDocData
- **Type:** number()
- **Category:** Content Quality
- **Impact:** Medium
- **Description:** Ratio of meaningful words to total tokens. Measures content density vs. boilerplate.
- **Optimization:** Maximize substantive content relative to navigation, ads, and boilerplate.

### bylineDate
- **Module:** QualityTimebasedSyntacticDate
- **Type:** String.t
- **Category:** Freshness
- **Impact:** Medium
- **Description:** Date extracted from the article byline. Google parses bylines to determine publish dates.
- **Optimization:** Include clear, visible publish dates in bylines. See `content-quality-signals.md`.

## C

### chard / chardEncoded / chardVariance
- **Module:** QualityNsrNsrData / QualityNsrPQData
- **Type:** integer() / number()
- **Category:** Quality
- **Impact:** High
- **Description:** Internal quality predictor (code name "CHARD"). Site-level and page-level variants with encoded and variance versions.
- **Optimization:** Maintain consistently high-quality content. The variance field suggests Google also measures quality consistency across the site.

### chromeInTotal
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Chrome & UX
- **Impact:** High
- **Description:** Total Chrome browser visits to the site. Contradicts Google's public denial of using Chrome data.
- **Optimization:** Build brand recognition and direct traffic. See `chrome-ux-signals.md`.

### clicks
- **Module:** QualityNavboostCrapsCrapsClickSignals
- **Type:** float()
- **Category:** NavBoost
- **Impact:** High
- **Description:** Total click count for the URL from search results.
- **Optimization:** Improve CTR through compelling titles and meta descriptions.

### clutterScore / clutterScores
- **Module:** QualityNsrNsrData
- **Type:** number() / list()
- **Category:** Quality
- **Impact:** Medium
- **Description:** Measures page/site clutter (ads, popups, interstitials). Versioned scores available.
- **Optimization:** Minimize intrusive ads, popups, and interstitials. Clean, focused layouts score better.

### commercialScore
- **Module:** PerDocData
- **Type:** number()
- **Category:** Content Quality
- **Impact:** Medium
- **Description:** Commercial intent classification of the document.
- **Optimization:** Align content with appropriate intent signals (informational vs. commercial vs. transactional).

### contentEffort
- **Module:** QualityNsrPQData
- **Type:** list()
- **Category:** Quality
- **Impact:** High
- **Description:** Content effort signal — likely related to the Helpful Content system. Measures how much genuine effort went into creating the content.
- **Optimization:** Create genuinely helpful, well-researched content. Avoid AI-generated or low-effort mass production.

### crapsNewUrlSignals / crapsNewHostSignals / crapsNewPatternSignals
- **Module:** CompressedQualitySignals
- **Type:** String.t / integer()
- **Category:** NavBoost
- **Impact:** Critical
- **Description:** Compressed click/impression data at URL, host, and pattern levels. CRAPS = Click, Rank, And Position Signals.
- **Optimization:** These aggregate NavBoost data at different granularities. See `navboost-signals.md`.

## D

### directFrac
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Chrome & UX
- **Impact:** High
- **Description:** Fraction of site traffic that comes from direct navigation (typed URLs, bookmarks).
- **Optimization:** Build brand awareness and type-in traffic. Higher direct traffic fraction signals brand trust.

### DocLevelSpamScore
- **Module:** PerDocData
- **Type:** integer() (0-127, 7-bit)
- **Category:** Spam
- **Impact:** High
- **Description:** Document-level spam score.
- **Optimization:** Avoid spammy patterns, thin content, and manipulative techniques.

### domainAge
- **Module:** PerDocData
- **Type:** integer() (16-bit)
- **Category:** Site Authority
- **Impact:** Medium
- **Description:** Domain registration age encoded as 16-bit value.
- **Optimization:** Cannot be changed directly. New domains face a sandbox period. See `site-authority-signals.md`.

## E

### exactMatchDomainDemotion
- **Module:** CompressedQualitySignals
- **Type:** integer()
- **Category:** Demotions
- **Impact:** Medium
- **Description:** Demotion from QualityBoost.emd.boost — reduces ranking advantage for exact-match keyword domains.
- **Optimization:** Build real topical authority beyond the domain name.

### experimentalQstarSignal / experimentalQstarDeltaSignal / experimentalQstarSiteSignal
- **Module:** CompressedQualitySignals
- **Type:** number()
- **Category:** Quality
- **Impact:** Unknown
- **Description:** Experimental Q* system signals at page and site level. Q* appears to be a next-generation quality scoring system.
- **Optimization:** Unknown — experimental. Focus on established quality signals.

## F

### freshboxArticleScores
- **Module:** PerDocData
- **Type:** integer()
- **Category:** Freshness
- **Impact:** Medium
- **Description:** Freshbox news/freshness classifier scores. Determines fresh content eligibility.
- **Optimization:** Publish timely content with clear dates for freshness-sensitive queries. See `content-quality-signals.md`.

## G

### GibberishScore
- **Module:** PerDocData
- **Type:** integer() (0-127, 7-bit)
- **Category:** Spam
- **Impact:** High
- **Description:** Nonsensical or auto-generated content detection score.
- **Optimization:** Ensure all content is coherent, well-written, and readable.

### goodClicks
- **Module:** QualityNavboostCrapsCrapsClickSignals
- **Type:** float()
- **Category:** NavBoost
- **Impact:** Critical
- **Description:** High-quality engagement clicks — user found what they were looking for (long dwell, no pogo-sticking).
- **Optimization:** Match content precisely to search intent. Be the page that ends the search. See `navboost-signals.md`.

## H

### healthScore
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** YMYL
- **Impact:** High (for health queries)
- **Description:** YMYL health site classification score at site level.
- **Optimization:** For health content, ensure expert authorship, cited sources, and organizational credibility.

### homepagePagerankNs
- **Module:** PerDocData
- **Type:** integer()
- **Category:** Site Authority
- **Impact:** High
- **Description:** Homepage PageRank using the NearestSeeds method. Has outsized influence on entire domain's ranking potential.
- **Optimization:** Ensure the homepage receives strong, diverse backlinks. See `site-authority-signals.md`.

### hostAge
- **Module:** PerDocData
- **Type:** integer() (16-bit)
- **Category:** Site Authority
- **Impact:** Medium
- **Description:** Host first-seen date. Used for "fresh spam sandboxing" — confirms new-domain sandbox exists.
- **Optimization:** New sites: build trust signals aggressively early. See `site-authority-signals.md`.

## I

### impressions
- **Module:** QualityNavboostCrapsCrapsClickSignals / QualityNsrNsrData
- **Type:** float() / number()
- **Category:** NavBoost / Quality
- **Impact:** Medium
- **Description:** Total SERP impressions for the URL (NavBoost) or site (NSR).
- **Optimization:** Broader keyword targeting increases impressions; CTR optimization turns impressions into clicks.

### isCovidLocalAuthority
- **Module:** QualityNsrNsrData
- **Type:** boolean()
- **Category:** Special Treatments
- **Impact:** High (for health queries)
- **Description:** COVID/health authority designation. Grants ranking preference for pandemic-related queries.
- **Optimization:** Typically reserved for government health agencies and medical institutions. See `special-treatments.md`.

### isElectionAuthority
- **Module:** QualityNsrNsrData
- **Type:** boolean()
- **Category:** Special Treatments
- **Impact:** High (for election queries)
- **Description:** Election authority designation. Grants ranking preference for election-related queries.
- **Optimization:** Typically reserved for government election agencies. See `special-treatments.md`.

### IsAnchorBayesSpam
- **Module:** PerDocData
- **Type:** boolean()
- **Category:** Spam
- **Impact:** High
- **Description:** Bayesian classifier flag for anchor text spam.
- **Optimization:** Audit inbound anchor text for spammy patterns.

### isHotdoc
- **Module:** PerDocData
- **Type:** boolean()
- **Category:** Freshness
- **Impact:** Medium
- **Description:** FreshDocs flag for extremely new or trending documents.
- **Optimization:** Publish quickly on trending topics with clear timestamps.

### isVideoFocusedSite
- **Module:** QualityNsrNsrData
- **Type:** boolean()
- **Category:** Quality
- **Impact:** Low
- **Description:** Classifier for video-focused websites.
- **Optimization:** N/A — classification flag.

## K

### keto
- **Module:** QualityNsrPQData
- **Type:** list()
- **Category:** Quality
- **Impact:** High
- **Description:** Internal quality predictor (code name "KETO"). Page-level quality signal.
- **Optimization:** Details unclear — focus on overall content quality, depth, and originality.

### KeywordStuffingScore
- **Module:** PerDocData
- **Type:** integer() (0-127, 7-bit)
- **Category:** Spam
- **Impact:** High
- **Description:** Keyword over-optimization detection score.
- **Optimization:** Write naturally. Avoid repetitive keyword insertion.

## L

### lastLongestClicks
- **Module:** QualityNavboostCrapsCrapsClickSignals
- **Type:** float()
- **Category:** NavBoost
- **Impact:** Critical
- **Description:** The last click in a search session that also had the longest dwell time. The strongest positive click signal in NavBoost.
- **Optimization:** Create comprehensive, definitive content that ends the search journey. See `navboost-signals.md`.

### lastSignificantUpdate
- **Module:** PerDocData
- **Type:** String.t (UNIX timestamp)
- **Category:** Freshness
- **Impact:** High
- **Description:** Timestamp of the last substantial content revision. Distinguishes meaningful updates from cosmetic changes.
- **Optimization:** Make substantive content updates, not trivial edits. See `content-quality-signals.md`.

### linkIncoming / linkOutgoing
- **Module:** QualityNsrPQData
- **Type:** number()
- **Category:** Links
- **Impact:** Medium
- **Description:** Page-level incoming and outgoing link counts from the PQ (Page Quality) data.
- **Optimization:** Build quality inbound links. Maintain reasonable outbound linking to authoritative sources.

### localityScore
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Quality
- **Impact:** Medium
- **Description:** Geographic relevance score. Component of the LocalAuthority signal.
- **Optimization:** For local businesses, maintain consistent NAP and local relevance signals.

### lowQuality
- **Module:** CompressedQualitySignals
- **Type:** integer()
- **Category:** Quality
- **Impact:** High
- **Description:** S2V low quality score from NSR data. Applied in Qstar scoring.
- **Optimization:** Eliminate thin, low-value pages. Maintain site-wide content quality standards.

## N

### navDemotion
- **Module:** CompressedQualitySignals
- **Type:** integer()
- **Category:** Demotions
- **Impact:** High
- **Description:** Navigation/UX demotion from QualityBoost.nav_demoted.boost.
- **Optimization:** Ensure clear navigation, good UX, and no doorway/thin pages.

### nsr
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Quality
- **Impact:** Critical
- **Description:** Normalized Site Rank — the primary site-level quality score.
- **Optimization:** Maintain consistent quality across all pages. NSR reflects site-wide quality, not just individual pages.

### nsrOverrideBid
- **Module:** QualityNsrNsrData / CompressedQualitySignals
- **Type:** number()
- **Category:** Quality
- **Impact:** Unknown
- **Description:** Emergency NSR override value for Q* system.
- **Optimization:** N/A — internal override mechanism.

### numOffdomainAnchors
- **Module:** QualityNsrPQData
- **Type:** number()
- **Category:** Links
- **Impact:** High
- **Description:** Count of anchors from external domains pointing to the page.
- **Optimization:** Build diverse external backlinks. See `link-signals.md`.

## O

### offdomainAnchorCount / ondomainAnchorCount / onsiteAnchorCount
- **Module:** IndexingDocjoinerAnchorStatistics
- **Type:** integer()
- **Category:** Links
- **Impact:** High / Medium
- **Description:** Anchor counts broken down by source relationship — external domains, same domain, same site.
- **Optimization:** Prioritize off-domain links. Use internal linking strategically. See `link-signals.md`.

### OriginalContentScore
- **Module:** PerDocData
- **Type:** integer() (0-512)
- **Category:** Content Quality
- **Impact:** High
- **Description:** Originality measurement of the document content. Higher = more original.
- **Optimization:** Create unique, original content. Avoid scraping, syndication, or rehashing existing content.

## P

### page2vecLq
- **Module:** QualityNsrPQData
- **Type:** number()
- **Category:** Quality
- **Impact:** Medium
- **Description:** Page2Vec low quality score — ML-based quality assessment using page embeddings.
- **Optimization:** Focus on genuine quality. ML classifiers detect patterns of low-quality content.

### pagerank / pagerankNs
- **Module:** PerDocData
- **Type:** number() / integer()
- **Category:** Site Authority
- **Impact:** High
- **Description:** PageRank scores. `pagerankNs` (NearestSeeds method) is the production variant. Multiple variants exist (pagerank0, pagerank1, pagerank2).
- **Optimization:** Build quality backlinks. Still actively computed for every indexed document. See `site-authority-signals.md`.

### pagerankWeight
- **Module:** AnchorsAnchor
- **Type:** number()
- **Category:** Links
- **Impact:** High
- **Description:** PageRank weight assigned to an individual anchor/link.
- **Optimization:** Earn links from high-PageRank pages. See `link-signals.md`.

### pandaDemotion
- **Module:** CompressedQualitySignals
- **Type:** integer()
- **Category:** Demotions
- **Impact:** High
- **Description:** Site-wide Panda algorithm demotion for low-quality content.
- **Optimization:** Remove thin/low-quality pages. Maintain consistent content standards across the entire site.

### penguinPenalty
- **Module:** IndexingDocjoinerAnchorStatistics
- **Type:** number()
- **Category:** Links / Spam
- **Impact:** High
- **Description:** Penguin link spam penalty score. Still active in the ranking system.
- **Optimization:** Audit and disavow manipulative links. Build natural link profiles. See `link-signals.md`.

### pnav / pnavClicks
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Quality
- **Impact:** Medium
- **Description:** Primary navigation signal and associated click data.
- **Optimization:** Ensure clear, intuitive site navigation.

### productReviewPPromotePage / productReviewPDemotePage / productReviewPPromoteSite / productReviewPDemoteSite / productReviewPUhqPage / productReviewPReviewPage
- **Module:** CompressedQualitySignals
- **Type:** integer()
- **Category:** Quality / Demotions
- **Impact:** High (for review content)
- **Description:** Suite of product review signals — page and site level promotions, demotions, ultra-high-quality flags, and review page classification.
- **Optimization:** Demonstrate genuine product experience (original photos, specific details). See `special-treatments.md`.

## R

### rhubarb
- **Module:** QualityNsrPQData
- **Type:** list() / number()
- **Category:** Quality
- **Impact:** High
- **Description:** Internal quality predictor (code name "RHUBARB"). Page-level quality signal with versioned and unversioned variants.
- **Optimization:** Focus on content quality, depth, and originality.

## S

### scamness
- **Module:** CompressedQualitySignals
- **Type:** integer() (0-1023)
- **Category:** Spam
- **Impact:** High
- **Description:** ML-derived deception/fraud likelihood score on a 0-1023 scale.
- **Optimization:** Maintain transparent, honest content. Disclose commercial relationships.

### semanticDate / semanticDateConfidence
- **Module:** PerDocData
- **Type:** integer() (32-bit UNIX date)
- **Category:** Freshness
- **Impact:** Medium
- **Description:** Semantic date extracted from content with associated confidence score.
- **Optimization:** Ensure clear, consistent dates across the page (byline, structured data, URL).

### serpDemotion
- **Module:** CompressedQualitySignals
- **Type:** integer()
- **Category:** Demotions
- **Impact:** High
- **Description:** Negative SERP behavior demotion. Applied in Qstar scoring.
- **Optimization:** Ensure good SERP behavior — accurate titles, relevant meta descriptions, no bait-and-switch.

### shoppingScore
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Quality
- **Impact:** Medium
- **Description:** E-commerce/shopping site classification score.
- **Optimization:** For e-commerce sites, maintain product quality signals, reviews, and structured data.

### siteAuthority
- **Module:** QualityNsrNsrData / CompressedQualitySignals
- **Type:** number() / integer()
- **Category:** Site Authority
- **Impact:** Critical
- **Description:** Site-level authority metric — confirms what Google publicly denied. Present in both NSR (site-level) and CompressedQualitySignals (applied in Qstar for page scoring).
- **Optimization:** Build through quality content, authoritative backlinks, brand recognition, and trust signals. See `site-authority-signals.md`.

### siteAutopilotScore
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Quality
- **Impact:** Medium
- **Description:** Aggregated autopilot quality assessment at the site level.
- **Optimization:** Consistent quality across all pages, not just key landing pages.

### siteLinkIn / siteLinkOut
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Links
- **Impact:** Medium
- **Description:** Site-level inbound and outbound link metrics.
- **Optimization:** Build quality inbound links. Maintain reasonable outbound linking to trusted sources.

### sitePr
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Site Authority
- **Impact:** High
- **Description:** Site-level PageRank score.
- **Optimization:** Earn quality backlinks to the domain overall, especially the homepage.

### siteQualityStddev
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Quality
- **Impact:** Medium
- **Description:** Standard deviation of page-level quality ratings across the site. Measures quality consistency.
- **Optimization:** Eliminate low-quality pages that drag down consistency. A narrow stddev (consistent quality) is better.

### smallPersonalSite
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Site Authority
- **Impact:** Medium
- **Description:** Classifier for small personal/independent sites. Google treats these differently in certain ranking contexts.
- **Optimization:** Build authority signals to overcome potential small-site limitations.

### sourceType
- **Module:** AnchorsAnchor
- **Type:** integer()
- **Category:** Links
- **Impact:** High
- **Description:** Source tier classification of a linking page — classifies the "corpus level" of the source.
- **Optimization:** Earn links from high-tier sources (authoritative news, .edu, .gov, established industry sites). See `link-signals.md`.

### spambrainLavcScore
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Spam
- **Impact:** High
- **Description:** SpamBrain LAVC (Link Analysis with Value Classification?) spam detection score at site level.
- **Optimization:** Avoid spam patterns. Maintain a natural link profile.

### spambrainTotalDocSpamScore
- **Module:** PerDocData
- **Type:** number() (0-1)
- **Category:** Spam
- **Impact:** High
- **Description:** AI-powered SpamBrain total document spam score on a 0-1 scale.
- **Optimization:** Avoid all spam signals — thin content, keyword stuffing, manipulative links, deceptive practices.

### spamLog10Odds
- **Module:** IndexingDocjoinerAnchorStatistics
- **Type:** number()
- **Category:** Spam
- **Impact:** High
- **Description:** Log-odds spam score for the page's anchor profile.
- **Optimization:** Maintain a clean, natural link profile. See `link-signals.md`.

### spamrank
- **Module:** PerDocData
- **Type:** integer() (0-65535)
- **Category:** Spam
- **Impact:** High
- **Description:** Link spam likelihood score on a 0-65535 scale.
- **Optimization:** Avoid link schemes and manipulative linking patterns.

### SpamWordScore
- **Module:** PerDocData
- **Type:** integer() (0-127, 7-bit)
- **Category:** Spam
- **Impact:** Medium
- **Description:** Spam word density detection in content.
- **Optimization:** Avoid spammy phrasing and excessive promotional language.

## T

### TagPageScore
- **Module:** PerDocData
- **Type:** integer() (0-100)
- **Category:** Content Quality
- **Impact:** Medium
- **Description:** Tag/taxonomy page quality score. High scores indicate thin tag pages.
- **Optimization:** Ensure tag/category pages have substantial, unique content — not just lists of links.

### timeSensitivity
- **Module:** PerDocData
- **Type:** integer()
- **Category:** Freshness
- **Impact:** Medium
- **Description:** Time sensitivity encoding for the document.
- **Optimization:** For time-sensitive content, include clear timestamps and update regularly.

### titlematchScore
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Content Quality
- **Impact:** High
- **Description:** Query-title alignment score. Measures how well page titles match user queries at site level.
- **Optimization:** Write descriptive, keyword-relevant titles that accurately match target queries.

### titleHardTokenCount
- **Module:** PerDocData
- **Type:** integer()
- **Category:** Content Quality
- **Impact:** Low
- **Description:** Count of meaningful (non-stopword) tokens in the title.
- **Optimization:** Use descriptive titles with meaningful keywords.

### tofu
- **Module:** QualityNsrNsrData / QualityNsrPQData
- **Type:** number()
- **Category:** Quality
- **Impact:** High
- **Description:** Internal quality predictor (code name "TOFU"). Present at both site level (NSR) and page level (PQData). Likely a top-of-funnel content quality signal.
- **Optimization:** Focus on comprehensive, well-structured content.

### toolbarPagerank
- **Module:** PerDocData
- **Type:** integer()
- **Category:** Site Authority
- **Impact:** Low
- **Description:** Legacy public-facing PageRank (the 0-10 scale from the old Google Toolbar).
- **Optimization:** Historical artifact. Focus on pagerankNs instead.

### trendspamScore
- **Module:** PerDocData
- **Type:** integer()
- **Category:** Spam
- **Impact:** Medium
- **Description:** Detection score for spam that targets trending topics.
- **Optimization:** When covering trending topics, ensure genuine expertise and value.

### trustSyntacticDateInRanking
- **Module:** QualityTimebasedSyntacticDate
- **Type:** boolean()
- **Category:** Freshness
- **Impact:** Medium
- **Description:** Boolean flag for whether the extracted syntactic date is trusted enough to use in ranking.
- **Optimization:** Ensure dates are clear, consistent, and unambiguous across the page.

## U

### uacSpamScore
- **Module:** PerDocData
- **Type:** integer() (0-127, 7-bit)
- **Category:** Spam
- **Impact:** High
- **Description:** UAC spam score. Values above 64 indicate spam.
- **Optimization:** Avoid all spam patterns — the 64 threshold is a hard spam classification line.

### ugcDiscussionEffortScore
- **Module:** CompressedQualitySignals
- **Type:** integer() (value × 1000)
- **Category:** Quality
- **Impact:** Medium
- **Description:** User-generated content substantive effort score. Multiplied by 1000 for integer storage.
- **Optimization:** For UGC sites, encourage substantive contributions and moderate low-effort content.

### ugcScore
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Quality
- **Impact:** Medium
- **Description:** User-generated content quality score at site level.
- **Optimization:** Moderate UGC quality. Implement editorial standards for user contributions.

### unauthoritativeScore
- **Module:** CompressedQualitySignals
- **Type:** integer() (0-1023)
- **Category:** Quality
- **Impact:** High
- **Description:** Penalty score for lack of authoritativeness on a 0-1023 scale.
- **Optimization:** Build authority through expert authorship, authoritative backlinks, and brand recognition.

### unicornClicks
- **Module:** QualityNavboostCrapsCrapsClickSignals
- **Type:** float()
- **Category:** NavBoost
- **Impact:** Medium
- **Description:** Clicks from a special subset of users ("Unicorn" users — likely a quality-filtered user panel).
- **Optimization:** These may represent higher-quality user signals. Focus on satisfying genuine user intent.

### unsquashedClicks / unsquashedImpressions / unsquashedLastLongestClicks
- **Module:** QualityNavboostCrapsCrapsClickSignals
- **Type:** float()
- **Category:** NavBoost
- **Impact:** High
- **Description:** New-format click data before Google's normalization/squashing functions. Raw signal values.
- **Optimization:** Google applies squashing to reduce noise and prevent manipulation. Focus on genuine user satisfaction.

### urlAutopilotScore
- **Module:** QualityNsrPQData
- **Type:** number()
- **Category:** Quality
- **Impact:** Medium
- **Description:** Page-level autopilot quality assessment.
- **Optimization:** Maintain consistent quality standards on every page.

## V

### videoScore
- **Module:** QualityNsrNsrData
- **Type:** number()
- **Category:** Quality
- **Impact:** Medium
- **Description:** Video content quality score at site level.
- **Optimization:** For video-heavy sites, maintain video quality standards.

### vlq / vlqNsr
- **Module:** QualityNsrNsrData / QualityNsrPQData / CompressedQualitySignals
- **Type:** number() / integer()
- **Category:** Quality
- **Impact:** High
- **Description:** Very Low Quality score — present at site level (NSR), page level (PQData), and in compressed signals. Multiple modules track VLQ.
- **Optimization:** Eliminate very low quality pages. Even one module flagging VLQ can impact rankings.

### voterTokenCount
- **Module:** QualityNavboostCrapsCrapsData
- **Type:** integer()
- **Category:** NavBoost
- **Impact:** Medium
- **Description:** Lower bound on the number of distinct users who contributed click data for the query-URL pair.
- **Optimization:** Higher voter counts make NavBoost data more reliable. Build traffic volume.

## W

### webrefEntities
- **Module:** PerDocData
- **Type:** RepositoryWebrefWebrefMustangAttachment.t
- **Category:** Author & Entity
- **Impact:** High
- **Description:** Knowledge Graph entity extraction and association for the document. Links pages to known entities.
- **Optimization:** Use structured data, consistent entity naming, and build Knowledge Graph presence. See `author-entity-signals.md`.

## Y

### ymylHealthScore / ymylNewsScore / ymylNewsV2Score
- **Module:** PerDocData / QualityNsrNsrData
- **Type:** integer() / number()
- **Category:** YMYL
- **Impact:** High (for YMYL queries)
- **Description:** Your Money or Your Life classification scores for health and news content. Present at both document and site level.
- **Optimization:** For YMYL content: expert authorship, cited sources, organizational credibility, editorial review. See `special-treatments.md`.

---

## Using This Catalog

- **For audits:** Walk through each Critical/High impact attribute and evaluate the target page
- **For Q&A:** Search for attribute names (e.g., `goodClicks`, `siteAuthority`) or categories (e.g., `NavBoost`, `Spam`)
- **For deeper context:** Follow the "See [reference].md" links for each category
- **HexDocs lookup:** All modules are documented at `hexdocs.pm/google_api_content_warehouse/0.4.0/GoogleApi.ContentWarehouse.V1.Model.{ModuleName}.html`
- **Key modules to search:** QualityNsrNsrData (58 fields), CompressedQualitySignals (38 fields), PerDocData (230+ fields), IndexingDocjoinerAnchorStatistics (53 fields)
