---
name: bycrawl-influencer-discovery
description: >
  Find and vet influencers and creators across TikTok, Instagram, YouTube, and X using bycrawl MCP.
  Use when user says "find influencers", "influencer discovery", "creator outreach",
  "find creators", "influencer marketing", "KOL research", "find brand ambassadors",
  "who is talking about [topic]".
---

# Influencer Discovery

## ICP (Ideal Customer Profile)
- **Marketing managers** running influencer campaigns
- **D2C brand owners** looking for micro-influencers in a niche
- **Agency teams** sourcing creators for client campaigns
- **Growth marketers** identifying organic advocates already talking about their product

## What This Skill Does
Discovers creators and influencers in a specific niche by searching content across TikTok, Instagram, YouTube, and X, then vets them by pulling profiles, engagement metrics, content quality, and audience signals.

## Required Input
- **Niche or topic** (required): e.g., "skincare", "AI tools", "home fitness"
- **Platform priority** (optional): Which platform to focus on (default: all)
- **Follower range** (optional): "micro" (1K-50K), "mid" (50K-500K), "macro" (500K+)
- **Region** (optional): Target geography

## Workflow

### Step 1: Content Search Across Platforms
Run in parallel:
```
tiktok_search_videos(keyword="{niche}", count=30)
youtube_search_videos(q="{niche}", count=15)
instagram_search_tags(query="{niche}")
x_search_posts(query="{niche}", product="Top", count=30)
```

### Step 2: Extract Unique Creators
From search results, deduplicate creators and rank by:
- View count / engagement on found content
- Relevance of content to the niche
- Saves and shares (high-intent signals, especially on TikTok)

### Step 3: Profile Deep Dive
For top 10-15 creators, pull profiles in parallel:
```
tiktok_get_user(username="{creator}")
instagram_get_user(username="{creator}")
youtube_get_channel(channelId="@{creator}")
x_get_user(username="{creator}")
```

### Step 4: Content Quality Check
For shortlisted creators, pull recent content:
```
tiktok_get_user_videos(username="{creator}", count=10)
instagram_get_user_posts(username="{creator}")
```

Evaluate:
- Consistency of posting
- Content quality and production value
- Brand safety (scan for controversial content)
- Audience engagement patterns (comments vs likes ratio)

### Step 5: Engagement Authenticity Check
Pull comments on their top posts:
```
tiktok_get_video_comments(videoId="{top_video_id}")
instagram_get_post_comments(shortcode="{top_post}")
youtube_get_video_comments(videoId="{top_video_id}")
```

Look for: generic/bot comments vs genuine engagement.

## Output Format

```markdown
## Influencer Discovery Report — {niche} — {date}

### Top Creators Found

| # | Creator | Platform | Followers | Avg Views | Engagement Rate | Niche Fit |
|---|---------|----------|-----------|-----------|-----------------|-----------|
| 1 | @name | TikTok | 45K | 12K | 8.2% | High |
| 2 | ... | ... | ... | ... | ... | ... |

### Creator Profiles

#### 1. @{creator_name}
- **Platforms**: TikTok (45K), Instagram (22K), YouTube (8K)
- **Content style**: {description}
- **Posting frequency**: {n}/week
- **Best post**: {link} — {views} views, {engagement}
- **Audience quality**: {authentic/suspicious}
- **Brand safety**: {clean/flagged}
- **Contact**: {bio link or email if public}

### Niche Insights
- Trending content formats in {niche}: ...
- Underserved sub-topics: ...
- Best platform for this niche: ...

### Recommended Outreach List
1. @{name} — {reason}
2. @{name} — {reason}
3. @{name} — {reason}
```
