---
name: bycrawl-deal-outreach-personalize
description: >
  Generate personalized M&A outreach messages for business owners using bycrawl MCP.
  Use when user says "write outreach for [owner]", "personalize outreach",
  "cold email for acquisition", "M&A outreach", "reach out to this owner",
  "write a letter of intent intro", "deal outreach", "personalized acquisition email",
  "customize outreach for [company owner]".
---

# Deal Outreach Personalization

## ICP (Ideal Customer Profile)
- **PE deal teams** sending personalized outreach to business owners
- **Search fund operators** writing warm intros to acquisition targets
- **Corporate development** teams reaching out about potential acquisitions
- **M&A advisors** crafting outreach on behalf of buy-side clients
- **Independent sponsors** differentiating from generic broker emails

## What This Skill Does
Researches a specific business owner across social platforms to find personal interests, recent milestones, communication style, and company context — then generates highly personalized M&A outreach messages that feel like they were written by someone who genuinely knows the owner, not a mass email tool.

## Required Input
- **Owner name** (required): The person to research and write outreach for
- **Company name** (required): Their business
- **Owner LinkedIn or X handle** (optional): Speeds up research
- **Buyer context** (optional): Who the buyer is and why they're interested
- **Outreach channel** (optional): "email", "LinkedIn DM", "letter", or "all"

## Workflow

### Step 1: Owner Research — Who Are They as a Person?
Pull everything available about the owner across platforms:

```
linkedin_get_user(username="{owner_linkedin}")
linkedin_search_users(query="{owner_name} {company}", count=5)
x_get_user(username="{owner_handle}")
x_get_user_posts(username="{owner_handle}", count=30)
instagram_get_user(username="{owner_handle}")
threads_get_user(username="{owner_handle}")
reddit_get_user(username="{owner_handle}")
```

Extract:
- **Professional identity**: Title, years running the business, career arc
- **Personal interests**: Hobbies, causes, sports teams, communities
- **Communication style**: Formal vs casual, short vs detailed, humor vs straight
- **Values**: What they post about, retweet, celebrate
- **Recent milestones**: Awards, anniversaries, personal achievements, company wins

### Step 2: Company Context — What's Their World?
```
linkedin_get_company(companyId="{company}")
linkedin_get_company_jobs(companyId="{company}", count=25)
x_search_posts(query="{company}", product="Latest", count=15)
facebook_search_posts(query="{company}", count=5)
```

Understand:
- Current company trajectory (growing, stable, transitioning)
- Recent company news or announcements
- Industry context and challenges
- What the owner is proud of about the business

### Step 3: Find the Personal Hook
This is what separates great outreach from spam. Look for:

```
x_get_user_posts(username="{owner_handle}", count=30)
```

Scan the owner's recent posts for:
- **Shared interests**: Something the buyer and owner have in common
- **Recent celebration**: Company milestone, personal achievement, industry award
- **Opinion or take**: Something they feel strongly about (agree with it)
- **Community involvement**: Local business groups, industry associations, charity work
- **Content they created**: Blog posts, podcast appearances, speaking events

Priority order for hooks:
1. Something personal they recently posted about (most authentic)
2. A company milestone or achievement (shows you did your homework)
3. A mutual connection or shared experience (warm intro angle)
4. Industry trend they've commented on (professional relevance)

### Step 4: Understand What NOT to Say
From their social presence, identify:
- Topics they're negative about (don't bring these up)
- Competitors they respect (don't trash them)
- Communication pet peeves (if detectable)
- Experiences with prior outreach they've complained about ("stop cold emailing me")

```
x_search_posts(query="from:{owner_handle} cold email OR spam OR stop reaching out OR not interested OR broker OR PE OR private equity", product="Latest", count=10)
```

If they've complained about M&A outreach specifically, note it — the message needs to be extra thoughtful and different.

### Step 5: Draft Personalized Outreach
Generate messages tailored to the detected communication style and channel.

**Key principles:**
- Lead with THEM, not you (reference their work, not your fund)
- Keep it short (busy owners scan, they don't read essays)
- One clear ask (a conversation, not a commitment)
- No jargon (don't say "proprietary deal flow" or "platform acquisition")
- Sound human (match their tone — if they're casual on X, be casual)
- Reference something specific and recent (proves it's not a template)

### Step 6: Create Follow-Up Sequence
Draft 2-3 follow-ups spaced appropriately:
- **Follow-up 1 (5-7 days)**: Add new value or reference something new they posted
- **Follow-up 2 (10-14 days)**: Different angle, shorter, with a soft close
- **Follow-up 3 (30 days)**: "Closing the loop" — final touchpoint, leave the door open

## Output Format

```markdown
## Personalized Outreach Package — {owner_name} at {company} — {date}

### Research Summary

**About {owner_name}:**
- Runs {company} since {year} ({n} years)
- Professional focus: {themes}
- Personal interests: {hobbies, causes, passions}
- Communication style: {formal/casual/mixed} — {evidence}
- Online presence: {platforms and activity level}

**Personalization Hooks Found:**
1. {hook_1} — from {source} on {date} — "{quote or reference}"
2. {hook_2} — from {source}
3. {hook_3} — from {source}

**Things to Avoid:**
- {avoid_1}
- {avoid_2}

---

### Email Outreach

**Subject line options:**
1. "{subject_1}" — references {hook}
2. "{subject_2}" — references {hook}
3. "{subject_3}" — curiosity-based

**Email — Version A (Lead with personal hook):**

> {personalized email body, 4-6 sentences max}

**Email — Version B (Lead with company admiration):**

> {alternative email body}

---

### LinkedIn DM

**Connection request note (300 chars max):**
> {short connection message}

**Follow-up DM (after connection accepted):**
> {DM body, 3-4 sentences}

---

### Follow-Up Sequence

**Follow-up #1 (Day 5-7):**
> Subject: {subject}
> {body — add new value, reference something new}

**Follow-up #2 (Day 12-14):**
> Subject: {subject}
> {body — different angle, shorter}

**Follow-up #3 (Day 30):**
> Subject: {subject}
> {body — closing the loop, leave door open}

---

### Outreach Strategy Notes
- **Best channel**: {email/LinkedIn/phone} — because {reasoning}
- **Best timing**: {day/time based on their posting patterns}
- **Warm intro opportunity**: {mutual connections or shared communities if detected}
- **Tone calibration**: {specific guidance based on their style}
- **If they've complained about cold outreach**: {adjusted approach}
```
