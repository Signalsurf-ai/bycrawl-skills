---
name: bycrawl-acquisition-target-qualify
description: >
  Deep qualification of a specific company as an acquisition target using bycrawl MCP.
  Use when user says "qualify this company", "is this a good acquisition target",
  "due diligence on [company]", "evaluate [company] for acquisition",
  "assess [company] as a target", "deep dive on [company] for M&A",
  "should we pursue [company]", "acquisition qualification".
allowed-tools: ["mcp_bycrawl_*"]
user-invocable: true
argument-hint: "<company_name> [owner_name] e.g. Smith HVAC, John Smith"
---

# Acquisition Target Qualification

## ICP (Ideal Customer Profile)
- **PE associates** doing deep dives on sourced companies before outreach
- **Search fund operators** evaluating whether to pursue a specific target
- **Corp dev analysts** building investment memos on potential acquisitions
- **M&A advisors** qualifying targets before presenting to clients
- **Independent sponsors** deciding where to spend limited outreach bandwidth

## What This Skill Does
Takes a specific company and runs a comprehensive qualification by pulling company data, owner profiles, growth signals, hiring trends, market sentiment, competitive positioning, and seller readiness indicators across LinkedIn, X, Reddit, and Facebook. Produces an acquisition-focused qualification report.

## Required Input
- **Company name** (required): The company to evaluate
- **Owner/founder name** (optional): If known, speeds up research
- **Acquisition thesis** (optional): What makes this company interesting to you
- **Deal criteria** (optional): Size, industry, geography, or financial thresholds


## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Workflow

### Step 1: Company Profile Pull
Get the foundational company data:

```
linkedin_get_company(companyId="{company}")
```

Extract:
- Employee count (size proxy)
- Founded date (maturity)
- Headquarters (geography)
- Industry classification
- Company description and specialties
- Website URL

### Step 2: Team & Org Structure Analysis
Understand the company's team composition:

```
linkedin_get_company_jobs(companyId="{company}", count=50)
linkedin_search_users(query="{company} manager director VP", count=30)
```

Analyze:
- **Management depth**: Are there managers/directors beyond the owner? (key-person risk if not)
- **Department structure**: Sales, ops, engineering, finance — how mature is the org?
- **Open roles**: What they're hiring for reveals current priorities and growth stage
- **Role churn**: Same roles posted repeatedly = retention problem

### Step 3: Owner Deep Dive
Find and research the owner/founder comprehensively:

```
linkedin_get_user(username="{owner_linkedin}")
linkedin_search_users(query="{owner_name} {company}", count=5)
x_get_user(username="{owner_handle}")
x_get_user_posts(username="{owner_handle}", count=30)
facebook_search_posts(query="{owner_name} {company}", count=5)
```

Build owner profile:
- **Career history**: Did they found the company or acquire it? How long have they run it?
- **Age/stage signals**: Years of experience, career arc, life stage indicators
- **Other ventures**: Do they have other businesses? (serial entrepreneur vs single-focus)
- **Public persona**: How active are they online? What do they talk about?
- **Network**: Who do they engage with? Industry peers, advisors, investors?

### Step 4: Seller Readiness Assessment
This is the critical differentiator. Search for signals the owner may be open to a transaction:

```
x_search_posts(query="from:{owner_handle} retire OR transition OR next chapter OR stepping back OR succession OR exit OR sell OR burnout OR tired OR break", product="Latest", count=20)
x_search_posts(query="from:{owner_handle} anniversary OR milestone OR years OR built OR journey OR grateful", product="Latest", count=20)
reddit_search_posts(query="{owner_name} OR {company} exit OR selling OR acquisition OR retire", sort="new", count=20)
```

**Strong sell signals:**
- Explicitly mentions retirement, transition, or "next chapter"
- Recently hired a GM/COO/President (delegating operations)
- Posting about burnout, fatigue, or wanting a break
- Celebrating big milestones ("25 years!") — reflection often precedes transition
- Reduced posting/engagement frequency over time
- Engaging with content about exits, wealth management, or post-exit life

**Moderate signals:**
- Hiring senior leadership for first time
- Talking about the business in past tense or reflective tone
- Engaging with PE/M&A content or advisors
- Life changes (kids leaving for college, health mentions)

**Counter-signals (less likely to sell):**
- Actively launching new products or services
- Aggressive hiring across departments
- Excited/energized tone about future plans
- Recently raised capital or took on debt

### Step 5: Market Position & Reputation
Understand how the market perceives this company:

```
x_search_posts(query="{company}", product="Top", count=20)
x_search_posts(query="{company}", product="Latest", count=20)
reddit_search_posts(query="{company}", sort="top", count=20)
reddit_search_posts(query="{company} review OR experience OR recommend", sort="top", count=15)
facebook_search_posts(query="{company}", count=5)
```

Assess:
- **Customer sentiment**: Are customers happy? Complaints?
- **Brand strength**: Is the company well-known in its niche?
- **Competitive mentions**: Who do customers compare them to?
- **Market position**: Leader, challenger, or niche player?

### Step 6: Competitive Landscape
Identify competitors and relative positioning:

```
x_search_posts(query="{company} vs OR versus OR alternative OR competitor OR compared to", product="Top", count=15)
reddit_search_posts(query="{company} vs OR alternative", sort="top", count=15)
linkedin_search_jobs(query="{company_industry} {geography}", count=30)
```

Map:
- Direct competitors and their relative size
- Market concentration (fragmented = roll-up opportunity)
- Competitive advantages mentioned by customers

### Step 7: Risk Scan
Check for red flags:

```
x_search_posts(query="{company} problem OR issue OR scam OR lawsuit OR complaint", product="Latest", count=20)
reddit_search_posts(query="{company} problem OR issue OR bad OR avoid", sort="new", count=20)
```

Flag:
- Legal issues or lawsuits mentioned
- Customer complaint patterns
- Employee complaints (Glassdoor mentions)
- Regulatory concerns
- Technology/obsolescence risks

## Output Format

```markdown
## Acquisition Qualification Report — {company} — {date}

### Verdict: {STRONG TARGET / WORTH PURSUING / NEEDS MORE INFO / PASS}

### Company Snapshot
| Field | Details |
|-------|---------|
| Company | {name} |
| Industry | {industry} |
| Founded | {year} ({n} years) |
| Employees | {count} |
| HQ | {location} |
| Website | {url} |
| LinkedIn | {url} |
| Description | {what they do} |

### Owner Profile
| Field | Details |
|-------|---------|
| Name | {name} |
| Title | {role} |
| Tenure | {years} at company |
| LinkedIn | {url} |
| X/Twitter | @{handle} |
| Career background | {summary} |
| Other ventures | {if any} |
| Online presence | {active/moderate/minimal} |

### Size & Growth Assessment
- **Employee count**: {n} (fits target range: {yes/no})
- **Hiring activity**: {n} open roles — {interpretation}
- **Growth trajectory**: {growing/stable/declining} based on {evidence}
- **Revenue proxies**: {team size, job levels, office locations, hiring velocity}
- **Management depth**: {owner-dependent / has management layer}
- **Key-person risk**: {high/medium/low} — {reasoning}

### Seller Readiness Score: {HIGH / MODERATE / LOW / UNKNOWN}

**Signals detected:**
- {signal_1} — {evidence and source link}
- {signal_2} — {evidence}

**Counter-signals:**
- {counter_signal} — {evidence}

**Assessment**: {narrative interpretation of likelihood to engage}

### Market Position
- **Brand strength**: {strong/moderate/weak} in {niche}
- **Customer sentiment**: {positive/mixed/negative} — {summary}
- **Competitive position**: {leader/challenger/niche player}
- **Key competitors**: {list with relative size}
- **Differentiation**: {what customers say sets them apart}

### Strategic Value
- **Platform potential**: {could this be a platform for add-ons?}
- **Roll-up opportunity**: {fragmented market?}
- **Recurring revenue signals**: {subscription, contracts, retainers}
- **Customer concentration risk**: {any signals of over-reliance}

### Risk Factors
| Risk | Severity | Evidence |
|------|----------|----------|
| {risk_1} | {High/Med/Low} | {source} |
| {risk_2} | {severity} | {source} |

### Recommended Next Steps
1. **If pursuing**: {specific outreach approach based on owner profile}
2. **Outreach channel**: {LinkedIn DM / email / warm intro / phone}
3. **Talking points**: {based on owner's interests and company milestones}
4. **Timing**: {good/neutral/bad based on signals}
5. **Information gaps**: {what you still need to learn}
```

## Error Handling

- If a platform returns **empty results**, skip it and note "No data found on {platform}" in the report — do not fail the entire workflow.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- **Always deliver partial results** rather than failing entirely — a report covering 5 of 7 platforms is still valuable.

## Estimated API Usage

- **Basic run**: ~25-30 API calls
- **Full run**: ~45-55 full qualification with risk scan API calls
- Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive usage.
