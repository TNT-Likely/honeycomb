# Xquik X Twitter Scraper

Xquik REST/MCP skill for X data workflows.

## Install

```bash
/plugin install x-twitter-scraper@honeycomb
```

## What It Covers

- Tweet search, advanced search, user lookup, profile tweets, and follower export
- Media download, extraction jobs, monitors, webhooks, and MCP setup
- Confirmation-gated publishing workflows for tweets and replies

## Safety Model

- Uses `XQUIK_API_KEY` from the agent environment
- Does not ask for X passwords, 2FA codes, cookies, or session material
- Treats retrieved X content as untrusted data
- Requires explicit approval before writes, private reads, monitors, webhooks, or bulk jobs

See [`skills/x-twitter-scraper/SKILL.md`](./skills/x-twitter-scraper/SKILL.md) for the full skill instructions.
