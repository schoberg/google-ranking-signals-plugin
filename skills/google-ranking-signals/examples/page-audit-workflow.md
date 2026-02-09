# Page Audit Workflow — Step-by-Step Signal Evaluation

Complete walkthrough for auditing a URL against the leaked Google ranking signals. Follow each step in order, noting findings for the final report.

## Step 1: Gather Page Data

Fetch the target URL and collect:

- **Title tag** and meta description
- **H1 and heading structure** (H2s, H3s)
- **Author byline** and credentials
- **Publish date** and last-modified date
- **Word count** and content structure
- **Internal and external links** (count and destinations)
- **Structured data** (JSON-LD, schema markup)
- **Page speed** (Core Web Vitals if available)
- **Mobile rendering** check

## Step 2: Evaluate NavBoost Potential

Assess whether the page is likely to generate positive click signals:

| Signal | Check | Finding |
|---|---|---|
| goodClicks potential | Does the title accurately promise what the content delivers? | |
| badClicks risk | Is there anything that would cause immediate back-button clicks? | |
| lastLongestClicks potential | Is the content comprehensive enough to end the search journey? | |
| Pogo-sticking risk | Does the page load fast? Is the content immediately visible? | |
| CTR potential | Is the title compelling in a SERP context? | |

**Key question:** If a user searching for the target query clicks this page, will they stop searching?

## Step 3: Evaluate Site Authority

| Signal | Check | Finding |
|---|---|---|
| siteAuthority | Is this an established domain with a recognized brand? | |
| hostAge | How old is the domain? (New domains face sandbox) | |
| PageRank | Does the page have strong inbound links? | |
| homepagePageRank | Does the homepage have strong external links? | |
| trustScore | Is the site transparent with contact info, about page, editorial standards? | |

## Step 4: Evaluate Content Quality

| Signal | Check | Finding |
|---|---|---|
| titlematchScore | Does the title include the target query's key terms? | |
| Token analysis | Is the content substantive with diverse vocabulary? | |
| originalContentScore | Is the content original, or aggregated/scraped? | |
| freshnessDays | When was it last meaningfully updated? | |
| QDF relevance | Is this a time-sensitive topic that rewards freshness? | |
| Date consistency | Do byline date, URL date, and structured data dates match? | |
| Version history | Has the content been updated and maintained over time? | |
| keywordStuffingScore risk | Is the language natural, or does it feel keyword-stuffed? | |

## Step 5: Evaluate Author & Entity Signals

| Signal | Check | Finding |
|---|---|---|
| isAuthor | Is there a named author with a byline? | |
| Author credentials | Are the author's qualifications relevant and verifiable? | |
| Entity recognition | Is the brand/author in Google's Knowledge Graph? | |
| Brand mentions | Are there unlinked mentions of the brand across the web? | |
| E-E-A-T evidence | First-person experience, expertise, authority, trust signals? | |
| Person schema | Is there structured data for the author? | |
| YMYL compliance | If YMYL topic, are elevated credentials and sourcing present? | |

## Step 6: Evaluate Link Signals

| Signal | Check | Finding |
|---|---|---|
| Referring domain count | How many unique domains link to this page? | |
| Source quality | Are linking sites in the high, medium, or low quality tier? | |
| linkDiversityScore | Are links from diverse domain types, IPs, and geographies? | |
| Anchor text distribution | Natural mix of branded, generic, and keyword anchors? | |
| anchorMismatchDemotion risk | Do anchors match the page's actual content? | |
| Link freshness | Are there recent editorial links? | |
| Internal links | Does the page receive sufficient internal links with good anchors? | |
| Homepage links | Does the homepage have strong external links? | |

## Step 7: Evaluate Chrome & UX Signals

| Signal | Check | Finding |
|---|---|---|
| Page speed | Does it pass Core Web Vitals (LCP, CLS, INP)? | |
| Above-the-fold quality | Is the first screen immediately relevant and engaging? | |
| Scroll engagement | Is there valuable content throughout, not just at the top? | |
| Mobile experience | Responsive layout, no intrusive interstitials? | |
| Session potential | Does the page encourage continued site browsing? | |

## Step 8: Check for Demotion Triggers

| Trigger | Check | Finding |
|---|---|---|
| exactMatchDomainDemotion | Is this an EMD relying on domain name rather than quality? | |
| productReviewDemotion | If a review, does it show genuine experience evidence? | |
| Thin content flags | Is the content substantive enough for the topic? | |
| Auto-generated flags | Does the content appear human-written and edited? | |
| Doorway page flags | Is this a real content page, not just a traffic funnel? | |

## Step 9: Compile Findings and Prioritize

### Signal Strength Summary

Rate each category as Strong / Adequate / Weak / Missing:

| Category | Rating | Priority |
|---|---|---|
| NavBoost Potential | | |
| Site Authority | | |
| Content Quality | | |
| Author & Entity | | |
| Link Signals | | |
| Chrome & UX | | |
| Demotion Risks | | |

### Prioritized Recommendations

Organize recommendations by impact and effort:

**Quick Wins (High Impact, Low Effort):**
- Title tag optimization for titlematchScore
- Add/improve meta description for CTR
- Add author byline and credentials
- Fix date consistency across sources
- Add Person/Organization schema markup

**Medium-Term (High Impact, Medium Effort):**
- Content depth improvements for lastLongestClicks
- Internal linking optimization
- Core Web Vitals fixes
- Author bio page and cross-site presence

**Strategic (High Impact, High Effort):**
- Link building for diversity and quality
- Brand recognition campaigns for entity signals
- Site-wide quality improvements for siteAuthority
- Publishing cadence for freshness signals

## Sample Audit Report Format

```
# SEO Audit: [Page Title]
URL: [target URL]
Date: [audit date]
Target Query: [primary keyword]

## Summary
[2-3 sentence overview of findings and top priority]

## Signal Evaluation
[Table from Step 9 with ratings]

## Critical Issues
1. [Most impactful issue with specific attribute reference]
2. [Second issue]
3. [Third issue]

## Recommendations
### Immediate Actions
- [Action 1] — addresses [signal] — estimated impact: [High/Medium/Low]
- [Action 2]

### 30-Day Actions
- [Action 1]
- [Action 2]

### 90-Day Strategy
- [Action 1]
- [Action 2]

## Competitive Context
[How this page's signals compare to top-ranking competitors for the target query]
```
