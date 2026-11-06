<p align="center">
  <a href="https://www.piloterr.com"><img src="images/banner.png" alt="Darty Scraper API by Piloterr – scrape Darty data as JSON" width="100%"></a>
</p>

# Darty Scraper API

[![Discord](https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white)](https://discord.gg/CFj3ph8aHU) [![Get API key](https://img.shields.io/badge/Piloterr-Get%20a%20free%20API%20key-111827)](https://app.piloterr.com) [![API docs](https://img.shields.io/badge/Docs-OpenAPI-0EA5E9)](https://app.piloterr.com/openapi) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

The **Piloterr Darty Scraper API** lets you extract public data from Darty as **clean, structured JSON** — no proxies, headless browsers or HTML parsing to maintain. Endpoints available in production:

- **Darty Product API** — Read one Darty product sheet from a product URL or a Darty product id.
- **Darty Search API** — Search Darty by keyword or search URL and page through the listing.

Use it for price monitoring, assortment tracking, competitor analysis, product research or catalog enrichment.

| Endpoint | Path | Credits | What you get |
|---|---|---|---|
| [Darty Product API](#darty-product-api) | `/v2/darty/product` | 2 | Read one Darty product sheet from a product URL or a Darty product id. You get price, brand, availability, rating, and specifications. |
| [Darty Search API](#darty-search-api) | `/v2/darty/search` | 2 | Search Darty by keyword or search URL and page through the listing. Each card has title, price, brand, seller, and product link. |

## Table of contents

- [How it works](#how-it-works)
- [Darty Product API](#darty-product-api)
- [Darty Search API](#darty-search-api)
- [Code examples in other languages](#code-examples-in-other-languages)
- [Practical use cases](#practical-use-cases)
- [Why Piloterr?](#why-piloterr)
- [FAQ](#faq)
- [Official SDKs](#official-sdks)
- [Related scrapers](#related-scrapers)
- [Support](#support)

## How it works

1. **Create a free Piloterr account** and copy your API key from the [dashboard](https://app.piloterr.com).
2. **Send a `GET` (or `POST`) request** to `https://api.piloterr.com/v2/...` with the header `x-api-key: YOUR_API_KEY`.
3. **Receive structured JSON** — Piloterr handles rendering, proxies, retries and parsing.

```bash
export PILOTERR_API_KEY="your-api-key"
pip install requests        # Python
# TypeScript: Node.js 18+ (native fetch), run with `npx tsx file.ts`
```

> Every endpoint accepts both `GET` (query string) and `POST` (JSON body with the same fields).

## Darty Product API

Read one Darty product sheet from a product URL or a Darty product id. You get price, brand, availability, rating, and specifications.

`GET | POST https://api.piloterr.com/v2/darty/product` · **2 credits per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | Product URL on www.darty.com. The path is under `/nav/achat/` and ends in `.html`. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/darty/product",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "https://www.darty.com/nav/achat/petit_electromenager/aspirateur-balai_main/aspirateur_balai/dyson_piston_animal.html",
    },
    timeout=120,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type DartyProduct = {
  url?: string;
  gtin?: string;
  name?: string;
  brand?: string;
  codic?: string;
  image?: string;
  price?: number;
  images?: string[];
  rating?: number;
  seller?: string;
  category?: string;
  currency?: string;
  delivery?: string;
  condition?: string;
  reference?: string;
  description?: string;
  availability?: string;
  review_count?: number;
  price_display?: string;
  rating_display?: string;
  specifications?: {
    name?: string;
    value?: string;
  }[];
};

const params = new URLSearchParams({
  "query": "https://www.darty.com/nav/achat/petit_electromenager/aspirateur-balai_main/aspirateur_balai/dyson_piston_animal.html",
});

const response = await fetch(`https://api.piloterr.com/v2/darty/product?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as DartyProduct;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/darty/product
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "https://www.darty.com/nav/achat/petit_electromenager/aspirateur-balai_main/aspirateur_balai/dyson_piston_animal.html"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
{
  "url": "https://www.darty.com/nav/achat/petit_electromenager/aspirateur-balai_main/aspirateur_balai/dyson_piston_animal.html",
  "gtin": "5025155114834",
  "name": "Aspirateur balai Dyson V16 Piston Animal",
  "brand": "Dyson",
  "codic": "8069573",
  "image": "https://image.darty.com/darty?type=image&source=/market/2025/09/04/28613358_3723_1.jpg&height=227",
  "price": 849,
  "images": [
    "https://image.darty.com/darty?type=image&source=/market/2025/09/04/28613358_3723_1.jpg&height=227",
    "https://image.darty.com/darty?type=image&source=/market/2025/09/04/28613358_3724_1.jpg"
  ],
  "rating": 3.9,
  "seller": "example",
  "category": "Aspirateur balai",
  "currency": "EUR",
  "delivery": "Livré dès le 31/10",
  "condition": "Neuf",
  "reference": "V16 Piston Animal",
  "description": "Le plus puissant des aspirateurs sans fil Dyson : le V16 Piston Animal Nouvelle brosse adaptative auto-démêlante All Floor Cones Sense : …",
  "availability": "En stock dans plus de 20 magasins",
  "review_count": 53,
  "price_display": "849,00 €",
  "rating_display": "3,9/5",
  "specifications": [
    {
      "name": "Pays de fabrication",
      "value": "Malaisie"
    },
    {
      "name": "Origine de fabrication",
      "value": "Fabriqué hors Europe"
    }
  ]
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `url` | string |  |
| `gtin` | string |  |
| `name` | string |  |
| `brand` | string |  |
| `codic` | string |  |
| `image` | string |  |
| `price` | integer |  |
| `images` | string[] |  |
| `rating` | number |  |
| `seller` | string |  |
| `category` | string |  |
| `currency` | string |  |
| `delivery` | string |  |
| `condition` | string |  |
| `reference` | string |  |
| `description` | string |  |
| `availability` | string |  |
| `review_count` | integer |  |
| `price_display` | string |  |
| `rating_display` | string |  |
| `specifications[]` | object[] |  |
| `specifications[].name` | string |  |
| `specifications[].value` | string |  |

## Darty Search API

Search Darty by keyword or search URL and page through the listing. Each card has title, price, brand, seller, and product link.

`GET | POST https://api.piloterr.com/v2/darty/search` · **2 credits per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | Keyword or a Darty search URL on www.darty.com. |
| `page` | number | — | 1-based page number. Defaults to `1`. Page 2 returns the next 30 products. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/darty/search",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "aspirateur",
    },
    timeout=120,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type DartySearch = {
  results?: {
    url?: string;
    name?: string;
    badge?: string;
    brand?: string;
    codic?: string;
    image?: string;
    price?: number;
    pickup?: string;
    rating?: number;
    seller?: string;
    category?: string;
    currency?: string;
    delivery?: string;
    position?: number;
    reference?: string;
    was_price?: number;
    shipped_by?: unknown;
    review_count?: number;
    price_display?: string;
    rating_display?: string;
    discount_display?: string;
    reparability_index?: number;
  }[];
  pagination?: {
    next?: string;
    page?: number;
    per_page?: number;
    total_pages?: number;
    total_results?: number;
  };
};

const params = new URLSearchParams({
  "query": "aspirateur",
});

const response = await fetch(`https://api.piloterr.com/v2/darty/search?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as DartySearch;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/darty/search
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "aspirateur"}
```
</details>

### Output example

Example response from the API reference, abridged — real values depend on the page you scrape.

```json
{
  "results": [
    {
      "url": "https://www.darty.com/nav/achat/petit_electromenager/aspirateur-avec_sac_sans_sac/aspirateur_sans_sac/rowenta_ro4b36.html",
      "name": "Aspirateur sans sac Rowenta COMPACT POWER XXL Modele Parquet RO4B36EA",
      "badge": "Bon Plan 21%",
      "brand": "Rowenta",
      "codic": "7522991",
      "image": "https://image.darty.com/server?type=image&source=photos%2F2024%2F04%2F20%2F7522991A_211747457.jpg&width=230&height=345",
      "price": 119.99,
      "pickup": "Retrait dès le 05/11",
      "rating": 4.4,
      "seller": "example",
      "category": "Aspirateur sans sac",
      "currency": "EUR",
      "delivery": "Livré dès le 05/11",
      "position": 1,
      "reference": "COMPACT POWER XXL Modele Parquet RO4B36EA",
      "was_price": 151.99,
      "shipped_by": null,
      "review_count": 461,
      "price_display": "119,99 €",
      "rating_display": "4,4/5",
      "discount_display": "21%",
      "reparability_index": 9.8
    }
  ],
  "pagination": {
    "next": "https://www.darty.com/nav/recherche?s=relevence&text=aspirateur&o=30",
    "page": 1,
    "per_page": 30,
    "total_pages": 300,
    "total_results": 17644
  }
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `results[]` | object[] |  |
| `results[].url` | string |  |
| `results[].name` | string |  |
| `results[].badge` | string |  |
| `results[].brand` | string |  |
| `results[].codic` | string |  |
| `results[].image` | string |  |
| `results[].price` | number |  |
| `results[].pickup` | string |  |
| `results[].rating` | number |  |
| `results[].seller` | string |  |
| `results[].category` | string |  |
| `results[].currency` | string |  |
| `results[].delivery` | string |  |
| `results[].position` | integer |  |
| `results[].reference` | string |  |
| `results[].was_price` | number |  |
| `results[].shipped_by` | object |  |
| `results[].review_count` | integer |  |
| `results[].price_display` | string |  |
| `results[].rating_display` | string |  |
| `results[].discount_display` | string |  |
| `results[].reparability_index` | number |  |
| `pagination` | object |  |
| `pagination.next` | string |  |

## Code examples in other languages

Ready-to-run examples live in the [`examples/`](examples) folder. Each one reads your key from the `PILOTERR_API_KEY` environment variable.

| Language | Product | Search |
|---|---|---|
| cURL (Bash) | [`product`](examples/curl/product.sh) | [`search`](examples/curl/search.sh) |
| PHP | [`product`](examples/php/product.php) | [`search`](examples/php/search.php) |
| C# | [`product`](examples/csharp/Product.cs) | [`search`](examples/csharp/Search.cs) |
| Go | [`product`](examples/go/product.go) | [`search`](examples/go/search.go) |
| Java | [`product`](examples/java/ProductExample.java) | [`search`](examples/java/SearchExample.java) |
| Ruby | [`product`](examples/ruby/product.rb) | [`search`](examples/ruby/search.rb) |
| Node.js | [`product`](examples/nodejs/product.mjs) | [`search`](examples/nodejs/search.mjs) |

## Practical use cases

- **Price monitoring** — Track prices, promotions and availability over time to react to competitors.
- **Assortment & catalog tracking** — Follow new products, delisted items and category changes.
- **Competitor analysis** — Benchmark ranges, brands and positioning across retailers.
- **Product research** — Spot best-sellers and trends from search results.
- **Catalog enrichment** — Pull titles, images and attributes into your PIM or marketplace listings.

## Why Piloterr?

- **No infrastructure** — no proxies, headless browsers or CAPTCHA solving to run yourself.
- **Structured JSON** — stable, documented fields instead of brittle HTML selectors.
- **Pay per request** — transparent credit cost per endpoint, failed requests are retried for you.
- **One API key, hundreds of sources** — Darty plus many other e-commerce, search and company data endpoints in the [Piloterr library](https://www.piloterr.com/library).
- **Any language** — plain HTTPS + JSON, with examples for Python, TypeScript, cURL, PHP, C#, Go, Java, Ruby and Node.js.

## FAQ

### Is it legal to scrape Darty?

Collecting publicly available data is generally allowed in many jurisdictions, but rules depend on your country, the data you collect and how you use it. Avoid personal data unless you have a lawful basis, respect the website's terms where they apply, and get legal advice for your specific use case. This is not legal advice.

### Do I need proxies or a headless browser?

No. Piloterr handles rendering, proxy rotation and retries on its side. You only send an HTTPS request with your `x-api-key` header and receive JSON.

### How much does a request cost?

Credits per call: `/v2/darty/product` **2**, `/v2/darty/search` **2**. See the [Piloterr website](https://www.piloterr.com) for plans.

### What data does the Darty API return?

Each endpoint returns structured JSON: **Darty Product** — `url`, `gtin`, `name`, `brand`, `codic`, `image`; **Darty Search** — `results[]`, `results[].url`, `results[].name`, `results[].badge`, `results[].brand`, `results[].codic`.

### How do I paginate Darty results?

Use the `page` parameter and keep requesting while the response indicates more results.

### Which languages are supported?

Any language that can make an HTTP request. This README shows Python and TypeScript; the [`examples/`](examples) folder contains cURL, PHP, C#, Go, Java, Ruby and Node.js.

## Official SDKs

Use Piloterr from your own language with a typed client:

| SDK | Package | Install |
| --- | --- | --- |
| Node.js | [`@piloterr/sdk`](https://www.npmjs.com/package/@piloterr/sdk) | `npm install @piloterr/sdk` |
| Python | [`piloterr`](https://pypi.org/project/piloterr/) | `pip install piloterr` |
| Go | [`piloterr-go`](https://pkg.go.dev/github.com/piloterr/piloterr-go) | `go get github.com/piloterr/piloterr-go` |
| .NET | [`Piloterr`](https://www.nuget.org/packages/Piloterr) | `dotnet add package Piloterr` |
| Ruby | [`piloterr`](https://rubygems.org/gems/piloterr) | `gem install piloterr` |
| PHP | [`piloterr/sdk`](https://packagist.org/packages/piloterr/sdk) | `composer require piloterr/sdk` |
| Java | [`piloterr-java`](https://github.com/piloterr/piloterr-java) | build from source, see its README |
| CLI | [`@piloterr/cli`](https://www.npmjs.com/package/@piloterr/cli) | `npm install -g @piloterr/cli` |
| MCP server | [`@piloterr/mcp`](https://www.npmjs.com/package/@piloterr/mcp) | `npx -y @piloterr/mcp` |

## Related scrapers

- [About You Scraper API](https://github.com/piloterr/aboutyou-scraper)
- [Action Scraper API](https://github.com/piloterr/action-scraper)
- [Aldi Scraper API](https://github.com/piloterr/aldi-scraper)
- [Alibaba Scraper API](https://github.com/piloterr/alibaba-scraper)
- [AliExpress Scraper API](https://github.com/piloterr/aliexpress-scraper)
- [Allegro Scraper API](https://github.com/piloterr/allegro-scraper)
- [All Piloterr scrapers on GitHub](https://github.com/piloterr)

## Support

- 💬 Join the [Piloterr Discord](https://discord.gg/CFj3ph8aHU) to ask questions and share what you build.
- 📚 Browse the [API reference (OpenAPI)](https://app.piloterr.com/openapi) and the [Piloterr library](https://www.piloterr.com/library).
- 🔑 [Get your API key](https://app.piloterr.com).

---

<p align="center">
  <a href="https://www.piloterr.com">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="images/logo-dark.svg">
      <img src="images/logo.svg" alt="Piloterr" height="32">
    </picture>
  </a>
  <br>
  <sub>Built by <a href="https://www.piloterr.com">Piloterr</a> — web scraping APIs for e-commerce, search, real estate and more, one API key.</sub>
</p>

<sub>Darty is a trademark of its respective owner. This repository is not affiliated with or endorsed by Darty. Examples are released under the MIT license.</sub>
