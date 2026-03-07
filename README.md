# bycrawl-skills

Claude Code skill for fetching the latest ByCrawl API documentation from [bycrawl.com](https://bycrawl.com).

## Skill

| Skill | Command | Description |
|-------|---------|-------------|
| bycrawl-docs | `/bycrawl-docs [section]` | Fetch latest ByCrawl API docs |

## Installation

```bash
npx skills add -g Signalsurf-ai/bycrawl-skills
```

## Usage

```
/bycrawl-docs threads
/bycrawl-docs python-sdk
/bycrawl-docs getting-started
/bycrawl-docs error-codes
```

The skill auto-triggers when Claude detects you're working with the ByCrawl API.

## Project Structure

```
bycrawl-skills/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
└── skills/
    └── bycrawl-docs/
        └── SKILL.md
```
