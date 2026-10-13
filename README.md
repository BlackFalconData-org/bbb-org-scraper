# BBB Business Scraper [Just 💰$0.54]

Extract structured data from [bbb.org](https://bbb.org) — Better Business Bureau (bbb.org) business profiles in the USA and Canada: BBB rating, accreditation, contact details, address, hours, reviews and complaint counts. Search by keyword and location or paste BBB URLs. Incremental mode flags new and changed profiles.

**[BBB Business Scraper [Just 💰$0.54] on Apify →](https://apify.com/blackfalcondata/bbb-org-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 🌍 country, 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, contact information for each record.

**Incremental mode** — Only get new or changed records since your last run. Content hash per record — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how records evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N records (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every record returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured records from bbb.org on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor records, track trends, and analyze market dynamics with structured, deduplicated data from bbb.org.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per record is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "plumber",
  "location": "Los Angeles, CA",
  "country": "USA",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Business name or category to search for, e.g. "plumber", "roofing", "auto repair". Required if no startUrls provided. Use a JSON array for multi-keyword runs, e.g. ["plumber","electrician"]. |
| `location` | string | — | City and state/province, e.g. "Los Angeles, CA". Also accepts a ZIP/postal code. Use a JSON array to sweep several locations, e.g. ["Los Angeles, CA","Phoenix, AZ"]. |
| `country` | enum | `"USA"` | Which BBB market to search. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). A single keyword + location search reaches a few hundred businesses; add more locations or keywords for wider coverage. |
| `maxPages` | integer | — | Highest result page to read for each search (15 businesses per page, 15 pages at most). Raised automatically when Max Results needs more pages. |
| `startUrls` | array | `[]` | Paste raw BBB search URLs. Each URL becomes its own search task; results are merged and deduped by business across all URLs. When provided, startUrls REPLACE the query/location fields (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Open each business profile for the full record: email, website, owner, hours, service areas, licences, accreditation dates, review and complaint counts. Off = search-result fields only (faster, fewer details). |
| `includeCompanyProfile` | boolean | `true` | Read each business profile page for the full record. Needs Include Full Details switched on. Off = search-result fields only. |
| `scrapeReviews` | boolean | `false` | Add each business's customer reviews to its record: star rating, reviewer name, date, review text and the business's reply. Off by default. Businesses without reviews get an empty list. |
| `maxReviewPages` | integer | `1` | How many pages of reviews to read for each business (10 reviews per page, newest first). Only used when Include Customer Reviews is on. |
| `descriptionMaxLength` | integer | `0` | Truncate the business description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip businesses whose profile matches one that disappeared in a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit businesses whose profile has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit businesses that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for new and updated businesses. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of businesses included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | This actor outputs only a plain-text `description`, so every option keeps it: `all` and `text` are equivalent, and `html` / `markdown` also keep the plain-text description (there is no HTML or Markdown variant to switch to). |
| `lat` | number | — | Latitude of a centre point. Pair with Longitude; only businesses inside the radius are kept. Works best together with a Location, because the search itself is run per place. |
| `lon` | number | — | Longitude of the centre point. Pair with Latitude. |
| `radiusKm` | integer | — | Radius in kilometres around the centre point (0 or empty = 25 km). Only businesses whose listed coordinates fall inside it are returned; businesses without coordinates are kept. |

---

## Output fields

Every record returns the same 74-field schema. Missing values are `null` — never omitted.

- `listingId`
- `listingKey`
- `profileUrl`
- `portalUrl`
- `businessId`
- `bbbId`
- `bbbOfficeName`
- `name`
- `title`
- `alternateNames`
- `bbbRating`
- `ratingScore`
- `ratingReasons`
- `ratingNotRatedReason`
- `isAccredited`
- `accreditedDate`
- `bbbFileOpenedDate`
- `phone`
- `additionalPhones`
- `faxNumbers`
- `email`
- `additionalEmails`
- `website`
- `ownerName`
- `ownerTitle`
- `streetAddress`
- `addressLine2`
- `city`
- `stateCode`
- `zipCode`
- `formattedAddress`
- `countryCode`
- `latitude`
- `longitude`
- `location`
- `serviceAreas`
- `hasServiceArea`
- `yearsInBusiness`
- `businessStartDate`
- `incorporatedDate`
- `businessType`
- `entityType`
- `numberOfEmployees`
- `isOutOfBusiness`
- `description`
- `paymentMethods`
- `licenses`
- `hours`
- `socialProfiles`
- `logoUrl`
- `primaryCategory`
- `categories`
- `categoryTree`
- `reviewsTotal`
- `averageReviewRating`
- `complaintsTotal`
- `complaintsClosedLast3Years`
- `complaintsClosedLast12Months`
- `reviewsUrl`
- `complaintsUrl`
- `leaveReviewUrl`
- `reviews`
- `acceptsQuoteRequests`
- `alerts`
- `governmentActions`
- `searchKeyword`
- `searchLocation`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per record. Here is a real example from a production run:

```json
{
  "listingId": "3f94395234ec85b9a88a543961aaa46761f39aa02194afef392b110c2f673f86",
  "listingKey": "1216_1000040787_246958",
  "profileUrl": "https://www.bbb.org/us/ca/los-angeles/profile/hydro-jetting/tingos-plumbing-inc-1216-1000040787",
  "portalUrl": "https://www.bbb.org/us/ca/los-angeles/profile/hydro-jetting/tingos-plumbing-inc-1216-1000040787",
  "businessId": "1000040787",
  "bbbId": "1216",
  "bbbOfficeName": "BBB of Los Angeles & Silicon Valley",
  "name": "Tingo's Plumbing",
  "title": "Tingo's Plumbing, Inc.",
  "alternateNames": [
    "Tingo's Plumbing, Inc."
  ],
  "bbbRating": "A+",
  "ratingScore": 100
}
```

*Truncated — full records contain 74 fields. See Output fields for the complete schema.*

**[Try BBB Business Scraper [Just 💰$0.54] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/bbb-org-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.00054 |

See the [actor on Apify](https://apify.com/blackfalcondata/bbb-org-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape bbb.org?**
Use this actor on Apify to extract structured data from bbb.org. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get bbb.org data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape bbb.org?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check bbb.org's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving time, compute, and storage. Expired records can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [BBB Business Scraper [Just 💰$0.54]](https://apify.com/blackfalcondata/bbb-org-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
