# Link Signals — Anchor Statistics, Anchor Attributes & Source Quality

Links are evaluated through three primary modules in the leak. `IndexingDocjoinerAnchorStatistics` (53 fields) provides aggregate link profile statistics. `AnchorsAnchor` (38 fields) describes individual anchor/link attributes. `AnchorsAnchorSource` (25 fields) evaluates the source page of each link. Together they reveal a link evaluation system far more granular than simple PageRank.

**Source module documentation:** [HexDocs](https://hexdocs.pm/google_api_content_warehouse/0.4.0/api-reference.html)

---

## IndexingDocjoinerAnchorStatistics — Aggregate Link Profile

The master link profile module with 53 fields tracking aggregate statistics about all links pointing to a document.

### Link Counts

| Field | Type | Description |
|---|---|---|
| `anchorCount` | integer() | **Total anchor/link count** |
| `offdomainAnchorCount` | integer() | **External links** — links from other domains |
| `ondomainAnchorCount` | integer() | **Internal links** — links from same domain |
| `compressedOriginalAnchors` | String.t | Compressed original anchor text data |
| `anchorStatsBySource` | list(nested) | Anchor statistics broken down by source type |

### Quality Tier Counts

| Field | Type | Description |
|---|---|---|
| `sourceTypeCount` | list(nested) | Link counts broken down by source quality tier |
| `highQualityAnchorCount` | integer() | Links from high-quality/trusted sources |
| `mediumQualityAnchorCount` | integer() | Links from medium-quality sources |
| `lowQualityAnchorCount` | integer() | Links from low-quality/untrusted sources |

### Spam & Penalty Fields

| Field | Type | Description |
|---|---|---|
| `penguinPenalty` | number() | **Penguin algorithm penalty score** |
| `badbacklinksPenalized` | boolean() | Whether bad backlink penalty is active |
| `spamLog10Odds` | number() | Log-odds spam probability for the link profile |
| `anchorSpamInfo` | nested | Detailed anchor spam analysis data |
| `anchorSpamCount` | integer() | Number of links classified as spam |
| `IsAnchorBayesSpam` | boolean() | Bayesian spam classifier result |

### Dropped Links

| Field | Type | Description |
|---|---|---|
| `droppedAnchorCount` | integer() | Links that were dropped/discounted |
| `droppedRedundantAnchorCount` | integer() | Redundant links dropped (same anchor from same domain) |
| `droppedSmallAnchorCount` | integer() | Small/insignificant anchors dropped |

### Domain-Level Metrics

| Field | Type | Description |
|---|---|---|
| `uniqueDomainCount` | integer() | **Referring domain diversity** — unique linking domains |
| `uniqueIpCount` | integer() | Unique IP addresses linking to the page |
| `uniqueCBlockCount` | integer() | Unique C-class IP blocks linking |
| `neighborAnchorCount` | integer() | Links from topically related/neighboring sites |
| `localAnchorCount` | integer() | Links from geographically local sources |

### Link Velocity & Freshness

| Field | Type | Description |
|---|---|---|
| `freshAnchorCount` | integer() | Recently acquired links |
| `oldAnchorCount` | integer() | Long-standing/aged links |
| `anchorAgeDistribution` | nested | Distribution of link ages |

### Key Insights

**penguinPenalty** — Confirms the Penguin algorithm penalty is a per-document numerical score, not just a binary filter. Pages can have varying degrees of Penguin penalty based on the severity of their link spam.

**droppedAnchorCount** — Google actively drops links it considers invalid, redundant, or manipulative. The dropped count reveals how many links Google ignores. A high dropped-to-total ratio suggests link quality problems.

**uniqueDomainCount vs anchorCount** — Domain diversity (unique domains linking) matters more than raw link count. Google tracks both, but `uniqueDomainCount` is the stronger authority signal. 100 links from 1 domain carries less weight than 10 links from 10 different domains.

---

## AnchorsAnchor — Individual Anchor Attributes

Each backlink is evaluated individually with 38 attributes:

### Core Anchor Data

| Field | Type | Description |
|---|---|---|
| `text` | String.t | The anchor text content |
| `fullLeftContext` | String.t | Text surrounding the anchor (left side) |
| `fullRightContext` | String.t | Text surrounding the anchor (right side) |
| `targetUrl` | String.t | URL the anchor points to |
| `sourceUrl` | String.t | URL containing the anchor |

### Quality & Weight

| Field | Type | Description |
|---|---|---|
| `pagerankWeight` | number() | **PageRank weight** of this specific link |
| `weight` | number() | Computed anchor weight in the link graph |
| `fontSize` | integer() | Font size of the anchor (prominence signal) |
| `isInTitle` | boolean() | Whether the anchor is in the page title |
| `isInHeading` | boolean() | Whether the anchor is in a heading tag |
| `context` | integer() | Placement context type (editorial, navigation, footer, etc.) |

### Timing

| Field | Type | Description |
|---|---|---|
| `creationDate` | integer() | When the link was first created |
| `firstseenDate` | integer() | When Google first discovered the link |
| `discoveryAge` | integer() | Age since discovery |

### Classification

| Field | Type | Description |
|---|---|---|
| `sourceType` | integer() | **Source quality tier classification** |
| `demotionreason` | integer() | Reason the link was demoted (if applicable) |
| `experimentTag` | integer() | Experimental scoring tag |
| `isNofollow` | boolean() | Whether the link has nofollow attribute |
| `isSponsoredOrUgc` | boolean() | Sponsored or user-generated content link tag |
| `isRedirect` | boolean() | Whether the link is via redirect |
| `linkType` | integer() | Classification of link type |

### Key Insights

**pagerankWeight** — Each individual link carries its own PageRank weight, not just the source page's overall PR. This means the specific link's prominence, placement, and context affect how much PageRank flows.

**context** — Link placement matters. An editorial link within main body content carries more weight than a sidebar, footer, or navigation link. The `context` field encodes this directly.

**fontSize** — Google even tracks the font size of the anchor text, suggesting that prominent, visually important links carry more signal than tiny footer links.

**creationDate vs firstseenDate** — Google distinguishes between when a link was created and when it was discovered. Links that appear on old pages long after the page was published can be flagged as insertions.

---

## AnchorsAnchorSource — Source Page Evaluation

Each linking page (source) is evaluated with 25 attributes:

### Source Authority

| Field | Type | Description |
|---|---|---|
| `pagerank` | number() | Source page's PageRank |
| `domain` | String.t | Source domain |
| `language` | String.t | Source page language |
| `country` | String.t | Source page geographic association |
| `docid` | integer() | Source document ID |

### homePageInfo — Homepage Trust Levels

| Field | Type | Description |
|---|---|---|
| `homePageInfo` | integer() | **Homepage trust classification** |

The `homePageInfo` field classifies linking pages by their trust relationship to their site's homepage. Four documented levels:

1. **NOT_HOMEPAGE** — The linking page is not a homepage. Standard link treatment.
2. **NOT_TRUSTED** — The linking page's site homepage has not earned trust. Links from these sources carry minimal weight.
3. **PARTIALLY_TRUSTED** — The site's homepage has some trust signals but not full authority. Links carry moderate weight.
4. **FULLY_TRUSTED** — The site's homepage is fully trusted by Google. Links from pages on this domain carry maximum weight.

This reveals that Google evaluates the homepage authority of every linking domain and uses it to weight individual links. A link from a page on a FULLY_TRUSTED domain carries significantly more value than an identical link from a NOT_TRUSTED domain.

### Source Quality Signals

| Field | Type | Description |
|---|---|---|
| `spamScore` | number() | Source page spam score |
| `outgoingLinkCount` | integer() | Total outgoing links on the source page |
| `relevanceScore` | number() | Topical relevance of source to target |
| `isNewsSite` | boolean() | Whether source is classified as news |
| `siteQuality` | number() | Source site overall quality score |

### Key Insights

**outgoingLinkCount** — Pages with many outgoing links dilute the PageRank passed per link. A link from a page with 10 outgoing links passes more value than the same link from a page with 500 outgoing links.

**relevanceScore** — Topical relevance between source and target affects link value. A link from a relevant industry site carries more weight than a link from an unrelated domain, even if the unrelated domain has higher authority.

**isNewsSite** — News sources receive special classification. Links from news sites may be evaluated differently (potentially higher initial value, potentially faster decay).

---

## sourceType — Link Quality Tiers

The `sourceType` field classifies links into quality tiers. While the exact enum values aren't fully documented, the classification hierarchy is:

**Tier 1 — High Trust:**
- Major news publications and wire services
- Government domains (.gov)
- Educational institutions (.edu)
- Established industry-leading publications
- Sites with FULLY_TRUSTED homePageInfo

**Tier 2 — Medium Trust:**
- Reputable blogs and content sites
- Industry directories and professional associations
- Community sites with editorial standards
- Sites with PARTIALLY_TRUSTED homePageInfo

**Tier 3 — Low Trust:**
- Sites with NOT_TRUSTED homePageInfo
- User-generated content platforms (lower weight per link)
- New or unestablished sites

**Tier 4 — Spam/Dropped:**
- Identified link farms and PBNs
- Sites with high spam scores
- Auto-generated content sites
- Links matching known manipulation patterns

---

## Link Signals in CompressedQualitySignals

Compressed link-related signals used during ranking:

| Field | Type | Description |
|---|---|---|
| `anchorMismatchDemotion` | integer() | Demotion for anchor text not matching page content |
| `serpDemotion` | integer() | SERP-level demotion from link signals |

### anchorMismatchDemotion

Triggers when inbound anchor text consistently doesn't match page content. This signal combats:
- Purchased links with keyword-stuffed anchors for unrelated pages
- Hacked site injections with off-topic anchor text
- Link schemes where anchors target commercial keywords rather than describing the actual content

---

## NoFollow & Link Attributes

The leak confirms Google's treatment of link attributes:

- **nofollow**: Treated as a "hint," not a directive. Google generally respects it for PageRank flow but may still use the link for discovery and entity/topical understanding.
- **sponsored**: Identifies paid/sponsored links. Should not pass PageRank.
- **ugc**: Identifies user-generated content links. Evaluated with lower trust by default.

High-traffic nofollow links (Reddit, Wikipedia, major forums) still provide value through:
- URL discovery and indexing
- Entity/brand association
- NavBoost click data from referral traffic
- Topical context signals

---

## Audit Checklist for Link Signals

- [ ] **uniqueDomainCount**: Sufficient referring domain diversity (not concentrated from few sources)
- [ ] **sourceType distribution**: Majority of links from Tier 1 or Tier 2 sources
- [ ] **homePageInfo levels**: Links from FULLY_TRUSTED and PARTIALLY_TRUSTED domains
- [ ] **penguinPenalty**: No active Penguin penalty (check for unnatural link patterns)
- [ ] **badbacklinksPenalized**: No bad backlinks penalty flag
- [ ] **droppedAnchorCount**: Low ratio of dropped-to-total links
- [ ] **anchorMismatchDemotion**: Anchor text matches page content
- [ ] **Anchor text diversity**: Natural mix of branded, generic, and keyword anchors
- [ ] **Link freshness**: Mix of fresh and aged links, steady acquisition pattern
- [ ] **Internal links**: Page receives contextual internal links with descriptive anchors
- [ ] **Context placement**: Links are editorial/in-content, not footer/sidebar/nav
- [ ] **Source relevance**: Links from topically related sites (high relevanceScore)
- [ ] **No spam patterns**: Clean spamLog10Odds, no IsAnchorBayesSpam flags
