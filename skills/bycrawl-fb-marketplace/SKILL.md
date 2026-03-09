---
name: bycrawl-fb-marketplace
description: "Search and analyze any product on Facebook Marketplace. Find deals, compare prices, and get market trends for electronics, furniture, vehicles, clothing, and more using ByCrawl API."
allowed-tools: ["Bash", "WebFetch"]
user-invocable: true
argument-hint: "<keyword> [location] e.g. 'iPhone 15 new-york', 'IKEA desk taipei', 'Toyota Corolla'"
---

# FB Marketplace Scout

Search Facebook Marketplace for any product, analyze pricing trends, and find underpriced deals.

## Prerequisites

- ByCrawl API key set as `BYCRAWL_API_KEY` environment variable
- Base API URL: `https://api.bycrawl.com`

## Workflow

### 1. Parse user input

The user provides `$ARGUMENTS` — a keyword and optional location for the product they want to search.

Parse `$ARGUMENTS` to extract:
- **keyword**: The product to search for (required)
- **location**: City or region (optional, omit the `location` parameter from the API call if not provided)

Example keywords by category:
- Electronics: iPhone 15, MacBook Pro, Switch OLED, AirPods Pro
- Furniture: IKEA desk, sofa, bookshelf, dining table
- Vehicles: Toyota Corolla, Gogoro, bicycle, electric scooter
- Fashion: Nike shoes, Uniqlo, designer bag, vintage jacket
- Home: Dyson vacuum, air purifier, coffee machine, blender
- Others: baby stroller, camping tent, guitar, LEGO

If `$ARGUMENTS` is empty, browse general trending listings.

### 2. Search FB Marketplace

Use `Bash` to call the ByCrawl API:

```bash
curl -s "https://api.bycrawl.com/facebook/marketplace/search?q={keyword}&location={location}" \
  -H "x-api-key: $BYCRAWL_API_KEY" | python3 -m json.tool
```

> If the user did not specify a location, omit the `&location=` parameter entirely.

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

```bash
curl -s "https://api.bycrawl.com/facebook/marketplace/items/{listing_id}" \
  -H "x-api-key: $BYCRAWL_API_KEY" | python3 -m json.tool
```

Check for:
- Seller info and credibility
- Item condition description
- Number of photos (more photos = more trustworthy)
- Delivery options available

### 5. Browse category trends (when no specific keyword)

If the user wants to browse a category or general trends:

```bash
curl -s "https://api.bycrawl.com/facebook/marketplace/listings?location={location}" \
  -H "x-api-key: $BYCRAWL_API_KEY" | python3 -m json.tool
```

> If no location was specified, omit the `?location=` parameter.

Optional category filter if the user specifies one (e.g. electronics, furniture, vehicles, apparel).

## Output Format

Present results in this structured format:

```
# FB Marketplace Scout — {keyword}
Location: {location or "Not specified"} | Source: Facebook Marketplace

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

- All prices are in the local currency of the marketplace location
- Location is optional; if not specified, results are not filtered by geography
- Credit cost: 1 credit per search, 1 credit per item detail
- Deal threshold: listings priced 20%+ below median are flagged
- Always remind users to meet in safe public places for transactions
