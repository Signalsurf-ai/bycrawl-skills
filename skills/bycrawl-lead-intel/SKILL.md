---
name: bycrawl-lead-intel
description: >
  Lead qualification and prospect research using social signals from bycrawl MCP.
  Use when user says "research this lead", "qualify this prospect", "find leads",
  "lead intelligence", "prospect research", "who is buying [product type]",
  "find people looking for [solution]", "buying signals".
allowed-tools: ["mcp__bycrawl__*"]
user-invocable: true
argument-hint: "<solution_or_prospect> [company] e.g. CRM software OR john_doe Acme Inc"
---

# Lead Intelligence

## ICP (Ideal Customer Profile)
- **SDRs and AEs** researching prospects before outreach
- **Founders** doing founder-led sales and need context on prospects
- **Agency owners** qualifying inbound leads
- **Growth teams** finding high-intent leads from social signals

## What This Skill Does
Finds people actively looking for a solution (buying signals) across social platforms, or deeply researches a specific prospect by pulling their cross-platform presence, interests, and recent activity.

## Required Input

**Mode A — Find Leads:**
- **Solution/product type** (required): What are people looking for?
- **Platforms** (optional): Where to search (default: Reddit, X, Threads)

**Mode B — Research a Prospect:**
- **Person name or handle** (required)
- **Company** (optional): For LinkedIn matching


## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Workflow — Mode A: Find Leads with Buying Signals

### Step 1: Search for Buying Intent
Run in parallel with intent-rich queries:
```
reddit_search_posts(query="{solution} recommendation", sort="new", count=30)
reddit_search_posts(query="looking for {solution}", sort="new", count=30)
x_search_posts(query="{solution} recommend OR suggestion OR alternative", product="Latest", count=30)
threads_search_posts(query="{solution}", search_type="recent", count=15)
```

### Step 2: Qualify Mentions
For each post, classify:
- **Hot lead**: Actively asking for recommendations right now
- **Warm lead**: Discussing pain points your product solves
- **Cold lead**: Casually mentioning the category

### Step 3: Enrich Hot Leads
For hot leads, pull profiles:
```
reddit_get_user(username="{user}")
x_get_user(username="{user}")
linkedin_search_users(query="{name from bio}")
```

### Step 4: Company Context
If company is identifiable:
```
linkedin_get_company(companyId="{company}")
linkedin_get_company_jobs(companyId="{company}", count=10)
```

## Workflow — Mode B: Prospect Deep Dive

### Step 1: Cross-Platform Profile Pull
```
linkedin_get_user(username="{prospect}")
x_get_user(username="{prospect}")
instagram_get_user(username="{prospect}")
reddit_get_user(username="{prospect}")
threads_get_user(username="{prospect}")
```

### Step 2: Recent Activity & Interests
```
x_get_user_posts(username="{prospect}", count=20)
```

Analyze:
- What topics they post about
- What they retweet (signals what they care about)
- Recent complaints or celebrations
- Industry events they attended

### Step 3: Company Context
```
linkedin_get_company(companyId="{company}")
linkedin_get_company_jobs(companyId="{company}", count=20)
```

Signals: company growth stage, current priorities from job postings.

## Output Format — Mode A

```markdown
## Lead Intelligence Report — "{solution}" — {date}

### Hot Leads (Actively Looking)
| # | User | Platform | Post | Signal | Link |
|---|------|----------|------|--------|------|
| 1 | u/{user} | Reddit | "Looking for a tool that..." | Direct ask | {link} |

### Warm Leads (Pain Point Match)
| # | User | Platform | Post Summary | Pain Point |
|---|------|----------|-------------|-----------|

### Outreach Talking Points
- For lead 1: "{personalized opener based on their post}"
- For lead 2: ...
```

## Output Format — Mode B

```markdown
## Prospect Dossier — {name} — {date}

### Profile Summary
- **Role**: {title} at {company}
- **LinkedIn**: {url} | **X**: @{handle} | **Other**: ...
- **Bio**: {combined bio}

### Recent Activity & Interests
- Talks about: {topics}
- Retweeted: {themes}
- Recent wins/frustrations: {context}

### Company Context
- {company}: {size}, {industry}, {stage}
- Currently hiring for: {roles} (signals: {interpretation})

### Recommended Approach
- **Opener**: {personalized based on activity}
- **Value prop angle**: {why your product matters to them}
- **Timing**: {good/bad based on signals}
```

## Error Handling

- If a platform returns **empty results**, skip it and note "No data found on {platform}" in the report — do not fail the entire workflow.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- **Always deliver partial results** rather than failing entirely — a report covering 5 of 7 platforms is still valuable.

## Estimated API Usage

- **Basic run**: ~10-15 (Mode A) API calls
- **Full run**: ~15-20 (Mode B with company context) API calls
- Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive usage.
