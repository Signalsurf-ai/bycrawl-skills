---
name: bycrawl-trend-visual-evidence
description: >
  Visual evidence companion for product trend reports using Playwright MCP.
  Use after running bycrawl-bag-product-trendy-research to capture Google Trends graphs,
  retail site screenshots, and product page visuals for presentation-ready decks.
  Use when user says "get visual evidence", "screenshot trends", "trend visuals",
  "make it presentation ready", "add charts to trend report", "Google Trends for [category]",
  "screenshot product pages", "visual proof for trends".
allowed-tools: ["mcp_playwright_*", "mcp_bycrawl_*"]
user-invocable: true
argument-hint: "<product_category> [styles_to_compare] e.g. backpacks, gorpcore vs quiet luxury"
---

# Trend Visual Evidence

## ICP (Ideal Customer Profile)
- **Sales teams** preparing pitch decks for retail buyer meetings
- **Brand strategists** presenting trend direction to leadership
- **Product managers** building business cases with visual proof
- **Anyone** who ran `/bycrawl-bag-product-trendy-research` and needs presentation-ready visuals

## What This Skill Does
Captures visual evidence — Google Trends graphs, retail product page screenshots, and brand site visuals — to supplement a trend research report with hard-to-argue-with imagery. Turns a data report into a boardroom-ready deck.

This is a **companion skill**. Run `/bycrawl-bag-product-trendy-research` first to identify trends, then run this to capture the visual proof.

## Required Input
- **Product category** (required): e.g. "backpacks", "tote bags", "crossbody bags"
- **Key trend styles** (required): e.g. "gorpcore, quiet luxury, techwear" — from the trend report
- **Competitor brands** (optional): e.g. "Osprey, Herschel, Fjallraven" — for product page screenshots
- **Region** (optional): e.g. "US", "TW", "JP" — for Google Trends geo targeting

## Prerequisites

- **Playwright MCP server** must be installed and configured. Install with:
  ```
  npx @anthropic-ai/claude-code mcp add playwright -- npx @anthropic-ai/mcp-playwright
  ```
- **ByCrawl MCP server** for supplementary data lookups. Install with:
  ```
  npx @anthropic-ai/claude-code mcp add bycrawl -- npx @bycrawl/mcp
  ```
- **ByCrawl API key** set as environment variable: `export BYCRAWL_API_KEY=sk_byc_...`
- Get your API key at [bycrawl.com](https://bycrawl.com)

## Workflow

### Step 1: Google Trends — Inclining vs. Declining Proof

This is the highest-value visual. Capture interest-over-time graphs comparing trend styles.

**Single trend timeline** (past 12 months):
```
playwright_navigate(url="https://trends.google.com/trends/explore?date=today%2012-m&q={style}%20{category}")
playwright_screenshot()  // capture the interest-over-time graph
```

**Head-to-head comparison** (up to 5 terms):
```
playwright_navigate(url="https://trends.google.com/trends/explore?date=today%2012-m&q={style_1}%20{category},{style_2}%20{category},{style_3}%20{category}")
playwright_screenshot()  // shows relative interest — powerful for inclining vs declining
```

**Regional breakdown** (if target market specified):
```
playwright_navigate(url="https://trends.google.com/trends/explore?date=today%2012-m&geo={region_code}&q={style}%20{category}")
playwright_screenshot()  // geo-specific demand
```

Recommended comparisons to capture:
- All inclining styles vs. all declining styles (from the trend report)
- Top 3 trending styles head-to-head
- Category term alone (overall category health)

### Step 2: Retail & Brand Site Product Pages

Screenshot product pages from accessible brand sites to show current market offerings.

**DTC / brand sites** (high success rate):
```
playwright_navigate(url="https://{brand_site}.com/collections/{category}")
playwright_screenshot()  // collection page showing current season lineup
```

```
playwright_navigate(url="https://{brand_site}.com/products/{specific_product}")
playwright_screenshot()  // hero product detail page
```

**Target these site types** (reliable, rarely block headless browsers):
- Brand DTC stores (Herschel, Fjallraven, Osprey, Cotopaxi, etc.)
- Specialty retailers (REI, Nordstrom product pages)
- Amazon product listings
- Kickstarter/Indiegogo trending products in the category

**Avoid these** (anti-bot protection, will likely fail):
- Zara, H&M, Nike, Adidas (Cloudflare/DataDome protected)
- Pinterest (login wall)
- Instagram/TikTok web (use ByCrawl API instead)

If a page blocks or fails to load, skip it — note "Screenshot unavailable for {site}" and move on.

### Step 3: Search Results Snapshot

Capture what consumers see when searching:
```
playwright_navigate(url="https://www.google.com/search?q={style}+{category}+2025&tbm=isch")
playwright_screenshot()  // Google Image results = visual landscape overview
```

```
playwright_navigate(url="https://www.google.com/search?q=best+{style}+{category}")
playwright_screenshot()  // organic results = what content ranks, shopping ads = price signals
```

### Step 4: Amazon / Marketplace Trends (Optional)

Amazon bestseller and search pages are accessible to Playwright:
```
playwright_navigate(url="https://www.amazon.com/s?k={style}+{category}&s=review-rank")
playwright_screenshot()  // top-rated products in the style
```

```
playwright_navigate(url="https://www.amazon.com/s?k={category}&s=date-desc-rank")
playwright_screenshot()  // newest arrivals = what brands are launching now
```

---

## Analysis & Annotation

After capturing screenshots, annotate each with context from the trend report:

For each Google Trends screenshot:
- Label which line is which style
- Call out the inflection point where an inclining style crossed a declining one
- Note the percentage change over the period

For each product page screenshot:
- Note which trend/aesthetic cluster it belongs to
- Highlight features that match the Feature Priority Matrix
- Note the price point and how it fits the Price Architecture

---

## Output Format

```markdown
# Visual Evidence Package
## {Category} Trend Report Supplement | {Date}

---

## Google Trends Evidence

### Overall Category Health
[Screenshot: {category} search interest, past 12 months]
**Reading**: {Trend direction — growing, stable, or declining category overall}

### Style Comparison: Inclining vs. Declining
[Screenshot: {style_1} vs {style_2} vs {style_3}, past 12 months]
**Key Insight**: {Which line is rising, which is falling, when they crossed}

### Regional Demand
[Screenshot: geo-specific interest if applicable]
**Key Insight**: {Regional variations — e.g. "gorpcore stronger in Pacific Northwest"}

---

## Retail Market Snapshot

### Current Brand Offerings

**{Brand 1}** — {Aesthetic position}
[Screenshot: collection/product page]
- Price range: ${n}-${n}
- Trend alignment: {which cluster}
- Notable: {key observation}

**{Brand 2}** — {Aesthetic position}
[Screenshot: collection/product page]
- Price range: ${n}-${n}
- Trend alignment: {which cluster}
- Notable: {key observation}

### Marketplace Signals
[Screenshot: Amazon/marketplace search results]
**Reading**: {What the bestseller list reveals about current demand}

---

## Search Landscape

### Visual Search Results
[Screenshot: Google Image results for {style} {category}]
**Reading**: {Dominant visual aesthetic in search results}

### Organic + Shopping Results
[Screenshot: Google search results]
**Reading**: {Price range in shopping ads, top content ranking}

---

## Evidence Summary

| Visual | What It Proves | Supports Report Section |
|--------|---------------|------------------------|
| Google Trends: {styles} | {style_1} inclining, {style_2} declining | Section II: Style Lifecycle |
| {Brand} product page | Market gap in {position} | Section VII: Competitive Landscape |
| Amazon bestsellers | {feature} dominates top sellers | Section IV: Feature Priority |
| Google Images | {aesthetic} is dominant visual | Section III: Trend Opportunities |

---

*Visual evidence captured: {date} · Powered by Playwright MCP*
*Pair with trend research report for complete intelligence package.*
```

---

## Error Handling

- If a **site blocks the browser** (Cloudflare, CAPTCHA, login wall), skip it and note "Screenshot unavailable — site blocked headless access." Do not retry.
- If **Google Trends shows no data** for a search term, try broader terms (drop the year, use just the style name).
- If **Playwright MCP is not installed**, stop and provide the install command shown in Prerequisites.
- If a **page loads but renders incomplete** (lazy-loaded content), try `playwright_evaluate` to scroll down first, then screenshot.
- **Always deliver partial results** — even 2-3 Google Trends screenshots add significant value to the report.
- **Do not attempt Pinterest, Instagram web, or TikTok web** — use ByCrawl APIs for those platforms instead.

## Estimated Usage

- **Quick run** (Google Trends only): 3-5 Playwright navigations + screenshots
- **Standard run** (Google Trends + 3-4 brand sites): 8-12 navigations
- **Full run** (Google Trends + brand sites + Amazon + Google Search): 15-20 navigations
- Each navigation takes ~3-10 seconds depending on page complexity.
- No ByCrawl credits consumed unless supplementary API lookups are needed.
