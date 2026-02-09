# NavBoost Signals — Click & User Interaction Data

NavBoost is the most significant ranking system revealed in the 2024 Google leak. It is a "Twiddler" — a re-ranking function that adjusts results after Google's primary ranking algorithm (Ascorer) has produced initial rankings. NavBoost uses aggregated click data from Chrome and Google Search over a rolling 13-month window.

Google publicly downplayed click data for years. The leak proves click signals are central to ranking.

**Source module documentation:** [HexDocs](https://hexdocs.pm/google_api_content_warehouse/0.4.0/api-reference.html)

---

## QualityNavboostCrapsCrapsClickSignals — Complete Field Reference

The core NavBoost click signal module. CRAPS = **C**lick, **R**ank, **A**nd **P**osition **S**ignals. Active since 2005.

| Field | Type | Description |
|---|---|---|
| `clicks` | float() | Total clicks on the result |
| `goodClicks` | float() | **Positive engagement clicks** — user found what they needed |
| `badClicks` | float() | **Negative clicks** — quick return to SERP (pogo-sticking) |
| `lastLongestClicks` | float() | **Last click in session + longest dwell time** — strongest positive signal |
| `impressions` | float() | Total SERP impressions |
| `absoluteImpressions` | float() | Host-level unsquashed impressions |
| `unicornClicks` | float() | Clicks from "Unicorn" user subset (quality-filtered panel) |
| `unsquashedClicks` | float() | Raw click data before normalization |
| `unsquashedImpressions` | float() | Raw impression data before normalization |
| `unsquashedLastLongestClicks` | float() | Raw longest-click data before normalization |

**All 10 fields are float types**, confirming these are aggregated statistical measures, not individual event counts.

---

## QualityNavboostCrapsCrapsData — NavBoost Context Data

The full NavBoost data container, providing context for click signals.

| Field | Type | Description |
|---|---|---|
| `url` | String.t | Target URL |
| `query` | String.t | Search query string |
| `country` | String.t | Two-letter country code (US, FR, BR) |
| `language` | String.t | Language slice (en, fr, pt-BR) |
| `device` | integer() | Device interface and OS classification |
| `mobileSignals` | nested | Mobile-specific click data |
| `unsquashedMobileSignals` | nested | Raw mobile click data |
| `unscaledIpPriorBadFraction` | integer() | IP-based prior spam penalty fraction |
| `voterTokenCount` | integer() | Lower bound on distinct users contributing data |
| `patternLevel` | integer() | Pattern abstraction level (0 = exact URL) |
| `packedIpAddress` | String.t | Network byte order IP address |
| `features` | nested | Feature-specific click signals |

### Key Implications

- **Country/Language segmentation:** NavBoost data is sliced by country and language. A page can have strong click signals in one market and weak signals in another.
- **Device segmentation:** Mobile and desktop click data are tracked separately. Mobile-specific signals exist via `mobileSignals`.
- **voterTokenCount:** Minimum number of distinct users. Low voter counts mean NavBoost data is unreliable for that query-URL pair — Google needs sufficient volume to act on click data.
- **patternLevel:** At level 0, signals are for the exact URL. Higher levels aggregate to URL patterns, useful for sites with low per-URL traffic.

---

## NavBoost in CompressedQualitySignals

NavBoost data is also compressed and stored in the `CompressedQualitySignals` module for fast access during ranking:

| Field | Type | Description |
|---|---|---|
| `crapsNewUrlSignals` | String.t | Compressed click/impression data at **URL level** |
| `crapsNewHostSignals` | String.t | Compressed click/impression data at **host level** |
| `crapsNewPatternSignals` | String.t | Compressed click/impression data at **pattern level** |
| `crapsAbsoluteHostSignals` | integer() | Absolute host-level click signals |
| `crapsUnscaledIpPriorBadFraction` | integer() | IP-based spam fraction |

These compressed signals are used in the Mustang system for rapid preliminary scoring before full NavBoost evaluation.

---

## How NavBoost Works

### Data Collection

Two primary sources:

1. **Google Search results** — Which result was clicked, how quickly the user returned, which was the "last click"
2. **Chrome browser data** — Direct visits, navigation patterns, engagement metrics (contradicting Google's public denial)

### Rolling 13-Month Window

- Recent click behavior has the strongest influence
- Seasonal patterns are captured (13 months covers year-over-year)
- Sustained high engagement over months builds ranking strength
- Single viral spikes fade if not sustained

### Query-Level Aggregation

NavBoost aggregates across **all users for a given query**. Individual clicks are noisy; aggregated patterns are stable. Cross-query attribution also occurs — if users search "Rand Fishkin," don't find a result, then search "SparkToro" and click it, SparkToro.com gets boosted for "Rand Fishkin."

### Squashing (Normalization)

Google applies "squashing" functions to normalize click data:
- Reduces influence of outliers
- Prevents manipulation through click farming
- The `unsquashed*` fields contain pre-normalization data
- The regular fields contain post-normalization data

---

## Signal Deep Dives

### goodClicks — The Positive Signal

Indicators that trigger goodClicks classification:
- Long dwell time on the page
- No return to search results
- Continued browsing on the same site
- Task completion signals

Optimization:
- Match content precisely to search intent
- Provide comprehensive answers above the fold
- Ensure fast page load times
- Design clear, scannable content structure

### badClicks — The Demotion Signal

A high badClicks ratio actively demotes a page. Indicators:
- Quick back-button clicks (short dwell time)
- Immediately clicking a different search result
- Returning to SERP within seconds

Common causes:
- Misleading titles or meta descriptions
- Slow-loading pages
- Content that doesn't match query intent
- Intrusive interstitials or pop-ups
- Thin or low-quality content

### lastLongestClicks — The Strongest Signal

The most powerful positive signal in NavBoost. Tracks which result was the **last click** in a search session AND had the **longest dwell time**.

Example: User clicks result A (30s), goes back, clicks result B (10s), goes back, clicks result C (5 min, no return) — result C receives the lastLongestClicks signal.

Optimization:
- Create comprehensive, definitive content that ends the search journey
- Cover the topic thoroughly enough that users don't need other results
- Include supporting media (images, videos, tables) that extend engagement

### unicornClicks — The Quality Panel

Clicks from a special subset of users. Likely a quality-filtered user panel whose behavior is considered more representative or trustworthy. These may receive higher weighting in the NavBoost calculation.

---

## NavBoost as a Twiddler

In Google's ranking architecture:

1. **Ascorer** produces initial rankings based on hundreds of signals
2. **NavBoost Twiddler** re-ranks results based on aggregated click data
3. Other Twiddlers (QualityBoost, RealTimeBoost) apply additional adjustments

NavBoost can **significantly re-order** results from Ascorer's initial ranking. A page with strong traditional SEO signals but poor click metrics can be demoted. Conversely, moderate SEO signals + exceptional click performance → promotion.

---

## Audit Checklist for Click Optimization

- [ ] **Title tag**: Accurately reflects content, compelling enough to earn clicks
- [ ] **Meta description**: Sets correct expectations, includes value proposition
- [ ] **Page speed**: Loads fast enough to prevent impatient back-button clicks
- [ ] **Above-the-fold content**: Immediately relevant to the target query
- [ ] **Content completeness**: Comprehensive enough to be the lastLongestClick
- [ ] **Mobile UX**: No intrusive interstitials, readable layout, touch-friendly
- [ ] **Intent match**: Content format matches search intent
- [ ] **Brand strength**: Direct Chrome traffic (chromeInTotal) indicates recognition
- [ ] **Traffic volume**: Sufficient voterTokenCount for reliable NavBoost data
