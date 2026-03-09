---
name: bycrawl-brand-monitoring
description: >
  Cross-platform brand mention monitoring and sentiment analysis using bycrawl MCP.
  Use when user says "monitor brand", "brand mentions", "track mentions", "social listening",
  "brand monitoring", "what are people saying about", "track our brand", "mention tracking".
---

# Brand Monitoring

## ICP (Ideal Customer Profile)
- **Marketing managers** tracking brand perception across social channels
- **Founders/CEOs** who want a pulse on how their brand is discussed online
- **PR teams** needing regular mention reports without expensive tools
- **Agency owners** delivering social listening reports to clients

## What This Skill Does
Searches for brand mentions across X/Twitter, Reddit, YouTube, TikTok, Instagram, Threads, and Facebook simultaneously, then synthesizes findings into a structured report with sentiment analysis, top mentions, engagement metrics, and strategic takeaways.

## Required Input
- **Brand name** (required): The brand or product to monitor
- **Competitors** (optional): Competitor names for share-of-voice comparison
- **Time focus** (optional): "latest" for real-time, "top" for highest engagement

## Workflow

### Step 1: Parallel Platform Search
Run ALL of these searches simultaneously for the brand name:

```
x_search_posts(query="{brand}", product="Latest", count=20)
reddit_search_posts(query="{brand}", sort="new", count=20)
youtube_search_videos(q="{brand}", count=10)
tiktok_search_videos(keyword="{brand}", count=15)
threads_search_posts(query="{brand}", search_type="recent", count=15)
instagram_search_tags(query="{brand}")
facebook_search_posts(query="{brand}", count=5)
```

### Step 2: Analyze & Classify
For each mention found:
- Classify sentiment: Positive / Neutral / Negative
- Identify if it's organic mention, retweet/share, review, complaint, or competitor comparison
- Note language and geographic signals
- Flag high-engagement posts (viral potential)

### Step 3: Deep Dive on High-Signal Mentions
For posts with high engagement or negative sentiment, optionally pull:
- Post comments for context (youtube_get_video_comments, tiktok_get_video_comments, etc.)
- Author profiles to assess influence level

### Step 4: Competitor Share of Voice (if competitors provided)
Run the same parallel search for each competitor, then calculate:
- Mention count per platform per brand
- Share of voice percentage
- Sentiment comparison

## Output Format

```markdown
## Brand Monitoring Report — {brand} — {date}

### Summary Table
| Platform | Mentions | Sentiment | Top Post |
|----------|----------|-----------|----------|
| X/Twitter | {n} | {sentiment} | {link} |
| Reddit | {n} | {sentiment} | {link} |
| ... | ... | ... | ... |

### Key Findings
1. {finding_1}
2. {finding_2}
3. {finding_3}

### Viral / High-Engagement Mentions
- {post_summary} — {engagement_metrics} — {link}

### Sentiment Breakdown
- Positive: {n}%
- Neutral: {n}%
- Negative: {n}%

### Top Themes
1. {theme_1}
2. {theme_2}

### Share of Voice (if competitors provided)
| Brand | X | Reddit | YouTube | TikTok | Total |
|-------|---|--------|---------|--------|-------|

### Recommended Actions
- {action_1}
- {action_2}

### Platform Gaps
- {platforms_with_low_or_zero_presence}
```
