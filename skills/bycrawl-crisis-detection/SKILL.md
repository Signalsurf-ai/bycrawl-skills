---
name: bycrawl-crisis-detection
description: >
  Real-time brand reputation monitoring and crisis detection using bycrawl MCP.
  Use when user says "crisis monitoring", "reputation check", "are people complaining",
  "negative mentions", "PR crisis", "damage control", "reputation management",
  "check for complaints about [brand]", "brand safety check".
allowed-tools: ["mcp_bycrawl_*"]
user-invocable: true
argument-hint: "<brand_name> [known_issue] e.g. Tesla, battery recall"
---

# Crisis Detection

## ICP (Ideal Customer Profile)
- **PR and communications teams** monitoring for reputation threats
- **Customer success leads** detecting complaint spikes before they escalate
- **Founders** who need early warning when something goes wrong publicly
- **Community managers** staying ahead of negative sentiment

## What This Skill Does
Searches for negative mentions, complaints, and controversy around a brand across all platforms simultaneously. Classifies severity, identifies the source and spread, and recommends response actions.

## Required Input
- **Brand name** (required): The brand to monitor
- **Known issues** (optional): Any current incidents to track specifically
- **Severity threshold** (optional): "all", "medium+", or "critical only"


## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Workflow

### Step 1: Negative Signal Search
Run ALL in parallel with negative-sentiment queries:
```
x_search_posts(query="{brand} scam OR broken OR terrible OR worst OR avoid OR disappointed OR fraud", product="Latest", count=30)
reddit_search_posts(query="{brand} problem OR issue OR bug OR complaint OR scam", sort="new", count=30)
facebook_search_posts(query="{brand} problem", count=5)
threads_search_posts(query="{brand}", search_type="recent", count=20)
youtube_search_videos(q="{brand} review OR problem OR issue", count=10)
```

### Step 2: Baseline Comparison
Also search for the brand without negative keywords to establish the ratio:
```
x_search_posts(query="{brand}", product="Latest", count=30)
reddit_search_posts(query="{brand}", sort="new", count=20)
```

Calculate: negative mentions / total mentions = negativity ratio.
- < 10%: Normal
- 10-25%: Elevated — monitor closely
- 25%+: Crisis territory — immediate action needed

### Step 3: Severity Classification
For each negative mention:
- **Critical**: Viral potential (high engagement), accusations of fraud/safety issues, media involvement
- **High**: Customer complaints with evidence, competitor amplification, influencer criticism
- **Medium**: Frustration posts, feature complaints, comparison losses
- **Low**: Minor gripes, isolated incidents, clearly unreasonable complaints

### Step 4: Source & Spread Analysis
For critical/high mentions, trace the spread:
```
x_get_post(postId="{viral_post_id}")  // check retweet/quote count
reddit_get_post(postId="{post_id}")   // check comment count and upvotes
youtube_get_video_comments(videoId="{id}", count=50)  // check if comments are piling on
```

### Step 5: Identify Key Voices
For high-severity mentions, check if the author has influence:
```
x_get_user(username="{author}")
reddit_get_user(username="{author}")
```

Flag if: verified account, high follower count, journalist, or industry figure.

## Output Format

```markdown
## Crisis Detection Report — {brand} — {date}

### Threat Level: {NORMAL / ELEVATED / CRISIS}
- Negativity ratio: {n}% ({negative_count}/{total_count} mentions)
- Trend vs last check: {increasing/stable/decreasing}

### Critical Alerts
| # | Platform | Post | Author | Reach | Severity | Link |
|---|----------|------|--------|-------|----------|------|
| 1 | X | "{summary}" | @{user} ({followers}) | {views/RTs} | CRITICAL | {link} |

### Issue Clusters
1. **{issue_theme_1}**: {n} mentions — {summary}
2. **{issue_theme_2}**: {n} mentions — {summary}

### Influential Voices to Watch
- @{user} ({follower_count} followers) — {what they said}

### Recommended Actions
1. **Immediate**: {action for critical items}
2. **Within 24h**: {action for high items}
3. **Monitor**: {items to watch}

### Draft Response Templates
- For {issue_1}: "{suggested response}"
- For {issue_2}: "{suggested response}"
```

## Error Handling

- If a platform returns **empty results**, skip it and note "No data found on {platform}" in the report — do not fail the entire workflow.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- **Always deliver partial results** rather than failing entirely — a report covering 5 of 7 platforms is still valuable.

## Estimated API Usage

- **Basic run**: ~12-15 API calls
- **Full run**: ~25-30 with source tracing and author checks API calls
- Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive usage.
