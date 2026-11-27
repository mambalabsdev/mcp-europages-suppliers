# Europages Suppliers MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-europages-suppliers/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-europages-suppliers) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-europages-suppliers%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-europages-suppliers&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-europages-suppliers)](https://www.npmjs.com/package/@mambalabsdev/mcp-europages-suppliers) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-europages-suppliers)](https://www.npmjs.com/package/@mambalabsdev/mcp-europages-suppliers) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-europages-suppliers)](https://github.com/mambalabsdev/mcp-europages-suppliers/blob/main/LICENSE)

An MCP server for the Mamba Labs Europages Suppliers actor on Apify. Europages suppliers as rows.

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

Takes product keywords and countries, or Europages URLs, and returns one row per supplier with name, address, website, employee band, founding year, business type, and description. Company details add phone, VAT ID, product keywords, and coordinates.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-europages-suppliers": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-europages-suppliers"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `find_europages_suppliers` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Find packaging manufacturers in Germany on Europages."
- "List furniture suppliers in Italy with their websites and phone numbers."

## Tool

`find_europages_suppliers`. One tool. Example arguments:

```json
{
  "keywords": [
    "packaging"
  ],
  "countries": [
    "germany"
  ],
  "max_suppliers_per_search": 30
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `keywords` | array | no | What the suppliers make or sell, for example packaging or olive oil. One search per keyword per country. |
| `countries` | array | no | Country names in English as Europages writes them in its URLs, for example germany, italy, united kingdom. Leave empty to search without a country filter; europages.co.uk then lists United Kingdom suppliers. |
| `start_urls` | array | no | Optional. Europages search pages (https://www.europages.co.uk/companies/...html) or company pages. A company page always returns a company details row. |
| `max_suppliers_per_search` | number | no | Suppliers to read for each keyword and country, or each search URL. Europages lists 30 per page. Default 30, maximum 900. |
| `include_details` | boolean | no | yes reads each company page and adds phone, VAT ID, product keywords, and coordinates, charged as a supplier detail row instead of a supplier row. Default no. |
| `dedupe` | boolean | no | yes (default) returns each company once per run, so a company found by two searches is charged once. no returns it again on each search, charged each time. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `supplier-row`, one supplier from a search page | $0.004 | $0.0038 | $0.0036 | $0.0034 |
| `supplier-detail-row`, one supplier with its company page read | $0.006 | $0.0057 | $0.0054 | $0.0051 |
| `apify-actor-start`, per run start | $0.004 | $0.004 | $0.004 | $0.004 |

Repeats, empty searches, summaries, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/europages-suppliers](https://apify.com/mambalabs/europages-suppliers)

## License

MIT. See [LICENSE](LICENSE).
