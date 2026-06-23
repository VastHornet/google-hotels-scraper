[Google Hotels Scraper](https://apify.com/kaix/google-hotels-scraper?fpr=data)

# Google Hotels & Vacation Rentals Scraper

Scrape Google Hotels and Vacation Rentals for property search results, details, pricing, reviews, and photos. Supports batch searches with filters, sorting, hotel URL lookups, and full photo gallery extraction.

## Why use this scraper?

- Both property types: hotels and vacation rentals in one actor
- Batch searches: run multiple location queries in a single actor run
- Hotel URL lookup: fetch details for specific hotels by URL without searching
- Full filter support: price range, hotel class, guest rating, amenities, property types, offers, eco-certified
- Booking links: pricing from multiple providers with logos, cancellation policies, member rates, and official site detection
- Reviews: aggregated from Google, Tripadvisor, Trip.com, and more - with sorting, free-text search, sub-ratings, photos, and highlights
- Photos: full gallery extraction (690+ photos per hotel), categorized as Bedroom, Exterior, Food & Drink, etc.
- Rich detail: amenities, descriptions, check-in/out times, neighborhood scores, nearby places with travel times, price history, eco certification, web results
- Structured output: typed HotelResult records with all data from the Google Hotels property page

## Use cases

- Monitor hotel prices across locations and dates
- Compare vacation rental prices across booking platforms
- Build accommodation databases for travel analytics
- Track pricing trends for specific properties or areas
- Feed hotel data into alerting or recommendation pipelines

## How to use

### Basic hotel search

```
{
  "searches": [
    { "location": "Hue", "checkInDate": "2026-05-01", "checkOutDate": "2026-05-03" }
  ]
}
```

### Vacation rentals only

```
{
  "type": "vacation_rentals",
  "searches": [
    { "location": "Paris", "checkInDate": "2026-06-15", "checkOutDate": "2026-06-22" }
  ]
}
```

### Batch searches

```
{
  "searches": [
    { "location": "New York", "checkInDate": "2026-06-15", "checkOutDate": "2026-06-18" },
    { "location": "Tokyo", "checkInDate": "2026-07-01", "checkOutDate": "2026-07-05" },
    { "location": "London", "checkInDate": "2026-08-10", "checkOutDate": "2026-08-14" }
  ]
}
```

### With filters

```
{
  "searches": [
    { "location": "Hue", "checkInDate": "2026-05-01", "checkOutDate": "2026-05-03" }
  ],
  "maxPrice": 100,
  "currency": "USD",
  "hotelClass": [4, 5],
  "guestRating": "4.0",
  "amenities": ["Pool", "Free Wi-Fi"],
  "propertyTypes": ["Resorts", "Boutique hotels"],
  "freeCancellation": true,
  "sortBy": "price_low",
  "adults": 2,
  "rooms": 1
}
```

### With details and reviews

```
{
  "searches": [
    { "location": "Hue", "checkInDate": "2026-05-01", "checkOutDate": "2026-05-03" }
  ],
  "includeDetails": true,
  "includeReviews": true,
  "maxResults": 10
}
```

### Lookup specific hotels by URL

```
{
  "hotelUrls": [
    "https://www.google.com/travel/hotels/entity/ChkIq_atzbv20-8ZGg0vZy8xMXFoMzRoa3pfEAE"
  ],
  "searches": [
    { "location": "Tokyo", "checkInDate": "2026-05-19", "checkOutDate": "2026-05-22" }
  ],
  "currency": "VND",
  "includeDetails": true,
  "includeReviews": true,
  "maxReviewsPerHotel": 100,
  "reviewSort": "highest_score",
  "maxPhotosPerHotel": 0
}
```

### Reviews with sorting and search

```
{
  "hotelUrls": ["https://www.google.com/travel/hotels/entity/..."],
  "searches": [{ "location": "Tokyo", "checkInDate": "2026-05-19", "checkOutDate": "2026-05-22" }],
  "includeReviews": true,
  "maxReviewsPerHotel": 50,
  "reviewSort": "highest_score",
  "reviewSearch": "breakfast"
}
```

### Multiple guests

```
{
  "searches": [
    { "location": "Bali", "checkInDate": "2026-07-01", "checkOutDate": "2026-07-08" }
  ],
  "adults": 4,
  "children": 2,
  "rooms": 2
}
```

## Input

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `hotelUrls` | string[] |  | Hotel entity URLs for lookup mode (skips search, fetches details directly) |
| `searches` | array |  | List of `{ location, checkInDate, checkOutDate }` |
| `type` | enum | `both` | `hotels`, `vacation_rentals`, `both` |
| `adults` | integer | `2` | Adult guests (1-9) |
| `children` | integer | `0` | Child guests (0-9) |
| `rooms` | integer | `1` | Number of rooms (1-9) |
| `currency` | string | `USD` | ISO 4217 currency code (e.g. `USD`, `EUR`, `VND`) |
| `minPrice` | integer |  | Minimum price per night in local currency |
| `maxPrice` | integer |  | Maximum price per night in local currency |
| `hotelClass` | integer[] |  | Star levels: `[2, 3, 4, 5]` |
| `guestRating` | enum |  | `3.5`, `4.0`, `4.5` |
| `propertyTypes` | string[] |  | `Apartment hotels`, `Bed and breakfasts`, `Spa hotels`, `Resorts`, `Motels`, `Inns`, `Hostels`, `Boutique hotels`, `Beach hotels`, `Other` |
| `amenities` | string[] |  | `Free Wi-Fi`, `Free breakfast`, `Restaurant`, `Bar`, `Kid-friendly`, `Pet-friendly`, `Free parking`, `Parking`, `EV charger`, `Room service`, `Fitness center`, `Spa`, `Pool`, `Indoor pool`, `Outdoor pool`, `Air-conditioned`, `Wheelchair accessible`, `Beach access`, `All-inclusive available` |
| `freeCancellation` | boolean | `false` | Properties with free cancellation |
| `specialOffers` | boolean | `false` | Properties with special offers |
| `ecoCertified` | boolean | `false` | Eco-certified properties only |
| `sortBy` | enum | `relevance` | `relevance`, `price_low`, `price_high`, `rating`, `distance` |
| `maxResults` | integer | `50` | Maximum results per search |
| `includeDetails` | boolean | `false` | Fetch booking links, amenities, description per property (slower) |
| `includeReviews` | boolean | `false` | Fetch reviews per property (slower) |
| `maxReviewsPerHotel` | integer | `40` | Max reviews per property. `0` = fetch all |
| `reviewSort` | enum | `most_helpful` | `most_helpful`, `most_recent`, `highest_score`, `lowest_score` |
| `reviewSearch` | string |  | Free-text search within reviews (e.g. `"breakfast"`, `"clean rooms"`) |
| `maxPhotosPerHotel` | integer | `0` | Max photos per property. `0` = fetch all from gallery (~690 unique) |
| `proxyConfiguration` | object |  | Proxy settings. Residential proxies recommended |

## Output

Each result is a `HotelResult` object. Fields marked with **(D)** require `includeDetails: true`. Fields marked with **(R)** require `includeReviews: true`.

### Example output

```
{
  "searchType": "hotel",
  "location": "Hue",
  "checkInDate": "2026-04-23",
  "checkOutDate": "2026-04-24",
  "name": "Alba Hotel",
  "propertyId": "17907820141034738872",
  "placeId": "ChIJmZg7HjmhQTER97bQ77pbnPc",
  "starRating": 3,
  "propertyType": "3-star hotel",
  "latitude": 16.4617,
  "longitude": 107.5904,
  "distance": "0.1 km away",
  "pricePerNight": 478989,
  "totalPrice": 478989,
  "currency": "VND",
  "reviewScore": 4.3,
  "reviewCount": 396,
  "reviewSource": "Google",
  "photos": [{"url": "https://lh3.googleusercontent.com/...", "label": null, "categoryIds": [1, 5]}],
  "amenities": ["Free parking", "Restaurant", "Bar", "Room service", "Free Wi-Fi", "Spa", "Pool", "..."],
  "ecoBadge": false,
  "highlights": ["Higher guest rating"],
  "phone": "0234 3839 998",
  "website": "https://www.booking.com/hotel/vn/hue-queen-2.vi.html...",
  "countryCode": "VN",
  "address": "12 Nguyễn Văn Cừ, Vĩnh Ninh, Thuận Hóa, Huế",
  "description": "Surrounded by restaurants and cafes, this laid-back hotel is 2 km from the Huế Museum of Royal Fine Arts...",
  "checkInTime": "2:00 PM",
  "checkOutTime": "12:00 PM",
  "neighborhood": "Thuan Hoa",
  "neighborhoodScores": [{"category": 4, "rating": "4.8"}, {"category": 1, "rating": "4.8"}, "..."],
  "ratingDistribution": [{"stars": 5, "count": 217}, {"stars": 4, "count": 109}, {"stars": 3, "count": 48}, "..."],
  "aspectSentiments": [
    {"aspect": "Breakfast", "score": 0.53, "positive": 16, "negative": 4, "total": 22},
    {"aspect": "Service", "score": 0.63, "positive": 47, "negative": 7, "total": 59},
    "... 12 more"
  ],
  "externalReviewSources": [{"source": "Tripadvisor", "rating": 4.6, "maxRating": 5, "count": 145}],
  "bookingLinks": [
    {"provider": "Alba Hotel", "price": 684000, "isOfficialSite": true, "freeCancellation": false},
    {"provider": "Agoda", "price": 478989, "isOfficialSite": false, "freeCancellation": false},
    "... 6 more"
  ],
  "priceHistory": {"currentPrice": 478989, "typicalLow": 630287, "typicalHigh": 756424, "percentile": 0.74},
  "nearbyPlaces": [
    {"name": "Hue Imperial City", "rating": 4.5, "category": "Point of interest", "travelTimes": [{"mode": "car", "duration": "7 min"}]},
    {"name": "Thiên Mụ Pagoda", "rating": 4.5, "travelTimes": [{"mode": "car", "duration": "12 min"}]},
    "... 29 more"
  ],
  "nearbyHotels": [{"name": "Moonlight Hotel Hue", "starRating": 4, "pricePerNight": 665394}, "..."],
  "sponsoredHotels": [{"name": "Meliá Vinpearl Hue", "reviewScore": 4.8, "pricePerNight": 2110876}, "..."],
  "nearbyVacationRentals": [{"name": "canary homestay", "sleeps": 10, "pricePerNight": 1980000}, "..."],
  "webResults": [
    {"title": "Alba Hotel - Booking.com", "url": "https://www.booking.com/...", "snippet": "Located in Hue...", "domain": "https://www.booking.com"}
  ],
  "photoCategories": [
    {"categoryId": 6, "name": "Bedroom", "count": 127, "thumbnailUrl": "https://..."},
    {"categoryId": 5, "name": "Exterior", "count": 187, "thumbnailUrl": "https://..."}
  ],
  "reviews": [
    {
      "source": "Google", "author": "Aaron Kraus", "rating": 5, "maxRating": 5,
      "text": "Thanh Phan helped us really well!...",
      "date": "3 months ago",
      "subRatings": [{"category": "Rooms", "score": 5, "maxScore": 5}, {"category": "Service", "score": 5, "maxScore": 5}],
      "highlights": ["Luxury"]
    },
    "... 2 more"
  ]
}
```

### Search result fields (always available)

| Field | Type | Description |
| --- | --- | --- |
| `searchType` | string | `"hotel"` or `"vacation_rental"` |
| `location` | string | Search location |
| `checkInDate` | string | Check-in date |
| `checkOutDate` | string | Check-out date |
| `name` | string | Hotel name |
| `propertyId` | string | Google property ID |
| `placeId` | string | Google Place ID |
| `starRating` | number | Star rating (2-5) |
| `propertyType` | string | e.g. `"3-star hotel"` |
| `latitude` | number | GPS latitude |
| `longitude` | number | GPS longitude |
| `distance` | string | Distance from search center |
| `pricePerNight` | number | Price per night |
| `totalPrice` | number | Total price for stay |
| `currency` | string | Currency code |
| `priceLabel` | string | Price highlight label |
| `reviewScore` | number | Review score (e.g. 4.3) |
| `reviewCount` | number | Total review count |
| `reviewSource` | string | Primary review source |
| `thumbnailUrl` | string | Thumbnail image URL |
| `photos` | array | `{ url, label, categoryIds }` - photo URL with category IDs (1=At a glance, 5=Exterior, 6=Bedroom, 8=Bathroom, etc.) |
| `highlights` | string[] | Highlight tags (e.g. `"Free parking"`) |
| `hashId` | string | Google hash ID |
| `capacity` | string[] | Capacity labels (VR only: `"Sleeps 6"`, `"2 bedrooms"`) |
| `sleeps` | number | Max guests (VR only) |
| `bedrooms` | number | Bedroom count (VR only) |
| `bathrooms` | number | Bathroom count (VR only) |
| `featured` | boolean | Featured flag |
| `bookingToken` | string | Token for detail/review lookups |
| `ecoBadge` | boolean | Eco-certified (detected from detail call) |

### Detail fields (D)

| Field | Type | Description |
| --- | --- | --- |
| `address` | string | Full street address |
| `phone` | string | Phone number |
| `website` | string | Hotel website URL |
| `countryCode` | string | ISO country code (e.g. `"VN"`) |
| `description` | string | Hotel description (paragraphs joined) |
| `checkInTime` | string | Check-in time (e.g. `"2:00 PM"`) |
| `checkOutTime` | string | Check-out time (e.g. `"12:00 PM"`) |
| `amenities` | string[] | Available amenities with human-readable names |
| `neighborhood` | string | Neighborhood name |
| `neighborhoodDescription` | string | Short neighborhood description |
| `neighborhoodScores` | array | `{ category, rating }` - 5 neighborhood category scores |
| `ratingDistribution` | array | `{ stars, count }` - star breakdown (5★ through 1★) |
| `aspectSentiments` | array | `{ aspect, score, positive, negative, total }` - e.g. Breakfast, Service, Location |
| `externalReviewSources` | array | `{ source, rating, maxRating, count }` - Tripadvisor, Trip.com, etc. |
| `bookingLinks` | array | `{ provider, price, url, logoUrl, isOfficialSite, freeCancellation, freeCancellationDeadline }` |
| `roomTypes` | array | `{ name, price }` - room types with pricing |
| `priceHistory` | object | `{ currentPrice, typicalLow, typicalHigh, percentile }` - price trend. `percentile` (0-1) = position in typical range; derive "X% less" via `(1 - percentile) * 100` |
| `featuredIn` | string[] | Editorial mentions (e.g. `"Featured in Best Hotels in Tokyo with a View"`) |
| `webResults` | array | `{ title, url, snippet, domain, favicon }` - web results from Booking.com, Tripadvisor, etc. |
| `photoCategories` | array | `{ categoryId, name, count, thumbnailUrl }` - photo gallery category summary |
| `nearbyPlaces` | array | `{ name, photoUrl, rating, reviewCount, description, latitude, longitude, category, travelTimes }` - POIs with travel times |
| `nearbyBusinesses` | array | `{ placeId, name, category, rating, reviewCount, phone, website, reservationUrl, photoUrl, priceRange, hours }` |
| `nearbyHotels` | array | Similar hotels (NearbyHotel type) |
| `sponsoredHotels` | array | Sponsored/popular hotels (NearbyHotel type) |
| `nearbyVacationRentals` | array | Nearby vacation rentals (NearbyHotel type) |

### Review fields (R)

| Field | Type | Description |
| --- | --- | --- |
| `reviews` | array | Review entries (see below) |

Each review:

| Field | Type | Description |
| --- | --- | --- |
| `source` | string | `"Google"`, `"Tripadvisor"`, `"Trip.com"` |
| `author` | string | Reviewer name |
| `authorUrl` | string | Profile URL |
| `avatarUrl` | string | Profile photo URL |
| `rating` | number | Rating (1-5) |
| `maxRating` | number | Max rating (always 5) |
| `text` | string | Full review text |
| `date` | string | Relative date (`"3 months ago"`) |
| `reviewId` | string | Unique review ID (for deduplication) |
| `reviewUrl` | string | Direct link (Tripadvisor only) |
| `subRatings` | array | `{ category, score, maxScore }` - Rooms, Service, Location (Google only) |
| `photos` | string[] | Review photo URLs (Google only) |
| `highlights` | string[] | Hotel tags (`"Luxury"`, `"Romantic"`, `"Great value"`) |
| `replyText` | string | Owner reply text |