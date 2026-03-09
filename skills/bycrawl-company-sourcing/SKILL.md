---
name: bycrawl-company-sourcing
description: >
  Find private companies matching acquisition criteria using bycrawl MCP.
  Use when user says "find companies to acquire", "source acquisition targets",
  "deal sourcing", "find businesses in [industry]", "proprietary sourcing",
  "find owner-operated businesses", "search for companies matching [thesis]",
  "PE sourcing", "M&A sourcing", "search fund sourcing".
allowed-tools: ["mcp_bycrawl_*"]
user-invocable: true
argument-hint: "<industry_or_thesis> [geography] [size] e.g. HVAC services, Texas, 10-100 employees"
---

# Company Sourcing for Acquisitions

## ICP (Ideal Customer Profile)
- **PE deal teams** running proprietary sourcing campaigns
- **Search fund operators** looking for owner-operated businesses to acquire
- **Corporate development** teams scouting bolt-on acquisitions
- **M&A advisors** building target lists for buy-side mandates
- **Independent sponsors** sourcing off-market deals

## What This Skill Does
Finds private companies matching acquisition criteria by searching LinkedIn for companies, analyzing their size/growth/hiring signals, identifying owner-operators, and cross-referencing social presence to build a qualified target list.

## Required Input
- **Industry or thesis** (required): e.g., "HVAC services", "B2B SaaS for logistics", "home healthcare"
- **Geography** (optional): Target region or city
- **Size range** (optional): Employee count or revenue proxy (e.g., "10-100 employees")
- **Additional criteria** (optional): Owner-operated, recurring revenue, niche market leader, etc.


## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Workflow

### Step 1: Company Discovery via LinkedIn
Search for companies matching the thesis. Run multiple queries in parallel to cast a wide net:

```
linkedin_search_users(query="{industry} founder CEO owner", count=50)
linkedin_search_jobs(query="{industry}", location="{geography}", count=50)
```

From job search results, extract unique company names — companies actively hiring in the target industry signal operational scale.

From user search, identify founders/owners and their companies.

### Step 2: Filter and Qualify Companies
For each promising company found, pull the full profile:

```
linkedin_get_company(companyId="{company_vanity_name}")
```

Qualify against criteria:
- **Employee count**: Does it match the target size range?
- **Founded date**: Established enough for acquisition? (typically 5+ years)
- **Headquarters**: Matches geography requirement?
- **Description**: Confirms industry/sector fit?
- **Website**: Exists and operational?

### Step 3: Growth Signal Detection
For qualified companies, check hiring activity:

```
linkedin_get_company_jobs(companyId="{company}", count=25)
```

Analyze:
- **Total open roles**: Proxy for growth/scale
- **Role types**: Revenue-generating roles (sales, account managers) = healthy business
- **Department mix**: Engineering-heavy = product company; ops-heavy = services company
- **No open roles**: Could signal stability OR stagnation — flag for further research

### Step 4: Owner Identification
Find the owner/founder on LinkedIn and cross-reference socially:

```
linkedin_search_users(query="{owner_name} {company_name}", count=5)
linkedin_get_user(username="{owner_linkedin}")
x_get_user(username="{owner_handle}")
x_get_user_posts(username="{owner_handle}", count=20)
```

Look for owner-operator signals:
- Founded the company (LinkedIn profile shows founder/CEO since founding)
- Long tenure (10+ years at the company)
- Active on social (engaged owners = reachable)
- Posts about the business directly

### Step 5: Seller Readiness Signals
From the owner's social activity, look for soft sell signals:

```
x_search_posts(query="from:{owner_handle} retire OR transition OR next chapter OR stepping back OR succession OR exit OR sell", product="Latest", count=20)
reddit_search_posts(query="{company_name} OR {owner_name} selling OR acquisition OR exit", sort="new", count=20)
```

Signals that suggest openness to a transaction:
- Talking about retirement, next chapter, or life transitions
- Hiring a GM or COO (delegating = potentially stepping back)
- Reduced posting frequency (disengagement)
- Posts about burnout or fatigue
- Company anniversary posts ("20 years of building this...")

### Step 6: Market Presence Validation
Confirm the company has real market presence:

```
x_search_posts(query="{company_name}", product="Top", count=15)
reddit_search_posts(query="{company_name}", sort="top", count=15)
facebook_search_posts(query="{company_name}", count=5)
```

Check:
- Do customers mention them positively?
- Any reviews, testimonials, or case studies visible?
- Community presence in their niche?

## Output Format

```markdown
## Acquisition Target Sourcing Report — {industry/thesis} — {date}

### Search Criteria
- **Thesis**: {description}
- **Geography**: {location}
- **Size target**: {employee range}
- **Additional filters**: {criteria}

### Pipeline Summary
- Companies identified: {n}
- Qualified targets: {n}
- High-priority (seller signals detected): {n}

### Qualified Targets

| # | Company | Industry | Employees | Founded | HQ | Owner | Seller Signals | Priority |
|---|---------|----------|-----------|---------|----|-------|----------------|----------|
| 1 | {name} | {sub-industry} | {n} | {year} | {city} | {owner_name} | {signals} | High/Med/Low |

### Target Profiles

#### 1. {Company Name}
- **LinkedIn**: {url}
- **Website**: {url}
- **Founded**: {year} ({n} years operating)
- **Employees**: {count}
- **HQ**: {location}
- **Description**: {what they do}

**Owner Profile**
- **Name**: {name}
- **Title**: {role} (since {year})
- **LinkedIn**: {url}
- **X/Twitter**: @{handle}
- **Tenure**: {n} years at company

**Growth Signals**
- Open roles: {n} ({department breakdown})
- Hiring trend: {growing/stable/contracting}
- Revenue proxies: {signals from job postings, team size}

**Seller Readiness**
- Signals detected: {list of signals or "None detected"}
- Owner engagement level: {active/moderate/declining}
- Succession planning: {evidence or "No evidence"}

**Market Presence**
- Social mentions: {positive/neutral/negative}
- Customer sentiment: {summary}
- Reputation: {summary}

**Fit Score**: {High/Medium/Low} — {reasoning}

### Outreach Priority List
1. **{Company}** — {owner_name} — {why prioritize}: {recommended outreach channel}
2. **{Company}** — {owner_name} — {reason}: {channel}
3. ...

### Sectors to Dig Deeper
- {sub-niche that surfaced multiple targets}
- {adjacent industry worth exploring}
```

## Error Handling

- If a platform returns **empty results**, skip it and note "No data found on {platform}" in the report — do not fail the entire workflow.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- **Always deliver partial results** rather than failing entirely — a report covering 5 of 7 platforms is still valuable.

## Estimated API Usage

- **Basic run**: ~15-20 API calls
- **Full run**: ~40-50 with owner research and market validation API calls
- Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive usage.
