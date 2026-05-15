# Page Audit Workflow — Step-by-Step Signal Evaluation

Complete walkthrough for auditing a URL against the leaked Google ranking signals. Follow each step in order, noting findings for the final report.

## Step 1: Gather Page Data

`<head>` analysis **and structured data detection** must use raw HTML. WebFetch converts HTML to markdown before its summarizer runs, which strips:

- `<meta>` and `<link>` tags (entire `<head>` content)
- `<script>` tags, including `<script type="application/ld+json">` blocks **wherever they appear** in the document (in `<head>` OR `<body>` — JSON-LD is valid in both)

No WebFetch prompt can recover this content — verified empirically against shopify.com (1 JSON-LD block present, WebFetch reports zero). Use curl for `<head>` and structured data; use WebFetch for visible content only.

Caveat: WebFetch *can* return JSON-LD that appears as **visible page content** (e.g., docs pages like schema.org showing code examples in `<pre>`/`<code>` blocks). That's documentation, not real schema markup, and shouldn't count as the page having structured data.

### 1a. Raw HTML for `<head>` (authoritative)

Run via Bash:

```
curl -sL --compressed --max-time 10 -A 'Mozilla/5.0 (compatible; ClaudeCodeSEOAudit/1.0)' <url>
```

From the response, extract the `<head>...</head>` block (the block can be on a single line with no newlines — extract using a regex that handles this) and read it verbatim. Capture every:

- `<meta>` tag (name, property, content)
- `<title>` tag
- `<link>` tag (rel, href) — canonical, alternate, hreflang, icons, preload, etc.
- `<script>` tag (src attribute)
- Open Graph and Twitter Card tags (these appear as `<meta property="og:*">` and `<meta name="twitter:*">`)

**Then search the entire document (not just `<head>`) for structured data.** This is critical — JSON-LD, microdata, and RDFa are all valid in `<body>` and many sites (Carrd, Squarespace, WordPress themes) place JSON-LD outside `<head>`. Search the full HTML response for:

- **JSON-LD blocks**: `<script type="application/ld+json">...</script>` — anywhere in the document. Parse and report the `@type` of each block (Person, Organization, Article, BreadcrumbList, etc.).
- **Microdata**: any element with `itemscope`, `itemtype`, or `itemprop` attributes. Note the `itemtype` URL (e.g. `https://schema.org/Person`).
- **RDFa**: any element with `typeof`, `vocab`, or `property` attributes (less common but still valid).

Only report structured data as missing if **all three formats** are absent from the entire document. A false "no schema" finding wastes the user's time and undermines the audit's credibility.

**Curl is unusable for `<head>` analysis if any of:**

- HTTP status is non-2xx
- Response body is under 1000 bytes (typical bot challenge stubs are 500–1000 bytes)
- The extracted `<head>` contains zero `<meta>` tags. This reliably indicates a JS-rendered shell, a bot challenge page (Cloudflare, Datadome, TikTok-style "Please wait..."), or a stub. A real audit-worthy page will always have at least viewport and charset meta tags.
- Bash permission was declined or curl is unavailable on the system

### 1b. Visible content (WebFetch)

Always use WebFetch for content where summarization is acceptable:

- **H1 and heading structure** (H2s, H3s)
- **Author byline** and credentials
- **Publish date** and last-modified date
- **Word count** and content structure
- **Internal and external links** (count and destinations)
- **Page speed** notes (Core Web Vitals if available)
- **Mobile rendering** check

### 1c. Confidence rule for the final report

When curl was unusable, **do not** attempt to use WebFetch as a fallback for `<head>` or for structured data — it cannot return either. Instead:

- **curl succeeded** → `<head>` findings AND structured data findings are authoritative. Report present/absent with confidence.
- **curl unusable** → Mark every `<head>`-related AND structured-data finding as **"could not verify — raw HTML unavailable"**. This includes:
  - All `<meta>` tags, `<title>`, `<link>`, canonical, robots, viewport, etc.
  - All Open Graph and Twitter Card tags
  - All JSON-LD blocks (in `<head>` or `<body>`)
  - All microdata and RDFa attributes
  
  Run the diagnostic in 1d below and include a "Why we couldn't verify technical SEO" section in the audit output with the cause and recovery steps.

Always include a line in the audit output such as: *"Raw HTML source: curl (authoritative — `<head>` and structured data verified)"* — or — *"Raw HTML source: unavailable (Cloudflare bot challenge). See 'Why we couldn't verify technical SEO' below. Visible content findings below are from WebFetch."*

### 1d. Diagnose why curl was unusable

When curl returns unusable data, classify the cause and tell the user how to recover. Inspect the HTTP status, body size, and body content against this table.

**First, ask whether this is the user's own site.** Most audits are own-site audits, and that opens up easier recovery paths than third-party-site recovery — the user has direct access to the CMS, template files, origin server, and an authenticated browser session. If you don't already know, ask in your first response: *"Is this your own site, or one you're auditing externally?"* and tailor the recovery suggestions accordingly. The table below lists both paths.

| Signal in response | Likely cause | Recovery — own site | Recovery — third-party site |
|---|---|---|---|
| Body contains `cf-chl-bypass`, `cf-mitigated`, "Checking your browser", "Just a moment...", "Please wait..." | **Cloudflare / Datadome / Akamai bot challenge** | Allowlist your IP in Cloudflare/WAF temporarily, fetch from the origin server (bypassing the CDN), or open the page in your browser and paste `<head>` from DevTools → Elements. | Re-run with a browser UA: `curl -sL --compressed -A 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)' <url>`. If still blocked, open in a browser → View Page Source → paste the `<head>` section. |
| Body has `<div id="root">`, `<div id="app">`, `data-reactroot`, `__NEXT_DATA__`, or near-empty `<body>` with large JS bundles | **JavaScript-rendered (SPA shell)** | Check the template/component that builds `<head>` (e.g. Next.js `<Head>`, React Helmet, Vue meta) — that's the source of truth. Or paste the rendered `<head>` from DevTools after the page loads. | Open the page in a browser, wait for full load, then in DevTools → Elements → copy the `<head>` element and paste it. |
| Body contains `<input type="password">`, `name="login"`, `class="paywall"`, or redirects to a login URL | **Auth wall / paywall** | Log in to the site in your browser, then paste `<head>` from DevTools → Elements. Or run curl with `--cookie` and your session cookie. | Same — but you'll need credentials. If you don't have them, the page isn't auditable as a search-engine-visible page. |
| HTTP 401 / 403 with normal-looking body | **Server rejected unauthenticated request** | Check your server/CMS auth config (sometimes staging sites accidentally require auth). Or paste HTML from a logged-in browser. | Provide an authenticated curl command or paste HTML from a logged-in browser. |
| HTTP 429 | **Rate limited** | Check your server's rate-limit config — if you're hitting your own rate limit, raise it for trusted IPs. | Wait a minute and retry, or paste HTML manually. |
| HTTP 451 | **Geoblocked** | Check geo-blocking rules in your CDN/WAF — they may be misconfigured. | Try from a different network/VPN, or paste HTML. |
| HTTP 5xx | **Server error** | Check server logs — this is a real bug on the site. Retry once the issue is fixed. | Retry in a few minutes; usually transient. |
| curl exit code non-zero, no body | **Network/DNS failure** | Verify DNS, SSL cert, and that the server is up. | Verify the URL is correct and the site is reachable. |
| Bash permission denied or curl not installed | **Tool unavailable** | Either approve the curl command for this session, or paste raw HTML (View Page Source → copy `<head>`). | Same. |
| Body looks normal but contains zero `<meta>` tags | **Unusual rendering or unknown** | Check your CMS/template — the `<head>` may genuinely be empty (a real SEO issue worth flagging). Or paste rendered HTML from DevTools. | Paste rendered HTML from DevTools → Elements to verify. |

Include the matched cause and recovery message in the audit output as a clearly labeled section. For an own-site audit:

```
## Why <head> couldn't be verified
Cause: JavaScript-rendered shell (raw HTML contained <div id="root"> with empty <head>)
Since this is your site, the easiest fixes:
  - Check the component or template that builds <head> (Next.js <Head>,
    React Helmet, Vue meta, etc.) — that's the authoritative source.
  - Or open the page in your browser, let it load, then in DevTools →
    Elements → right-click the <head> element → "Copy element" and paste here.
```

For a third-party audit, swap in the third-party recovery text from the table.

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
