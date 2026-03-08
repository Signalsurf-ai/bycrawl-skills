# bycrawl-skills

Claude Code skill for fetching the latest ByCrawl API documentation from [bycrawl.com](https://bycrawl.com).

## Skills

| Skill | Command | Description |
|-------|---------|-------------|
| bycrawl-docs | `/bycrawl-docs [section]` | Fetch latest ByCrawl API docs |
| bycrawl-fb-marketplace | `/bycrawl-fb-marketplace [keyword]` | Search deals on FB Marketplace |

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
    └── bycrawl-fb-marketplace/
        └── SKILL.md
```
