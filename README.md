# How to Scrape Alibaba Listings in Node.js

This example shows how to scrape Alibaba product listings in Node.js using the [Alibaba Listings Scraper](https://apify.com/piotrv1001/alibaba-listings-scraper) actor on Apify. Instead of building a scraper from scratch, you call a ready-made actor via the Apify API client — no browser automation or HTML parsing required.

## What this example does

- Calls the Alibaba Listings Scraper actor with a search query and result limit
- Waits for the actor run to complete
- Fetches the results from Apify's dataset storage
- Prints each listing to the console

## Prerequisites

- [Node.js](https://nodejs.org/) v18 or higher
- An [Apify account](https://console.apify.com/sign-up) (free tier available)
- An [Apify API token](https://console.apify.com/settings/integrations)

## Installation

```bash
npm install
```

## Environment setup

Copy `.env.example` to `.env` and add your Apify API token:

```bash
cp .env.example .env
```

Then open `.env` and replace `your_apify_token_here` with your actual token from [Apify Console → Settings → Integrations](https://console.apify.com/settings/integrations).

## Usage

```bash
npm start
```

## Code example

```js
import { ApifyClient } from 'apify-client';
import 'dotenv/config';

// Initialize the ApifyClient with your Apify API token
// Set APIFY_TOKEN in your .env file (copy .env.example to get started)
const client = new ApifyClient({
    token: process.env.APIFY_TOKEN,
});

// Prepare Actor input
const input = {
    "search": "samsung galaxy",
    "limit": 60
};

// Run the Actor and wait for it to finish
const run = await client.actor("piotrv1001/alibaba-listings-scraper").call(input);

// Fetch and print Actor results from the run's dataset (if any)
console.log('Results from dataset');
console.log(`💾 Check your data here: https://console.apify.com/storage/datasets/${run.defaultDatasetId}`);
const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach((item) => {
    console.dir(item);
});

// 📚 Want to learn more 📖? Go to → https://docs.apify.com/api/client/js/docs
```

## Example output

See [`sample-output.json`](./sample-output.json) for a full example. Each listing object contains the following fields:

| Field | Description |
|---|---|
| `title` | Product title |
| `price` | Listed price (may be a range) |
| `promotionPrice` | Discounted price, if available |
| `discount` | Discount percentage, if available |
| `moq` | Minimum order quantity |
| `companyName` | Name of the supplier company |
| `countryCode` | Supplier country code (e.g. `CN`) |
| `productUrl` | Direct link to the product page |
| `mainImage` | URL of the product's main image |
| `reviewScore` | Supplier review score |
| `reviewCount` | Number of reviews |
| `deliveryEstimate` | Estimated delivery timeframe |

## Use cases

- **Product sourcing** — search for suppliers of specific products and compare prices and MOQs
- **Price monitoring** — track price fluctuations and promotional discounts over time
- **Market research** — analyze which suppliers dominate a category and what price ranges are typical
- **Lead generation** — collect supplier contact info for B2B outreach
- **Competitor analysis** — discover what products competitors may be sourcing and at what cost

## Try the actor on Apify

**[Open the Alibaba Listings Scraper on Apify](https://apify.com/piotrv1001/alibaba-listings-scraper)**

## License

MIT
