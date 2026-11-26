# Made-in-China.com Product Scraper: export to CSV or JSON with Python, Node.js or curl

Made in China scraper for Made-in-China.com product search pages by keyword. Returns product name, category, USD price range, supplier name, stock status, image, and URL. About 20 products per page; supports multiple keywords and pages.

This repository has ready scripts for the [Made-in-China.com Product Scraper](https://apify.com/v0iddo/made-in-china-scraper) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv --max-items 50
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/made-in-china-scraper), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "keywords": [
    "LED Light",
    "Solar Panel"
  ],
  "maxPages": 1,
  "maxItems": 20
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `keywords` | array | Product keywords to search on Made-in-China.com, e.g. |
| `maxPages` | integer | Number of result pages to read per keyword (about 20 products each), up to 100. |
| `maxItems` | integer | The run stops after this many result rows. |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "keyword": "LED Light",
  "page": 1,
  "position": 1,
  "name": "Romanso Outdoor Flood Lights LED ETL Dlc IP65 Waterproof Solar Flood Light 100W 150W 200W High Power 50W LED Flood Lighting Outdoor",
  "category": "Solar Light",
  "lowPriceUsd": 31,
  "highPriceUsd": 29,
  "supplierName": "Shenzhen Romanso Electronic Co., Ltd.",
  "inStock": true,
  "image": "https://image.made-in-china.com/43f34j00zROKwWfsluoF/Romanso-Outdoor-Flood-Lights-LED-ETL-Dlc-IP65-Waterproof-Solar-Flood-Light-100W-150W-200W-High-Power-50W-LED-Flood-Lighting-Outdoor.jpg",
  "url": "https://www.made-in-china.com/products-search/hot-china-products/LED_Light.html#product-1"
}
```

Fields: `keyword`, `page`, `position`, `name`, `category`, `lowPriceUsd`, `highPriceUsd`, `supplierName`, `inStock`, `image`, `url`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/made-in-china-scraper').call({"keywords": ["LED Light", "Solar Panel"], "maxPages": 1, "maxItems": 20});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~made-in-china-scraper/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"keywords": ["LED Light", "Solar Panel"], "maxPages": 1, "maxItems": 20}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$1.44 per 1,000 results**, nothing per run. `maxItems` caps the rows, and so the cost, of one run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
