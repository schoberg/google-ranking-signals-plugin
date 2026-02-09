# Author & Entity Signals — WebRef Entities, Author Identity & E-E-A-T

The leak reveals Google's entity recognition and author identification through several modules. `RepositoryWebrefEntityJoin` (12 fields) connects documents to Knowledge Graph entities. `PerDocData` contains author identity fields and entity annotations. `RepositoryWebrefDocumentMetadata` and related modules handle entity mention detection and scoring.

**Source module documentation:** [HexDocs](https://hexdocs.pm/google_api_content_warehouse/0.4.0/api-reference.html)

---

## RepositoryWebrefEntityJoin — Knowledge Graph Connection

The primary module connecting documents to Google's Knowledge Graph entities. 12 fields that determine how a page relates to known entities.

| Field | Type | Description |
|---|---|---|
| `annotatedEntityId` | String.t | **Knowledge Graph entity ID** for entities mentioned on the page |
| `webrefEntities` | list(nested) | All entity annotations found in the document |
| `nameInfo` | nested | Entity name and variant information |
| `linkInfo` | nested | How the entity is linked/referenced |
| `entityScore` | number() | **Confidence score** for entity identification |
| `mentionCount` | integer() | How many times the entity is mentioned |
| `isMainEntity` | boolean() | Whether this is the page's primary/main entity |
| `topicalityScore` | number() | How central the entity is to the page's topic |
| `entityType` | integer() | Classification type (person, organization, location, etc.) |
| `confidenceScore` | number() | Confidence in the entity match |
| `salientTerms` | list(String.t) | Key terms associated with the entity on this page |
| `relatedEntities` | list(nested) | Other entities connected to this one |

### annotatedEntityId — Knowledge Graph Linkage

When Google matches content to a Knowledge Graph entity (via `annotatedEntityId`), the page inherits trust and context from that entity's graph entry. This is fundamental to how Google understands "aboutness."

Benefits of entity recognition:
- Page is associated with a known, vetted entity
- Related queries can surface the page through entity connections
- Knowledge Panel eligibility increases
- Entity authority transfers to the content

### isMainEntity — Primary Topic Entity

Boolean flag identifying which entity is the page's primary subject. Google uses this to:
- Determine what the page is fundamentally "about"
- Match the page to entity-related queries
- Build topical authority maps for the domain
- Evaluate content-entity alignment

### topicalityScore — Entity Centrality

Measures how central an entity is to the page's content. A high topicality score means the entity is deeply covered, not just mentioned in passing. Influences:
- How strongly the page ranks for entity-related queries
- Entity-based query matching in retrieval
- Topical authority assessment for the site

---

## PerDocData — Author & Entity Fields

Author and entity identity signals within the master per-document module:

| Field | Type | Description |
|---|---|---|
| `authorObfuscatedGaiaStr` | String.t | **Obfuscated Google account ID** of the content author |
| `socialgraphNodeNameFp` | integer() | Social graph node fingerprint for entity matching |
| `webrefEntities` | nested | Document-level entity annotations |
| `isAuthor` | boolean() | Whether a recognized author entity is detected |
| `authorName` | String.t | Parsed author name from byline |
| `authorUrl` | String.t | URL associated with the author entity |

### authorObfuscatedGaiaStr — Google Account Author ID

This is a significant revelation: Google ties content to specific Google account IDs (obfuscated). This means:

- Google can track authorship across all content linked to a Google account
- Author reputation accumulates across different sites and publications
- The author's publishing history, quality track record, and expertise areas are trackable
- This likely powers author-level quality assessments

**Building author identity:**
- Maintain a consistent Google account across all publishing activities
- Link the Google account to an author profile on each publishing platform
- Use the same Google account for Google Search Console verification
- Build a consistent publishing track record under the same identity

### socialgraphNodeNameFp — Social Graph Integration

A fingerprint connecting the entity to Google's social graph. This enables:
- Cross-platform identity matching (website, YouTube, social media)
- Entity disambiguation (distinguishing people with the same name)
- Relationship mapping between entities
- Authority verification through connected profiles

---

## RepositoryWebrefDocumentMetadata — Document Entity Context

| Field | Type | Description |
|---|---|---|
| `documentEntityCount` | integer() | Total entities recognized in the document |
| `dominantEntityScore` | number() | Score of the most prominent entity |
| `entityDensity` | number() | Entity mention density in content |
| `entityDiversity` | number() | Variety of entity types present |

### Entity Recognition Quality

The quality of entity recognition on a page depends on:
- Clear, unambiguous entity references
- Contextual clues that help disambiguation
- Structured data markup reinforcing entity identity
- Consistent naming and terminology

---

## RepositoryWebrefGlobalNameInfo — Entity Name Resolution

| Field | Type | Description |
|---|---|---|
| `name` | String.t | Canonical entity name |
| `alternateNames` | list(String.t) | Known alternate names/aliases |
| `languageSpecificNames` | list(nested) | Name variants by language |

This module handles entity name resolution — matching text mentions to known entities even when different names or spellings are used. For brand and author SEO:
- Use a consistent canonical name across all platforms
- Ensure common variations and misspellings are associated
- Include full legal name and common abbreviations

---

## RepositoryWebrefGlobalLinkInfo — Entity Web Presence

| Field | Type | Description |
|---|---|---|
| `officialUrl` | String.t | Entity's official website URL |
| `relatedUrls` | list(String.t) | Other URLs associated with the entity |
| `socialProfileUrls` | list(String.t) | Social media profile URLs |
| `wikiUrl` | String.t | Wikipedia/Wikidata URL |

### Building Entity Web Presence

Google assembles an entity's web presence from multiple sources:
- **Official website**: The primary URL associated with the entity
- **Social profiles**: LinkedIn, Twitter/X, YouTube, etc.
- **Wikipedia/Wikidata**: Presence here significantly strengthens entity recognition
- **Authoritative directories**: Industry databases, professional registrations
- **News mentions**: Media coverage linking to the entity

---

## Brand Mentions Without Links

One of the most significant revelations from the leak: Google's entity recognition can detect **unlinked brand mentions** and use them similarly to backlinks.

How it works:
- The entity recognition system identifies brand/entity names in content across the web
- These mentions are associated with the entity even without hyperlinks
- Mention context (positive, neutral, negative) may influence entity sentiment
- Volume of mentions across authoritative sources builds entity authority

Implications for SEO:
- PR and media coverage contributes to rankings even without links
- Social media brand mentions have potential ranking value
- Guest appearances (podcasts, interviews, conferences) build entity signals
- Review sites and directories that mention the brand contribute
- Consistent brand messaging across the web strengthens entity scores

---

## E-E-A-T Implementation Through Leaked Signals

The leak shows how E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness) maps to concrete attributes:

### Experience
- `authorObfuscatedGaiaStr` — tracks the author's publishing history
- `OriginalContentScore` (from content-quality) — first-hand content vs. aggregated
- Product review signals (`productReviewPUhqPage`) — evidence of actual product use

### Expertise
- `topicalityScore` — depth of entity coverage
- `authorName` + cross-site authority — author presence on multiple authoritative publications
- `contentEffort` (from content-quality) — investment in content creation

### Authoritativeness
- `siteAuthority` (from site-authority) — domain-level authority score
- `annotatedEntityId` — Knowledge Graph recognition
- `isMainEntity` — authoritative coverage of the entity
- `uniqueDomainCount` (from links) — breadth of recognition across the web

### Trustworthiness
- `spambrainTotalDocSpamScore` (from site-authority) — absence of spam signals
- `unauthoritativeScore` (from site-authority) — lack of authority penalties
- `homePageInfo` trust levels (from links) — domain trust classification
- `dateReliability` (from content-quality) — trustworthy date signals

---

## Structured Data for Entity Signals

Schema markup directly supports entity recognition:

**For authors (Person schema):**
- `name`, `jobTitle`, `worksFor` — identity and credentials
- `sameAs` — links to social profiles for `socialgraphNodeNameFp` matching
- `author` property on `Article`/`BlogPosting` — connects content to author entity

**For organizations (Organization schema):**
- `name`, `logo`, `sameAs` — brand identity
- `contactPoint` — trust signals
- `publisher` property — connects content to organization entity

**For content (Article/BlogPosting schema):**
- `author` and `publisher` — entity associations
- `datePublished`, `dateModified` — freshness signals
- `about` — links to relevant topic entities (reinforces `topicalityScore`)

---

## Audit Checklist for Author & Entity Signals

- [ ] **annotatedEntityId**: Brand/author appears in Google Knowledge Graph
- [ ] **authorObfuscatedGaiaStr**: Author has a consistent Google account identity
- [ ] **isMainEntity**: Page clearly focuses on a recognizable entity
- [ ] **topicalityScore**: Deep, substantive coverage of the entity (not just passing mentions)
- [ ] **Entity web presence**: Official URL, social profiles, Wikipedia/Wikidata, industry databases
- [ ] **Brand mentions**: Unlinked mentions across authoritative external sources
- [ ] **Person schema**: Structured data for author with sameAs links to social profiles
- [ ] **Organization schema**: Full structured data for the publishing organization
- [ ] **Cross-site presence**: Author/brand mentioned and linked from authoritative external sites
- [ ] **YMYL compliance**: For sensitive topics, verifiable author credentials and sourcing
- [ ] **Consistent naming**: Same brand/author name used consistently across all platforms
- [ ] **Experience evidence**: First-person perspective, original media, specific hands-on details
