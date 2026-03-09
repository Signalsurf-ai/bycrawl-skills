---
name: bycrawl-fb-marketplace
description: "Search and analyze any product on Facebook Marketplace. Find deals, compare prices, and get market trends for electronics, furniture, vehicles, clothing, and more using ByCrawl MCP."
allowed-tools: ["mcp__bycrawl__*"]
user-invocable: true
argument-hint: "<keyword> e.g. iPhone 15, IKEA desk, Toyota Corolla, Nike shoes"
---

# FB Marketplace Scout

Search Facebook Marketplace for any product, analyze pricing trends, and find underpriced deals — powered by the ByCrawl MCP server.

## Prerequisites

This skill requires the **ByCrawl MCP server** to be configured in Claude Code.

### Configure MCP

Add the ByCrawl MCP server to your project settings (`.claude/settings.json`) or user settings (`~/.claude/settings.json`):

```json
{
  "mcpServers": {
    "bycrawl": {
      "command": "npx",
      "args": ["-y", "@bycrawl/mcp"],
      "env": {
        "BYCRAWL_API_KEY": "your-api-key-here"
      }
    }
  }
}
```

Or configure via the Claude Code CLI:

```bash
claude mcp add bycrawl -- npx -y @bycrawl/mcp
```

Then set your API key in the environment or in the MCP server config above.

- Get your API key at: https://bycrawl.com
- MCP package: `@bycrawl/mcp`

## Workflow

### 1. Parse user input

The user provides `$ARGUMENTS` — a keyword for the product they want to search.

Example keywords by category:
- Electronics: iPhone 15, MacBook Pro, Switch OLED, AirPods Pro
- Furniture: IKEA desk, sofa, bookshelf, dining table
- Vehicles: Toyota Corolla, Gogoro, bicycle, electric scooter
- Fashion: Nike shoes, Uniqlo, designer bag, vintage jacket
- Home: Dyson vacuum, air purifier, coffee machine, blender
- Others: baby stroller, camping tent, guitar, LEGO

If `$ARGUMENTS` is empty, browse general trending listings.

### 2. Search FB Marketplace

Use the ByCrawl MCP tool to search Facebook Marketplace:

- Call `mcp__bycrawl__facebook_marketplace_search` with parameters:
  - `q`: the search keyword
  - `location`: user-specified city (default: `taipei`)

### 3. Analyze the results

From the search results, extract and organize:

1. **Total listings found**
2. **Price analysis:**
   - Lowest price
   - Highest price
   - Median price (estimated)
   - Most common price range
3. **Deal detection:** Flag listings priced 20%+ below the median as potential deals
4. **Listing details:** For each listing, note: title, price, location, condition, photos count

### 4. Get details for top deals (optional)

If there are promising deals (significantly below median price), fetch full details:

- Call `mcp__bycrawl__facebook_marketplace_item` with parameters:
  - `listing_id`: the ID of the listing to fetch

Check for:
- Seller info and credibility
- Item condition description
- Number of photos (more photos = more trustworthy)
- Delivery options available

### 5. Browse category trends (when no specific keyword)

If the user wants to browse a category or general trends:

- Call `mcp__bycrawl__facebook_marketplace_listings` with parameters:
  - `location`: user-specified city (default: `taipei`)
  - `category`: optional category filter (e.g. electronics, furniture, vehicles, apparel)

## Output Format

Present results in this structured format:

```
# FB Marketplace Scout — {keyword}
Location: Taipei | Source: Facebook Marketplace

## Market Overview
- Total listings: X
- Price range: $ X,XXX ~ $ XX,XXX
- Median price: ~$ X,XXX

## Price Distribution
| Price Range      | Count | Percentage |
|-----------------|-------|------------|
| $ 0-5,000     | X     | XX%        |
| $ 5,001-10,000| X     | XX%        |
| $ 10,001+     | X     | XX%        |

## Top Deals (Below Market Price)
| # | Title | Price | vs Median | Condition | Link |
|---|-------|-------|-----------|-----------|------|
| 1 | ...   | $   | -XX%      | ...       | ...  |
| 2 | ...   | $   | -XX%      | ...       | ...  |

## All Listings
| # | Title | Price | Location | Photos |
|---|-------|-------|----------|--------|
| 1 | ...   | $   | ...      | X      |
| ...                                     |

## Tips
- [Context-specific buying tips for this product category]
```

## Notes

- All prices are in USD ($)
- Default location is Taipei; user can specify other cities
- Credit cost: 1 credit per search, 1 credit per item detail
- Deal threshold: listings priced 20%+ below median are flagged
- Always remind users to meet in safe public places for transactions
