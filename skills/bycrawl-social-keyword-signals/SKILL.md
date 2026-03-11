---
name: bycrawl-social-keyword-signals
description: >
  Surface trending keywords and topics from social platforms before they appear in search volume tools using bycrawl MCP.
  Use when user says "social keyword signals", "trending keywords from social",
  "pre-search signals", "keyword trends from reddit", "what's trending before google",
  "social SEO signals", "keyword discovery from social", "社群關鍵字", "搜尋趨勢預測".
allowed-tools: ["mcp__bycrawl__*", "WebFetch", "WebSearch"]
user-invocable: true
argument-hint: "<seed_keyword> [region] e.g. AI coding tools, US/en"
---

# Social Keyword Signals

## ICP (Ideal Customer Profile)
- **SEO platforms** wanting leading indicators for keyword trends
- **Content teams** identifying topics before search volume spikes
- **Keyword research tools** enriching data with social-sourced signals
- **Growth marketers** catching trends early for content and paid campaigns

## What This Skill Does
Mines social platforms (Reddit, X, Threads, TikTok, YouTube) for emerging terms, phrases, and topics that indicate rising search intent — before they show up in traditional keyword tools like Google Trends or Ahrefs. Outputs keyword signals ranked by momentum, platform spread, and intent type.

## Required Input
- **Seed keyword or niche** (required): The topic area to monitor — e.g., "AI coding tools", "膠原蛋白", "home gym equipment"
- **Region/language** (optional): Market focus (e.g., "Taiwan/zh-TW", "US/en")
- **Competitor keywords** (optional): Terms competitors rank for that you want to track socially


## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Workflow

### Step 1: Current Social Volume Scan
Establish baseline — what's being discussed RIGHT NOW for this topic. Run in parallel:

```
reddit_search_posts(query="{seed_keyword}", sort="new", count=30)
reddit_search_posts(query="{seed_keyword}", sort="top", count=30)
x_search_posts(query="{seed_keyword}", product="Latest", count=30)
x_search_posts(query="{seed_keyword}", product="Top", count=30)
threads_search_posts(query="{seed_keyword}", search_type="recent", count=20)
tiktok_search_videos(keyword="{seed_keyword}", count=30)
youtube_search_videos(q="{seed_keyword}", count=15)
```

### Step 2: Extract Emerging Terms
From all results, extract unique phrases and terms people are using that are NOT the seed keyword itself:

**From Reddit posts and titles:**
- New product names, brand mentions
- Problem descriptions ("how do I...", "is there a way to...")
- Comparison terms ("{X} vs {Y}", "alternative to {Z}")
- Jargon and slang unique to the community

**From X/Threads:**
- Hashtags co-occurring with the seed keyword
- New terminology being coined
- Hot-take phrases that signal emerging debate

**From TikTok:**
- Video captions and hashtags on trending content
- Sound/trend names associated with the topic
- Creator-coined terms gaining traction

**From YouTube:**
- Video titles using specific long-tail phrases
- Auto-suggest patterns visible in search results

### Step 3: Question Mining (High-Intent Keywords)
Questions = search intent. Pull the richest question sources:

```
reddit_search_posts(query="{seed_keyword} how OR what OR why OR best OR which OR recommend", sort="new", count=30)
youtube_search_videos(q="{seed_keyword} how to", count=10)
youtube_search_videos(q="{seed_keyword} best", count=10)
```

For top YouTube videos, pull comments for additional questions:
```
youtube_get_video_comments(videoId="{id}", count=50)
```

For top Reddit posts, note:
- Post title phrasing (often matches exact search queries)
- Upvote count (proxy for how many people have the same question)

### Step 4: Trend Velocity Detection
Identify which terms are ACCELERATING — appearing more in "Latest/New" than in "Top/All-time":

Compare results from Step 1:
- Terms appearing in "Latest" but NOT in "Top" = newly emerging
- Terms appearing in BOTH = established but still active
- Terms only in "Top" = mature/saturated

For emerging terms, dig deeper:
```
x_search_posts(query="{emerging_term}", product="Latest", count=20)
reddit_search_posts(query="{emerging_term}", sort="new", count=20)
tiktok_search_videos(keyword="{emerging_term}", count=15)
```

Check: Is it spreading across platforms? Multi-platform presence = stronger signal.

### Step 5: Long-Tail Keyword Expansion
From the social discussions, extract long-tail variations:

```
reddit_search_posts(query="{seed_keyword} for {use_case}", sort="top", count=20)
youtube_search_videos(q="{seed_keyword} for beginners", count=10)
youtube_search_videos(q="best {seed_keyword} 2024", count=10)
tiktok_search_videos(keyword="{seed_keyword} hack", count=15)
```

Pattern-match:
- "{seed} for {audience}" — audience-specific long-tails
- "{seed} vs {alternative}" — comparison long-tails
- "best {seed} for {use case}" — buyer-intent long-tails
- "{seed} hack/tip/trick" — engagement long-tails

### Step 6: Competitor Keyword Gap
If competitor keywords provided, check their social presence:

```
x_search_posts(query="{competitor_keyword}", product="Latest", count=20)
reddit_search_posts(query="{competitor_keyword}", sort="new", count=20)
tiktok_search_videos(keyword="{competitor_keyword}", count=15)
```

Compare social volume: Are their keywords trending or declining socially?

## Output Format

```markdown
## Social Keyword Signals Report — {seed_keyword} — {date}

### Signal Summary
- Total social mentions scanned: {n} across {n} platforms
- Emerging terms detected: {n}
- High-intent questions found: {n}
- Fastest-rising signal: **{term}** — {evidence}

### Emerging Keywords (Pre-Search Signals)

| # | Keyword/Phrase | Platform Spread | Velocity | Intent Type | Evidence |
|---|---------------|----------------|----------|-------------|----------|
| 1 | {term} | Reddit, X, TikTok | Rising fast | Informational | {n} mentions in 7d |
| 2 | {term} | Reddit, YouTube | Emerging | Commercial | {n} mentions |
| 3 | {term} | TikTok, Threads | New | Navigational | {n} videos |

### High-Intent Question Keywords
Questions people are asking = search queries waiting to happen:

| # | Question | Platform | Volume Proxy | Intent |
|---|----------|----------|-------------|--------|
| 1 | "{question}" | Reddit ({n} upvotes) | High | Buyer research |
| 2 | "{question}" | YouTube ({n} views) | High | How-to |
| 3 | "{question}" | X ({n} engagements) | Medium | Comparison |

### Long-Tail Keyword Opportunities

| # | Long-Tail Keyword | Source | Competition Signal | Recommended Content |
|---|-------------------|--------|-------------------|-------------------|
| 1 | "{long_tail}" | Reddit titles | Low (few results) | Blog post |
| 2 | "{long_tail}" | YouTube titles | Medium | Video |
| 3 | "{long_tail}" | TikTok captions | Low | Short-form |

### Trending vs Declining (Velocity Map)

**Rising (social volume increasing):**
- {term} — up across {platforms} — {evidence}
- {term} — new in last 7 days — {evidence}

**Stable (consistent social volume):**
- {term} — steady on {platforms}

**Declining (social volume dropping):**
- {term} — fewer recent mentions than historical

### Platform-Specific Signals

**Reddit** (deepest intent signals):
- Top subreddits discussing this: {list}
- Recurring question patterns: {patterns}
- Sentiment: {positive/neutral/negative toward category}

**TikTok** (fastest trend signals):
- Trending hashtags: {list}
- Video format trends: {what's working}
- Creator adoption: {n} creators posting about this

**YouTube** (strongest buyer intent):
- Top search phrases in titles: {list}
- Video volume trend: {growing/stable/declining}
- Comment questions: {unanswered demand}

**X/Twitter** (real-time pulse):
- Active conversations: {themes}
- Influencer mentions: {who's talking}
- Hashtag velocity: {trending hashtags}

### Competitor Keyword Comparison (if provided)
| Keyword | Your Social Volume | Competitor Social Volume | Gap |
|---------|-------------------|------------------------|-----|

### Actionable Recommendations
1. **Target immediately**: {keyword} — strong social signal, likely low search competition still
2. **Create content for**: {keyword} — {n} people asking this question, no good answer exists
3. **Monitor**: {keyword} — emerging but not confirmed yet
4. **Ignore**: {keyword} — declining socially, likely saturated in search
```

## Error Handling

- If a platform returns **empty results**, skip it and note "No data found on {platform}" in the report — do not fail the entire workflow.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- **Always deliver partial results** rather than failing entirely — a report covering 5 of 7 platforms is still valuable.

## Estimated API Usage

- **Basic run**: ~20-25 API calls
- **Full run**: ~35-45 with trend velocity and competitor gaps API calls
- Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive usage.
