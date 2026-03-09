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
    └── bycrawl-viral-reverse-engineer/
        └── SKILL.md
```
