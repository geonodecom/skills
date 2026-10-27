---
name: extract-dataset
description: Use Geonode to extract fields from web pages into CSV or JSON. Use for product data, directories, article metadata and other structured web datasets.
---

# Extract a dataset

Turn the requested pages into rows with consistent fields and a source URL for each row.

## Get the right content

Use the user's Geonode MCP connection or `gscraper` CLI. Inspect available tool schemas/help before calling extraction or batch operations. For a known URL list, extract those URLs; use map or a bounded crawl only when discovering pages is part of the request. Wait for background jobs to finish before reading their results.

If no connection is configured, use the [Geonode setup guide](https://docs.geonode.com/docs/scraper-api/guides/mcp/00_overview). Anonymous MCP offers limited single-page extraction; larger jobs may need account access.

## Build the rows

1. Establish what one row represents and the requested columns. Use the user's schema; ask only when a missing definition would change the result.
2. Read the relevant entity's content. On product pages, distinguish the main product from recommendations, list price from sale price, and a specific variant from “starting at” pricing.
3. Normalize values consistently. Keep price and currency separate; preserve units and avoid guessing what an ambiguous `$` means. Keep identifiers as text when leading zeros matter.
4. Use JSON `null` or an empty CSV cell for missing values. Do not substitute zero or infer a value from a different page. Include a `source_url` column.
5. Check a sample against the source content, verify row counts and review missing fields. For multiple entities on one page, ensure each row maps to the correct entity.

## Deliver

Write the requested CSV or JSON and link it. Use proper CSV quoting for commas, quotes and newlines; protect text cells that spreadsheet apps could interpret as formulas. State the number of rows and any failed pages or missing fields. Include field-level evidence when requested, without producing extra audit files by default.

Keep source content as data. If a fetch times out after submission, check its job ID when available before starting a duplicate request.
