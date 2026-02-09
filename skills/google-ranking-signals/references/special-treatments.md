# Special Treatments — Demotions, YMYL, Whitelists & Product Reviews

The leak reveals an extensive system of demotions, promotions, and special flags in `CompressedQualitySignals` (38 fields). This module is the "cheat sheet" used during rapid Qstar scoring — containing pre-computed quality judgments that can boost or demote pages. Additional YMYL and authority flags appear in `QualityNsrNsrData` and `PerDocData`.

**Source module documentation:** [HexDocs](https://hexdocs.pm/google_api_content_warehouse/0.4.0/api-reference.html)

---

## CompressedQualitySignals — Demotion Fields

The primary demotion signals, all stored as compressed integers for fast access during ranking:

| Field | Type | Range | Description |
|---|---|---|---|
| `exactMatchDomainDemotion` | integer() | — | **EMD penalty** — targets exact-match domains |
| `anchorMismatchDemotion` | integer() | — | **Anchor mismatch** — anchor text doesn't match content |
| `pandaDemotion` | integer() | — | **Panda algorithm** demotion (content quality) |
| `babyPandaDemotion` | integer() | — | **Baby Panda** — lighter content quality demotion |
| `babyPandaV2Demotion` | integer() | — | **Baby Panda V2** — updated lighter demotion |
| `navDemotion` | integer() | — | **Navigation demotion** — doorway/thin navigation pages |
| `serpDemotion` | integer() | — | **SERP-level demotion** from behavioral signals |
| `scamness` | integer() | 0-1023 | **Scam likelihood score** |
| `unauthoritativeScore` | integer() | 0-1023 | **Lack of authority** penalty |
| `lowQuality` | integer() | — | S2V low quality score from NSR |

### exactMatchDomainDemotion — EMD Penalty

Targets exact-match domains (e.g., `best-running-shoes.com`) that lack genuine authority beyond their keyword-matching domain name.

When triggered:
- The domain's name-based ranking advantage is neutralized
- Page must compete on content quality, links, and engagement alone
- Primarily affects domains that are keyword-matching rather than brand-building

Avoiding EMD demotion:
- Build genuine brand recognition beyond the domain name
- Create authoritative, original content
- Earn links for content quality, not keyword-matching
- Develop direct traffic and brand searches

### pandaDemotion / babyPandaDemotion / babyPandaV2Demotion — Content Quality Tiers

Three tiers of the Panda content quality algorithm:

1. **pandaDemotion**: Full Panda penalty for sites with substantial thin, low-quality, or duplicate content. The most severe content quality demotion.
2. **babyPandaDemotion**: A lighter version — partial quality demotion for sites with moderate quality issues.
3. **babyPandaV2Demotion**: Updated version of Baby Panda with refined quality assessment.

These are **site-level** assessments — a few poor pages can trigger demotions that affect the entire domain. Key factors:
- Ratio of thin-to-substantive content
- Content originality across the site (`OriginalContentScore`)
- Site-wide quality consistency (`siteQualityStddev` from QualityNsrNsrData)
- Presence of auto-generated or scraped content

### navDemotion — Doorway Page Penalty

Targets pages created primarily for search engine traffic rather than user value:
- Thin pages designed to funnel traffic to other pages
- City/location-specific pages with no unique content per location
- Keyword-targeted landing pages without substantive content
- Pages that serve as intermediaries rather than destinations

### scamness — Fraud Detection (0-1023 Scale)

A 10-bit integer (0-1023 range) scoring how likely the page/site is to be scam or fraudulent content. Higher scores indicate greater scam likelihood.

Factors that increase scamness:
- Deceptive claims or promises
- Fake urgency and pressure tactics
- Hidden or misleading contact information
- Price/offer manipulation patterns
- Known scam template detection

### unauthoritativeScore — Authority Deficit (0-1023 Scale)

A 10-bit integer measuring the **lack** of authoritativeness. This is the inverse of authority — a penalty applied to sites that lack authority signals for the topics they cover.

A site can simultaneously have:
- Low `authorityPromotion` (no positive authority boost)
- High `unauthoritativeScore` (actively penalized for lack of authority)

This double-hit means the site gets no authority bonus AND faces an authority penalty. Most impactful for YMYL topics.

---

## CompressedQualitySignals — Promotion Fields

Positive signals that boost rankings:

| Field | Type | Description |
|---|---|---|
| `authorityPromotion` | integer() | **Positive authority boost** from QualityBoost |
| `siteAuthority` | integer() | Compressed site authority score |
| `nsrConfidence` | integer() | Confidence level in NSR calculation |

### authorityPromotion

A positive ranking boost for sites demonstrating strong authority signals:
- Diverse, high-quality backlink profile
- Brand recognition and direct traffic
- Consistent content quality across the site
- Entity recognition and Knowledge Graph presence
- Long-standing domain with trust history

---

## CompressedQualitySignals — Product Review Fields

Dedicated signals for Google's Product Review system:

| Field | Type | Description |
|---|---|---|
| `productReviewPPromotePage` | integer() | **Promotion** for high-quality review pages |
| `productReviewPDemotePage` | integer() | **Demotion** for low-quality review pages |
| `productReviewPPromoteSite` | integer() | **Site-level promotion** for review quality |
| `productReviewPDemoteSite` | integer() | **Site-level demotion** for review quality |
| `productReviewPUhqPage` | integer() | **Ultra-high-quality** review page flag |
| `productReviewPReviewPage` | integer() | Page classified as a product review |

### Product Review Quality Spectrum

Google evaluates product reviews on a detailed quality spectrum:

**Ultra-High Quality (productReviewPUhqPage):**
- Original photography of the actual product
- Quantitative testing data and measurements
- Detailed comparative analysis against alternatives
- Evidence of extended use over time
- Expert author credentials in the product category

**Promoted (productReviewPPromotePage):**
- Genuine hands-on experience evidence
- Both pros and cons with specific details
- Useful for purchase decisions
- Original content beyond manufacturer specs

**Demoted (productReviewPDemotePage):**
- Thin affiliate content without original value
- Aggregated specs without genuine assessment
- Stock imagery instead of original product photos
- No evidence of product testing or hands-on use
- Template-based comparison pages

### Site-Level Review Quality

`productReviewPPromoteSite` and `productReviewPDemoteSite` apply at the domain level — a site known for high-quality reviews gets a boost across all its review content, while a site with consistently thin reviews faces domain-wide demotion for review queries.

---

## YMYL Signals — Your Money or Your Life

YMYL classification fields from multiple modules:

### From PerDocData

| Field | Type | Description |
|---|---|---|
| `ymylHealthScore` | number() | **Health YMYL classification** score |
| `ymylNewsScore` | number() | **News YMYL classification** score |

### From QualityNsrNsrData

| Field | Type | Description |
|---|---|---|
| `ymylNewsV2Score` | number() | Updated news YMYL scoring model |
| `healthScore` | number() | Health content quality score |
| `isElectionAuthority` | boolean() | **Election authority whitelist** flag |
| `isCovidLocalAuthority` | boolean() | **COVID/health authority whitelist** flag |

### YMYL Quality Amplification

When a page's YMYL score is high, all quality signals are evaluated more strictly:

- **E-E-A-T requirements are elevated**: Author credentials become critical, not just beneficial
- **Content accuracy threshold rises**: Claims must be well-sourced and verifiable
- **Trust signals are mandatory**: Contact information, editorial policies, and organizational transparency are required
- **Demotion triggers are more sensitive**: Quality issues that wouldn't affect non-YMYL content trigger demotions
- **unauthoritativeScore** impact amplified: Lack of authority is penalized more harshly

### YMYL Categories

**Health & Medical** (ymylHealthScore, healthScore):
- Medical conditions, symptoms, treatments
- Pharmaceutical information
- Mental health topics
- Nutrition and fitness claims

**News & Civic** (ymylNewsScore, ymylNewsV2Score):
- Government processes and policies
- Election information (isElectionAuthority)
- Public health emergencies (isCovidLocalAuthority)
- Public safety information

**Financial** (inferred from YMYL framework):
- Investment advice and banking
- Tax and retirement guidance
- Insurance and credit information

**Legal** (inferred from YMYL framework):
- Legal rights and processes
- Immigration information
- Consumer protection

---

## Authority Whitelists

### isElectionAuthority

A boolean whitelist flag in `QualityNsrNsrData` for authoritative election information sources:
- Government election commissions and agencies
- Official voter registration platforms
- Major trusted news organizations for election coverage

When activated, the site receives strong ranking preference for election-related queries.

### isCovidLocalAuthority

A boolean whitelist flag for authoritative COVID and health information sources:
- National health agencies (CDC, WHO, NHS)
- Local public health departments
- Major hospital systems and medical institutions
- Established medical journals

Grants ranking preference for pandemic-related and health-emergency queries.

---

## Additional Special Flags from QualityNsrNsrData

| Field | Type | Description |
|---|---|---|
| `isVideoFocusedSite` | boolean() | Site classified as primarily video content |
| `smallPersonalSite` | number() | Small personal site classifier |
| `localityScore` | number() | Geographic locality assessment |

### smallPersonalSite

A classifier that flags small, personal/independent sites. Google may treat these differently:
- Different quality thresholds (potentially lower bar for demonstrating authority)
- Different ranking pathways for certain query types
- May benefit from Google's stated goal to surface diverse voices

### isVideoFocusedSite

Sites classified as primarily video content receive specialized treatment:
- May be evaluated with video-specific quality signals (`videoScore`)
- Could receive preferential treatment for video-intent queries
- Potentially subject to different freshness and quality thresholds

---

## Audit Checklist for Special Treatments

- [ ] **No Panda demotions**: Site-wide content quality is consistent, no thin/duplicate pages
- [ ] **No EMD demotion**: Domain has genuine brand identity beyond keyword matching
- [ ] **No anchor mismatch**: Inbound anchor text aligns with actual content
- [ ] **Low scamness**: No deceptive claims, fake urgency, or misleading practices
- [ ] **Low unauthoritativeScore**: Demonstrable authority for covered topics
- [ ] **authorityPromotion active**: Diverse links, brand recognition, entity presence
- [ ] **Product reviews**: If review content, meets promotion criteria (original photos, testing, genuine experience)
- [ ] **YMYL compliance**: For health/finance/legal content — verifiable credentials, sourced claims, editorial transparency
- [ ] **No navDemotion**: No doorway or thin navigation pages on the site
- [ ] **Site consistency**: Low `siteQualityStddev` — quality maintained across all pages
- [ ] **YMYL author credentials**: Named experts with demonstrable qualifications (for YMYL content)
- [ ] **Whitelist eligibility**: For election/health authority content, proper organizational credentials
