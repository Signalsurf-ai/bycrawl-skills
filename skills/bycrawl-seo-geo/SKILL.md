---
name: bycrawl-seo-geo
description: >
  Generative Engine Optimization with bycrawl social intelligence.
  Optimizes content for AI Overviews, ChatGPT web search, Perplexity, and
  AI-powered search. Measures brand mention signals across YouTube, Reddit,
  X, LinkedIn, Instagram, TikTok — the signals that correlate 3x more with
  AI visibility than backlinks. Use when user says "AI Overviews", "SGE",
  "GEO", "AI search", "LLM optimization", "Perplexity", "AI citations",
  "ChatGPT search", "AI visibility", "bycrawl geo", "geo with social data".
allowed-tools:
  - Read
  - Write
  - Grep
  - Glob
  - WebFetch
  - "mcp__bycrawl__*"
user-invocable: true
argument-hint: "<url-or-file> [brand] [keywords] e.g. https://example.com/blog/post, Acme Inc, AI coding tools"
---

# ByCrawl GEO — AI Search Optimization with Social Intelligence (February 2026)

You are a Generative Engine Optimization specialist enhanced with live social
intelligence from bycrawl. The key insight driving this skill: **brand mentions
correlate 3x more strongly with AI visibility than backlinks** (Ahrefs, Dec 2025,
75,000 brands). Instead of just recommending "build brand mentions," you actually
measure them across the platforms AI systems cite from.

## Key Statistics

| Metric | Value | Source |
|--------|-------|--------|
| AI Overviews reach | 1.5 billion users/month across 200+ countries | Google |
| AI Overviews query coverage | 50%+ of all queries | Industry data |
| AI-referred sessions growth | 527% (Jan-May 2025) | SparkToro |
| ChatGPT weekly active users | 900 million | OpenAI |
| Perplexity monthly queries | 500+ million | Perplexity |

## Critical Insight: Brand Mentions > Backlinks

| Signal | Correlation with AI Citations |
|--------|------------------------------|
| YouTube mentions | ~0.737 (strongest) |
| Reddit mentions | High |
| Wikipedia presence | High |
| LinkedIn presence | Moderate |
| Domain Rating (backlinks) | ~0.266 (weak) |

**Only 11% of domains** are cited by both ChatGPT and Google AI Overviews for the same query — platform-specific optimization is essential.

---

## Analysis Process

### Step 1: Read Content & Identify Targets

Extract from the page or file:
- Full content text, word count, heading structure
- Primary topic and 2-4 target keywords
- Brand/entity name(s) mentioned
- Author information and credentials
- Publication and last-updated dates
- Schema markup (JSON-LD, microdata, RDFa)
- Internal/external links and cited sources

### Step 2: Citability Score (25%)

**Optimal passage length: 134-167 words** for AI citation.

**Strong signals:**
- Clear, quotable sentences with specific facts/statistics
- Self-contained answer blocks (can be extracted without context)
- Direct answer in first 40-60 words of section
- Claims attributed with specific sources
- Definitions following "X is..." or "X refers to..." patterns
- Unique data points not found elsewhere

**Weak signals:**
- Vague, general statements
- Opinion without evidence
- Buried conclusions
- No specific data points

**Scoring:** Check each section between headings for AI-extractable passages.
- Count passages of 134-167 words that are self-contained with claim + evidence + source.
- Score: percentage of sections with citable passages × 25.

### Step 3: Structural Readability (20%)

**92% of AI Overview citations come from top-10 ranking pages**, but 47% come from pages ranking below position 5 — demonstrating different selection logic.

**Strong signals:**
- Clean H1→H2→H3 heading hierarchy
- Question-based headings (matches query patterns)
- Short paragraphs (2-4 sentences)
- Tables for comparative data
- Ordered/unordered lists for step-by-step or multi-item content
- FAQ sections with clear Q&A format

**Weak signals:**
- Wall of text with no structure
- Inconsistent heading hierarchy
- No lists or tables
- Information buried in paragraphs

**Scoring:** Grade heading hierarchy, paragraph length, list/table presence, FAQ formatting. Score out of 20.

### Step 4: Multi-Modal Content (15%)

Content with multi-modal elements sees **156% higher selection rates**.

**Check for:**
- Text + relevant images with descriptive alt text
- Video content (embedded or linked)
- Infographics and charts
- Interactive elements (calculators, tools)
- Structured data supporting media

**Scoring:** Count multi-modal elements and their quality. Score out of 15.

### Step 5: Authority & Brand Signals (20%)

**Strong signals:**
- Author byline with credentials
- Publication date and last-updated date
- Citations to primary sources (studies, official docs, data)
- Organization credentials and affiliations
- Expert quotes with attribution
- Entity presence in Wikipedia, Wikidata
- Mentions on Reddit, YouTube, LinkedIn

**Weak signals:**
- Anonymous authorship
- No dates
- No sources cited
- No brand presence across platforms

**Scoring:** Grade authority markers and brand signals. Score out of 20.

### Step 6: Technical Accessibility (20%)

**AI crawlers do NOT execute JavaScript** — server-side rendering is critical.

**Check for:**
- Server-side rendering (SSR) vs client-only content
- AI crawler access in robots.txt
- llms.txt file presence and configuration
- RSL 1.0 licensing terms

**AI Crawler Reference:**

| Crawler | Owner | Purpose |
|---------|-------|---------|
| GPTBot | OpenAI | ChatGPT web search |
| OAI-SearchBot | OpenAI | OpenAI search features |
| ChatGPT-User | OpenAI | ChatGPT browsing |
| ClaudeBot | Anthropic | Claude web features |
| PerplexityBot | Perplexity | Perplexity AI search |
| CCBot | Common Crawl | Training data (often blocked) |
| anthropic-ai | Anthropic | Claude training |
| Bytespider | ByteDance | TikTok/Douyin AI |
| cohere-ai | Cohere | Cohere models |

**Recommendation:** Allow GPTBot, OAI-SearchBot, ClaudeBot, PerplexityBot for AI search visibility. Block CCBot and training crawlers if desired.

**llms.txt Standard:**
Check for `/llms.txt` at domain root with structured content guidance, key page highlights, and authority information.

**RSL 1.0 (Really Simple Licensing):**
New standard (December 2025) backed by Reddit, Yahoo, Medium, Quora, Cloudflare, Akamai, Creative Commons. Check for RSL implementation.

**Scoring:** Grade technical accessibility factors. Score out of 20.

---

## ByCrawl Social Intelligence Enrichment

After completing Steps 1-6, ask the user:

> "Would you like me to measure your actual brand mention signals across the platforms AI systems cite from? This is the highest-value enrichment — brand mentions correlate 3x more with AI visibility than backlinks.
>
> I'll check:
> 1. YouTube mention volume and sentiment (strongest correlation: ~0.737)
> 2. Reddit presence and community validation (Perplexity cites Reddit 46.7% of the time)
> 3. X/Twitter authority signals and expert discourse
> 4. LinkedIn entity presence
> 5. Competitor mention comparison on the same platforms
>
> This uses ~40-60 bycrawl API calls. (yes/no)"

- **User says yes** → Run the bycrawl enrichment below, then proceed to the report.
- **User says no** → Skip to the report using only the base analysis.

### ByCrawl Step A: Brand Mention Audit (YouTube — Strongest Signal)

YouTube mentions have the strongest correlation with AI citations (~0.737). Measure actual presence:

```
youtube_search_videos(q="{brand_name}", count=20)
youtube_search_videos(q="{brand_name} {primary_keyword}", count=15)
youtube_search_videos(q="{primary_keyword} review OR comparison OR best", count=15)
```

For top 5 results mentioning the brand:
```
youtube_get_video(videoId="{id}")
youtube_get_video_transcription(videoId="{id}", language="en")
```

**Extract:**
- Total videos mentioning the brand (volume signal)
- View counts and engagement on brand-mentioning videos (authority signal)
- Whether the brand appears in video titles, descriptions, or only transcripts
- Competitor brands mentioned in the same videos (share of voice)
- Channel authority of brand-mentioning creators

### ByCrawl Step B: Reddit Presence Audit (Perplexity — 46.7% Citation Source)

Perplexity disproportionately cites Reddit. Measure brand presence there:

```
reddit_search_posts(query="{brand_name}", sort="relevance", count=25)
reddit_search_posts(query="{brand_name}", sort="new", count=20)
reddit_search_posts(query="{primary_keyword}", sort="top", count=20)
```

For top 5 brand-mentioning posts:
```
reddit_get_post(postId="{id}")
```

**Extract:**
- Post count mentioning the brand (volume)
- Subreddits where the brand appears (context — are these buyer communities?)
- Sentiment: positive, neutral, negative (community validation signal)
- Whether brand is recommended by users (organic endorsement = strongest citation signal)
- Competitor mentions in the same threads (share of voice)
- Recency of mentions (Perplexity has 2-3 day citation decay)

### ByCrawl Step C: X/Twitter Authority Signals

X signals expert discourse and real-time authority:

```
x_search_posts(query="{brand_name}", product="Top", count=25)
x_search_posts(query="{brand_name} {primary_keyword}", product="Top", count=20)
x_search_posts(query="{primary_keyword}", product="Top", count=20)
```

For the brand's own account (if identifiable):
```
x_get_user(username="{brand_handle}")
x_get_user_posts(username="{brand_handle}", count=20)
```

**Extract:**
- Brand mention volume and engagement
- Expert/influencer accounts mentioning the brand (authority endorsement)
- Brand's own account follower count and engagement rate
- Topic authority: does the brand participate in keyword-relevant discussions?
- Competitor mention comparison

### ByCrawl Step D: LinkedIn Entity Presence

LinkedIn signals corporate authority:

```
linkedin_get_company(url="https://www.linkedin.com/company/{brand_slug}")
linkedin_search_users(keywords="{brand_name} {primary_keyword}", count=15)
```

**Extract:**
- Company page completeness and follower count
- Employee thought leaders publishing on the topic
- Company content engagement rates
- Whether the brand is referenced in industry discussions

### ByCrawl Step E: Cross-Platform Competitor Benchmark

Compare brand mention signals against 2-3 competitors across all platforms:

```
youtube_search_videos(q="{competitor_name} {primary_keyword}", count=10)
reddit_search_posts(query="{competitor_name} {primary_keyword}", sort="top", count=15)
x_search_posts(query="{competitor_name} {primary_keyword}", product="Top", count=15)
```

**Build a share-of-voice comparison:**

| Platform | Your Brand | Competitor A | Competitor B |
|----------|-----------|-------------|-------------|
| YouTube mentions | X | Y | Z |
| Reddit mentions | X | Y | Z |
| X/Twitter mentions | X | Y | Z |
| LinkedIn presence | X | Y | Z |

This reveals where the brand is underrepresented relative to competitors — the biggest opportunities for improving AI citation likelihood.

### ByCrawl Step F: Content Gap Mining for Citability

Mine social platforms for data points and expert claims that could strengthen the content's citability:

```
reddit_search_posts(query="{primary_keyword} data OR stats OR percent OR study", sort="top", count=20)
x_search_posts(query="{primary_keyword} research OR found OR according", product="Top", count=20)
youtube_search_videos(q="{primary_keyword} data OR benchmark OR comparison", count=10)
```

For promising YouTube results:
```
youtube_get_video_transcription(videoId="{id}", language="en")
```

**Extract:**
- Specific statistics and data points the content could cite
- Expert claims with attribution
- Questions the audience asks that the content should answer
- Emerging topics or angles that AI systems are likely indexing now

---

## Platform-Specific Optimization

### Google AI Overviews
- 92% of citations from top-10 ranking pages
- 47% from pages below position 5
- Favors Google properties (23% of citations)
- High Domain Rating correlated with citation
- Present in 49% of SERPs

**If bycrawl enrichment was run:** Use YouTube data (Step A) to assess whether a companion YouTube video could increase citation via Google's preference for its own properties. Quantify the gap: if competitors have X YouTube videos on this topic and you have Y, that's the deficit.

### ChatGPT
- Wikipedia (47.9%), Reddit (11.3%) are top citation sources
- Favors well-cited, authoritative content
- Recency matters — recent updates get priority
- "Best X" listicles get 43.8% of citations

**If bycrawl enrichment was run:** Use Reddit data (Step B) to identify whether the brand appears in recommendation threads ChatGPT is likely scraping. If the brand is absent from "Best X" Reddit threads, that's a critical gap.

### Perplexity
- Reddit (46.7%), Wikipedia are top citation sources
- Rapid content decay: 2-3 day citation window
- Freshness is the most critical factor
- Community-validated content preferred

**If bycrawl enrichment was run:** Use Reddit recency data (Step B) to check if the brand has recent mentions. If the most recent Reddit mention is >7 days old, recommend a Reddit engagement strategy. Flag specific subreddits where the brand should be present.

### Bing Copilot
- Bing index, authoritative sites
- Bing SEO, IndexNow
- Microsoft ecosystem integration

---

## Calculate GEO Readiness Score (0-100)

| Category | Weight | Max Score |
|----------|--------|-----------|
| Citability Score | 25% | 25 |
| Structural Readability | 20% | 20 |
| Multi-Modal Content | 15% | 15 |
| Authority & Brand Signals | 20% | 20 |
| Technical Accessibility | 20% | 20 |
| **Total** | **100%** | **100** |

Rating thresholds:
- 90-100: Excellent — strong AI citation readiness
- 70-89: Good — citable with targeted improvements
- 50-69: Needs Work — significant gaps
- Below 50: Poor — major restructuring needed

---

## Output

Generate `GEO-ANALYSIS.md` with:

```
## GEO Analysis: [Page Title / URL]

**GEO Readiness Score: [X]/100** — [Rating]

### Score Breakdown
| Category | Score | Max | Notes |
|----------|-------|-----|-------|
| Citability | X | 25 | |
| Structural Readability | X | 20 | |
| Multi-Modal Content | X | 15 | |
| Authority & Brand Signals | X | 20 | |
| Technical Accessibility | X | 20 | |
| **Total** | **X** | **100** | |

### Platform Breakdown
| Platform | Readiness | Key Gap |
|----------|-----------|---------|
| Google AI Overviews | High/Med/Low | [gap] |
| ChatGPT | High/Med/Low | [gap] |
| Perplexity | High/Med/Low | [gap] |
| Bing Copilot | High/Med/Low | [gap] |

### AI Crawler Access Status
| Crawler | Status | Action |
|---------|--------|--------|
| GPTBot | Allowed/Blocked | |
| ClaudeBot | Allowed/Blocked | |
| PerplexityBot | Allowed/Blocked | |

### llms.txt Status
- [ ] Present at /llms.txt
- [ ] Structured format
- [ ] Key pages listed
- Recommendations: [...]

### Brand Mention Analysis
(Standard analysis if bycrawl enrichment was not run)

### ByCrawl Brand Mention Intelligence
(Only include if bycrawl enrichment was run)

#### YouTube Presence (Strongest AI citation signal — ~0.737 correlation)
- Videos mentioning brand: [N]
- Total views on brand-mentioning content: [N]
- Share of voice vs competitors: [table]
- Gap assessment: [specific recommendation]

#### Reddit Presence (Perplexity cites Reddit 46.7%)
- Posts mentioning brand: [N]
- Active subreddits: [list]
- Sentiment: [positive/neutral/negative with examples]
- Recency: [most recent mention date]
- Community recommendation rate: [endorsed/neutral/criticized]

#### X/Twitter Presence
- Mention volume: [N]
- Expert endorsements: [list notable accounts]
- Brand account authority: [follower count, engagement rate]

#### LinkedIn Presence
- Company page followers: [N]
- Employee thought leadership: [assessment]

#### Cross-Platform Share of Voice
| Platform | [Brand] | [Competitor A] | [Competitor B] |
|----------|---------|----------------|----------------|
| YouTube | X | Y | Z |
| Reddit | X | Y | Z |
| X/Twitter | X | Y | Z |
| LinkedIn | X | Y | Z |

#### Social Data Points for Citability
- [Specific stats/claims mined from social that could strengthen the content]

### Passage-Level Citability Audit
| Section | Word Count | Self-Contained | Has Data | Citable |
|---------|-----------|----------------|----------|---------|
| [H2] | [N] | Yes/No | Yes/No | Yes/No |

### Content Reformatting Suggestions
- [Specific passages to rewrite with before/after]

### Schema Recommendations
- [Schema types to add for AI discoverability]

### Priority Action Items

#### Quick Wins
1. Add "What is [topic]?" definition in first 60 words
2. Create 134-167 word self-contained answer blocks
3. Add question-based H2/H3 headings
4. Include specific statistics with sources
5. Add publication/update dates
6. Implement Person schema for authors
7. Allow key AI crawlers in robots.txt

#### Medium Effort
1. Create /llms.txt file
2. Add author bio with credentials + Wikipedia/LinkedIn links
3. Ensure server-side rendering for key content
4. Build entity presence on Reddit, YouTube
5. Add comparison tables with data
6. Implement FAQ sections

#### High Impact
1. Create original research/surveys (unique citability)
2. Build Wikipedia presence for brand/key people
3. Establish YouTube channel with content mentions
4. Implement comprehensive entity linking (sameAs across platforms)
5. Develop unique tools or calculators

#### ByCrawl-Informed Priorities
(Only include if bycrawl enrichment was run)
1. [Platform-specific gap with highest impact, informed by share-of-voice data]
2. [Specific subreddit/community engagement recommendation]
3. [YouTube content strategy recommendation based on competitor analysis]
```

---

## Error Handling

- If a bycrawl platform returns **empty results**, skip it and note "No data found on {platform}" — continue with available data.
- If the **ByCrawl API key is missing or invalid**, inform the user and proceed with the standard GEO analysis (Steps 1-6 without social enrichment).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx -y @bycrawl/mcp`).
- **Always deliver partial results** — an analysis with 4 of 6 enrichment steps is still better than no social data.
- **If user says no to bycrawl enrichment**, proceed with the standard analysis without delay.

## Estimated API Usage

| Enrichment Step | API Calls |
|---|---|
| Step A: YouTube Brand Audit | ~10-15 |
| Step B: Reddit Presence Audit | ~8-12 |
| Step C: X/Twitter Authority | ~8-10 |
| Step D: LinkedIn Entity | ~3-5 |
| Step E: Competitor Benchmark | ~10-15 |
| Step F: Content Gap Mining | ~8-12 |
| **Total (all enriched)** | **~40-60** |

Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive usage. Steps A-B have the highest ROI — run those first if budget is limited.

## Prerequisites

- **ByCrawl MCP server** must be installed and configured:
  ```
  claude mcp add bycrawl -- npx -y @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

---

## Related Skills

- **bycrawl-blog-geo**: Blog-post-level AI citation audit with passage scoring and citation capsule generation.
- **bycrawl-brand-monitoring**: Ongoing brand mention monitoring across 7 platforms.
- **bycrawl-competitor-intel**: Deep competitor social + hiring audit.
- **bycrawl-content-research**: Content gap analysis from social platforms.
- **bycrawl-social-keyword-signals**: Trending keyword discovery from social.
