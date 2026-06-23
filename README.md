[Google Hotels Scraper](https://apify.com/scrape.badger/google-hotels-scraper?fpr=data)

## What does Google Hotels Scraper do?

Scrape [Google Hotels](https://www.google.com/travel/hotels) — hotel search with prices, ratings, amenities, plus full property detail (rating breakdown, check-in/out policies, thumbnails).

## Why use Google Hotels Scraper?

- **Search + Detail in one actor.** Two modes: listing search, and deep property detail by `property_token`.
- **Dynamic pricing.** Prices are for the specific check-in / check-out window you pass.
- **Currency + country targeting.** `gl: uk` returns GBP, UK-first rankings.
- **Sponsored ads + organic bundled.** `ad` records tagged separately from organic properties.
- **Cheap detail fetches.** $0.008 per Get Hotel Details — deep enrichment on demand.

## What data can Google Hotels Scraper extract?

| Field | Type | Description |
| --- | --- | --- |
| record_type | string | `property` / `ad` / `brands` / `property_detail` |
| name | string | Hotel name |
| rating | number | Star rating |
| reviews | number | Review count |
| price | string | Per-night price string |
| check_in / check_out | string | Query window |
| link | string | Google Hotels property URL |
| thumbnail | string | Primary image |
| amenities | array | Amenity list |
| property_token | string | Detail-fetch token |

## How to scrape Google Hotels

1. Click **Try for free**.
2. Pick `mode`: Search Hotels or Get Hotel Details.
3. For Search: set `q` (destination or hotel name), `check_in`, `check_out`, `adults`.
4. For Details: set `property_token` (from a previous Search run), `check_in`, `check_out`.
5. Optional: `currency`, `gl`.
6. Click **Start** — hotels stream into the dataset.

## How much will it cost?

**$8 per 1,000 searches · $0.008 per Get Hotel Details.** Search = one call per query; Detail = one call per property.

### Competitor benchmark

| Actor | Author | Price | Notes |
| --- | --- | --- | --- |
| voyager/booking-scraper | voyager | ~$10 / 1k hotels | Booking.com-focused |
| compass/crawler-google-places | Compass | ~$9 / 1k places | Places actor with hotels subset |
| apify/google-search-scraper | Apify | ~$3.50 / 1k pages | SERP actor, shallow hotel info |
| **scrape-badger/google-hotels-scraper** | **ScrapeBadger** | **$8 / 1k searches** | **20% below closest hotel-focused competitor** |

## Input

Configure the run in the **Input** tab above, or pass a JSON object matching the fields below when calling the Actor via the Apify API.

| Field | Required | Description |
| --- | --- | --- |
| mode | ✅ | `Search Hotels` or `Get Hotel Details`. |
| check_in / check_out | ✅ | `YYYY-MM-DD`. check_out > check_in. |
| q | Search only | Destination or hotel name. |
| property_token | Details only | From a previous Search. |
| adults | — | 1-8, default 2. |
| currency | — | ISO currency (default `USD`). |
| gl | — | Country (default `us`). |

## Output

Every successful run streams records into the run's dataset. Download as JSON, CSV, XML, Excel, or HTML from the **Dataset** tab; consume programmatically via the Apify API or webhooks.

Example record:

```
{
  "record_type": "property",
  "name": "The Ritz-Carlton, New York",
  "rating": 4.7,
  "reviews": 1845,
  "price": "$895 / night",
  "check_in": "2026-12-01",
  "check_out": "2026-12-03",
  "link": "https://www.google.com/travel/hotels/\u2026",
  "thumbnail": "https://\u2026",
  "amenities": [
    "Spa",
    "Free WiFi",
    "Gym"
  ],
  "property_token": "ChoI5pXL1PzbxuDCARoNL2cvMTFqY3h3bWhsMxAB"
}
```

## Tips / Advanced options

- **Pipe Search → Detail.** Run Search to collect `property_token`s, pipe each through Get Hotel Details for amenity-level depth.
- **Set `check_in` / `check_out` matching your customer's stay.** Prices are highly date-sensitive — stale queries return stale prices.
- **Filter ads vs. organic.** `record_type: property` for organic; `record_type: ad` for sponsored. Filter downstream.
- **Regional currency.** `currency: EUR, gl: de` for German travellers — local merchants and local prices.

## FAQ, Disclaimers, Support

### Can I book through the actor?

No — booking is out of scope. The actor returns prices and Google Hotels URLs; customers book on the provider's site.

### What's `property_token`?

Google's internal token for direct-lookup of a hotel's deep detail. Surfaced in Search mode output.

### Why does `check_out` have to be after `check_in`?

Google rejects zero-night or invalid ranges with HTTP 400. The actor validates before calling.

### Can I filter by star rating?

Not directly at the query level — post-filter on the `rating` field downstream.

### Disclaimer

This Actor scrapes public Google data only. You're responsible for compliance with Google's Terms of Service and any applicable data-protection laws (GDPR, CCPA, etc.) in your jurisdiction. ScrapeBadger does not store the scraped results — they are delivered directly to your Apify dataset.

### Support

Something not working? Open a ticket in the **Issues** tab above — we triage within one business day. Full API reference: [docs.scrapebadger.com](https://docs.scrapebadger.com).

### Related Actors

- [`google-flights-scraper`](https://apify.com/scrape-badger/google-flights-scraper) — Flight fares

### Powered by

[ScrapeBadger](https://scrapebadger.com) — Google-optimised residential proxy pool + browser-farm fallback, 99.7% uptime, unmetered bandwidth. No CAPTCHAs reach you.