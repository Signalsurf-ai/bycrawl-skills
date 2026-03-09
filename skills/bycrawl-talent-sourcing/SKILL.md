---
name: bycrawl-talent-sourcing
description: >
  Recruiting intelligence and talent sourcing across LinkedIn, X, Reddit, and GitHub using bycrawl MCP.
  Use when user says "find candidates", "talent sourcing", "recruiting research",
  "who is hiring for [role]", "find engineers", "find developers",
  "employer brand check", "hiring intelligence", "talent market".
---

# Talent Sourcing

## ICP (Ideal Customer Profile)
- **Recruiters and sourcers** building candidate pipelines
- **Hiring managers** researching the talent landscape for a role
- **Founders** doing early-stage recruiting without a recruiter
- **HR teams** monitoring employer brand perception

## What This Skill Does
Finds potential candidates by searching across LinkedIn, X, and Reddit, then enriches profiles with cross-platform activity to assess fit, expertise, and cultural alignment. Also monitors employer brand sentiment.

## Required Input

**Mode A — Find Candidates:**
- **Role or skill** (required): e.g., "senior backend engineer", "growth marketer"
- **Location** (optional): Geographic preference
- **Seniority** (optional): Junior, mid, senior, lead

**Mode B — Employer Brand Check:**
- **Company name** (required): Your company or a competitor

## Workflow — Mode A: Find Candidates

### Step 1: LinkedIn Candidate Search
```
linkedin_search_users(query="{role} {skills}", count=30)
linkedin_search_jobs(query="{role}", location="{location}", count=20)
```

Use job posting analysis to understand:
- What competing companies pay for this role
- What skills are most demanded
- Seniority distribution

### Step 2: Community Sourcing
Search for active experts in relevant communities:
```
reddit_search_posts(query="{skill} tutorial OR guide OR built OR launched", sort="top", count=20)
x_search_posts(query="{skill} building OR shipped OR open source", product="Top", count=20)
```

People who share knowledge publicly = strong candidates.

### Step 3: Profile Enrichment
For promising candidates, cross-reference:
```
linkedin_get_user(username="{candidate}")
x_get_user(username="{candidate}")
reddit_get_user(username="{candidate}")
```

### Step 4: Activity & Expertise Assessment
```
x_get_user_posts(username="{candidate}", count=20)
```

Evaluate:
- Technical depth from their posts
- Communication skills
- Cultural signals
- Open source contributions or side projects

## Workflow — Mode B: Employer Brand Check

### Step 1: Search for Employee Sentiment
```
reddit_search_posts(query="working at {company} OR {company} culture OR {company} interview", sort="new", count=30)
x_search_posts(query="{company} hiring OR {company} work OR {company} culture", product="Latest", count=20)
threads_search_posts(query="{company} work", search_type="recent", count=15)
```

### Step 2: Company Profile
```
linkedin_get_company(companyId="{company}")
linkedin_get_company_jobs(companyId="{company}", count=30)
```

Analyze: role churn (same roles posted repeatedly = retention issues).

## Output Format — Mode A

```markdown
## Talent Sourcing Report — {role} — {date}

### Market Overview
- LinkedIn profiles matching: {n}
- Competing job postings: {n}
- Salary signals: {range}
- Most demanded skills: {list}

### Candidate Pipeline

| # | Name | Current Role | Company | LinkedIn | X/Reddit | Fit Score |
|---|------|-------------|---------|----------|----------|-----------|
| 1 | {name} | {role} | {co} | {url} | @{handle} | {High/Med} |

### Candidate Highlights
#### 1. {name}
- **Current**: {role} at {company}
- **Expertise signals**: {from posts/activity}
- **Cultural fit signals**: {from social presence}
- **Outreach angle**: {personalized approach}

### Sourcing Channels
- Best subreddits: {list}
- Best X hashtags: {list}
- Best communities: {list}
```

## Output Format — Mode B

```markdown
## Employer Brand Report — {company} — {date}

### Sentiment: {POSITIVE / MIXED / NEGATIVE}

### What Employees/Candidates Say
- Positive themes: {list}
- Negative themes: {list}
- Interview experience: {summary}

### Red Flags
- {repeated_job_postings = retention issues}
- {negative_reddit_threads}

### Recommendations
1. {action}
2. {action}
```
