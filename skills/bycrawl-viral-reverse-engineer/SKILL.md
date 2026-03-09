---
name: bycrawl-viral-reverse-engineer
description: >
  Reverse engineer viral social media content to understand what made it spread, using bycrawl MCP.
  Use when user says "why did this go viral", "reverse engineer this post",
  "analyze this viral content", "what makes content go viral in [niche]",
  "viral content analysis", "deconstruct this post", "replicate viral content".
allowed-tools: ["mcp_bycrawl_*"]
user-invocable: true
argument-hint: "<post_url_or_topic> [niche] e.g. https://x.com/... OR AI tools"
---

# Viral Content Reverse Engineering

## ICP (Ideal Customer Profile)
- **Content creators** wanting to replicate viral success
- **Social media managers** building playbooks for virality
- **Marketing teams** studying what resonates with their audience
- **Growth hackers** looking for repeatable content patterns

## What This Skill Does
Takes a viral post (URL or topic) and deconstructs it: analyzes the content structure, cross-platform spread, audience reaction in comments, author's history, and what specifically triggered sharing behavior. Produces a replicable playbook.

## Required Input
- **Viral post URL or topic** (required): A specific post to analyze, or a topic to find viral examples in
- **Your niche** (optional): To make recommendations specific to your audience


## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Workflow

### Step 1: Get the Source Post
If URL provided, pull the full post:
```
x_get_post(postId="{id}")
// OR
reddit_get_post(postId="{id}")
// OR
tiktok_get_video(videoId="{id}")
// OR
youtube_get_video(videoId="{id}")
```

If topic provided, find top-performing content:
```
x_search_posts(query="{topic}", product="Top", count=20)
reddit_search_posts(query="{topic}", sort="top", count=20)
tiktok_search_videos(keyword="{topic}", count=20)
youtube_search_videos(q="{topic}", count=10)
```

Select the posts with highest engagement ratios.

### Step 2: Analyze the Content Structure
Deconstruct:
- **Hook**: First line / first 3 seconds — what stopped the scroll?
- **Format**: Thread, single post, video, meme, carousel?
- **Length**: Short and punchy vs long and detailed?
- **Emotional trigger**: Curiosity, outrage, humor, FOMO, inspiration?
- **CTA**: Did it ask for engagement? How?

For videos, pull subtitles/transcripts:
```
tiktok_get_video_subtitles(videoId="{id}")
youtube_get_video_transcription(videoId="{id}")
```

### Step 3: Comment Analysis
Pull comments to understand WHY people engaged:
```
tiktok_get_video_comments(videoId="{id}")
youtube_get_video_comments(videoId="{id}", count=50)
instagram_get_post_comments(shortcode="{id}")
```

Categorize comments:
- Agreement/validation ("so true", "needed this")
- Debate/disagreement (controversy drives engagement)
- Tagging friends (= organic distribution)
- Questions (= content gap to fill)
- "Saved" / "Bookmarked" mentions (= high-value content)

### Step 4: Cross-Platform Spread
Check if the content spread to other platforms:
```
x_search_posts(query="{key phrase from post}", product="Top", count=10)
reddit_search_posts(query="{key phrase}", sort="top", count=10)
threads_search_posts(query="{key phrase}", search_type="top", count=10)
```

Map the spread: where it started, where it jumped to, how it mutated.

### Step 5: Author Analysis
Understand if the author's profile contributed to virality:
```
x_get_user(username="{author}")
tiktok_get_user(username="{author}")
instagram_get_user(username="{author}")
```

Check:
- Follower count at time of posting (was this punching above their weight?)
- Posting history (did they try this format before?)
- Previous viral hits (one-hit wonder or repeatable?)

### Step 6: Pattern Matching
Pull the author's other content to find what's different about the viral post:
```
x_get_user_posts(username="{author}", count=30)
tiktok_get_user_videos(username="{author}", count=20)
```

Compare: engagement on viral post vs average engagement. What changed?

## Output Format

```markdown
## Viral Content Analysis — {date}

### The Post
- **Platform**: {platform}
- **Author**: @{handle} ({followers} followers)
- **Link**: {url}
- **Metrics**: {views} views | {likes} likes | {shares} shares | {comments} comments
- **Engagement rate**: {rate}% (vs author's average of {avg}%)

### Why It Went Viral

#### Hook Analysis
- First line: "{text}"
- Hook type: {curiosity gap / bold claim / relatable pain / controversy}
- Scroll-stop factor: {what made people pause}

#### Emotional Trigger
- Primary: {emotion}
- Secondary: {emotion}
- Shareability driver: {why people shared — identity signaling, helping others, debate, humor}

#### Content Structure
- Format: {thread / video / image / text}
- Length: {word count or duration}
- Structure: {listicle / story / tutorial / hot take / comparison}
- Key technique: {specific structural element that worked}

### Audience Reaction (from {n} comments analyzed)
- {n}% agreement/praise
- {n}% debate/disagreement
- {n}% tagging friends
- {n}% asking follow-up questions
- Top comment: "{comment}" ({likes})

### Cross-Platform Spread
| Platform | Reposts | Time After Original | Mutation |
|----------|---------|--------------------|-----------|

### Replication Playbook

#### Do This:
1. **Hook template**: "{fill-in-the-blank version of their hook}"
2. **Format**: Use {format} — it's working for {niche} right now
3. **Emotional angle**: Lead with {emotion} because {reason}
4. **Length**: Keep it {length} — that's the sweet spot for {platform}
5. **CTA**: {what to ask for}

#### Don't Do This:
1. {common mistake when copying this format}
2. {what to avoid}

### 3 Post Ideas Using This Template
1. **"{title}"** — {brief outline}
2. **"{title}"** — {brief outline}
3. **"{title}"** — {brief outline}
```

## Error Handling

- If a platform returns **empty results**, skip it and note "No data found on {platform}" in the report — do not fail the entire workflow.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- **Always deliver partial results** rather than failing entirely — a report covering 5 of 7 platforms is still valuable.

## Estimated API Usage

- **Basic run**: ~10-15 (single post) API calls
- **Full run**: ~30-40 (topic search with pattern matching) API calls
- Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive usage.
