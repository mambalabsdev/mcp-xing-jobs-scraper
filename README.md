# XING Jobs Scraper MCP Server

[![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-xing-jobs-scraper%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-xing-jobs-scraper&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-xing-jobs-scraper)](https://www.npmjs.com/package/@mambalabsdev/mcp-xing-jobs-scraper) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-xing-jobs-scraper)](https://www.npmjs.com/package/@mambalabsdev/mcp-xing-jobs-scraper) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-xing-jobs-scraper)](https://github.com/mambalabsdev/mcp-xing-jobs-scraper/blob/main/LICENSE)

An MCP server for the Mamba Labs XING Jobs Scraper actor on Apify. XING jobs as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Takes keywords and locations, or XING URLs, and returns one row per job with title, company, city, employment type, salary, apply link, and dates. Job details add posting date, description, career level, discipline, industry, and remote options.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-xing-jobs-scraper": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-xing-jobs-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `search_xing_jobs` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Find Python developer jobs in Berlin on XING."
- "List sales jobs in Munich on XING with salaries and apply links."

## Tool

`search_xing_jobs`. One tool. Example arguments:

```json
{
  "keywords": [
    "python"
  ],
  "locations": [
    "Berlin"
  ],
  "max_jobs_per_search": 20
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `keywords` | array | no | Job title, skill, or field, for example python or vertrieb. One search per keyword per location. XING's search is semantic, so it also returns related jobs; the keyword_in_title field marks exact title matches. |
| `locations` | array | no | City names, for example Berlin, München, Wien, or Zürich. XING does not recognize country names or postal codes. Leave empty to search all of XING. |
| `start_urls` | array | no | Optional. XING job search pages (https://www.xing.com/jobs/search?keywords=...) or job pages (https://www.xing.com/jobs/<city>-<title>-<id>). A job page always returns a job detail row. |
| `max_jobs_per_search` | number | no | Jobs to read for each keyword and location, or each search URL. XING lists 20 per page and counts up to 1,000 per search. Default 20, maximum 1000. |
| `include_details` | boolean | no | yes reads each job page and adds posting date, description, postal code, career level, discipline, industry, and remote options, charged as a job detail row instead of a job row. Default no. |
| `dedupe` | boolean | no | yes (default) returns each job once per run, so a job found by two searches is charged once. no returns it again on each search, charged each time. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `job-row`, one job from a search page | $0.001 | $0.00095 | $0.0009 | $0.00085 |
| `job-detail-row`, one job with its job page read | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `apify-actor-start`, per run start | $0.005 | $0.005 | $0.005 | $0.005 |

Repeats, empty searches, summaries, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/xing-jobs-scraper](https://apify.com/mambalabs/xing-jobs-scraper)

## License

MIT. See [LICENSE](LICENSE).
