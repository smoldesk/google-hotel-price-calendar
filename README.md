# Google Hotels Price Calendar

![Google Hotels Price Calendar](https://raw.githubusercontent.com/smoldesk/apify-scrapers/main/assets/hotel-calendar/banner.webp)

Get the cheapest nightly rate for every check-in date, up to a year ahead, for any hotel on Google Hotels. Give it a hotel name or Google Hotels link, your guests, stay length and currency. No login, no API key. Runs on [Apify](https://apify.com/smoldesk/google-hotel-price-calendar).

## What you get

- **One row per hotel and check-in date**: nightly price, stay total, total before taxes, taxes and fees, weekday and an `isCheapestDate` flag.
- **Up to 366 dates in one run**, a full year for one hotel in a few seconds.
- **Taxes made explicit**: Google shows prices with or without taxes depending on the visitor's country. `pricePerNightWithTaxes` and `pricePerNightBeforeTaxes` are always filled in, and `displayedPriceIncludesTaxes` tells you what Google showed.
- **Booking-site prices** (optional): what Booking.com, Expedia, Hotels.com, Agoda, the official site and others list for each date, with the cheapest site.
- **Any currency, 1 to 28 nights, adults and children's ages.**

## Run it

Open the [Google Hotels Price Calendar on Apify](https://apify.com/smoldesk/google-hotel-price-calendar), fill in the form and click Start. Or call it from code with your [Apify token](https://console.apify.com/settings/integrations):

```bash
curl -X POST "https://api.apify.com/v2/acts/smoldesk~google-hotel-price-calendar/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "hotels": ["Hilton Paris Opera", "Marina Bay Sands"],
    "startDate": "2026-12-01",
    "days": 90,
    "nights": 2,
    "adults": 2,
    "currency": "EUR"
  }'
```

## Sample output

```json
{
  "type": "date-price",
  "hotelName": "Hilton Paris Opera",
  "checkIn": "2027-08-21",
  "checkOut": "2027-08-23",
  "weekday": "Saturday",
  "nights": 2,
  "available": true,
  "pricePerNight": 266.41,
  "pricePerNightWithTaxes": 291.36,
  "pricePerNightBeforeTaxes": 249.44,
  "displayedPriceIncludesTaxes": false,
  "totalPrice": 582.72,
  "totalBeforeTaxes": 498.88,
  "totalTaxesAndFees": 83.84,
  "currency": "EUR",
  "isCheapestDate": true
}
```

Export as JSON, CSV or Excel, or pipe it into Google Sheets, Make, Zapier or n8n.

## Pricing

Pay per result. No monthly fee.

| Result | Per 1,000 |
|---|---|
| Date price (one hotel, one check-in date) | $1 |
| Booking-site prices for a date (optional) | +$1.50 |

Dates with no rate and hotels Google shows no price for are not charged. A full year for one hotel costs about $0.37.

## Limits

- The calendar price is Google's lowest rate for the date, not every room type. Turn on booking-site prices for per-site rates.
- Some hotels show no price on Google (not bookable through Google). The Actor says so and charges nothing.
- Google's calendar covers about a year ahead and stays of 1 to 28 nights.
- A hotel name that matches several hotels is rejected. Add the city or paste the Google Hotels link.

## FAQ

**How do I find the cheapest weekend to book a hotel?**
Run a year of dates for the hotel and sort by `pricePerNightWithTaxes`, or filter on `weekday`.

**How do I track hotel prices every day?**
Schedule the run with the same hotels and compare `pricePerNight` per `checkIn` between runs.

**Can I price a whole city?**
Use the [Google Hotels Scraper](https://github.com/smoldesk/google-hotels-scraper) to list the hotels in a city, then pass their `entityId` values here.

**Do I need an account or a proxy?**
No. Requests go out from the run's own IP. If Google starts answering with captcha pages, the Actor slows down and stops early with what it has.

## More scrapers

Google Hotels Price Calendar is one of 20 no-login scrapers by Smoldesk Studios. See the full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).
