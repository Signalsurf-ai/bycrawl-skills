---
name: bycrawl-market-research
description: >
  Product-market fit validation and market sentiment research using bycrawl MCP.
  Use when user says "market research", "product-market fit", "validate my idea",
  "is there demand for", "what do people think about [category]",
  "market sentiment", "TAM research from social", "customer discovery".
---

# Market Research

## ICP (Ideal Customer Profile)
- **Founders** validating a product idea before building
- **Product managers** researching demand for a new feature
- **Investors** doing due diligence on a market category
- **Indie hackers** looking for underserved niches with real demand

## What This Skill Does
Validates product-market fit by searching for real user demand signals, pain points, existing alternatives, and willingness to pay across social platforms. Answers: "Do people actually want this?"

## Required Input
- **Product idea or category** (required): What to validate
- **Target audience** (optional): Who would use it
- **Known alternatives** (optional): Existing solutions to compare against

## Workflow

### Step 1: Demand Signal Search
Run in parallel:
```
reddit_search_posts(query="{category} recommendation OR looking for OR need a", sort="top", count=30)
reddit_search_posts(query="{category} alternative to OR replacement for OR better than", sort="top", count=30)
x_search_posts(query="{category} need OR wish OR looking for", product="Top", count=30)
youtube_search_videos(q="{category} best OR review OR comparison", count=15)
tiktok_search_videos(keyword="{category}", count=20)
```

### Step 2: Pain Point Extraction
From Reddit and X results, extract:
- What specific problems people describe
- What they've tried and why it failed
- What they wish existed
- Price sensitivity signals ("too expensive", "worth paying for")

### Step 3: Alternative Landscape
Search for known alternatives:
```
reddit_search_posts(query="{alternative_1} vs {alternative_2}", sort="top", count=20)
x_search_posts(query="{alternative_1} OR {alternative_2} review", product="Top", count=20)
```

Map: what each alternative does well, where users complain.

### Step 4: Audience Profiling
From the most active discussants, pull profiles to understand who cares about this:
```
reddit_get_user(username="{active_user}")
x_get_user(username="{active_user}")
linkedin_search_users(query="{relevant_title}", count=20)
```

### Step 5: Market Size Signals
```
linkedin_search_jobs(query="{category}", count=30)
instagram_search_tags(query="{category}")
```

Job postings mentioning the category = enterprise demand.
Instagram hashtag volume = consumer awareness.

## Output Format

```markdown
## Market Research Report — {category} — {date}

### Demand Verdict: {STRONG / MODERATE / WEAK / NO SIGNAL}

### Evidence of Demand
- Reddit: {n} posts asking for solutions (top post: {upvotes} upvotes)
- X: {n} people expressing need
- YouTube: {n} review/comparison videos (top: {views} views)
- TikTok: {n} videos (top: {views} views)

### Top Pain Points (ranked by frequency)
1. **{pain_1}** — mentioned {n} times — "{example quote}"
2. **{pain_2}** — mentioned {n} times
3. **{pain_3}** — mentioned {n} times

### Existing Alternatives & Their Weaknesses
| Alternative | Strengths | Weaknesses (from users) | Pricing |
|-------------|-----------|-------------------------|---------|

### Willingness to Pay
- Price anchors from discussions: {range}
- Complaints about pricing: {summary}
- Free vs paid preference: {signal}

### Target Audience Profile
- Typical user: {description from profiles}
- Common job titles: {from LinkedIn}
- Where they hang out: {platforms/subreddits}

### Market Size Indicators
- LinkedIn jobs mentioning this: {n}
- Instagram hashtag volume: {n} posts
- YouTube content volume: {n} videos

### Go/No-Go Recommendation
{recommendation with reasoning}

### If Go: Positioning Suggestion
- Lead with: {pain point to solve}
- Differentiate on: {what competitors miss}
- Price at: {suggested range based on signals}
```
