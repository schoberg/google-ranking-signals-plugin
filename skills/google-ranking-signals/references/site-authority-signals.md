# Site Authority Signals — Domain Trust, Age & PageRank

The leak confirmed Google maintains domain-level authority metrics. The `QualityNsrNsrData` module (58 fields) contains the core site-level signals, while `PerDocData` (230+ fields) holds PageRank variants and domain age data.

**Source module documentation:** [HexDocs](https://hexdocs.pm/google_api_content_warehouse/0.4.0/api-reference.html)

---

## QualityNsrNsrData — Site Quality Fields (Authority Subset)

The primary site-level quality module. 58 total fields — these are the authority-relevant ones:

| Field | Type | Description |
|---|---|---|
| `nsr` | number() | **Normalized Site Rank** — primary site quality score |
| `siteAuthority` | number() | **Confirmed domain authority metric** (Google denied this existed) |
| `sitePr` | number() | Site-level PageRank |
| `siteAutopilotScore` | number() | Aggregated quality score across URL autopilot scores |
| `siteQualityStddev` | number() | **Quality variance** — spread of page-level quality ratings |
| `smallPersonalSite` | number() | Small personal site classifier |
| `chromeInTotal` | number() | Chrome browser views for behavior tracking |
| `directFrac` | number() | Direct navigation traffic fraction |
| `impressions` | number() | Total SERP impressions |
| `siteLinkIn` | number() | Inbound links to site |
| `siteLinkOut` | number() | Outbound links from site |
| `siteChunk` | String.t | Primary NSR sitechunk (usually HOST_LEVEL_V3) |
| `secondarySiteChunk` | String.t | Secondary grouping identifier |
| `nsrEpoch` | String.t | NSR calculation timestamp |
| `nsrVariance` | number() | NSR calculation variance |
| `nsrOverrideBid` | number() | Emergency NSR override for Q* system |
| `clusterId` | integer() | Site cluster identifier (for experiments) |
| `clusterUplift` | nested | Cluster-based ranking boost |

### siteAuthority — The Big Revelation

Confirms what Google publicly denied for years: a domain-level authority score exists and influences rankings. Present in both:
- `QualityNsrNsrData.siteAuthority` (site-level, type: number())
- `CompressedQualitySignals.siteAuthority` (compressed for Qstar scoring, type: integer())

Contributing factors include backlink profile, historical ranking performance, brand recognition, user engagement patterns, and content quality consistency.

### nsr — Normalized Site Rank

The primary site-level quality score. All other site-level signals feed into or relate to NSR. Key characteristics:
- Computed at the "sitechunk" level (usually HOST_LEVEL_V3)
- Has associated confidence (`nsrConfidence`) and variance (`nsrVariance`)
- Can be overridden manually (`nsrOverrideBid`) for the Q* system
- Versioned data available (`nsrVersionedData`) for historical tracking

### siteQualityStddev — Quality Consistency

Measures how consistent quality is across all pages on the site. A **narrow stddev** (consistent quality) is better than a wide one. Implications:
- A few excellent pages can't overcome many mediocre ones
- Removing low-quality pages improves the stddev
- Consistent editorial standards matter

### smallPersonalSite — Site Classification

A classifier that flags small, personal/independent sites. Google treats these differently — they may face different quality thresholds or be subject to different ranking pathways for some queries.

---

## PerDocData — PageRank Variants

The `PerDocData` module (230+ fields) contains multiple PageRank variants:

| Field | Type | Description |
|---|---|---|
| `pagerank` | number() | Experimental/deprecated PageRank |
| `pagerankNs` | integer() | **Production PageRank** (NearestSeeds method) |
| `pagerank0` | number() | PageRank variant 0 |
| `pagerank1` | number() | PageRank variant 1 |
| `pagerank2` | number() | PageRank variant 2 |
| `homepagePagerankNs` | integer() | **Homepage PageRank** (outsized domain influence) |
| `crawlPagerank` | integer() | Internal crawl forward PageRank |
| `toolbarPagerank` | integer() | Legacy toolbar PageRank (0-10 scale) |

### pagerankNs — The Production Variant

The NearestSeeds method is the **current production PageRank**. Unlike classic PageRank, NearestSeeds:
- Uses a set of trusted "seed" pages as starting points
- Propagates trust through the link graph from these seeds
- More resistant to link spam than classic PageRank
- Still computed for every indexed document

### homepagePagerankNs — Homepage Influence

Homepage PageRank receives special treatment. The homepage's PR has outsized influence because:
- Typically receives the most external links
- Internal link structure flows from homepage downward
- Serves as a proxy for overall domain strength
- Google uses it to assess the "ceiling" of a domain's ranking potential

---

## PerDocData — Domain Age & Sandbox

| Field | Type | Description |
|---|---|---|
| `domainAge` | integer() (16-bit) | Domain registration age |
| `hostAge` | integer() (16-bit) | Host first-seen date |

### hostAge — New Domain Sandbox

Confirms the "sandbox" for new domains. New sites face restricted ranking potential until trust signals accumulate over time.

How the sandbox works:
- Newly registered domains have low hostAge values
- Low hostAge triggers conservative ranking treatment
- Rankings are suppressed (not blocked) for competitive queries
- The sandbox lifts gradually as trust signals accumulate

Escaping the sandbox faster:
- Build high-quality backlinks from trusted sources early
- Produce substantial, original content immediately
- Establish entity recognition (Google Knowledge Graph)
- Generate branded search volume
- Maintain consistent publishing cadence

---

## CompressedQualitySignals — Authority in Scoring

Authority signals from NSR are compressed and stored for fast access during Qstar scoring:

| Field | Type | Description |
|---|---|---|
| `siteAuthority` | integer() | Compressed site authority |
| `authorityPromotion` | integer() | Positive authority boost from QualityBoost.authority.boost |
| `unauthoritativeScore` | integer() (0-1023) | Penalty for lack of authoritativeness |
| `lowQuality` | integer() | S2V low quality score from NSR |
| `nsrConfidence` | integer() | NSR confidence level |

### authorityPromotion vs unauthoritativeScore

These are two sides of the same coin:
- `authorityPromotion` — a positive boost for authoritative sites
- `unauthoritativeScore` — a penalty (0-1023 scale) for sites lacking authority

A site can have low authorityPromotion AND high unauthoritativeScore — meaning it gets no boost and also gets penalized.

---

## Spam & Trust Signals (from PerDocData)

| Field | Type | Description |
|---|---|---|
| `spambrainTotalDocSpamScore` | number() (0-1) | AI-powered SpamBrain total spam score |
| `spamrank` | integer() (0-65535) | Link spam likelihood |
| `DocLevelSpamScore` | integer() (0-127) | Document-level spam |
| `uacSpamScore` | integer() (0-127) | UAC spam (threshold: 64 = spam) |

### SpamBrain

Google's AI-powered spam detection system. The `spambrainTotalDocSpamScore` (0-1 scale) is the master spam signal. Also present at site level via `spambrainLavcScore` in QualityNsrNsrData.

---

## Audit Checklist for Site Authority

- [ ] **siteAuthority indicators**: Diverse backlinks, brand recognition, entity presence
- [ ] **Domain age**: Established domain or new (potential sandbox)
- [ ] **Homepage PageRank**: Strong external links pointing to the homepage
- [ ] **NSR consistency**: Quality maintained across ALL pages (check siteQualityStddev)
- [ ] **Internal linking**: Clear hierarchy distributing PageRank effectively
- [ ] **Chrome/direct traffic**: chromeInTotal and directFrac indicate brand strength
- [ ] **No spam flags**: Clean spambrainTotalDocSpamScore and spamrank
- [ ] **authorityPromotion**: Building positive authority signals
- [ ] **Low unauthoritativeScore**: Not being penalized for lack of authority
- [ ] **Publishing cadence**: Regular content updates (NSR decays over time)
