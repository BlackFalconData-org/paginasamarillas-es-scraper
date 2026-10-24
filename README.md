# Paginasamarillas Spain Scraper

Extract structured data from [paginasamarillas.es](https://paginasamarillas.es) — paginasamarillas.es — Spanish business listings by keyword and city. Each record has the business name and address with phone and website and email and coordinates, plus opening hours and branch addresses where listed. Export clean JSON or CSV for CRM and lead lists.

**[Paginasamarillas Spain Scraper on Apify →](https://apify.com/blackfalcondata/paginasamarillas-es-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword and location. Filter by description format, and more.

**Detail enrichment** — Fetch full descriptions, contact information for each company.

**Incremental mode** — Only get new or changed companies since your last run. Content hash per company — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how companies evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N companies (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every company returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured companies from paginasamarillas.es on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor companies, track trends, and analyze market dynamics with structured, deduplicated data from paginasamarillas.es.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per company is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "fontaneros",
  "location": "Madrid",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | What to look for, in Spanish — an activity, trade or business type (e.g. "fontaneros", "dentistas", "talleres de coches"), or a company name. Required if no startUrls provided. |
| `location` | string | — | Spanish city or province to search in (e.g. "Madrid", "Barcelona", "Sevilla"). Leave empty to search all of Spain. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw search URLs from the target site. Each URL becomes its own search task; results are merged and deduped by listing ID across all URLs. When provided, startUrls REPLACE the query/location fields (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Open each business page for the full record (email, coordinates, opening hours, brands, extra addresses). Off = the search-result fields only. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
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

---

## Output fields

Every company returns the same 100-field schema. Missing values are `null` — never omitted.

- `id`
- `title`
- `location`
- `url`
- `name`
- `category`
- `activities`
- `description`
- `about`
- `phone`
- `phones`
- `website`
- `address`
- `streetAddress`
- `postalCode`
- `city`
- `province`
- `latitude`
- `longitude`
- `logoUrl`
- `rating`
- `reviewCount`
- `isAdvertiser`
- `isFeatured`
- `adId`
- `advertiserId`
- `openingHours`
- `paymentMethods`
- `cuisines`
- `additionalInfo`
- `brands`
- `technicalService`
- `images`
- `yearsOnDirectory`
- `otherLocations`
- `position`
- `scrapedAt`
- `searchUrl`
- `email`
- `emails`
- `listingId`
- `paginasamarillasUrl`
- `profileUrl`
- `detailUrl`
- `listingUrl`
- `canonicalUrl`
- `sourceUrl`
- `street`
- `country`
- `countryCode`
- `sourceCountry`
- `region`
- `locality`
- `source`
- `sourceSite`
- `recordType`
- `listingType`
- `businessName`
- `categories`
- `primaryCategory`
- `industry`
- `slogan`
- `services`
- `hours`
- `whatsapp`
- `linkWhatsapp`
- `facebook`
- `instagram`
- `linkedin`
- `twitter`
- `youtube`
- `tiktok`
- `pinterest`
- `socialLinks`
- `logo`
- `imageUrl`
- `imageUrls`
- `coordinates`
- `mapUrl`
- `googleMapsUrl`
- `additionalPhones`
- `allPhones`
- `allAddresses`
- `branches`
- `branchCount`
- `hasPhone`
- `hasEmail`
- `hasWebsite`
- `hasWhatsapp`
- `isPremium`
- `sponsored`
- `searchTerm`
- `searchQuery`
- `searchKeyword`
- `searchLocation`
- `portalUrl`
- `contentQuality`
- `detailFetched`
- `contentHash`
- `changeType`

---

## Sample output

One object per company. Here is a real example from a production run:

```json
{
  "id": "8DdsfCdjkf",
  "title": "ASISTENCIA HOGAR 24H",
  "location": "Madrid, MADRID",
  "url": "https://www.paginasamarillas.es/f/madrid/asistencia-hogar-24h_8DdsfCdjkf.html",
  "name": "ASISTENCIA HOGAR 24H",
  "category": "Multiservicios: empresas",
  "activities": [
    "Multiservicios: empresas"
  ],
  "description": "REPARACIONES e INSTALACIONES Rápidas 24h y Festivos. Económicos, Equipo de técnicos propio, Garantía 100%, Atendemos todas las Zonas. Asistencia Inmediata del hogar.",
  "about": "Información y Servicios\nASISTENCIA INMEDIATA DEL HOGAR\nSomos una empresa especializada en las reparaciones urgentes del hogar. Disponemos de muy buenos profesionales con capacidad …",
  "phone": "910553918",
  "phones": [
    "910553918"
  ],
  "website": "http://asistencia-hogar.com"
}
```

*Truncated — full records contain 100 fields. See Output fields for the complete schema.*

**[Try Paginasamarillas Spain Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/paginasamarillas-es-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.003591 |

See the [actor on Apify](https://apify.com/blackfalcondata/paginasamarillas-es-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape paginasamarillas.es?**
Use this actor on Apify to extract structured data from paginasamarillas.es. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get paginasamarillas.es data as JSON, CSV, or Excel?**
The actor writes each company to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape paginasamarillas.es?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check paginasamarillas.es's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each company gets a content hash. On subsequent runs, only new or changed companies are emitted — saving time, compute, and storage. Expired companies can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Paginasamarillas Spain Scraper](https://apify.com/blackfalcondata/paginasamarillas-es-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
