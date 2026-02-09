# Content Optimization Checklist — Signal-by-Signal Framework

Structured optimization framework organized by signal category. Work through each section, checking items that apply to the content being optimized.

## Pre-Optimization: Identify the Target

Before optimizing, establish:

- **Target query**: The primary search query this content should rank for
- **Query intent**: Informational, navigational, transactional, or local
- **QDF status**: Is this a time-sensitive query that rewards freshness?
- **YMYL classification**: Does this topic require elevated quality standards?
- **Competitive landscape**: What are the top 3-5 results doing well?

---

## Title & Meta (titlematchScore, CTR)

- [ ] Title tag includes primary keyword terms
- [ ] Most important terms appear early in the title
- [ ] Title is under 60 characters (avoids truncation)
- [ ] Title accurately describes the page content (prevents badClicks)
- [ ] Title is compelling enough to earn clicks in a competitive SERP
- [ ] Meta description sets correct expectations and includes a value proposition
- [ ] Meta description is 120-155 characters
- [ ] H1 aligns with title tag (reduces Google title rewriting)

## Content Depth & Quality (contentScore, originalContentScore)

- [ ] Content comprehensively covers the topic (aim for lastLongestClicks)
- [ ] Vocabulary is diverse and natural (healthy uniqueTokens ratio)
- [ ] No keyword stuffing or unnatural repetition
- [ ] Content includes original insights, data, or perspectives
- [ ] Supporting elements: tables, lists, examples, case studies
- [ ] Content format matches query intent (guide for how-to, comparison for vs. queries)
- [ ] Above-the-fold content immediately addresses the query
- [ ] Content is structured with clear H2/H3 headings for scannability

## Freshness (QDF, freshnessDays, dateReliability)

- [ ] Visible publish date on the page (BylineDate parseable)
- [ ] Publish date matches structured data (datePublished in JSON-LD)
- [ ] URL date pattern matches actual publish date (if URL contains date)
- [ ] dateModified reflects the most recent substantive update
- [ ] For QDF-active topics: content is recently published or updated
- [ ] For evergreen topics: last-updated date reflects ongoing maintenance
- [ ] No date manipulation (changing date without changing content)

## Author & Entity (isAuthor, E-E-A-T)

- [ ] Named author with a visible byline
- [ ] Author bio with relevant credentials and expertise
- [ ] Author page linking to other content by the same author
- [ ] Person schema markup for the author
- [ ] Author has cross-site presence (LinkedIn, industry publications)
- [ ] For YMYL: author has verifiable professional qualifications
- [ ] First-person experience evidence where relevant (original photos, testing results)
- [ ] Organization schema for the publisher
- [ ] About page with organizational credibility information

## Internal Linking (PageRank distribution)

- [ ] Page receives internal links from high-authority pages on the site
- [ ] Internal link anchor text is descriptive (not "click here")
- [ ] Related content is linked contextually within the body
- [ ] Breadcrumb navigation provides hierarchical links
- [ ] Page is within 2-3 clicks of the homepage
- [ ] No orphaned pages (every page has at least one internal link)

## Structured Data (SERP feature eligibility)

- [ ] Article/BlogPosting schema with full metadata
- [ ] Author and publisher properties populated
- [ ] datePublished and dateModified set correctly
- [ ] FAQ schema for pages with question-answer content
- [ ] HowTo schema for step-by-step guides
- [ ] Review schema for product reviews (with genuine ratings)
- [ ] Breadcrumb schema for navigation structure

## Technical UX (Chrome & engagement signals)

- [ ] Core Web Vitals pass: LCP < 2.5s, CLS < 0.1, INP < 200ms
- [ ] Mobile-responsive layout
- [ ] No intrusive interstitials or pop-ups
- [ ] Images optimized (compressed, properly sized, lazy-loaded)
- [ ] Clean, readable typography
- [ ] Logical content flow that encourages scrolling
- [ ] Related content sections for extended session duration

## YMYL-Specific (if applicable)

- [ ] Expert author with verifiable credentials in the field
- [ ] Claims sourced with references to authoritative sources
- [ ] Editorial review process mentioned or evident
- [ ] Financial disclosures and disclaimers where needed
- [ ] Content is current (no outdated medical, financial, or legal info)
- [ ] Contact information and organizational transparency
- [ ] Professional presentation appropriate for the topic sensitivity

## Demotion Risk Check

- [ ] No exact-match domain exploitation
- [ ] No keyword stuffing or unnatural language
- [ ] For reviews: genuine experience evidence (original photos, testing data)
- [ ] Not a doorway page (has genuine standalone value)
- [ ] No deceptive practices (hidden text, cloaking, misleading titles)
- [ ] Anchor text of inbound links matches page content

---

## Quick Wins vs. Long-Term Priorities

### Quick Wins (implement immediately)

1. Optimize title tag for titlematchScore and CTR
2. Add/update meta description
3. Add visible publish date and ensure consistency
4. Add author byline with credentials
5. Implement Article schema with author/publisher
6. Fix any above-the-fold content issues

### Medium-Term (1-4 weeks)

1. Deepen content for comprehensiveness
2. Add original research, data, or first-person experience
3. Build author bio page and cross-site presence
4. Improve internal linking to and from the page
5. Optimize Core Web Vitals

### Long-Term (1-3 months)

1. Build diverse, quality backlinks
2. Strengthen brand recognition for entity signals
3. Establish publishing cadence for freshness
4. Develop topical authority through content clusters
5. Grow direct/Chrome traffic through brand awareness
