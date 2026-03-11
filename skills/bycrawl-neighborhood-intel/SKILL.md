---
name: bycrawl-neighborhood-intel
description: >
  Hyper-local neighborhood intelligence, rental/lease sourcing, and real estate social overlay using ByCrawl MCP.
  Use when user says "neighborhood research", "what's the vibe of [area]", "neighborhood sentiment",
  "find rentals in [area]", "available leases in [neighborhood]", "apartments for rent in [area]",
  "is [area] a good place to live", "what do residents say about [area]", "local hidden gems",
  "community voice for [neighborhood]", "living experience in [area]", "commute reality check",
  "resident reviews for [district]", "life circle analysis", "neighborhood safety sentiment",
  "trending neighborhoods", "up-and-coming areas", "real estate social signals",
  "what's it like living in [area]", "neighborhood report for [location]",
  "rental listings from social", "off-platform rentals", "find places to rent".
  Built for real estate platforms, property tech, and location intelligence partners.
allowed-tools: ["mcp_bycrawl_*"]
user-invocable: true
argument-hint: "<neighborhood_or_address> [city] e.g. Da'an District, Taipei"
---

# Neighborhood Intelligence — Social Overlay for Real Estate

## ICP (Ideal Customer Profile)
- **Real estate platforms** (like Alleyguide) sourcing off-platform rental listings and enriching with resident sentiment
- **PropTech companies** building "Life Circle" or neighborhood score features with lease inventory
- **Property managers** tracking available units and market pricing across social channels
- **Relocation services** helping clients find rentals AND understand the neighborhood
- **Real estate agents** sourcing lease leads from social and preparing neighborhood briefs for renters
- **Renters** looking for off-market or social-only rental listings traditional platforms miss

## What This Skill Does
Two things in one report:

1. **Sources available rental/lease properties** from Facebook Marketplace, Facebook Groups, Dcard rental forums, and social posts — surfacing off-platform listings that traditional aggregators miss.

2. **Layers neighborhood intelligence** on top — resident sentiment, hidden gems, commute reality, safety, and trajectory — so every listing comes with context about what it's actually like to live there.

Answers two questions: **"What's available for rent here?"** and **"What's it really like to live here?"**

## Required Input
- **Neighborhood or address** (required): e.g. "Da'an District", "Xinyi Road Section 4", "Fubon Grand Villa"
- **City** (required): e.g. "Taipei", "Taichung", "Kaohsiung"
- **Property type** (optional): "apartment", "studio", "suite", "whole floor", "office space"
- **Budget range** (optional): e.g. "15000-25000/month", "under 20000"
- **Radius / scope** (optional): "this street", "this district", "within 10-minute walk"
- **Renter persona** (optional): e.g. "young couple", "family with kids", "pet owner", "remote worker", "student"

## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Workflow

### IMPORTANT: Collecting Source Links

For EVERY listing and post found, you MUST extract and preserve the source URL. Use these URL patterns based on IDs returned by the ByCrawl API:

| Platform | URL Pattern |
|----------|------------|
| FB Marketplace | `https://www.facebook.com/marketplace/item/{listing_id}` |
| FB Posts | `https://www.facebook.com/{postId}` |
| Dcard | `https://www.dcard.tw/f/{forum_alias}/p/{postId}` |
| Threads | `https://www.threads.net/post/{postId}` |
| X / Twitter | `https://x.com/{username}/status/{tweetId}` |
| Instagram | `https://www.instagram.com/p/{shortcode}` |

Collect ALL links as you go — they will be included in individual tables AND compiled into the "All Listing Links" appendix (Section X).

---

### PART A: PROPERTY SOURCING — Available Rentals & Leases

### Step 1: Facebook Marketplace Rental Listings
Facebook Marketplace is a major source of direct-from-landlord rentals in Taiwan, often listed before (or instead of) 591/Alleyguide. Run in parallel:
```
facebook_marketplace_search(query="租屋 {neighborhood}", location="{city_slug}")
facebook_marketplace_search(query="出租 {neighborhood}", location="{city_slug}")
facebook_marketplace_search(query="套房 {neighborhood}", location="{city_slug}")
facebook_marketplace_search(query="公寓 {neighborhood}", location="{city_slug}")
facebook_marketplace_browse(location="{city_slug}", category="propertyrentals")
```

For each promising listing, pull full details:
```
facebook_marketplace_item(listing_id="{id}")
```

Extract per listing:
- **Source URL / Link**: Construct the direct link — `https://www.facebook.com/marketplace/item/{listing_id}`
- Price (monthly rent)
- Property type (studio/suite/apartment/whole floor)
- Size (ping or sqm if listed)
- Key features mentioned (furnished, balcony, parking, pet-allowed)
- Photos available (count)
- Posting date (freshness)
- Landlord type (owner-direct vs. agent — check profile)

### Step 2: Facebook Group Rental Posts
Local housing groups are where landlords and agents post available units, especially for neighborhoods without strong 591 coverage:
```
facebook_search_posts(query="{neighborhood} 出租 OR 租屋 OR 空房 OR 招租", count=30)
facebook_search_posts(query="{neighborhood} 套房 OR 雅房 OR 整層 OR 分租", count=25)
facebook_search_posts(query="{neighborhood} 月租 OR 房租 OR 含水電", count=20)
```

For listings with engagement (comments = demand signal), pull details:
```
facebook_get_post(postId="{id}")
facebook_get_post_comments(postId="{id}", count=20)
```

Extract:
- **Source URL / Link**: Construct the direct link — `https://www.facebook.com/{postId}` (use the post ID returned by the API)
- Listed price and what's included (utilities, internet, management fee)
- Move-in conditions (deposit, minimum lease term)
- Demand signals from comments ("still available?" count = how competitive this listing is)
- Landlord responsiveness (do they reply in comments?)

### Step 3: Dcard Rental Forum & Posts
Dcard has dedicated rental/housing forums and many "subletting" or "lease transfer" posts:
```
dcard_get_forum(alias="house")
dcard_get_forum_posts(alias="house", limit=30, popular=true)
dcard_search_posts(query="{neighborhood} 租 OR 轉租 OR 退租 OR 空房", count=30)
dcard_search_posts(query="{neighborhood} 找室友 OR 分租 OR 合租", count=20)
dcard_search_posts(query="{neighborhood} 租屋推薦 OR 租屋心得", count=20)
```

Extract:
- **Source URL / Link**: Construct the direct link — `https://www.dcard.tw/f/{forum}/p/{postId}` (use the post ID and forum alias returned by the API)
- Sublease / lease transfer opportunities (often below market rate)
- Roommate-wanted posts (shared apartment opportunities)
- Rental experience reviews (which buildings/landlords to target or avoid)
- Price benchmarks from what people report paying

### Step 4: Threads & X Rental Buzz
People occasionally post available units or ask for leads on Threads and X:
```
threads_search_posts(query="{neighborhood} 出租 OR 租屋 OR 空房", count=20)
threads_search_posts(query="{neighborhood} 找房 OR 搬家 OR 推薦租屋", count=15)
x_search_posts(query="{neighborhood} 出租 OR 租屋 OR 退租", product="Latest", count=20)
```

Extract:
- **Source URL / Link**: For Threads — `https://www.threads.net/post/{postId}`; For X — `https://x.com/{username}/status/{tweetId}` (use IDs returned by the API)
- Fresh listings not yet on aggregators
- "Looking for" posts (indicates demand — what renters want but can't find)
- Agent/landlord accounts that frequently post in this area (lead sources)

---

### PART B: NEIGHBORHOOD INTELLIGENCE — Living Experience Overlay

### Step 5: Resident Sentiment Scan (Dcard — Highest Signal for Taiwan)
Dcard is where Taiwanese residents share unfiltered living experiences. Run in parallel:
```
dcard_search_posts(query="{neighborhood} 住 OR 租 OR 搬 OR 生活", count=30)
dcard_search_posts(query="{neighborhood} 推薦 OR 心得 OR 評價", count=30)
dcard_search_posts(query="{neighborhood} 缺點 OR 吵 OR 問題 OR 後悔", count=20)
dcard_search_posts(query="{residential_complex_name} 住戶 OR 管理 OR 社區", count=20)
```

For the top 5 posts by engagement, pull full comments:
```
dcard_get_post(postId="{id}")
dcard_get_post_comments(postId="{id}", count=50)
```

Extract:
- Overall resident satisfaction (positive vs negative ratio)
- Recurring praise themes (convenient, quiet, friendly neighbors, good management)
- Recurring complaint themes (noise, parking, management issues, pests)
- Specific residential complex mentions and their reputation

### Step 6: Facebook Community Groups (Local Resident Voice)
Facebook Groups are where Taiwan neighborhood communities organize and vent:
```
facebook_search_posts(query="{neighborhood} 社區 OR 鄰居 OR 住戶", count=25)
facebook_search_posts(query="{neighborhood} 推薦 OR 美食 OR 好店", count=25)
facebook_search_posts(query="{neighborhood} 噪音 OR 安全 OR 治安 OR 施工", count=20)
facebook_search_posts(query="{neighborhood} 停車 OR 交通 OR 通勤", count=20)
```

For posts with high engagement, pull comments for deeper sentiment:
```
facebook_get_post(postId="{id}")
facebook_get_post_comments(postId="{id}", count=30)
```

Extract:
- Community event activity (active community = higher quality of life signal)
- Local business recommendations from actual residents
- Infrastructure complaints (roads, utilities, construction)
- Safety incidents or concerns mentioned

### Step 7: Instagram Hidden Gems & Lifestyle Mapping
Instagram reveals the "lifestyle layer" — what's actually popular and photogenic nearby:
```
instagram_search_tags(query="{neighborhood}")
instagram_search_tags(query="{neighborhood}美食")
instagram_search_tags(query="{neighborhood}咖啡")
instagram_search_tags(query="{neighborhood}生活")
```

For trending tags, pull recent posts:
```
instagram_get_user_posts(username="{local_popular_account}", count=20)
```

Extract:
- Trending cafes, restaurants, and "hidden gems" with real check-in popularity
- Visual neighborhood aesthetic (tree-lined streets, vibrant night markets, quiet alleys)
- Lifestyle activities available (yoga studios, parks, art spaces, night life)
- Foot traffic proxies from post volume and tag counts

### Step 8: Threads Real-Time Buzz & Living Tips
Threads captures spontaneous, real-time neighborhood chatter:
```
threads_search_posts(query="{neighborhood} 住 OR 生活 OR 搬家", count=25)
threads_search_posts(query="{neighborhood} 推 OR 愛 OR 最棒", count=20)
threads_search_posts(query="{neighborhood} 雷 OR 避 OR 千萬別", count=20)
```

Extract:
- Real-time buzz about new openings, closures, or changes
- Spontaneous recommendations ("just moved to {area}, loving the...")
- Warnings or tips from current residents
- Pet-friendliness signals (pet cafes, dog parks, vet mentions)

### Step 9: Commute Reality Check
Social data captures what commute calculators can't — the lived experience of daily commutes:
```
dcard_search_posts(query="{neighborhood} 通勤 OR 捷運 OR 公車 OR 塞車", count=25)
dcard_search_posts(query="{nearby_MRT_station} 擠 OR 等 OR 誤點 OR 轉乘", count=20)
threads_search_posts(query="{nearby_MRT_station} 通勤 OR 尖峰", count=15)
x_search_posts(query="{nearby_MRT_station} 延誤 OR delay OR 人多", product="Top", count=15)
```

Extract:
- Specific bus routes or MRT lines with frequent complaints
- Peak hour crowding reality (vs. theoretical transit time)
- Parking availability and cost sentiment
- Bike-friendliness and walkability from resident perspective
- Common commute workarounds locals use

### Step 10: Safety & Quality of Life Signals
Capture the nuances that official crime stats miss:
```
dcard_search_posts(query="{neighborhood} 安全 OR 治安 OR 可怕 OR 小心", count=20)
facebook_search_posts(query="{neighborhood} 治安 OR 偷 OR 監視器 OR 巡邏", count=20)
dcard_search_posts(query="{neighborhood} 環境 OR 乾淨 OR 髒 OR 吵 OR 臭", count=20)
threads_search_posts(query="{neighborhood} 安全 OR 治安", count=15)
```

Extract:
- Safety perception (do residents feel safe walking at night?)
- Cleanliness and environmental quality mentions
- Noise pollution sources (construction, nightlife, traffic, temples)
- Street lighting and infrastructure complaints
- Community watch or management security mentions

### Step 11: Neighborhood Trajectory — Up-and-Coming or Declining?
Detect whether a neighborhood is on the rise or losing appeal:
```
dcard_search_posts(query="{neighborhood} 變 OR 改變 OR 發展 OR 都更", count=20)
threads_search_posts(query="{neighborhood} 越來越 OR 最近 OR 新開", count=15)
facebook_search_posts(query="{neighborhood} 新開 OR 開幕 OR 即將", count=15)
x_search_posts(query="{neighborhood} 漲 OR 投資 OR 潛力", product="Top", count=15)
```

Extract:
- New business openings (rising signal) vs. closures (declining signal)
- Urban renewal or redevelopment mentions
- Price trend sentiment ("getting expensive" = gentrifying; "still affordable" = opportunity)
- Demographic shift signals (young professionals moving in, families moving out, etc.)
- Infrastructure investment (new MRT stations, parks, road improvements)

---

## Analysis Framework

After data collection, synthesize across platforms using these lenses:

### Rental Market Analysis
From the sourced listings (Steps 1-4), build a market picture:

**Supply Classification**:
- **Direct-from-owner**: Landlord posts directly (best value, negotiable)
- **Agent-listed**: Agency posts (more professional, less negotiable)
- **Sublease / Transfer**: Current tenants leaving (often below market, immediate availability)
- **Roommate-wanted**: Shared apartments (budget option, social living)

**Pricing Intelligence**:
- Median rent by property type (studio vs. 1BR vs. 2BR vs. whole floor)
- Price range (floor to ceiling) for the neighborhood
- Price-per-ping comparison across listings
- What's included vs. extra (utilities, internet, parking, management fee)
- Deposit norms (1 month, 2 months, negotiable signals)

**Demand Signals**:
- Comment count on listings = demand intensity ("still available?" frequency)
- How fast listings disappear = market velocity
- "Looking for" posts volume vs. "available" posts volume = supply/demand ratio
- Seasonal patterns (student move-in cycles, Lunar New Year turnover)

**Listing Quality Flags**:
- Photos included vs. text-only (quality signal)
- Detailed description vs. minimal (trustworthiness signal)
- Landlord response rate in comments (reliability signal)
- Red flags: no photos, vague location, too-good-to-be-true pricing

### Resident Sentiment Score
Aggregate positive vs. negative mentions into an overall score:
- **LOVED** (>75% positive): Residents actively recommend the area
- **LIKED** (50-75% positive): Generally positive with known trade-offs
- **MIXED** (30-50% positive): Significant pros and cons — depends on buyer priorities
- **CAUTION** (<30% positive): Recurring complaints dominate the conversation

### Life Circle Quality Dimensions
Score each dimension on a 5-point scale based on social evidence:

| Dimension | What to measure |
|-----------|----------------|
| **Food & Dining** | Density and quality of restaurant/cafe mentions, hidden gem count |
| **Daily Convenience** | Grocery, pharmacy, laundry mentions and satisfaction |
| **Green Space & Recreation** | Park visits, outdoor activity posts, walkability praise |
| **Nightlife & Entertainment** | Bar, cinema, event mentions — positive or noise complaints? |
| **Family Friendliness** | School mentions, playground posts, kid-friendly venue tags |
| **Pet Friendliness** | Pet cafe mentions, vet proximity, dog park posts, pet policy complaints |
| **Safety & Quiet** | Safety praise vs. crime/noise complaints ratio |
| **Transit & Commute** | Commute satisfaction vs. complaints ratio |
| **Community & Neighbors** | Community event activity, neighbor interaction sentiment |
| **Value for Money** | Price satisfaction signals, "worth it" vs. "overpriced" ratio |

### Neighborhood Trajectory
Classify based on social signal trajectory:
- **RISING** — New businesses opening, positive buzz increasing, demographic influx, "up-and-coming" mentions
- **ESTABLISHED** — Stable sentiment, mature amenity ecosystem, consistent community activity
- **TRANSITIONING** — Mixed signals, some displacement, gentrification debate, rapid change
- **DECLINING** — Business closures, increasing complaints, resident exodus mentions

### Hidden Gem Index
From Instagram + Threads + Facebook, rank local spots by:
- Social mention volume (popularity proxy)
- Sentiment quality (loved vs. just known)
- Recency (trending now vs. legacy reputation)
- Uniqueness (only-here vs. chain/franchise)

---

## Output Format

```markdown
# Neighborhood Intelligence Report
## {Neighborhood}, {City} | {Date}
### Prepared for: [Client/Platform Name]

---

## Executive Summary

{3-4 sentences: Overall neighborhood character, standout strength, biggest concern, and trajectory. Written for a property buyer making a decision.}

**Rental Market**: {n} active listings found across social platforms | Median rent: ${n}/month
**Resident Verdict**: {LOVED / LIKED / MIXED / CAUTION} — based on {n} social data points across {n} platforms
**Neighborhood Trajectory**: {RISING / ESTABLISHED / TRANSITIONING / DECLINING}

---

## I. Available Rentals & Lease Listings

### Market Snapshot

| Metric | Value |
|--------|-------|
| Total listings found | {n} across {n} platforms |
| Median rent | NT${n}/month |
| Price range | NT${low} — NT${high}/month |
| Most common type | {studio/1BR/2BR/suite} |
| Supply vs. demand | {oversupplied / balanced / competitive / scarce} |
| Freshest listing | {date} on {platform} |

### Listings by Source

| Source | Listings Found | Price Range | Avg Quality |
|--------|---------------|-------------|-------------|
| FB Marketplace | {n} | NT${low}-${high} | {high/medium/low — based on photo/description quality} |
| FB Groups | {n} | NT${low}-${high} | {quality} |
| Dcard | {n} (includes subleases) | NT${low}-${high} | {quality} |
| Threads / X | {n} | NT${low}-${high} | {quality} |

### Top Listings — Best Value

| # | Source | Type | Size | Rent/mo | Includes | Highlights | Demand Signal | Link |
|---|--------|------|------|---------|----------|------------|--------------|------|
| 1 | {platform} | {type} | {ping/sqm} | NT${n} | {utilities/internet/parking} | {key features} | {n} comments asking "available?" | [View]({url}) |
| 2 | {platform} | {type} | {size} | NT${n} | {includes} | {highlights} | {demand} | [View]({url}) |
| 3 | {platform} | {type} | {size} | NT${n} | {includes} | {highlights} | {demand} | [View]({url}) |
| 4 | {platform} | {type} | {size} | NT${n} | {includes} | {highlights} | {demand} | [View]({url}) |
| 5 | {platform} | {type} | {size} | NT${n} | {includes} | {highlights} | {demand} | [View]({url}) |

### Sublease & Transfer Opportunities
Subleases often come at below-market rates with immediate availability:

| # | Source | Original Rent | Transfer Terms | Why Leaving | Available From | Link |
|---|--------|--------------|---------------|-------------|----------------|------|
| 1 | Dcard | NT${n}/mo | {deposit transfer, remaining lease term} | "{reason}" | {date} | [View]({url}) |
| 2 | {source} | NT${n}/mo | {terms} | "{reason}" | {date} | [View]({url}) |

### Roommate-Wanted / Shared Living

| # | Source | Location | Your Share | Room Type | Existing Tenants | Vibe | Link |
|---|--------|----------|-----------|-----------|-----------------|------|------|
| 1 | {platform} | {area} | NT${n}/mo | {private room/shared} | {description} | "{quote about household}" | [View]({url}) |

### Pricing Intelligence

| Property Type | Median Rent | Range | What's Typically Included |
|--------------|------------|-------|--------------------------|
| Studio / 套房 | NT${n} | NT${low}-${high} | {common inclusions} |
| 1BR / 一房一廳 | NT${n} | NT${low}-${high} | {inclusions} |
| 2BR / 兩房 | NT${n} | NT${low}-${high} | {inclusions} |
| Shared room / 雅房 | NT${n} | NT${low}-${high} | {inclusions} |
| Whole floor / 整層 | NT${n} | NT${low}-${high} | {inclusions} |

**Deposit norms**: {typical deposit in this area — 1 month, 2 months, negotiable?}
**Lease terms**: {typical minimum — 1 year, 6 months, flexible?}
**Move-in costs**: {first month + deposit + agent fee if applicable}

### Landlord & Agent Intel (from social activity)

| Landlord/Agent | Platform | Listings Count | Response Rate | Tenant Reviews |
|---------------|----------|---------------|---------------|----------------|
| {name/handle} | {FB/Dcard} | {n} active | {fast/slow/no reply} | "{quote from tenant}" |

### Demand Heatmap — What Renters Want But Can't Find
From "looking for" posts across platforms:

| Unmet Demand | Frequency | Typical Budget | Source |
|-------------|-----------|---------------|--------|
| {e.g. "pet-friendly studio under 15K"} | {n} posts | NT${range} | Dcard, FB |
| {e.g. "furnished 1BR near MRT"} | {n} posts | NT${range} | {source} |
| {e.g. "short-term lease 3-6 months"} | {n} posts | NT${range} | {source} |

---

## II. Resident Sentiment Overview

### Overall Sentiment Breakdown

| Sentiment | Percentage | Volume | Top Platform |
|-----------|-----------|--------|--------------|
| Positive | {n}% | {n} mentions | {platform} |
| Neutral | {n}% | {n} mentions | {platform} |
| Negative | {n}% | {n} mentions | {platform} |

### What Residents Love
1. **{theme_1}** — mentioned {n} times — "{example quote}"
2. **{theme_2}** — mentioned {n} times — "{quote}"
3. **{theme_3}** — mentioned {n} times — "{quote}"

### What Residents Complain About
1. **{complaint_1}** — mentioned {n} times — "{example quote}"
2. **{complaint_2}** — mentioned {n} times — "{quote}"
3. **{complaint_3}** — mentioned {n} times — "{quote}"

### Specific Residential Complex Reputation (if data available)

| Complex / Building | Sentiment | Key Praise | Key Complaints | Source |
|-------------------|-----------|-----------|----------------|--------|
| {complex_1} | {positive/mixed/negative} | {praise} | {complaints} | Dcard, FB |
| {complex_2} | {sentiment} | {praise} | {complaints} | {source} |

---

## III. Life Circle Quality Score

| Dimension | Score (1-5) | Evidence | Buyer Persona Fit |
|-----------|------------|---------|-------------------|
| Food & Dining | {n}/5 | {key evidence} | {who benefits most} |
| Daily Convenience | {n}/5 | {evidence} | {persona} |
| Green Space & Recreation | {n}/5 | {evidence} | {persona} |
| Nightlife & Entertainment | {n}/5 | {evidence} | {persona} |
| Family Friendliness | {n}/5 | {evidence} | {persona} |
| Pet Friendliness | {n}/5 | {evidence} | {persona} |
| Safety & Quiet | {n}/5 | {evidence} | {persona} |
| Transit & Commute | {n}/5 | {evidence} | {persona} |
| Community & Neighbors | {n}/5 | {evidence} | {persona} |
| Value for Money | {n}/5 | {evidence} | {persona} |

**Overall Life Circle Score**: {n}/50

**Best fit for**: {buyer persona — e.g. "Young professionals who value food scene and nightlife over quiet"}
**Not ideal for**: {persona — e.g. "Light sleepers or families with young children due to nightlife noise"}

---

## IV. Hidden Gems — What Locals Actually Love

### Top-Rated by Residents (not Google Maps tourists)

| # | Place | Type | Why Locals Love It | Social Proof | Trending? |
|---|-------|------|-------------------|-------------|-----------|
| 1 | {name} | {cafe/restaurant/park/etc.} | "{resident quote}" | IG: {n} tags, Threads: {n} mentions | {yes/no} |
| 2 | {name} | {type} | "{quote}" | {evidence} | {trending} |
| 3 | {name} | {type} | "{quote}" | {evidence} | {trending} |
| 4 | {name} | {type} | "{quote}" | {evidence} | {trending} |
| 5 | {name} | {type} | "{quote}" | {evidence} | {trending} |

### Recently Trending (New openings or rising popularity)
- **{spot_1}** — {what it is} — first mentioned {date range}, now {n} mentions
- **{spot_2}** — {what it is} — {evidence of rising buzz}

### Closing / Declining
- **{spot}** — {was popular, now closing/declining} — signals neighborhood shift?

---

## V. Commute Reality Check

### Transit Score: {Official estimate} vs. {Resident Reality}

| Route | Official Time | Resident Reality | Pain Points | Peak Hour Impact |
|-------|-------------|-----------------|-------------|-----------------|
| To {destination_1} via {MRT/bus} | {n} min | {n} min (resident avg) | "{complaint quote}" | {severe/moderate/minor} |
| To {destination_2} via {route} | {n} min | {n} min | "{quote}" | {impact} |

### What Commuters Actually Say
> "{Direct quote about commute experience}" — Dcard, {engagement}
> "{Quote about transit frustration or praise}" — Threads

### Parking Reality
- **Availability**: {easy/difficult/nightmare} — based on {n} mentions
- **Cost range**: {from social mentions}
- **Resident tips**: "{local workaround or advice}"

### Walkability & Bikeability
- Walking sentiment: {positive/negative — evidence}
- YouBike availability: {mentioned positively/negatively}
- Sidewalk quality: {complaints or praise}

---

## VI. Safety & Living Experience Index

### Safety Perception

| Factor | Rating | Evidence | Volume |
|--------|--------|---------|--------|
| Walking at night | {safe/caution/avoid} | "{resident quote}" | {n} mentions |
| Property security | {good/average/poor} | "{quote}" | {n} mentions |
| Street lighting | {good/poor} | "{quote}" | {n} mentions |
| Traffic safety | {good/concerning} | "{quote}" | {n} mentions |

### Noise & Environment

| Source | Severity | Time Pattern | Evidence |
|--------|----------|-------------|---------|
| {noise_source_1} | {high/medium/low} | {e.g. "weekends after 10pm"} | "{quote}" |
| {noise_source_2} | {severity} | {pattern} | "{quote}" |

### Community Quality
- Management quality (for complexes): {good/average/poor — evidence}
- Neighbor relations: {friendly/neutral/complaints}
- Community events: {active/inactive — examples}

---

## VII. Neighborhood Trajectory & Outlook

### Current Phase: {RISING / ESTABLISHED / TRANSITIONING / DECLINING}

**Evidence**:
- New openings (last 6 months): {list of new businesses/venues mentioned}
- Closures (last 6 months): {list if any}
- Development mentions: {urban renewal, new MRT station, redevelopment projects}
- Price sentiment: {getting expensive / stable / declining — quotes}
- Demographic signals: {who's moving in/out}

### Trajectory Indicators

| Signal | Direction | Evidence |
|--------|----------|---------|
| New business openings | {up/stable/down} | {n} mentions of new spots |
| Resident satisfaction trend | {improving/stable/declining} | Sentiment shift in recent posts |
| Price perception | {rising/stable/falling} | "{quote about prices}" |
| Infrastructure investment | {active/none} | {MRT, roads, parks mentioned} |
| Demographic shift | {younging/aging/stable} | {evidence} |

### 12-Month Outlook
{2-3 sentence forecast based on trajectory signals: is this area likely to appreciate, stay stable, or face challenges? What catalysts or risks are visible in social data?}

---

## VIII. Persona Fit Matrix

| Buyer Persona | Fit Score | Why | Watch Out For |
|--------------|----------|-----|---------------|
| Young professional (single) | {1-5}/5 | {reasons} | {concerns} |
| Young couple (no kids) | {1-5}/5 | {reasons} | {concerns} |
| Family with young kids | {1-5}/5 | {reasons} | {concerns} |
| Remote worker | {1-5}/5 | {reasons} | {concerns} |
| Pet owner | {1-5}/5 | {reasons} | {concerns} |
| Retiree | {1-5}/5 | {reasons} | {concerns} |
| Investor | {1-5}/5 | {reasons} | {concerns} |

---

## IX. Voice of Residents — Verbatim Evidence

### Living Here Is Great Because...
> "{Positive quote}" — {platform}, {engagement}
> "{Positive quote}" — {platform}, {engagement}
> "{Positive quote}" — {platform}, {engagement}

### I Wish I Knew Before Moving Here...
> "{Warning/tip quote}" — {platform}, {engagement}
> "{Quote}" — {platform}, {engagement}

### I'm Thinking of Leaving Because...
> "{Negative quote}" — {platform}, {engagement}
> "{Quote}" — {platform}, {engagement}

---

## X. All Listing Links by Platform

All source links collected during research, organized by platform for easy browsing:

### Facebook Marketplace Listings
| # | Title / Description | Rent/mo | Posted | Link |
|---|-------------------|---------|--------|------|
| 1 | {brief description} | NT${n} | {date} | [FB Marketplace](https://www.facebook.com/marketplace/item/{listing_id}) |
| 2 | {description} | NT${n} | {date} | [FB Marketplace](https://www.facebook.com/marketplace/item/{listing_id}) |

### Facebook Group Posts
| # | Group / Context | Type | Posted | Link |
|---|---------------|------|--------|------|
| 1 | {group name or topic} | {rental/sublease/roommate} | {date} | [FB Post](https://www.facebook.com/{postId}) |
| 2 | {group/context} | {type} | {date} | [FB Post](https://www.facebook.com/{postId}) |

### Dcard Posts
| # | Forum | Title / Topic | Type | Link |
|---|-------|-------------|------|------|
| 1 | {forum alias} | {title snippet} | {rental/sublease/review} | [Dcard](https://www.dcard.tw/f/{forum}/p/{postId}) |
| 2 | {forum} | {title} | {type} | [Dcard](https://www.dcard.tw/f/{forum}/p/{postId}) |

### Threads Posts
| # | Author | Topic | Posted | Link |
|---|--------|-------|--------|------|
| 1 | @{username} | {topic snippet} | {date} | [Threads](https://www.threads.net/post/{postId}) |
| 2 | @{username} | {topic} | {date} | [Threads](https://www.threads.net/post/{postId}) |

### X (Twitter) Posts
| # | Author | Topic | Posted | Link |
|---|--------|-------|--------|------|
| 1 | @{username} | {topic snippet} | {date} | [X Post](https://x.com/{username}/status/{tweetId}) |
| 2 | @{username} | {topic} | {date} | [X Post](https://x.com/{username}/status/{tweetId}) |

### Instagram Tags & Posts
| # | Tag / Account | Content Type | Engagement | Link |
|---|-------------|-------------|-----------|------|
| 1 | #{tag} | {photo/reel/carousel} | {likes/comments} | [Instagram](https://www.instagram.com/p/{shortcode}) |
| 2 | @{username} | {type} | {engagement} | [Instagram](https://www.instagram.com/p/{shortcode}) |

**Total links collected**: {n} across {n} platforms

> **Note**: Social media links may expire or become unavailable over time. Links were valid as of the research date. For Facebook posts in private groups, you may need to be a group member to view.

---

## XI. Data Sources & Methodology

| Platform | Search Queries | Content Analyzed | Key Signal |
|----------|---------------|-----------------|------------|
| Dcard | {queries} | {n} posts, {n} comments | Resident sentiment, commute complaints |
| Facebook | {queries} | {n} posts, {n} comments | Community groups, local recommendations |
| Instagram | {queries} | {n} tags checked | Hidden gems, lifestyle mapping |
| Threads | {queries} | {n} posts | Real-time buzz, spontaneous opinions |
| X/Twitter | {queries} | {n} posts | Transit delays, real-time incidents |

**Total Data Points**: {n} pieces of content across {n} platforms
**Research Date**: {date}
**Coverage Area**: {neighborhood description and approximate boundaries}

---

*This report was generated using cross-platform social intelligence powered by ByCrawl.*
*Data reflects real resident voices — not surveys, not marketing copy.*
*For property platform integration, this data can be delivered as a structured API overlay.*
```

---

## Platform Integration Notes (for PropTech Partners)

This skill's output maps directly to a **Social Overlay API** layer:

| Data Type | Source Platforms | Suggested Feature |
|-----------|----------------|-------------------|
| **Rental Listings** | FB Marketplace, FB Groups, Dcard | Off-platform inventory feed — listings not on 591/traditional aggregators |
| **Rental Pricing** | FB Marketplace, Dcard, FB Groups | "Market Rent" benchmark on listings — is this above or below median? |
| **Demand Heatmap** | Dcard, FB Groups, Threads | "Renter Demand" indicator — shows what people want but can't find |
| **Landlord Reputation** | FB Groups, Dcard comments | "Landlord Score" badge — based on tenant reviews and response rate |
| Resident Sentiment Score | Dcard, FB, Threads | "Resident Vibe" badge on listings |
| Hidden Gem Rankings | IG, Threads, FB | "Trending Nearby" in Life Circle maps |
| Commute Reality | Dcard, Threads, X | "Reality Check" tooltip on transit times |
| Safety Perception | Dcard, FB | "Living Experience Index" score |
| Neighborhood Trajectory | All platforms | "Up-and-Coming" or "Established" tag |
| Persona Fit | Aggregated | "Best For: Young Professionals" filter |

---

## Error Handling

- If a platform returns **empty results**, skip it and note "No data on {platform}" in the relevant sections — do not stop the report.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- If the neighborhood is **too broad** (e.g. "Taipei"), ask the user to narrow to a specific district or street.
- If **no Dcard results** are found (non-Taiwan location), shift primary sentiment source to Reddit and Facebook.
- **Always deliver partial results** rather than failing entirely — even 3 platforms provide valuable neighborhood intelligence.

## Estimated API Usage

- **Listings only** (Part A — FB Marketplace + FB Groups + Dcard): ~20-30 API calls
- **Quick run** (Listings + Dcard sentiment + Facebook sentiment): ~35-45 API calls
- **Standard run** (Listings + all sentiment + Instagram + Threads): ~60-75 API calls
- **Full run** (all platforms + comment pulls + trajectory analysis): ~85-110 API calls
- Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive runs.
