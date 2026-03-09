---
name: bycrawl-competitor-intel
description: >
  Competitive intelligence gathering across social media and job boards using bycrawl MCP.
  Use when user says "competitor analysis", "competitive intelligence", "competitor audit",
  "what is [company] doing", "spy on competitor", "competitor social strategy",
  "competitor hiring", "share of voice".
allowed-tools: ["mcp_bycrawl_*"]
user-invocable: true
argument-hint: "<competitor_name> [your_brand] e.g. Stripe, our brand: Paddle"
---

# Competitor Intelligence

## ICP (Ideal Customer Profile)
- **Product managers** assessing competitor positioning and feature launches
- **Marketing leads** benchmarking social strategy against competitors
- **Founders** tracking competitor growth signals (hiring, content, engagement)
- **Sales teams** needing competitive battlecards with fresh data

## What This Skill Does
Builds a comprehensive competitive profile by pulling a competitor's social presence, content strategy, hiring activity, audience sentiment, and community engagement across all platforms.

## Required Input
- **Competitor name or handle** (required): The company to research
- **Your brand** (optional): For direct comparison
- **Focus area** (optional): "social", "hiring", "sentiment", or "all"


## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Workflow

### Step 1: Company Profile Pull
Run in parallel:
```
linkedin_get_company(companyId="{competitor}")
x_get_user(username="{competitor}")
instagram_get_user(username="{competitor}")
tiktok_get_user(username="{competitor}")
facebook_get_page(username="{competitor}")
youtube_get_channel(channelId="@{competitor}")
reddit_get_subreddit(name="{competitor}")
```

### Step 2: Content Strategy Analysis
Pull recent content from each platform in parallel:
```
x_get_user_posts(username="{competitor}", count=40)
instagram_get_user_posts(username="{competitor}")
tiktok_get_user_videos(username="{competitor}", count=30)
facebook_get_page_posts(username="{competitor}")
```

Analyze:
- Posting frequency per platform
- Content themes and formats (video, text, carousel, etc.)
- Engagement rates (likes/followers ratio)
- Best-performing content by engagement
- Hashtag strategy

### Step 3: Hiring Signal Detection
```
linkedin_get_company_jobs(companyId="{competitor}", count=50)
linkedin_search_jobs(query="{competitor}", count=20)
```

Analyze:
- Total open roles and growth rate
- Department breakdown (engineering, sales, marketing)
- Strategic signals (e.g., "hiring 5 AI engineers" = building AI product)
- Location expansion signals

### Step 4: Public Sentiment
```
x_search_posts(query="{competitor}", product="Latest", count=30)
reddit_search_posts(query="{competitor}", sort="new", count=30)
threads_search_posts(query="{competitor}", search_type="recent", count=15)
```

Classify sentiment and identify:
- Customer complaints and praise
- Feature requests from users
- Comparison mentions ("{competitor} vs {your_brand}")

### Step 5: Community Health
If subreddit exists:
```
reddit_get_subreddit_posts(name="{competitor}", sort="hot", count=20)
```

Check: post frequency, engagement, moderator activity, top user concerns.

## Output Format

```markdown
## Competitive Intelligence Report — {competitor} — {date}

### Company Overview
- LinkedIn: {followers}, {employee_count}, HQ: {location}
- Description: {tagline}

### Social Footprint
| Platform | Followers | Posts/week | Avg Engagement | Top Format |
|----------|-----------|------------|----------------|------------|

### Content Strategy Summary
- Primary themes: ...
- Top-performing post: {link} ({engagement})
- Content gap vs your brand: ...

### Hiring Signals
- Total open roles: {n}
- Top departments: ...
- Strategic reads: ...

### Public Sentiment
- Positive: {n}% | Neutral: {n}% | Negative: {n}%
- Top complaints: ...
- Top praise: ...

### Competitive Positioning
- They emphasize: ...
- They ignore: ...
- Your advantage: ...
- Their advantage: ...

### Recommended Actions
1. ...
2. ...
```

## Error Handling

- If a platform returns **empty results**, skip it and note "No data found on {platform}" in the report — do not fail the entire workflow.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- **Always deliver partial results** rather than failing entirely — a report covering 5 of 7 platforms is still valuable.

## Estimated API Usage

- **Basic run**: ~20-25 API calls
- **Full run**: ~35-45 with community deep dive API calls
- Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive usage.
