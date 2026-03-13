---
name: bycrawl-se-account-research
description: >
  Sales Engineering account research and intelligence using bycrawl MCP + web search.
  Creates comprehensive company dossiers, stakeholder intelligence, and strategic account
  briefings by combining social media intelligence with public web data.
  Use when user says "research this company", "account research", "company briefing",
  "tell me about [company]", "account intelligence", "prospect research", "company dossier",
  "SE research", "pre-call research for [company]", "strategic account plan".
allowed-tools: ["mcp__bycrawl__*", "WebFetch", "WebSearch"]
user-invocable: true
argument-hint: "<company_name> [your_product] [research_tier: 1|2|3] e.g. Stripe, developer tools, 2"
---

# SE Account Research

Create actionable intelligence briefings by combining bycrawl social data with public web research.

## ICP (Ideal Customer Profile)
- **Sales Engineers** preparing technical discovery calls
- **Account Executives** building strategic account plans
- **Founders** doing founder-led sales research
- **BD teams** evaluating partnerships or prospects
- **Pre-sales teams** preparing demos and POC strategies

## What This Skill Does
Builds a comprehensive account briefing by pulling company and stakeholder data from LinkedIn, X, Reddit, YouTube, and other platforms via bycrawl MCP, then enriches with web search for news, tech stack signals, funding data, and competitive landscape. For Taiwan-based companies, also pulls from 104 Job Bank (Taiwan's largest job board) for local hiring signals and company data. Synthesizes everything into a structured briefing with pain hypotheses, discovery questions, and entry strategy.

## Required Input
- **Company name** (required)
- **Your product/service** (optional but recommended): For relevance scoring and competitive positioning
- **Research tier** (optional, default: 2):
  - **Tier 1 - Quick Intel**: Company snapshot, recent news, key stakeholders
  - **Tier 2 - Standard Briefing**: Full profile, tech stack, stakeholder mapping, pain hypotheses
  - **Tier 3 - Strategic Account Plan**: Deep competitive landscape, org dynamics, multi-threaded entry strategy
- **Meeting context** (optional): "discovery call", "demo", "executive briefing", "renewal"
- **Region** (optional): If "Taiwan" or company is Taiwan-based, enables 104 Job Bank integration for local hiring and company intelligence

## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Intensity by Tier

| Parameter | Tier 1 | Tier 2 | Tier 3 |
|-----------|--------|--------|--------|
| LinkedIn company deep dive | Basic | Full + jobs | Full + jobs + related companies |
| Stakeholder profiling | Top 3-5 | Top 10 | All relevant + org mapping |
| Cross-platform enrichment | LinkedIn only | LinkedIn + X + Reddit | All platforms |
| Web search queries | 3-5 | 8-12 | 15-25 |
| WebFetch pages | 2-3 | 5-8 | 10-15 |
| Job posting analysis | Skip | Top 10 | Top 30 |
| 104 Job Bank (Taiwan) | Skip | If Taiwan-based | Full company + jobs |
| Competitive analysis | Skip | Basic | Deep with social signals |

## Workflow

### Step 1: Company Foundation (All Tiers)
Run in parallel:
```
linkedin_get_company(companyId="{company_name}")
WebSearch(query="{company_name} company overview funding")
WebSearch(query="{company_name} recent news 2024 2025")
```

From LinkedIn data, extract: company size, industry, description, specialties, headquarters.
From web search: funding rounds, recent announcements, press coverage.

**If Taiwan-based company** — also run in parallel:
```
job104_search_jobs(query="{company_name}", count=20)
job104_get_company(companyId="{104_company_id}")
```
104 provides: local employee count, industry classification, capital, benefits, and Taiwan-specific hiring velocity.

If LinkedIn returns a company URL or website, fetch it:
```
WebFetch(url="{company_website}/about")
WebFetch(url="{company_website}/careers")
```

### Step 2: Stakeholder Discovery (All Tiers — depth varies)
```
linkedin_search_users(query="CEO {company_name}", count={tier_count})
linkedin_search_users(query="CTO VP Engineering {company_name}", count={tier_count})
linkedin_search_users(query="VP Director {relevant_department} {company_name}", count={tier_count})
```

For matched stakeholders, profile them:
```
linkedin_get_user(username="{stakeholder}")
```

**Tier 2+**: Cross-platform enrichment for key stakeholders:
```
x_get_user(username="{handle}")
x_get_user_posts(username="{handle}", count=20)
threads_search_users(query="{full_name}", count=5)
```

**Tier 3**: Full social footprint:
```
reddit_search_posts(query="{stakeholder_name} {company}", count=10)
youtube_search_videos(q="{stakeholder_name}", count=5)
instagram_get_user(username="{handle_guess}")
```

### Step 3: Technology & Hiring Signals (Tier 2+)
```
linkedin_get_company_jobs(companyId="{company}", count={10|30})
linkedin_search_jobs(query="{company_name}", count={10|30})
WebSearch(query="{company_name} tech stack technology")
WebSearch(query="{company_name} engineering blog")
```

**If Taiwan-based company** — also pull 104 job details:
```
job104_search_jobs(query="{company_name}", count={10|30})
job104_get_job(jobId="{job_id}")  // for top results
```
104 job listings often include salary ranges, required skills, and detailed job descriptions in Traditional Chinese — extract tech stack signals from these.

Analyze job postings (LinkedIn + 104 combined) for:
- Tools and platforms mentioned (CRM, cloud provider, frameworks)
- Team growth areas (what problems are they investing in?)
- Seniority of hires (scaling vs. building?)
- **Taiwan-specific**: Salary benchmarks, local benefits, bilingual requirements (signals for international expansion)

**Tier 2+**: Fetch engineering blog or tech pages if found:
```
WebFetch(url="{engineering_blog_url}")
```

### Step 4: Competitive & Market Context (Tier 2+)
```
WebSearch(query="{company_name} competitors alternatives")
WebSearch(query="{company_name} vs")
WebSearch(query="{company_name} {your_product_category} market")
```

**Tier 3**: Social sentiment and competitive chatter:
```
x_search_posts(query="{company_name}", product="Top", count=20)
reddit_search_posts(query="{company_name}", sort="top", count=20)
reddit_search_posts(query="{company_name} review", sort="relevance", count=10)
youtube_search_videos(q="{company_name} review", count=5)
```

### Step 5: Company Culture & Social Presence (Tier 2+)
```
x_get_user(username="{company_handle}")
x_get_user_posts(username="{company_handle}", count=20)
instagram_get_user(username="{company_handle}")
threads_get_user(username="{company_handle}")
WebSearch(query="{company_name} glassdoor culture values")
WebSearch(query="{company_name} company culture remote work")
```

### Step 6: Synthesize & Score (All Tiers)
Combine all data into the output format below. For each section, cite the source (LinkedIn, X post, web article, job posting).

Score buying signals:
- **Green lights**: Recent funding, hiring in your domain, executive posts about your problem space, competitor evaluation signals
- **Red flags**: Layoffs, leadership turnover, acquisition rumors, budget freeze signals

## Output Format

```markdown
# Account Briefing: {Company Name}
**Prepared:** {Date}
**Research Tier:** {1/2/3}
**Prepared by:** Claude (ByCrawl SE Account Research)

---

## Company Snapshot

| Attribute | Value |
|-----------|-------|
| **Company** | {Name} |
| **Website** | {URL} |
| **Industry** | {Primary industry/vertical} |
| **Founded** | {Year} |
| **Headquarters** | {Location} |
| **Employees** | {Count or range} |
| **Revenue** | {If public/estimated} |
| **Funding** | {Total raised, last round} |
| **Business Model** | {B2B/B2C/Marketplace/SaaS/etc.} |

### What They Do
{2-3 sentence description of their core business}

### Target Market
{Who their customers are}

---

## Recent News & Signals

### Last 90 Days
| Date | Event | Source | Relevance |
|------|-------|--------|-----------|
| {Date} | {News item} | {Web/LinkedIn/X} | {Why it matters for your sale} |

### Signal Assessment
| Signal Type | Details | Buying Impact |
|-------------|---------|---------------|
| Green Light | {e.g., Series B closed, hiring 5 engineers} | {Budget available, scaling} |
| Red Flag | {e.g., CFO departed} | {Decision delays possible} |

---

## Technology Stack

### Confirmed/Likely Tools
| Category | Tool | Confidence | Source |
|----------|------|------------|--------|
| {Category} | {Tool} | {High/Medium/Low} | {Job posting/Website/Social} |

### Hiring Signals
| Role | Department | What It Tells Us |
|------|-----------|-----------------|
| {Title} | {Team} | {Interpretation: investing in X, scaling Y} |

### 104 Job Bank Insights (Taiwan Only)
| Metric | Value |
|--------|-------|
| **Open Positions on 104** | {count} |
| **Salary Range** | {range if disclosed} |
| **Top Skills Required** | {skills from 104 listings} |
| **Benefits & Perks** | {notable benefits} |
| **Hiring Velocity** | {interpretation: aggressive growth / steady / winding down} |

### Integration Relevance
- **Connects to your product:** {Relevant integrations}
- **Potential friction:** {Competing or complex systems}

---

## Stakeholder Intelligence

### Leadership Team
| Name | Title | LinkedIn | Background | Likely Priorities |
|------|-------|----------|------------|-------------------|
| {Name} | {Title} | {URL} | {Previous companies, tenure} | {Focus areas} |

### Social Activity (Key Stakeholders)
| Name | Platform | Recent Theme | Notable Post |
|------|----------|-------------|-------------|
| {Name} | X/LinkedIn | {Topic} | "{excerpt}" — {date} |

### Potential Champions
- **{Name}, {Title}**: {Why they'd champion — their stated priorities align with your value prop}

### Potential Blockers
- **{Name}, {Title}**: {Why they might resist — incumbent vendor loyalty, competing priorities}

---

## Pain Hypotheses

### Hypothesis 1: {Pain Statement}
**Evidence:** {Signals from job postings, social posts, news}
**Source:** {Where you found this signal}
**Discovery question:** "{Question to validate}"
**If confirmed:** {How your solution addresses it}

### Hypothesis 2: {Pain Statement}
**Evidence:** {Supporting signals}
**Source:** {Source}
**Discovery question:** "{Question to validate}"
**If confirmed:** {Your value proposition}

### Hypothesis 3: {Pain Statement}
{Same structure}

---

## Competitive Landscape

### Their Competitors
{Who they compete against — market pressures they face}

### Your Competitors (Likely)
| Competitor | Likelihood | Evidence | Your Advantage |
|------------|------------|----------|----------------|
| {Competitor} | {High/Medium} | {Signal from web/social} | {Differentiator} |

### Incumbent Solutions
{What they're likely using today for the problem you solve}

---

## Company Culture & Values

### Cultural Signals
- {From Glassdoor, LinkedIn posts, X presence, press}
- {Work style: remote/hybrid, startup/enterprise culture}

### Communication Style
{Formal/casual, data-driven/relationship-driven, fast/deliberate — based on social presence}

---

## Strategic Account Context

### Why They'd Buy
- {Business driver 1 with evidence}
- {Business driver 2}
- {Timing factor}

### Why They Might Not
- {Blocker 1 with evidence}
- {Blocker 2}

### Ideal Entry Point
{Recommended first contact, message angle, team to target}

### Outreach Draft
"{2-3 sentence personalized message referencing a specific signal}"

---

## Recommended Next Steps
1. {Specific action with named stakeholder}
2. {Research gap to fill}
3. {Content to prepare — case study, demo, POC plan}

---

## Sources
| # | Source | Type | URL/Reference |
|---|--------|------|---------------|
| 1 | {Source} | {LinkedIn/Web/X/Reddit} | {URL if available} |
```

## Output Variations

**For AE (sales-focused):**
Emphasize buying signals, stakeholder priorities, competitive positioning, entry strategy

**For SE (technical):**
Emphasize tech stack, integration points, technical stakeholder backgrounds, pain hypotheses

**For Executive briefing:**
Emphasize strategic fit, deal size potential, relationship angles, reference customers

**For First meeting:**
Emphasize pain hypotheses, discovery questions, stakeholder context

**For Late-stage deal:**
Emphasize decision maker priorities, competitive threats, risk mitigation

## Error Handling

- If a platform returns **empty results**, skip it and note "No data found on {platform}" in the report — do not fail the entire workflow.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- If **WebSearch or WebFetch fails**, note the gap and continue with available bycrawl data — social intelligence alone is still valuable.
- If a **website blocks WebFetch**, try alternative pages (e.g., /about, /team, /blog) or fall back to cached web search snippets.
- **Always deliver partial results** rather than failing entirely — a report covering most sections with gaps noted is still actionable.

## Estimated API Usage

| Tier | ByCrawl Credits | Web Searches | Web Fetches | Total Calls |
|------|----------------|-------------|------------|-------------|
| Tier 1 | 10-20 | 3-5 | 2-3 | ~20-30 |
| Tier 2 | 40-80 | 8-12 | 5-8 | ~60-100 |
| Tier 3 | 100-200+ | 15-25 | 10-15 | ~150-250+ |

Each ByCrawl API call consumes 1 credit. WebSearch and WebFetch are included in Claude's capabilities at no additional credit cost.

**Taiwan 104 add-on**: +5-15 additional credits when 104 Job Bank integration is active (company lookup + job searches + individual job details).
