---
name: bycrawl-content-research
description: >
  Content gap analysis, idea mining, and trend research across social platforms using bycrawl MCP.
  Use when user says "content ideas", "content research", "what should I post about",
  "content gap", "trending topics", "video ideas", "blog ideas from social",
  "what questions are people asking about [topic]".
---

# Content Research

## ICP (Ideal Customer Profile)
- **Content marketers** planning editorial calendars
- **YouTube/TikTok creators** looking for video ideas with proven demand
- **SEO writers** mining social platforms for content gaps
- **Founders** who need to create thought leadership content

## What This Skill Does
Mines social platforms for trending content, audience questions, and gaps in existing coverage for a given topic. Extracts ideas from comments, transcripts, and discussions — then synthesizes into an actionable content plan.

## Required Input
- **Topic or niche** (required): What to research
- **Content format** (optional): "video", "blog", "social posts", or "all"
- **Platform focus** (optional): Specific platforms to prioritize

## Workflow

### Step 1: Surface Existing Content
Run in parallel:
```
youtube_search_videos(q="{topic}", count=15)
tiktok_search_videos(keyword="{topic}", count=20)
reddit_search_posts(query="{topic}", sort="top", count=20)
x_search_posts(query="{topic}", product="Top", count=20)
```

### Step 2: Extract Transcripts from Top Videos
For the top 3-5 YouTube videos by views:
```
youtube_get_video_transcription(videoId="{id}", language="en")
```

For top 3-5 TikTok videos with subtitles:
```
tiktok_get_video_subtitles(videoId="{id}", language="en")
```

Analyze transcripts to identify:
- Key points every video covers (saturated topics)
- Points NONE of them cover (content gaps)
- Unique angles that got high engagement

### Step 3: Comment Mining for Questions
Pull comments from top-performing content:
```
youtube_get_video_comments(videoId="{id}", count=50)
tiktok_get_video_comments(videoId="{id}")
```

Extract:
- Recurring questions (= content demand)
- Complaints about existing content (= improvement opportunities)
- "I wish someone would explain..." patterns

### Step 4: Reddit Deep Dive
```
reddit_search_posts(query="{topic}", sort="comments", count=20)
```

Reddit posts sorted by comments reveal the most-discussed sub-topics. Scan for:
- Unanswered questions
- Debates (= controversy content performs well)
- "How do I..." posts (= tutorial demand)

### Step 5: Trend Detection
```
tiktok_get_category_videos(category="{relevant_category}", count=20)
threads_get_public_feed(count=20)
```

Identify emerging formats and angles before they saturate.

## Output Format

```markdown
## Content Research Report — {topic} — {date}

### Content Landscape
- Total videos found on YouTube: {n} (top video: {views} views)
- Total TikTok videos: {n} (top video: {views} views)
- Reddit discussions: {n} (most commented: {n} comments)

### What Everyone Already Covers
1. {saturated_topic_1}
2. {saturated_topic_2}
3. {saturated_topic_3}

### Content Gaps (Your Opportunity)
1. **{gap_1}** — No one covers this despite {n} people asking in comments
2. **{gap_2}** — Only 1 video exists, and it has {views} views (demand > supply)
3. **{gap_3}** — Reddit thread with {n} comments, zero video coverage

### Top Questions from Audiences
1. "{question_1}" — asked {n} times across platforms
2. "{question_2}"
3. "{question_3}"

### Content Ideas (Ready to Execute)

| # | Title Idea | Format | Platform | Why It'll Work |
|---|-----------|--------|----------|----------------|
| 1 | {title} | Video | YouTube | {reason} |
| 2 | {title} | Thread | X | {reason} |
| 3 | {title} | Blog | SEO | {reason} |
| 4 | {title} | Short | TikTok | {reason} |
| 5 | {title} | Post | Reddit | {reason} |

### Trending Formats in This Niche
- {format_1}: {description}
- {format_2}: {description}
```
