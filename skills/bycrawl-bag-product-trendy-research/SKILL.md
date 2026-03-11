---
name: bycrawl-bag-product-trendy-research
description: >
  Comprehensive product trend intelligence and pitch deck research using ByCrawl MCP.
  Use when user says "what's trending", "latest trends", "trend report", "pitch deck research",
  "what styles are popular", "trending products", "consumer trend analysis",
  "what are customers buying", "trending backpack styles", "bag trends", "fashion product trends",
  "market trend report", "trend intelligence for [category]", "what's hot in [product category]".
  Also triggers for any fashion/accessories/bag/luggage/backpack trend queries.
allowed-tools: ["mcp_bycrawl_*"]
user-invocable: true
argument-hint: "<product_category> [target_market] e.g. backpacks, Gen Z commuters"
---

# Product Trend Research

## ICP (Ideal Customer Profile)
- **Bag & accessory manufacturers** pitching new seasonal collections to retail buyers
- **Product designers** identifying emerging silhouettes, materials, and colors
- **Sales teams** building trend-backed proposals for wholesale/retail buyers
- **Merchandisers** curating assortments based on real consumer demand signals
- **Brand strategists** validating creative direction with social proof

## What This Skill Does
Gathers comprehensive, real-time trend intelligence for a product category by scanning social platforms, influencer content, and consumer conversations. Produces a **McKinsey-grade trend pitch report** answering: "What styles, features, and aesthetics are consumers excited about right now?"

Unlike generic trend reports, this skill synthesizes **cross-platform social signals** into actionable product recommendations with evidence density suitable for board presentations, buyer meetings, and investor decks.

**Need visual proof?** After running this skill, use `/bycrawl-trend-visual-evidence` to capture Google Trends graphs, retail site screenshots, and product page visuals for a presentation-ready deck.

## Required Input
- **Product category** (required): e.g. "backpacks", "tote bags", "crossbody bags", "luggage"
- **Target market** (optional): e.g. "Gen Z", "office workers", "travelers", "students"
- **Season / pitch window** (optional): e.g. "Spring/Summer 2025", "back-to-school"

## Prerequisites

- **ByCrawl MCP server** must be installed and configured. If MCP tools are not available, install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Workflow

### Step 1: TikTok Trend Pulse (Highest Signal for Consumer Products)
TikTok is the fastest indicator of emerging product aesthetics. Run in parallel:
```
tiktok_search_videos(keyword="{category} trend 2025", count=30)
tiktok_search_videos(keyword="{category} haul OR unboxing OR review", count=30)
tiktok_search_videos(keyword="{category} aesthetic", count=20)
tiktok_search_videos(keyword="best {category}", count=20)
```
Extract: dominant visual styles, recurring features mentioned, hashtags used, view counts (proxy for interest volume).

For the top 3-5 videos by engagement, pull subtitles for deeper analysis:
```
tiktok_get_video_subtitles(videoId="{id}", language="en")
```

### Step 2: Instagram Aesthetic Mapping
```
instagram_search_tags(query="{category}trend")
instagram_search_tags(query="{category}style")
instagram_search_tags(query="{category}2025")
instagram_search_tags(query="{adjective}{category}")
```
Use category-relevant adjectives like "vintage", "minimal", "luxury", "gorpcore", etc.

Extract: visual aesthetic clusters (minimalist, Y2K, gorpcore, quiet luxury, etc.), color palettes appearing frequently, styling contexts (gym, office, travel, school).

### Step 3: Reddit Consumer Voice (Unfiltered Opinions)
```
reddit_search_posts(query="{category} recommendation 2025", sort="top", count=30)
reddit_search_posts(query="best {category} for {use_case}", sort="top", count=25)
reddit_search_posts(query="{category} what features do you want", sort="top", count=20)
reddit_search_posts(query="{category} worth it OR disappointed", sort="top", count=20)
```
For the top 3-5 posts by engagement, pull comments for deeper sentiment:
```
reddit_get_post(postId="{id}")
```

Extract: must-have features, deal-breaker flaws in existing products, price sensitivity, brand mentions.

### Step 4: YouTube Long-Form Reviews (Mid-Funnel Intent)
```
youtube_search_videos(q="{category} best 2025", count=15)
youtube_search_videos(q="{category} collection review OR haul", count=15)
youtube_search_videos(q="{category} buying guide", count=10)
```

For the top 3 videos by views, pull transcripts and comments:
```
youtube_get_video_transcription(videoId="{id}", language="en")
youtube_get_video_comments(videoId="{id}", count=30)
```

Extract: reviewer criteria (what they test/judge), most-praised product attributes, content formats dominating the category, audience questions from comments.

### Step 5: X / Twitter Real-Time Buzz
```
x_search_posts(query="{category} obsessed OR love OR need this", product="Top", count=30)
x_search_posts(query="{category} drop OR restock OR sold out", product="Top", count=20)
x_search_posts(query="{category} wishlist", product="Top", count=20)
```
Extract: viral product moments, limited-edition hype signals, influencer-driven demand spikes.

### Step 6: Threads Emerging Conversations
```
threads_search_posts(query="{category} trend", count=20)
threads_search_posts(query="{category} favorite OR obsessed", count=20)
```
Extract: emerging conversation themes not yet saturated on other platforms.

### Step 7: LinkedIn B2B / Retail Industry Signals (Optional — for trade/wholesale pitches)
```
linkedin_search_users(query="buyer {retail_category} accessories", count=20)
linkedin_search_jobs(query="{category} product development OR trend forecasting", count=20)
```
Extract: what retail buyers are hiring for = what categories they are investing in. Job posting trends indicate enterprise-level demand.

---

## Analysis Framework

After data collection, synthesize across platforms using these lenses:

### Aesthetic Clusters
Group visual styles into named trend buckets. Common product/fashion clusters to check for:
- **Gorpcore / Utilitarian** — technical materials, molle webbing, outdoorsy
- **Quiet Luxury** — minimal branding, premium materials, neutral tones
- **Y2K Revival** — metallics, transparent panels, logo-heavy
- **Cottagecore / Soft Life** — natural materials, earth tones, organic shapes
- **Techwear / Cyberpunk** — dark colors, angular shapes, modular features
- **Preppy / Clean Girl** — structured shapes, monogram, classic colorways
- **Dopamine Dressing** — bold colors, playful shapes, maximalist expression
- **Quiet Outdoor** — muted earth tones, technical-but-subtle, crossover appeal

Assign each cluster a **momentum score**: HIGH (dominant across 3+ platforms), RISING (growing fast on 1-2 platforms), EMERGING (early signals, <6 months old), or DECLINING (peaked, engagement dropping).

### Feature Demand Map
Rank by frequency of mentions across all platforms:
- Organizational features (laptop sleeves, water bottle pockets, anti-theft)
- Material preferences (recycled, leather, canvas, waterproof nylon)
- Comfort / ergonomics (padded straps, weight distribution, chest clips)
- Size preferences (mini / micro vs. oversized, expandable)
- Sustainability signals (recycled materials, ethical production, certifications)
- Tech integration (USB charging, AirTag pockets, built-in power banks)

### Price Architecture
From Reddit + TikTok + YouTube comments, identify:
- Budget tier appetite (<$50, $50-150, $150-300, $300+)
- "Worth it" thresholds for premium features
- Brand comparisons used as price anchors
- Willingness-to-pay ceiling by consumer segment

### Style Lifecycle Classification
For every style/aesthetic identified, classify it into a lifecycle stage based on cross-platform signal trajectory:

**INCLINING (Rising Styles)**
Detect by: rapidly growing view counts on TikTok, new hashtags appearing on Instagram, increasing Reddit discussion volume, early-adopter influencers posting.
- Compare recent content volume (last 30 days) vs. older content — a 2x+ increase signals inclining
- Look for "just discovered" / "new favorite" / "obsessed" language
- Check if mainstream brands have NOT yet adopted it (early = opportunity)

**PEAKING (At-Height Styles)**
Detect by: dominant across 3+ platforms, major brands already producing, high content volume but growth rate flattening.
- Look for "everyone has this" / "basic" / "overdone" language starting to appear
- Still commercially viable but differentiation is harder
- Mass-market retailers carrying the style = peak signal

**DECLINING (Fading Styles)**
Detect by: engagement rates dropping despite high content volume, "over it" / "dated" / "so last year" sentiment, clearance/sale mentions increasing.
- Compare engagement rates (likes/views ratio) over time — declining ratio = fading
- Look for "replacing my {style} with {new style}" transition posts
- Reddit threads asking "is {style} still cool?" = decline signal
- Influencers pivoting away = leading indicator

**EMERGING (Pre-Trend)**
Detect by: <100 posts across platforms, niche creator adoption only, no mainstream brand presence, appears in fashion-forward or subculture communities first.
- Highest risk but highest reward — 12-18 month lead time before mainstream
- Look for signals in Threads and niche subreddits before TikTok picks it up

For each style, also note the **transition direction** — e.g. "Gorpcore: PEAKING, transitioning toward DECLINING as Quiet Outdoor rises as its successor."

### Competitive Whitespace Analysis
From brand mentions across all platforms:
- Map which brands own which aesthetic/price position
- Identify positions no brand currently owns
- Note brands losing share (rising complaints) vs. gaining (rising praise)

---

## Output Format

```markdown
# Product Trend Intelligence Report
## {Category} | {Season/Year}
### Prepared for: [Client Name] | Research Date: {date}

---

## Executive Summary

{3-4 sentence strategic overview: the single biggest trend opportunity, supporting evidence strength, urgency signal, and recommended action. Written for a C-suite audience.}

**Bottom Line**: {One sentence go/no-go on the opportunity with confidence level}

---

## I. Macro Trend Landscape

### Trend Momentum Matrix

| Trend | Momentum | Platform Signals | Consumer Segment | Revenue Potential |
|-------|----------|-----------------|------------------|-------------------|
| {trend_1} | HIGH | TikTok + IG + Reddit | {segment} | {$-estimate or relative} |
| {trend_2} | RISING | {platforms} | {segment} | {estimate} |
| {trend_3} | EMERGING | {platforms} | {segment} | {estimate} |
| {trend_4} | DECLINING | {platforms} | {segment} | Avoid |

---

## II. Style Lifecycle Tracker — Inclining vs. Declining

### Inclining Styles (Invest Now)

| Style | Lifecycle | Velocity | Key Evidence | Opportunity Window |
|-------|-----------|----------|-------------|-------------------|
| {style_1} | INCLINING | Fast | TikTok +{n}% content growth, {n} new IG hashtags | 6-12 months before peak |
| {style_2} | INCLINING | Moderate | Reddit buzz up {n}%, early influencer adoption | 12-18 months before peak |
| {style_3} | EMERGING | Early | Niche only — {n} posts, zero mainstream brands | 18-24 months (high risk/reward) |

**What's driving the rise**: {Cultural shifts, demographic changes, or macro trends fueling these styles}

### Peaking Styles (Harvest / Optimize)

| Style | Lifecycle | Saturation Level | Evidence | Recommended Action |
|-------|-----------|-----------------|----------|-------------------|
| {style_4} | PEAKING | High | Mass-market adoption, {brand} + {brand} carrying it | Differentiate or pivot |
| {style_5} | PEAKING | Medium | Strong across platforms but growth flattening | Last window to enter |

### Declining Styles (Exit / Avoid)

| Style | Lifecycle | Decline Signal | Evidence | Risk if Invested |
|-------|-----------|---------------|----------|-----------------|
| {style_6} | DECLINING | Engagement drop | Views steady but likes/comments down {n}% | Inventory risk — clearance likely |
| {style_7} | DECLINING | Sentiment shift | "Over it" / "dated" mentions up {n}x | Brand perception risk |

**Style Succession Map**: {Which declining styles are being replaced by which inclining styles — e.g. "Gorpcore (declining) → Quiet Outdoor (inclining)", "Y2K Maximalism (declining) → Clean Minimalism (inclining)"}

---

## III. Top 3 Trend Opportunities (Deep Dive) — Focus on INCLINING Styles

### Opportunity 1: {Trend Name}
**Momentum**: {HIGH / RISING / EMERGING} | **Time Horizon**: {months until mainstream}

**What It Is**
{2-3 sentence description of the trend, its cultural driver, and why it resonates now}

**Cross-Platform Evidence**
- **TikTok**: {n} videos analyzed, top video {views}M views, hashtag #{tag} ({volume} posts)
- **Instagram**: #{hashtag} ({volume} posts), aesthetic cluster: {name}
- **Reddit**: {n} posts, top post {upvotes} upvotes, r/{subreddit} discussion
- **YouTube**: {n} review videos, avg {views} views, top creator: {name}
- **X/Twitter**: {n} posts, viral moment: {description}

**Key Design Elements**
- Colors: {specific colors with context}
- Materials: {specific materials}
- Silhouettes: {shapes, proportions}
- Details: {hardware, closures, accents}

**Target Consumer**
- Demographics: {age, gender, location signals}
- Psychographics: {lifestyle, values, purchase triggers}
- Use context: {where/when they use the product}

**Product Implication**
{Specific feature/design recommendation for the client's product line}

**Competitive Gap**
{What existing brands are missing — the whitespace opportunity}

### Opportunity 2: {Trend Name}
{Same structure as above}

### Opportunity 3: {Trend Name}
{Same structure as above}

---

## IV. Feature Priority Matrix

| Feature | Demand Signal | Platform Evidence | Frequency | Priority |
|---------|--------------|-------------------|-----------|----------|
| {feature_1} | "{direct quote}" | Reddit ({n}), TikTok ({n}) | Very High | MUST-HAVE |
| {feature_2} | "{direct quote}" | YouTube ({n}), Reddit ({n}) | High | MUST-HAVE |
| {feature_3} | "{direct quote}" | TikTok ({n}) | Medium | DIFFERENTIATOR |
| {feature_4} | "{direct quote}" | Reddit ({n}) | Medium | DIFFERENTIATOR |
| {feature_5} | "{direct quote}" | X ({n}) | Low | NICE-TO-HAVE |

---

## V. Color & Material Intelligence

### Color Palette — This Season

| Color Direction | Platforms Observed | Frequency | Trend Stage |
|----------------|-------------------|-----------|-------------|
| {color_1} | TikTok, IG | High | Peak |
| {color_2} | TikTok, IG, Reddit | Rising | Growth |
| {color_3} | TikTok | Low | Emerging |

### Material Trends

| Material | Consumer Sentiment | Sustainability Signal | Price Impact |
|----------|-------------------|----------------------|--------------|
| {material_1} | {positive/negative + quote} | {yes/no + detail} | {+/-$} |
| {material_2} | {sentiment} | {signal} | {impact} |

---

## VI. Price Architecture & Willingness to Pay

| Segment | Price Range | Consumer Evidence | Recommended Position |
|---------|------------|-------------------|---------------------|
| Entry | <${n} | "{quote about budget}" | {recommendation} |
| Mid | ${n}-${n} | "{quote about value}" | {recommendation} |
| Premium | ${n}-${n} | "{quote about worth it}" | {recommendation} |
| Luxury | >${n} | "{quote or lack of signal}" | {recommendation} |

**Price Anchors**: Consumers compare against {brand_1} (${price}), {brand_2} (${price}), {brand_3} (${price})

---

## VII. Competitive Landscape

| Brand/Product | Consumer Love | Consumer Complaints | Price | Aesthetic Position |
|--------------|---------------|---------------------|-------|--------------------|
| {competitor_1} | {what they praise} | {what they complain about} | ${n} | {cluster} |
| {competitor_2} | {praise} | {complaints} | ${n} | {cluster} |
| {competitor_3} | {praise} | {complaints} | ${n} | {cluster} |
| {competitor_4} | {praise} | {complaints} | ${n} | {cluster} |

**Whitespace Opportunity**: {Where no brand is winning yet — specific position + evidence}

---

## VIII. Voice of Consumer — Verbatim Evidence

### Unmet Needs
> "{Direct quote from Reddit/TikTok about what they wish existed}" — r/{subreddit}, {upvotes} upvotes
> "{Quote about gap in current products}" — YouTube comment, {likes} likes

### Purchase Triggers
> "{Quote about what made them buy}" — TikTok comment
> "{Quote about recommendation}" — Reddit

### Dealbreakers
> "{Quote about what makes them return/avoid}" — Reddit, {upvotes} upvotes
> "{Quote about disappointment}" — YouTube comment

---

## IX. Strategic Recommendations

### Recommended Collection Concept: {Collection Name}

**Lead Story**: {Trend name} — backed by {evidence summary}

**Hero Product Concept**
- Description: {detailed product concept}
- Key features: {top 3 from Feature Priority Matrix}
- Target aesthetic: {cluster name}
- Materials: {recommended}
- Colorways: {recommended 3-4 colors}

**Target Consumer Profile**
- Primary: {segment description}
- Secondary: {segment description}

**Suggested Retail Price**: ${range} (based on willingness-to-pay signals and competitive positioning)

**Retail Buyer Pitch Points**
1. {Evidence-backed selling point}
2. {Evidence-backed selling point}
3. {Evidence-backed selling point}

### Timing & Urgency

| Signal | Assessment |
|--------|-----------|
| Trend lifecycle stage | {Early / Growth / Peak / Declining} |
| Competitive saturation | {Low / Medium / High} |
| Window of opportunity | {e.g. "12-18 months before mass-market saturation"} |
| Recommended action | {e.g. "Develop for Fall/Winter 2026 launch"} |

---

## X. Research Methodology & Data Summary

| Platform | Search Queries Used | Content Analyzed | Key Metric |
|----------|-------------------|------------------|------------|
| TikTok | {queries} | {n} videos | {total views} |
| Instagram | {queries} | {n} hashtags | {total posts} |
| Reddit | {queries} | {n} posts | {total upvotes} |
| YouTube | {queries} | {n} videos | {total views} |
| X/Twitter | {queries} | {n} posts | {total engagement} |
| Threads | {queries} | {n} posts | {total engagement} |
| LinkedIn | {queries} | {n} profiles/jobs | {insight} |

**Total Data Points Analyzed**: {n} pieces of content across {n} platforms
**Research Window**: Real-time social data as of {date}

---

*This report was generated using cross-platform social intelligence powered by ByCrawl.*
*Data reflects real-time consumer signals — not projections or surveys.*
```

---

## Error Handling

- If a platform returns **empty results**, skip it and note "No data on {platform}" in the relevant sections — do not stop the report.
- If the **API key is missing or invalid**, stop and instruct the user: set `BYCRAWL_API_KEY` environment variable with a valid key from [bycrawl.com](https://bycrawl.com).
- If a **rate limit** is hit, reduce `count` parameters by half and retry once.
- If a specific **MCP tool is unavailable**, check that the ByCrawl MCP server is installed (`npx @bycrawl/mcp`).
- If the category is too broad (e.g. "bags"), **ask the user to narrow it** to a specific silhouette or use-case before proceeding.
- **Always deliver partial results** rather than failing entirely — a report from 4 of 7 platforms is still pitch-worthy.

## Estimated API Usage

- **Quick run** (TikTok + Instagram + Reddit only): ~15-20 API calls
- **Standard run** (TikTok + Instagram + Reddit + YouTube + X): ~35-45 API calls
- **Full run** (all platforms + transcripts + comments + LinkedIn): ~55-70 API calls
- Each API call consumes 1 ByCrawl credit. Reduce `count` parameters for cost-sensitive runs.
