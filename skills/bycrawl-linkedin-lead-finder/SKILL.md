---
name: bycrawl-linkedin-lead-finder
description: >
  LinkedIn lead generation and prospect scoring using bycrawl MCP.
  Use when user says "find LinkedIn leads", "LinkedIn prospecting", "find decision makers",
  "build a prospect list", "LinkedIn lead generation", "LinkedIn lead finder",
  "find prospects on LinkedIn", "LinkedIn outbound".
allowed-tools: ["mcp__bycrawl__*"]
user-invocable: true
argument-hint: "<target_titles>, <industry_or_keywords> [company_size] [geo] [your_product] e.g. VP of Engineering, SaaS, Series A-B, SF, developer tools"
---

# LinkedIn Lead Finder

## ICP (Ideal Customer Profile)
- **SDRs and AEs** building outbound pipeline
- **Founders** doing founder-led sales
- **Agency owners** prospecting for clients
- **Growth teams** finding high-intent leads from LinkedIn

## What This Skill Does
Finds and scores LinkedIn prospects matching an ICP using ByCrawl LinkedIn search + job search + cross-platform enrichment, then generates personalized outreach messages.

## Required Input

- **Target titles/roles** (required): e.g. "VP of Engineering", "Head of Marketing"
- **Industry or keywords** (required): e.g. "SaaS", "AI", "fintech"
- **Company size/stage** (optional): e.g. "50-200 employees", "Series A-B"
- **Geographic focus** (optional): e.g. "San Francisco", "DACH region"
- **Your product/service** (optional but recommended): For personalized outreach generation

## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Intensity Selection

Before starting the workflow, ask the user to pick a mode:

| Mode | Description | Est. Credits |
|------|------------|-------------|
| **Blitz** | Max everything — all query combos, all profiles, all companies, all jobs, full cross-platform enrichment | 500-1000+ |
| **Balanced** | Moderate queries, profile top 50, enrich top 20 | 150-300 |
| **Lite** | Focused queries, profile top 20, enrich top 10 | 50-100 |

### Scaling Parameters

| Parameter | Lite | Balanced | Blitz |
|-----------|------|----------|-------|
| Search query variations | 2-3 | 4-6 | As many as ICP warrants |
| `count` per search | 20 | 30 | 50 |
| Profiles pulled | Top 20 | Top 50 | All |
| Company deep dives | Top 10 | Top 25 | All |
| Jobs per company | 10 | 30 | 50 |
| Cross-platform enrichment | Top 10 | Top 20 | All |
| Outreach generation | Priority only | Priority + Nurture | All tiers |

## Workflow

### Step 1: Build Search Queries
Transform ICP criteria into multiple search queries — title synonyms, keyword combos, geo variations. Scale number of variations by intensity mode.

### Step 2: Search LinkedIn Users
Run in parallel:
```
linkedin_search_users(query="{title_v1} {industry}", count={mode_count})
linkedin_search_users(query="{title_v2} {industry}", count={mode_count})
linkedin_search_users(query="{title_v3} {keyword_1}", count={mode_count})
linkedin_search_users(query="{title_v1} {geo}", count={mode_count})
... (scale by intensity)
```
Deduplicate results by username.

### Step 3: Profile Prospects
```
linkedin_get_user(username="{prospect}")
```
For all deduplicated results (Blitz), top 50 (Balanced), or top 20 (Lite).

### Step 4a: Search Jobs
Run in parallel:
```
linkedin_search_jobs(query="{solution} {industry}", count={mode_count})
linkedin_search_jobs(query="{keyword_1} {role}", count={mode_count})
linkedin_search_jobs(query="{keyword_2} {title}", count={mode_count})
linkedin_search_jobs(query="{solution} {geo}", count={mode_count})
... (scale by intensity)
```
Extract unique companies from job listings.

### Step 4b: Company Deep Dive
For all unique companies (scaled by mode):
```
linkedin_get_company(companyId="{company}")
linkedin_get_company_jobs(companyId="{company}", count={mode_count})
```

### Step 4c: Find Decision Makers at Companies
For high-signal companies (scaled by mode):
```
linkedin_search_users(query="{target_title} {company_name}", count={mode_count})
linkedin_get_user(username="{new_prospect}")
```

### Step 5: Cross-Platform Enrichment
For prospects (scaled by mode), run in parallel per prospect:
```
x_get_user(username="{name_match}")
x_get_user_posts(username="{handle}", count={mode_count})
threads_search_users(query="{full_name}", count={mode_count})
threads_get_user(username="{handle}")
reddit_search_posts(query="{full_name} OR {company}", count={mode_count})
instagram_get_user(username="{handle_guess}")
youtube_search_videos(query="{full_name} {company}", count={mode_count})
youtube_get_channel(channelId="{channel_id}")
```

### Step 6: Score & Tier
5 dimensions, 100 points total:
- **Authority (30pts)**: C-suite/Founder=30, VP=25, Director=20, Manager=10
- **Scale (20pts)**: Company size/stage fit to offering
- **Signals (25pts)**: Hiring activity, social engagement, content themes matching space
- **Proximity (15pts)**: Industry/vertical closeness to ICP
- **Reachability (10pts)**: Cross-platform presence, public email, content activity

Tiers:
- **70-100: Priority** — reach out this week
- **50-69: Nurture** — engage within 30 days
- **30-49: Watch** — monitor for trigger events

### Step 7: Generate Personalized Outreach
For leads based on intensity mode (Priority only / Priority+Nurture / All tiers):
- 2-3 sentences maximum
- Reference something specific (recent post, company milestone, hiring signal)
- Sound like a smart peer, not a vendor
- Soft CTA (open door, not a pitch)
- Never open with "I came across your profile"

## Output Format

```markdown
## LinkedIn Lead Finder Report — "{ICP description}" — {date}
### Run Config: {Blitz/Balanced/Lite} | Credits Used: ~{count}

### Summary
- **Prospects found**: {n} unique across {x} searches
- **Companies mapped**: {n} with {n} job listings analyzed
- **Priority leads**: {n} | **Nurture**: {n} | **Watch**: {n}

### Priority Leads (Score 70-100) — Reach Out This Week
| # | Name | Title | Company | Score | Top Signal | Outreach Message |
|---|------|-------|---------|-------|-----------|-----------------|
| 1 | {name} | {title} | {company} | 92 | Hiring 3 engineers, posted about {topic} | "{2-3 sentence message}" |

### Nurture Leads (Score 50-69) — Engage Within 30 Days
| # | Name | Title | Company | Score | Top Signal | Outreach Message |
|---|------|-------|---------|-------|-----------|-----------------|

### Watch List (Score 30-49) — Monitor for Triggers
| # | Name | Title | Company | Score | Top Signal |
|---|------|-------|---------|-------|-----------|

### Score Breakdown (Top 20)
| Name | Authority | Scale | Signals | Proximity | Reachability | Total |
|------|-----------|-------|---------|-----------|-------------|-------|

### Company Intelligence
| Company | Size | Industry | Open Roles | Hiring Velocity | Key Signal |
|---------|------|----------|-----------|----------------|-----------|

### Cross-Platform Insights
| Name | X/Twitter | Threads | Reddit | Key Themes |
|------|-----------|---------|--------|-----------|

### Recommended Next Steps
1. {Prioritized action items based on findings}
```

## Error Handling

- If a platform returns **empty results**, skip it and note "No data found on {platform}" in the report — do not fail the entire workflow.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- **Always deliver partial results** rather than failing entirely — a report covering 5 of 7 platforms is still valuable.

## Estimated API Usage

| Mode | API Calls | Credits |
|------|-----------|---------|
| Lite | 50-100 | ~50-100 |
| Balanced | 150-300 | ~150-300 |
| Blitz | 500-1000+ | ~500-1000+ |

Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive usage.
