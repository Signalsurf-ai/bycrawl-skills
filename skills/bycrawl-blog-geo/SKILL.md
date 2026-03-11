---
name: bycrawl-blog-geo
description: >
  AI citation optimization audit with bycrawl social intelligence enrichment.
  Scores blog posts for ChatGPT, Perplexity, and Google AI Overview citability.
  Use when user says "geo audit", "ai citation audit", "ai optimization",
  "citation audit with social data", "aeo", "perplexity optimization",
  "chatgpt citation", "bycrawl geo", "blog geo audit".
  Enhances the standard GEO audit with live social signals for citation capsule
  generation and competitor citation analysis.
allowed-tools:
  - Read
  - Write
  - Grep
  - Glob
  - WebFetch
  - "mcp__bycrawl__*"
user-invocable: true
argument-hint: "<blog-file-or-url> e.g. ./blog/ai-coding-tools.md"
---

# ByCrawl Blog GEO -- AI Citation Optimization Audit

Scores blog posts for AI citation readiness across ChatGPT, Perplexity, and
Google AI Overviews. Enhanced with live social intelligence from bycrawl to
generate stronger citation capsules, validate entity prominence, and benchmark
against competitor content that AI systems are already citing.

The key advantage: instead of generating citation capsules from the post alone,
you pull real social signals — Reddit threads Perplexity favors, YouTube content
ChatGPT references, trending discussions that prove topic authority — and weave
them into actionable recommendations.

## Key Research Data

Reference these benchmarks throughout the audit:

- Only 12% of sources cited match across ChatGPT, Perplexity, and AI Overviews
- 80% of LLM citations don't rank in Google's top 100
- Brands 6.5x more likely cited through third-party sources
- 120-180 word sections get 70% more ChatGPT citations
- Comparison tables with `<thead>` achieve 47% higher AI citation rates
- Content freshness: 76.4% of top citations updated within 30 days

## Audit Process

### Step 1: Read Content

Extract from the blog post:
- Full content text and word count
- Heading structure (H1, H2, H3 hierarchy)
- Individual paragraphs and their word counts
- FAQ sections (if present)
- Schema markup (JSON-LD, microdata, RDFa)
- robots.txt mentions or meta robots directives
- Any TL;DR or summary boxes
- Comparison tables and their HTML structure
- Numbered/ordered lists
- Definition-style formatting

**Identify the primary topic and 2-3 secondary keywords** from the content — you'll need these for bycrawl enrichment.

### Step 2: Passage-Level Citability (4 pts)

Check each section between headings for AI-extractable passages:

| Check | Criteria |
|-------|----------|
| Word count | Each section contains 120-180 word self-contained passages |
| Context independence | Each passage makes sense extracted from surrounding context |
| Claim structure | Passages contain: specific claim + supporting evidence + source attribution |
| Completeness | Passage answers a question without requiring reader to read adjacent sections |

**Scoring:** Count passages meeting all criteria vs total sections.
- 4 pts: 80%+ sections have citable passages
- 3 pts: 60-79%
- 2 pts: 40-59%
- 1 pt: 20-39%
- 0 pts: <20%

### Step 3: Q&A Formatting (3 pts)

Check heading format and answer structure:

| Check | Criteria |
|-------|----------|
| Question headings | 60-70% of H2s are phrased as questions |
| Answer-first format | Opening paragraph under each H2 provides a direct answer |
| FAQ section | Dedicated FAQ section with structured question-answer pairs |

**Scoring:**
- 3 pts: All three criteria met
- 2 pts: Two criteria met
- 1 pt: One criterion met
- 0 pts: None met

### Step 4: Entity Clarity (3 pts)

Check topic consistency and disambiguation:

| Check | Criteria |
|-------|----------|
| Canonical topic | One unambiguous primary topic per page |
| Consistent naming | Same entity name used throughout (no confusing synonyms) |
| Intro statement | Clear topic statement in the introduction paragraph |
| Title-content match | Title accurately reflects the content focus |

**Scoring:**
- 3 pts: All four criteria met
- 2 pts: Three criteria met
- 1 pt: One or two criteria met
- 0 pts: None met

### Step 5: Content Structure for Extraction (3 pts)

Check for AI-extractable content patterns:

| Check | Criteria |
|-------|----------|
| TL;DR box | 40-60 word standalone summary present at top |
| Comparison tables | Tables with proper HTML `<thead>` (47% higher citation rate) |
| Ordered lists | Numbered lists for processes and step-by-step instructions |
| Definition formatting | Key terms formatted with clear definition patterns |
| Citation capsules | 40-60 word definitive statements in each major section |

**Scoring:**
- 3 pts: 4-5 elements present
- 2 pts: 3 elements present
- 1 pt: 1-2 elements present
- 0 pts: None present

### Step 6: AI Crawler Accessibility (2 pts)

Check technical requirements for AI crawler indexing:

| Check | Criteria |
|-------|----------|
| Static HTML | Content rendered in static HTML, not behind JavaScript |
| robots.txt | Allows AI crawlers: GPTBot, ChatGPT-User, ClaudeBot, PerplexityBot |
| Schema in HTML | Schema markup in static HTML, not JS-injected |
| Page size | Reasonable page size within AI crawler limits |

**Scoring:**
- 2 pts: All criteria met
- 1 pt: Most criteria met but one issue
- 0 pts: Multiple issues blocking AI crawlers

---

## ByCrawl Social Intelligence Enrichment

After completing the base audit (Steps 1-6), ask the user:

> "Would you like me to enrich this GEO audit with live social data from bycrawl? This will:
> 1. Find what AI systems are already citing on this topic (competitor citation landscape)
> 2. Pull Reddit threads and community-validated content Perplexity favors
> 3. Mine real data points and expert quotes to strengthen citation capsules
> 4. Validate entity prominence across social platforms
>
> This uses ~25-40 bycrawl API calls. (yes/no)"

- **User says yes** → Run the bycrawl enrichment below, then continue to Steps 7-10.
- **User says no** → Skip directly to Steps 7-10 using only the base audit data.

### ByCrawl Step A: Competitor Citation Landscape

Find what content AI systems are likely citing on this topic by identifying the most authoritative social content:

```
reddit_search_posts(query="{primary_topic}", sort="top", count=20)
reddit_search_posts(query="{primary_topic} explained", sort="top", count=15)
youtube_search_videos(q="{primary_topic}", count=15)
x_search_posts(query="{primary_topic} guide OR explained OR how to", product="Top", count=20)
```

For top 3 Reddit posts with high engagement:
```
reddit_get_post(postId="{id}")
```

For top 3 YouTube videos:
```
youtube_get_video(videoId="{id}")
youtube_get_video_transcription(videoId="{id}", language="en")
```

**Extract:**
- Which sources/domains appear repeatedly (these are likely already being cited by AI)
- What specific data points and claims get the most engagement (= citation-worthy content)
- Authoritative voices on this topic (experts AI systems may preferentially cite)

### ByCrawl Step B: Reddit & Community Signals (Perplexity Focus)

Perplexity disproportionately cites Reddit (6.6% of all citations). Mine Reddit for the exact language and questions the audience uses:

```
reddit_search_posts(query="{primary_topic}", sort="relevance", count=20)
reddit_search_posts(query="{primary_keyword} ELI5 OR explain OR help", sort="top", count=15)
```

For top 3 posts with substantial discussion:
```
reddit_get_post(postId="{id}")
```

**Extract:**
- Top questions asked about this topic (= Q&A headings the post should have)
- Community-validated answers that get upvoted (= claims worth echoing)
- Specific pain points and use cases mentioned (= passages to create)
- Subreddits where this topic is discussed (= entity context for AI)

### ByCrawl Step C: Data Point Mining for Citation Capsules

Citation capsules need specific claims with evidence. Mine social for real data points:

```
x_search_posts(query="{primary_topic} percent OR % OR data OR study OR research", product="Top", count=20)
youtube_search_videos(q="{primary_topic} data OR statistics OR benchmark", count=10)
reddit_search_posts(query="{primary_topic} numbers OR stats OR comparison", sort="top", count=15)
```

For YouTube videos with data:
```
youtube_get_video_transcription(videoId="{id}", language="en")
```

**Extract:**
- Specific statistics and benchmarks mentioned across platforms
- Expert claims with attribution (name + credential + claim)
- Comparison data that could populate tables
- Recent data points (within 30 days = high citation freshness signal)

### ByCrawl Step D: Entity Prominence Validation

Check how the post's primary entity/brand is discussed across platforms to validate entity clarity:

```
x_search_posts(query="{entity_or_brand_name}", product="Top", count=15)
reddit_search_posts(query="{entity_or_brand_name}", sort="relevance", count=15)
youtube_search_videos(q="{entity_or_brand_name}", count=10)
```

**Extract:**
- How the entity is referred to across platforms (official name vs nicknames vs abbreviations)
- Whether the entity is associated with the topic in social discussions
- Third-party mentions (brands 6.5x more likely cited through third-party sources)
- Sentiment signals that affect authority perception

---

## Steps 7-10: Analysis & Report Generation

### Step 7: Platform-Specific Analysis

Evaluate the post for each AI platform's citation preferences.

#### ChatGPT
- Favors "Best X" listicles (43.8% of citations)
- Prefers well-cited, authoritative content
- Recency matters — recent updates get priority
- Domain authority influences citation likelihood

**If bycrawl enrichment was run:** Cross-reference the competitor citation landscape (Step A) — flag if competitor content on YouTube or authoritative blogs is more likely to be cited than this post, and explain why.

#### Perplexity
- Favors Reddit sources (6.6% of all citations)
- Rapid content decay: 2-3 day citation window
- Freshness is the most critical factor
- Community-validated content preferred

**If bycrawl enrichment was run:** Use Reddit signals (Step B) to identify the exact questions and answer patterns Perplexity is likely pulling from. Recommend specific Q&A headings that mirror high-engagement Reddit threads.

#### Google AI Overviews
- Favors Google properties (23% of citations)
- High Domain Rating strongly correlated with citation
- Present in 49% of SERPs
- Prefers content that already ranks well organically

**If bycrawl enrichment was run:** Use YouTube data (Step A) to recommend whether a companion video could increase citation likelihood via Google's preference for its own properties.

For each platform, provide:
- Current citability rating (High / Medium / Low)
- Specific improvements to increase citation likelihood
- Content format recommendations

### Step 8: Generate Citation Capsules

For each H2 section in the post, write a citation capsule:

- **Length**: 40-60 words, self-contained
- **Structure**: Specific claim + data point + source attribution
- **Purpose**: A passage AI could directly quote as a citation
- **Format**: Present as a suggested addition the author can embed

**If bycrawl enrichment was run:** Use the data points mined in Step C to generate capsules backed by real statistics and expert claims found on social platforms. Each capsule should reference a verifiable source discovered during enrichment. This produces dramatically stronger capsules than generating from the post alone.

**Without bycrawl enrichment:**

Example:
```
According to [Source], [specific claim with number]. This represents
[context/comparison], making it [significance]. [Supporting detail
that reinforces the claim].
```

Generate one capsule per H2 section. Label each with the section heading it belongs under.

### Step 9: Calculate AI Citation Readiness Score (0-100)

Map the 15-point subcategory scores to a 0-100 display score:

| Category | Raw Points | Display Weight | Max Display Score |
|----------|-----------|----------------|-------------------|
| Passage-Level Citability | /4 | x6.75 | 27 |
| Q&A Formatting | /3 | x6.67 | 20 |
| Entity Clarity | /3 | x6.67 | 20 |
| Content Structure | /3 | x6.67 | 20 |
| AI Crawler Accessibility | /2 | x6.5 | 13 |
| **Total** | **/15** | | **100** |

Rating thresholds:
- 90-100: Excellent — highly citable by AI systems
- 70-89: Good — citable with minor improvements
- 50-69: Needs Work — significant gaps in citability
- Below 50: Poor — major restructuring needed

### Step 10: Generate Report

Output the following report:

```
## AI Citation Readiness Report: [Title]

**AI Citation Readiness Score: [X]/100** — [Rating]

### Score Breakdown
| Category | Raw | Display | Max |
|----------|-----|---------|-----|
| Passage-Level Citability | X/4 | X | 27 |
| Q&A Formatting | X/3 | X | 20 |
| Entity Clarity | X/3 | X | 20 |
| Content Structure | X/3 | X | 20 |
| AI Crawler Accessibility | X/2 | X | 13 |
| **Total** | **X/15** | **X** | **100** |

### Per-Section Citability Analysis
| Section (H2) | Word Count | Self-Contained | Claim+Evidence | Citable |
|---------------|-----------|----------------|----------------|---------|
| [heading] | [N] | Yes/No | Yes/No | Yes/No |

### Platform-Specific Optimization
#### ChatGPT
- [specific recommendations]

#### Perplexity
- [specific recommendations]

#### Google AI Overviews
- [specific recommendations]

### Generated Citation Capsules

#### [H2 Section 1]
> [40-60 word citation capsule]
> **Source**: [social data source if bycrawl enrichment was used]

#### [H2 Section 2]
> [40-60 word citation capsule]
> **Source**: [social data source if bycrawl enrichment was used]

### ByCrawl Social Intelligence Findings
(Only include this section if bycrawl enrichment was run)

#### Competitor Citation Landscape
- [Top sources AI is likely already citing on this topic]
- [Content gaps where this post could win citations]

#### Reddit/Community Signals (Perplexity)
- [Top questions from Reddit this post should answer]
- [Community-validated claims to echo]

#### Entity Prominence
- [How the entity is discussed across platforms]
- [Third-party mention opportunities]

### Technical Recommendations
- [ ] [Technical fix with specifics]

### Priority Action Items
1. [Most impactful improvement]
2. [Second most impactful]
3. [Third most impactful]
```

---

## Error Handling

- If a bycrawl platform returns **empty results**, skip it and note "No data found on {platform}" — continue with the data you have.
- If the **ByCrawl API key is missing or invalid**, inform the user and proceed with the standard audit (Steps 1-6, 7-10 without enrichment).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx -y @bycrawl/mcp`).
- **Always deliver partial results** — an audit with 3 of 4 enrichment steps is still better than no social data.
- **If user says no to bycrawl enrichment**, proceed with the standard audit without delay.

## Estimated API Usage

| Enrichment Step | API Calls |
|---|---|
| Step A: Competitor Citation Landscape | ~10-15 |
| Step B: Reddit & Community Signals | ~5-10 |
| Step C: Data Point Mining | ~8-12 |
| Step D: Entity Prominence Validation | ~5-8 |
| **Total (all enriched)** | **~25-40** |

Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive usage.

## Prerequisites

- **ByCrawl MCP server** must be installed and configured:
  ```
  claude mcp add bycrawl -- npx -y @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

---

## Related Skills

- **bycrawl-content-research**: Standalone content gap analysis from social platforms.
- **bycrawl-competitor-intel**: Standalone competitor social audit.
- **bycrawl-social-keyword-signals**: Trending keyword discovery from social.
- **bycrawl-seo-blog-production**: Full blog production pipeline with bycrawl enrichment.
- **content-production**: Base content pipeline without social data.
