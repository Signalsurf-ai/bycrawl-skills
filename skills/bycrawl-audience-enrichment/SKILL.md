---
name: bycrawl-audience-enrichment
description: >
  Enrich ad audience segments with real-time cross-platform social behavior signals using bycrawl MCP.
  Use when user says "enrich audience", "audience intelligence", "social signals for targeting",
  "behavior enrichment", "audience profiling", "interest signals", "social audience data",
  "what is my audience doing on social", "real-time audience insights", "受眾分析", "社群行為".
---

# Audience Enrichment

## ICP (Ideal Customer Profile)
- **Ad platforms and DSPs** enriching device/interest profiles with social behavior
- **Brand advertisers** wanting deeper audience understanding beyond demographics
- **Media agencies** building richer audience segments for programmatic campaigns
- **Data teams** adding social engagement layers to existing customer profiles

## What This Skill Does
Takes a target audience definition (persona, brand, product category, or interest segment) and builds a real-time social behavior profile by analyzing what that audience is actively talking about, sharing, reacting to, and engaging with across Threads, Instagram, TikTok, X, Reddit, YouTube, and Facebook. Outputs enrichment signals that layer on top of existing device/interest profiles.

## Required Input
- **Audience definition** (required): Who to profile — e.g., "25-35 year old women interested in skincare", "Tesla owners", "parents of toddlers", or a brand name whose audience to analyze
- **Platforms** (optional): Which platforms to prioritize (default: all)
- **Region** (optional): Geography focus (e.g., "Taiwan", "US", "Japan")
- **Existing segments** (optional): Current targeting criteria to enrich

## Workflow

### Step 1: Seed Topic Extraction
Identify the core topics and hashtags this audience gravitates toward. Run in parallel:

```
instagram_search_tags(query="{audience_interest}")
tiktok_search_videos(keyword="{audience_interest}", count=30)
x_search_posts(query="{audience_interest}", product="Top", count=30)
reddit_search_posts(query="{audience_interest}", sort="top", count=30)
threads_search_posts(query="{audience_interest}", search_type="top", count=20)
youtube_search_videos(q="{audience_interest}", count=15)
```

From results, extract:
- Top hashtags and related tags (Instagram, TikTok)
- Most-used language and phrases
- Adjacent topics the audience also discusses

### Step 2: Engagement Pattern Analysis
Pull top-performing content to understand WHAT this audience engages with:

```
tiktok_search_videos(keyword="{audience_interest}", count=30)
youtube_search_videos(q="{audience_interest}", count=15)
```

For top 5 videos by engagement, pull comments:
```
tiktok_get_video_comments(videoId="{id}")
youtube_get_video_comments(videoId="{id}", count=50)
```

Analyze:
- **Content formats** that get highest engagement (short video, tutorial, review, UGC)
- **Emotional triggers** in top comments (excitement, curiosity, frustration, aspiration)
- **Purchase intent signals** ("where can I buy", "link?", "price?", "worth it?")
- **Time patterns**: When are these posts most active?

### Step 3: Cross-Platform Behavior Mapping
Identify how the audience behaves differently across platforms:

```
x_search_posts(query="{audience_interest}", product="Latest", count=30)
threads_search_posts(query="{audience_interest}", search_type="recent", count=20)
reddit_search_posts(query="{audience_interest}", sort="new", count=30)
facebook_search_posts(query="{audience_interest}", count=5)
```

Map per platform:
- **X/Twitter**: Opinions, hot takes, real-time reactions
- **Threads**: Casual discussion, community conversations
- **Reddit**: Deep questions, product research, honest reviews
- **TikTok**: Discovery behavior, trend participation
- **Instagram**: Aspirational content, brand affinity
- **YouTube**: Research mode, tutorial consumption
- **Facebook**: Local community, marketplace behavior

### Step 4: Adjacent Interest Discovery
Find what ELSE this audience cares about — the enrichment gold:

From profiles of highly engaged users in Step 2:
```
x_get_user(username="{engaged_user}")
instagram_get_user(username="{engaged_user}")
tiktok_get_user(username="{engaged_user}")
```

Pull their other content/interests:
```
x_get_user_posts(username="{engaged_user}", count=20)
instagram_get_user_posts(username="{engaged_user}")
tiktok_get_user_videos(username="{engaged_user}", count=10)
```

Cluster adjacent interests:
- What other topics do they post about?
- What other brands/products do they mention?
- What communities/subreddits are they active in?
- What other hashtags do they use?

### Step 5: Brand Affinity Signals
If a specific brand is provided, check audience overlap:

```
instagram_get_user(username="{brand}")
instagram_get_user_posts(username="{brand}")
tiktok_get_user(username="{brand}")
tiktok_get_user_videos(username="{brand}", count=20)
x_get_user(username="{brand}")
facebook_get_user(username="{brand}")
```

Pull comments on brand content to see WHO engages:
```
instagram_get_post_comments(shortcode="{top_post}")
tiktok_get_video_comments(videoId="{top_video}")
```

### Step 6: Real-Time Trend Signals
Identify what's trending RIGHT NOW for this audience:

```
tiktok_get_category_videos(category="{relevant_category}", count=20)
x_search_posts(query="{audience_interest}", product="Latest", count=30)
threads_get_public_feed(count=20)
```

Flag:
- Emerging topics (mentioned in last 48 hours, growing fast)
- Viral content in the niche
- Seasonal or event-driven spikes

## Output Format

```markdown
## Audience Enrichment Report — {audience_definition} — {date}

### Audience Summary
{2-3 sentence description of who this audience is based on social behavior}

### Platform Behavior Map

| Platform | Activity Level | Primary Behavior | Content Preference | Peak Signal |
|----------|---------------|-----------------|-------------------|-------------|
| TikTok | High | Discovery, trends | Short-form video, UGC | {signal} |
| Instagram | High | Aspiration, brand | Reels, carousels | {signal} |
| X/Twitter | Medium | Opinion, real-time | Hot takes, threads | {signal} |
| Reddit | Medium | Research, reviews | Long-form, Q&A | {signal} |
| Threads | Low-Med | Casual chat | Conversational | {signal} |
| YouTube | Medium | Research, tutorials | Long-form video | {signal} |
| Facebook | Low-Med | Local, community | Groups, marketplace | {signal} |

### Core Interest Signals
1. **{interest_1}** — {evidence from n mentions across platforms}
2. **{interest_2}** — {evidence}
3. **{interest_3}** — {evidence}

### Adjacent Interests (Enrichment Layer)
These are topics this audience ALSO cares about — use for lookalike expansion and creative angles:

| Adjacent Interest | Overlap Strength | Source Platform | Evidence |
|-------------------|-----------------|----------------|----------|
| {interest} | Strong | TikTok, Reddit | {evidence} |
| {interest} | Moderate | X, Instagram | {evidence} |

### Engagement Triggers
What makes this audience click, share, and comment:
- **Emotional triggers**: {list with examples}
- **Content formats that work**: {ranked list}
- **Language patterns**: {phrases, slang, tone they use}
- **Purchase intent signals**: {what buying behavior looks like for this audience}

### Real-Time Trending Topics
Topics this audience is actively discussing right now:
1. **{topic}** — {platform} — {volume signal} — {sentiment}
2. **{topic}** — {platform} — {volume signal}
3. **{topic}** — {platform} — {volume signal}

### Brand Affinity Map (if applicable)
| Brand | Platform | Engagement Level | Sentiment | Audience Overlap |
|-------|----------|-----------------|-----------|-----------------|

### Targeting Recommendations
Based on social behavior, these signals can enrich existing segments:

1. **Interest layer**: Add {interests} to existing demographic targeting
2. **Behavioral layer**: Target users who engage with {content types} on {platforms}
3. **Contextual layer**: Place ads adjacent to {trending topics} content
4. **Lookalike seed**: Use {adjacent interests} for audience expansion
5. **Creative direction**: Lead with {emotional trigger} in {format}

### Data Freshness
- Report generated: {date}
- Most recent social signal: {timestamp}
- Recommended refresh: {frequency based on how fast this niche moves}
```
