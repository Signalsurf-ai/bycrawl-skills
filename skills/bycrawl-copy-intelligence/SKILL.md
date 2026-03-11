---
name: bycrawl-copy-intelligence
description: >
  Mine social platforms for copy angles, audience language, and content hooks using bycrawl MCP.
  Use when user says "copy research", "what copy works", "find copy angles",
  "audience language research", "social copy mining", "copywriting research",
  "what messaging resonates", "find hooks for [topic]", "文案靈感", "社群文案分析".
allowed-tools: ["mcp__bycrawl__*"]
user-invocable: true
argument-hint: "<topic_or_product> [audience] [copy_type] e.g. protein powder, fitness enthusiasts, ad copy"
---

# Copy Intelligence

## ICP (Ideal Customer Profile)
- **Copywriting tools** enriching AI-generated copy with real audience language
- **Content marketing teams** finding angles that resonate before writing
- **Brand strategists** understanding how their audience actually talks
- **Creative agencies** backing copy decisions with social engagement data

## What This Skill Does
Mines social platforms for the exact language, hooks, emotional triggers, and messaging angles that drive engagement in a specific niche. Instead of guessing what copy works, this skill extracts proof from real audience reactions — comments, shares, and engagement patterns across TikTok, YouTube, X, Reddit, Instagram, and Threads.

## Required Input
- **Topic or product category** (required): What you're writing copy for — e.g., "protein powder", "CRM software", "skincare routine"
- **Target audience** (optional): Who the copy is for — e.g., "busy moms", "startup founders"
- **Copy type** (optional): "ad copy", "landing page", "email", "social post", or "all"
- **Brand voice** (optional): Existing tone to match — e.g., "casual and fun", "professional and authoritative"


## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Workflow

### Step 1: Find What's Already Working
Search for top-performing content in the niche. Run in parallel:

```
tiktok_search_videos(keyword="{topic}", count=30)
youtube_search_videos(q="{topic}", count=15)
x_search_posts(query="{topic}", product="Top", count=30)
reddit_search_posts(query="{topic}", sort="top", count=30)
instagram_search_tags(query="{topic}")
threads_search_posts(query="{topic}", search_type="top", count=20)
```

Rank by engagement. The highest-performing content = copy that resonated.

### Step 2: Hook Extraction
Pull the opening lines/hooks from top-performing content:

For TikTok — get subtitles from top 5 videos:
```
tiktok_get_video_subtitles(videoId="{id}", language="en")
```

For YouTube — get transcripts from top 5 videos:
```
youtube_get_video_transcription(videoId="{id}", language="en")
```

Extract the first 10-15 seconds / first 2 lines. Categorize hooks:
- **Curiosity gap**: "You won't believe what happened when..."
- **Bold claim**: "This changed everything about how I..."
- **Pain point**: "Tired of [problem]? Here's what actually works"
- **Social proof**: "10,000 people switched to this and here's why"
- **Contrarian**: "Everything you know about [topic] is wrong"
- **Question**: "What if you could [desired outcome] in [timeframe]?"

### Step 3: Audience Language Mining
Comments reveal how the audience ACTUALLY talks. Pull comments from top content:

```
tiktok_get_video_comments(videoId="{id}")
youtube_get_video_comments(videoId="{id}", count=50)
instagram_get_post_comments(shortcode="{top_post}")
```

Extract:
- **Pain point phrases**: How they describe their problems (use THEIR words in copy)
- **Desire phrases**: How they describe what they want
- **Objections**: What holds them back ("but what about...", "too expensive", "does it really...")
- **Praise language**: What makes them excited ("finally!", "game changer", "this is exactly...")
- **Slang and tone**: Casual vs formal, emoji usage, generation-specific language

### Step 4: Emotional Trigger Mapping
From the top-performing posts and their comments, map the emotional triggers:

```
x_search_posts(query="{topic} love OR hate OR wish OR need OR finally OR can't believe", product="Top", count=20)
reddit_search_posts(query="{topic} frustrated OR amazing OR changed my life OR waste of money", sort="top", count=20)
```

Categorize:
- **Aspiration**: "I want to be the kind of person who..."
- **Fear/FOMO**: "Everyone else is already doing this..."
- **Frustration**: "I've tried everything and nothing works..."
- **Relief**: "Finally something that actually..."
- **Identity**: "As a [type of person], I need..."
- **Social proof**: "Everyone I know uses..."

### Step 5: Competitor Messaging Audit
What are competitors saying and how does the audience react?

```
x_search_posts(query="{competitor_brand} OR {competitor_product}", product="Top", count=20)
tiktok_search_videos(keyword="{competitor_brand}", count=15)
instagram_get_user_posts(username="{competitor_handle}")
```

Analyze competitor copy:
- What claims do they make?
- How does the audience react? (positive, skeptical, bored)
- What messaging gaps exist? (things NO competitor is saying)
- What's overused? (avoid these angles — audience is fatigued)

### Step 6: UGC and Testimonial Mining
Find organic user-generated content and real testimonials:

```
tiktok_search_videos(keyword="{brand_or_product} review", count=20)
youtube_search_videos(q="{brand_or_product} honest review", count=10)
reddit_search_posts(query="{brand_or_product} experience OR review OR honest", sort="top", count=20)
```

Extract:
- Real customer language describing the product
- Before/after narratives
- Specific results or outcomes mentioned
- Authentic phrases that can be adapted for copy

## Output Format

```markdown
## Copy Intelligence Report — {topic} — {date}

### Top-Performing Hooks (Proven to Stop Scrolls)

| # | Hook | Platform | Format | Engagement | Trigger Type |
|---|------|----------|--------|-----------|-------------|
| 1 | "{hook}" | TikTok | Video opener | {views} views | Curiosity gap |
| 2 | "{hook}" | X | Tweet | {likes} likes | Bold claim |
| 3 | "{hook}" | Reddit | Post title | {upvotes} upvotes | Pain point |
| 4 | "{hook}" | YouTube | Title | {views} views | How-to |
| 5 | "{hook}" | Instagram | Caption | {likes} likes | Social proof |

### Audience Language Bank
Words and phrases your audience ACTUALLY uses — pull directly into copy:

**Pain Point Phrases:**
- "{phrase}" — appeared {n} times across comments
- "{phrase}"
- "{phrase}"

**Desire Phrases:**
- "{phrase}" — what they want, in their words
- "{phrase}"
- "{phrase}"

**Objection Phrases:**
- "{phrase}" — must address in copy
- "{phrase}"

**Excitement Phrases:**
- "{phrase}" — use in testimonials and social proof
- "{phrase}"

### Emotional Trigger Map

| Trigger | Strength | Evidence | Best For |
|---------|----------|----------|----------|
| {emotion} | Strong | {n} mentions, high engagement | Headlines, ads |
| {emotion} | Strong | {evidence} | Email subject lines |
| {emotion} | Moderate | {evidence} | Landing page body |

### Messaging Angles (Ranked by Social Proof)

1. **{angle_1}**: "{example from top content}" — {why it works}
2. **{angle_2}**: "{example}" — {why it works}
3. **{angle_3}**: "{example}" — {why it works}

### Messaging Gaps (Opportunities)
Things the audience cares about that NO competitor is addressing:
1. **{gap}** — {evidence from comments/posts}
2. **{gap}** — {evidence}

### Overused Messaging (Avoid)
Angles the audience is tired of:
1. **{overused_angle}** — {evidence of fatigue: low engagement, negative comments}
2. **{overused_angle}** — {evidence}

### Ready-to-Use Copy Fragments

**Headlines/Subject Lines:**
1. "{headline}" — based on {hook type} + {audience language}
2. "{headline}"
3. "{headline}"

**Opening Lines:**
1. "{opener}" — mirrors audience pain point phrasing
2. "{opener}"

**CTAs:**
1. "{cta}" — matches audience desire language
2. "{cta}"

**Social Proof Lines:**
1. "{line}" — adapted from real UGC
2. "{line}"

### Platform-Specific Copy Guidance

| Platform | Tone | Length | Format | Top Trigger |
|----------|------|--------|--------|------------|
| TikTok | {tone} | {length} | {format} | {trigger} |
| Instagram | {tone} | {length} | {format} | {trigger} |
| X | {tone} | {length} | {format} | {trigger} |
| Facebook | {tone} | {length} | {format} | {trigger} |

### Competitor Copy Benchmark
| Competitor | Primary Message | Audience Reaction | Your Differentiation |
|------------|----------------|-------------------|---------------------|
| {brand} | "{their angle}" | {positive/mixed/negative} | {your opportunity} |
```

## Error Handling

- If a platform returns **empty results**, skip it and note "No data found on {platform}" in the report — do not fail the entire workflow.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- **Always deliver partial results** rather than failing entirely — a report covering 5 of 7 platforms is still valuable.

## Estimated API Usage

- **Basic run**: ~20-25 API calls
- **Full run**: ~35-45 with competitor audit and UGC mining API calls
- Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive usage.
