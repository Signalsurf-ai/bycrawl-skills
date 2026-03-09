---
name: bycrawl-prospect-dossier
description: >
  Build a complete cross-platform profile on a person or company using bycrawl MCP.
  Use when user says "research this person", "build a dossier", "who is [person]",
  "tell me about [person]", "prep for a meeting with [person]",
  "learn about [company]", "pre-call research", "meeting prep".
allowed-tools: ["mcp_bycrawl_*"]
user-invocable: true
argument-hint: "<person_name_or_handle> [company] e.g. johndoe Acme Corp"
---

# Prospect Dossier

## ICP (Ideal Customer Profile)
- **Sales reps** preparing for discovery calls
- **Founders** meeting investors, partners, or customers
- **BD teams** doing pre-meeting research
- **Journalists or analysts** profiling a person or company

## What This Skill Does
Creates a comprehensive profile by pulling someone's presence across LinkedIn, X, Instagram, Reddit, Threads, and YouTube. Synthesizes their interests, recent activity, communication style, and professional background into pre-meeting talking points.

## Required Input
- **Person's name or handle** (required)
- **Company** (optional, helps with LinkedIn matching)
- **Meeting context** (optional): "sales call", "partnership", "investor meeting"


## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Workflow

### Step 1: Find Them Everywhere
Run in parallel:
```
linkedin_get_user(username="{handle}")
linkedin_search_users(query="{full_name} {company}", count=5)
x_get_user(username="{handle}")
instagram_get_user(username="{handle}")
threads_get_user(username="{handle}")
reddit_get_user(username="{handle}")
youtube_get_channel(channelId="@{handle}")
```

### Step 2: Recent Activity Deep Dive
For platforms where they're active:
```
x_get_user_posts(username="{handle}", count=30)
threads_get_user_posts(userId="{id}", count=20)
instagram_get_user_posts(username="{handle}")
tiktok_get_user_videos(username="{handle}", count=10)
```

Analyze:
- Topics they care about right now
- Opinions they've shared publicly
- Content they engage with (retweets, replies)
- Tone and communication style

### Step 3: Company Context
```
linkedin_get_company(companyId="{company}")
linkedin_get_company_jobs(companyId="{company}", count=20)
```

Understand: company stage, priorities, growth areas.

### Step 4: Media & Public Mentions
```
youtube_search_videos(q="{full_name}", count=5)
x_search_posts(query="{full_name}", product="Top", count=10)
reddit_search_posts(query="{full_name}", sort="top", count=10)
```

Find: podcast appearances, conference talks, interviews, press mentions.

## Output Format

```markdown
## Prospect Dossier — {name} — {date}

### Quick Summary
{2-3 sentence overview of who they are and what they care about}

### Professional Profile
- **Title**: {role} at {company}
- **Company**: {description, size, funding stage}
- **Location**: {city}
- **LinkedIn**: {url}

### Social Presence
| Platform | Handle | Followers | Activity Level |
|----------|--------|-----------|---------------|
| X | @{handle} | {n} | {Daily/Weekly/Inactive} |
| Instagram | @{handle} | {n} | {level} |
| ... | ... | ... | ... |

### What They Care About Right Now
1. {topic from recent posts}
2. {topic}
3. {topic}

### Recent Notable Posts
- "{post_excerpt}" — {date} on {platform} — {engagement}

### Communication Style
- Tone: {formal/casual/technical/opinionated}
- Posts about: {themes}
- Engages with: {types of content they retweet/like}

### Company Signals
- Currently hiring: {key roles}
- Growth signals: {interpretation}
- Recent news: {if found}

### Talking Points for Your Meeting
1. **Ice breaker**: "{based on their recent post or interest}"
2. **Relevance hook**: "{connect your product to their stated priority}"
3. **Shared interest**: "{something you have in common}"

### Things to Avoid
- {topics they seem negative about}
- {competitors they praise — don't bash them}
```

## Error Handling

- If a platform returns **empty results**, skip it and note "No data found on {platform}" in the report — do not fail the entire workflow.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- **Always deliver partial results** rather than failing entirely — a report covering 5 of 7 platforms is still valuable.

## Estimated API Usage

- **Basic run**: ~15-20 API calls
- **Full run**: ~25-30 with media and company context API calls
- Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive usage.
