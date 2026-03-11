# bycrawl-skills

Claude Code skills for ByCrawl — social media intelligence, brand monitoring, competitor analysis, and more using the [ByCrawl API](https://bycrawl.com).

## Skills

| Skill | Command | Description |
|-------|---------|-------------|
| bycrawl-docs | `/bycrawl-docs [section]` | Fetch latest ByCrawl API docs |
| bycrawl-fb-marketplace | `/bycrawl-fb-marketplace [keyword]` | Search deals on FB Marketplace |
| bycrawl-brand-monitoring | `/bycrawl-brand-monitoring` | Track brand mentions across 7 platforms |
| bycrawl-competitor-intel | `/bycrawl-competitor-intel` | Full competitor social + hiring audit |
| bycrawl-influencer-discovery | `/bycrawl-influencer-discovery` | Find and vet creators in a niche |
| bycrawl-content-research | `/bycrawl-content-research` | Mine content gaps and video ideas |
| bycrawl-lead-intel | `/bycrawl-lead-intel` | Find buying signals or research a prospect |
| bycrawl-crisis-detection | `/bycrawl-crisis-detection` | Detect negative sentiment spikes |
| bycrawl-market-research | `/bycrawl-market-research` | Validate product-market fit from social data |
| bycrawl-talent-sourcing | `/bycrawl-talent-sourcing` | Source candidates or check employer brand |
| bycrawl-prospect-dossier | `/bycrawl-prospect-dossier` | Build a full profile for meeting prep |
| bycrawl-viral-reverse-engineer | `/bycrawl-viral-reverse-engineer` | Deconstruct why content went viral |
| bycrawl-company-sourcing | `/bycrawl-company-sourcing` | Find acquisition targets matching a thesis |
| bycrawl-acquisition-target-qualify | `/bycrawl-acquisition-target-qualify` | Deep qualification of a company for M&A |
| bycrawl-deal-outreach-personalize | `/bycrawl-deal-outreach-personalize` | Personalized M&A outreach to business owners |
| bycrawl-audience-enrichment | `/bycrawl-audience-enrichment` | Enrich ad audiences with social behavior signals |
| bycrawl-social-keyword-signals | `/bycrawl-social-keyword-signals` | Surface trending keywords before they hit search tools |
| bycrawl-copy-intelligence | `/bycrawl-copy-intelligence` | Mine social for copy angles and audience language |
| bycrawl-bag-product-trendy-research | `/bycrawl-bag-product-trendy-research` | McKinsey-grade product trend intelligence report |
| bycrawl-trend-visual-evidence | `/bycrawl-trend-visual-evidence` | Google Trends graphs & retail screenshots for trend decks |
| bycrawl-neighborhood-intel | `/bycrawl-neighborhood-intel` | Rental/lease sourcing + neighborhood intelligence from social data |

## Skill Chains — Recommended Workflows

Skills are modular — use them standalone or chain them together for deeper workflows. Below are recommended chains by role.

### For Product Managers & Designers

> "I need to know what to build next season."

**Commands** (run in order):
```bash
/bycrawl-bag-product-trendy-research backpacks, Gen Z commuters
/bycrawl-trend-visual-evidence backpacks, gorpcore vs quiet luxury
/bycrawl-market-research backpacks, college students
```

| Step | Skill | What it adds |
|------|-------|-------------|
| 1 | `/bycrawl-bag-product-trendy-research` | Inclining/declining styles, feature demand matrix, price architecture |
| 2 | `/bycrawl-trend-visual-evidence` | Google Trends graphs and retail screenshots as visual proof |
| 3 | `/bycrawl-market-research` | Product-market fit validation with demand signals and willingness to pay |

---

### For Sales Teams & Retail Buyers

> "I need a trend-backed pitch deck for my buyer meeting."

**Commands** (run in order):
```bash
/bycrawl-bag-product-trendy-research tote bags, office workers
/bycrawl-trend-visual-evidence tote bags, quiet luxury vs preppy
/bycrawl-competitor-intel Herschel
```

| Step | Skill | What it adds |
|------|-------|-------------|
| 1 | `/bycrawl-bag-product-trendy-research` | Data-driven trend report with style lifecycle tracker |
| 2 | `/bycrawl-trend-visual-evidence` | Google Trends charts and competitor product page screenshots |
| 3 | `/bycrawl-competitor-intel` | Deep dive on a specific competitor's social presence and hiring signals |

---

### For Brand & Marketing Teams

> "I need to understand our market position and plan content."

**Commands** (run in order):
```bash
/bycrawl-brand-monitoring [your brand]
/bycrawl-competitor-intel [competitor name]
/bycrawl-content-research [your category]
/bycrawl-copy-intelligence [your category]
```

| Step | Skill | What it adds |
|------|-------|-------------|
| 1 | `/bycrawl-brand-monitoring` | Track your brand mentions and sentiment across 7 platforms |
| 2 | `/bycrawl-competitor-intel` | Audit competitor social + hiring activity |
| 3 | `/bycrawl-content-research` | Find content gaps and trending topics to create around |
| 4 | `/bycrawl-copy-intelligence` | Mine audience language and copy angles from social |

---

### For Influencer Marketing

> "I need to find creators and understand what content works."

**Commands** (run in order):
```bash
/bycrawl-influencer-discovery [niche]
/bycrawl-viral-reverse-engineer [viral post URL]
/bycrawl-audience-enrichment [target segment]
```

| Step | Skill | What it adds |
|------|-------|-------------|
| 1 | `/bycrawl-influencer-discovery` | Find and vet influencers across TikTok, Instagram, YouTube |
| 2 | `/bycrawl-viral-reverse-engineer` | Deconstruct why specific content went viral |
| 3 | `/bycrawl-audience-enrichment` | Enrich audience targeting with real social behavior data |

---

### For M&A / Business Development

> "I'm sourcing and qualifying acquisition targets."

**Commands** (run in order):
```bash
/bycrawl-company-sourcing [acquisition criteria]
/bycrawl-acquisition-target-qualify [company name]
/bycrawl-deal-outreach-personalize [owner name]
/bycrawl-prospect-dossier [person or company]
```

| Step | Skill | What it adds |
|------|-------|-------------|
| 1 | `/bycrawl-company-sourcing` | Find companies matching your acquisition thesis |
| 2 | `/bycrawl-acquisition-target-qualify` | Deep qualification with social signals |
| 3 | `/bycrawl-deal-outreach-personalize` | Personalized outreach messages based on their social presence |
| 4 | `/bycrawl-prospect-dossier` | Full cross-platform profile for meeting prep |

---

### For Sales Prospecting

> "I need to research a lead before reaching out."

**Commands** (run in order):
```bash
/bycrawl-lead-intel [prospect name or company]
/bycrawl-prospect-dossier [person name]
/bycrawl-social-keyword-signals [industry topic]
```

| Step | Skill | What it adds |
|------|-------|-------------|
| 1 | `/bycrawl-lead-intel` | Find buying signals and research the prospect |
| 2 | `/bycrawl-prospect-dossier` | Build a complete cross-platform profile |
| 3 | `/bycrawl-social-keyword-signals` | Surface trending keywords the prospect's industry cares about |

---

### For Crisis & Reputation Management

> "Something's going wrong — I need to understand the situation fast."

**Commands** (run in order):
```bash
/bycrawl-crisis-detection [brand name]
/bycrawl-brand-monitoring [brand name]
```

| Step | Skill | What it adds |
|------|-------|-------------|
| 1 | `/bycrawl-crisis-detection` | Detect negative sentiment spikes in real time |
| 2 | `/bycrawl-brand-monitoring` | Ongoing monitoring across all platforms |

---

### For Real Estate & PropTech (Alleyguide-style)

> "I need to find available rentals AND know what it's actually like to live there."

**Commands** (run in order):
```bash
/bycrawl-neighborhood-intel Da'an District, Taipei
/bycrawl-content-research Da'an District living experience
/bycrawl-brand-monitoring [residential complex or developer name]
```

| Step | Skill | What it adds |
|------|-------|-------------|
| 1 | `/bycrawl-neighborhood-intel` | Sources rental listings from FB Marketplace/Groups/Dcard + full neighborhood intelligence (sentiment, hidden gems, commute, safety, trajectory) |
| 2 | `/bycrawl-content-research` | Content gaps about the area — what questions aren't being answered online |
| 3 | `/bycrawl-brand-monitoring` | Track sentiment for a specific residential complex or property developer |

**Use cases:**

| Use Case | Command | Output |
|----------|---------|--------|
| "Find rentals in this area" | `/bycrawl-neighborhood-intel Da'an, Taipei` | Off-platform rental listings from FB Marketplace + Groups + Dcard with pricing intel |
| "What's the vibe here?" | `/bycrawl-neighborhood-intel Xinyi, Taipei` | Resident sentiment score + community voice summary |
| "Hidden gems near this listing" | `/bycrawl-neighborhood-intel Yongkang Street, Taipei` | Trending cafes, restaurants, parks from IG + Threads |
| "Is the commute actually doable?" | `/bycrawl-neighborhood-intel Banqiao, New Taipei` | Commute reality check with resident quotes |
| "Is this area up-and-coming?" | `/bycrawl-neighborhood-intel Nangang, Taipei` | Neighborhood trajectory — rising, established, or declining |
| "Is this complex well-managed?" | `/bycrawl-neighborhood-intel [complex name], Taipei` | Resident reviews + landlord reputation from Dcard + FB groups |
| "What do renters want here?" | `/bycrawl-neighborhood-intel Zhongshan, Taipei` | Demand heatmap — what people search for but can't find |

## Installation

### Via Plugin Marketplace (recommended)

```shell
/plugin marketplace add Signalsurf-ai/bycrawl-skills
/plugin install bycrawl@bycrawl-skills
```

### Via npx skills

```bash
npx skills add Signalsurf-ai/bycrawl-skills
```

## Updating

### Via Plugin Marketplace

```shell
/plugin marketplace update bycrawl@bycrawl-skills
```

### Via npx skills

```bash
npx skills update Signalsurf-ai/bycrawl-skills
```

### Manual update

If you installed by cloning the repo, pull the latest changes:

```bash
cd ~/.claude/skills/bycrawl-skills  # or wherever you cloned
git pull origin main
```

After updating, restart Claude Code to pick up the new skills.

## MCP Setup

### ByCrawl MCP (required for all skills)

```bash
claude mcp add bycrawl -- npx -y @bycrawl/mcp
```

Or add to your `.claude/settings.json`:

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

Get your API key at [bycrawl.com](https://bycrawl.com).

### Playwright MCP (optional — for visual evidence skill)

Required only for `/bycrawl-trend-visual-evidence`. Captures Google Trends graphs and retail site screenshots.

```bash
claude mcp add playwright -- npx -y @anthropic-ai/mcp-playwright
```

## Usage

```
/bycrawl-docs threads
/bycrawl-docs python-sdk
/bycrawl-docs getting-started
/bycrawl-docs error-codes

/bycrawl-fb-marketplace iPhone 15
/bycrawl-fb-marketplace IKEA desk
/bycrawl-fb-marketplace Toyota Corolla

/bycrawl-brand-monitoring
/bycrawl-competitor-intel
/bycrawl-influencer-discovery
/bycrawl-content-research
/bycrawl-lead-intel
/bycrawl-crisis-detection
/bycrawl-market-research
/bycrawl-talent-sourcing
/bycrawl-prospect-dossier
/bycrawl-viral-reverse-engineer

/bycrawl-company-sourcing
/bycrawl-acquisition-target-qualify
/bycrawl-deal-outreach-personalize

/bycrawl-audience-enrichment
/bycrawl-social-keyword-signals
/bycrawl-copy-intelligence

/bycrawl-bag-product-trendy-research backpacks, Gen Z
/bycrawl-trend-visual-evidence backpacks, gorpcore vs quiet luxury

/bycrawl-neighborhood-intel Da'an District, Taipei
/bycrawl-neighborhood-intel Yongkang Street, Taipei, young couple
/bycrawl-neighborhood-intel Banqiao, New Taipei, family with kids
```

The skill auto-triggers when Claude detects you're working with the ByCrawl API.

## Project Structure

```
bycrawl-skills/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
└── skills/
    ├── bycrawl-docs/
    │   └── SKILL.md
    ├── bycrawl-fb-marketplace/
    │   └── SKILL.md
    ├── bycrawl-brand-monitoring/
    │   └── SKILL.md
    ├── bycrawl-competitor-intel/
    │   └── SKILL.md
    ├── bycrawl-content-research/
    │   └── SKILL.md
    ├── bycrawl-crisis-detection/
    │   └── SKILL.md
    ├── bycrawl-influencer-discovery/
    │   └── SKILL.md
    ├── bycrawl-lead-intel/
    │   └── SKILL.md
    ├── bycrawl-market-research/
    │   └── SKILL.md
    ├── bycrawl-prospect-dossier/
    │   └── SKILL.md
    ├── bycrawl-talent-sourcing/
    │   └── SKILL.md
    ├── bycrawl-viral-reverse-engineer/
    │   └── SKILL.md
    ├── bycrawl-company-sourcing/
    │   └── SKILL.md
    ├── bycrawl-acquisition-target-qualify/
    │   └── SKILL.md
    ├── bycrawl-deal-outreach-personalize/
    │   └── SKILL.md
    ├── bycrawl-audience-enrichment/
    │   └── SKILL.md
    ├── bycrawl-social-keyword-signals/
    │   └── SKILL.md
    ├── bycrawl-copy-intelligence/
    │   └── SKILL.md
    ├── bycrawl-bag-product-trendy-research/
    │   └── SKILL.md
    ├── bycrawl-trend-visual-evidence/
    │   └── SKILL.md
    └── bycrawl-neighborhood-intel/
        └── SKILL.md
```
