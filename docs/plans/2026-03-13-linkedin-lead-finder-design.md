# LinkedIn Lead Finder Skill Design

**Linear Issue**: SIG-40
**Date**: 2026-03-13
**Approach**: Search-First (ByCrawl-Native)

## Skill Identity

- **Name**: `bycrawl-linkedin-lead-finder`
- **Trigger phrases**: "find LinkedIn leads", "LinkedIn prospecting", "find decision makers", "build a prospect list", "LinkedIn lead generation"

## ICP

- SDRs and AEs building outbound pipeline
- Founders doing founder-led sales
- Agency owners prospecting for clients
- Growth teams finding high-intent leads

## What It Does

Finds and scores LinkedIn prospects matching an ICP using ByCrawl LinkedIn search + job search + cross-platform enrichment, then generates personalized outreach messages. Inspired by [Steve Arneson's LinkedIn prospecting methodology](https://stevearneson.com/prospect), adapted for ByCrawl MCP.

## Required Input

- **Target titles/roles** (required): e.g. "VP of Engineering", "Head of Marketing"
- **Industry or keywords** (required): e.g. "SaaS", "AI", "fintech"
- **Company size/stage** (optional): e.g. "50-200 employees", "Series A-B"
- **Geographic focus** (optional): e.g. "San Francisco", "DACH region"
- **Your product/service** (optional but recommended): For personalized outreach generation

## Intensity Selection (User Chooses at Runtime)

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

Generate as many query variations as possible from the ICP — every title synonym, every keyword combo, every geo variation. Scale number of variations by intensity mode.

### Step 2: Search LinkedIn Users

```
linkedin_search_users(query="{title_v1} {industry}", count={mode_count})
linkedin_search_users(query="{title_v2} {industry}", count={mode_count})
linkedin_search_users(query="{title_v3} {keyword_1}", count={mode_count})
linkedin_search_users(query="{title_v4} {keyword_2}", count={mode_count})
linkedin_search_users(query="{title_v1} {geo}", count={mode_count})
linkedin_search_users(query="{title_v2} {geo}", count={mode_count})
... (scale by intensity mode)
```

All in parallel. Deduplicate by username.

### Step 3: Profile Prospects

```
linkedin_get_user(username="{prospect}")
```

For all deduplicated results (Blitz), top 50 (Balanced), or top 20 (Lite).

### Step 4a: Search Jobs Aggressively

```
linkedin_search_jobs(query="{solution} {industry}", count={mode_count})
linkedin_search_jobs(query="{keyword_1} {role}", count={mode_count})
linkedin_search_jobs(query="{keyword_2} {title}", count={mode_count})
linkedin_search_jobs(query="{solution} {geo}", count={mode_count})
... (scale by intensity mode)
```

All in parallel. Extract unique companies from job listings.

### Step 4b: Company Deep Dive

For every unique company from both user search and job search:

```
linkedin_get_company(companyId="{company}")
linkedin_get_company_jobs(companyId="{company}", count={mode_count})
```

Scale coverage by intensity mode.

### Step 4c: Find Decision Makers at Every Company

For high-signal companies (all in Blitz, top 25 in Balanced, top 10 in Lite):

```
linkedin_search_users(query="{target_title} {company_name}", count={mode_count})
linkedin_get_user(username="{new_prospect}")
```

Profile any new prospects discovered.

### Step 5: Cross-Platform Enrichment

```
x_get_user(username="{name_match}")
x_get_user_posts(username="{handle}", count={mode_count})
threads_search_users(query="{full_name}", count={mode_count})
threads_get_user(username="{handle}")
reddit_search_posts(query="{full_name} OR {company}", count={mode_count})
instagram_get_user(username="{handle_guess}")
```

Scale: all prospects (Blitz), top 20 (Balanced), top 10 (Lite). All in parallel per prospect.

### Step 6: Score & Tier

5 dimensions, 100 points total (adapted from Arneson methodology):

- **Authority (30pts)**: C-suite/Founder = 30, VP = 25, Director = 20, Manager = 10
- **Scale (20pts)**: Company size/stage fit to offering
- **Signals (25pts)**: Hiring activity, social engagement, content themes matching space
- **Proximity (15pts)**: Industry/vertical closeness to ICP
- **Reachability (10pts)**: Cross-platform presence, public email, content activity

Tiers:
- 70-100: **Priority** — reach out this week
- 50-69: **Nurture** — engage within 30 days
- 30-49: **Watch** — monitor for trigger events

### Step 7: Generate Personalized Outreach

For leads based on intensity mode (Priority only / Priority+Nurture / All tiers):

- 2-3 sentences maximum
- Reference something specific (recent post, company milestone, hiring signal)
- Sound like a smart peer, not a vendor
- Soft CTA (open door, not a pitch)

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

- Platform returns empty → skip, note "No data on {platform}"
- API key missing → stop, instruct user to set `BYCRAWL_API_KEY`
- Rate limit → reduce `count` by half, retry once
- MCP tool unavailable → check ByCrawl MCP installation
- Always deliver partial results — never fail the entire workflow

## Estimated API Usage

| Mode | API Calls | Credits |
|------|-----------|---------|
| Lite | 50-100 | ~50-100 |
| Balanced | 150-300 | ~150-300 |
| Blitz | 500-1000+ | ~500-1000+ |
