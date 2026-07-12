# People Data Labs (peopledatalabs)

People Data Labs (PDL) is a B2B data enrichment and web intelligence provider offering a REST API over a dataset of nearly three billion person profiles and tens of millions of company records. The `api.peopledatalabs.com/v5` API lets developers enrich, identify, and search person and company data, resolve contacts and firmographics, look up companies from a domain or LinkedIn URL, and clean and standardize job titles, skills, schools, companies, and locations. Every endpoint is HTTPS REST, authentication is a single `X-Api-Key` header, and PDL publishes an official OpenAPI specification.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/peopledatalabs/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/peopledatalabs/refs/heads/main/apis.yml)

## Access Model

PDL is a commercial, key-gated data API - there is no anonymous access. You create an account, obtain an API key from the PDL dashboard, and pass it in the `X-Api-Key` header (an `api_key` query parameter is also accepted). Usage is metered on **credits**: a credit is consumed each time an endpoint returns a matched record, and person, company, and IP endpoints draw on separate credit pools. A free-forever plan (roughly 100 person/company lookups and 25 IP lookups per month, with sensitive contact fields restricted) is available for evaluation, along with a time-limited free trial that grants API credits. Paid self-serve and custom enterprise plans raise credit allowances and per-minute rate limits and unlock full field access and bulk data licensing. All numbers should be reconciled against the PDL pricing pages, as they change over time.

## Tags

- Data Enrichment
- Web Intelligence
- Person Data
- Company Data
- B2B Data
- Contact Discovery
- Reference Data
- Firmographics
- Identity Resolution

## Timestamps

- **Created:** 2026-07-11
- **Modified:** 2026-07-11

## APIs

### People Data Labs Person Enrichment API

Performs a one-to-one match against nearly three billion person profiles and returns the single best profile for identifying attributes such as name, email, phone, LinkedIn or other social profile, and company. Returns the full Person Schema - names, contact details, employment history, education, location, and social accounts - via `GET /person/enrich`, with bulk person enrichment for up to 100 records per request.

- **Human URL:** [https://docs.peopledatalabs.com/docs/person-enrichment-api](https://docs.peopledatalabs.com/docs/person-enrichment-api)
- **Base URL:** `https://api.peopledatalabs.com/v5`

#### Tags

- Person Data
- Data Enrichment
- Contact Discovery
- Identity Resolution

#### Properties

- [Documentation](https://docs.peopledatalabs.com/docs/person-enrichment-api)
- [API Reference](https://docs.peopledatalabs.com/docs/reference-person-enrichment-api)
- [OpenAPI](openapi/peopledatalabs-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/peopledatalabs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/peopledatalabs.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### People Data Labs Person Search API

Finds every profile that satisfies a set of criteria using Elasticsearch or SQL queries (`GET` and `POST /person/search`), returns a ranked selection of candidate profiles for broadly identifying inputs via `/person/identify`, and retrieves specific records by PDL id via `/person/retrieve/{person_id}` and `/person/retrieve/bulk`. Built for list building, audience discovery, and identity resolution across the person dataset.

- **Human URL:** [https://docs.peopledatalabs.com/docs/person-endpoints](https://docs.peopledatalabs.com/docs/person-endpoints)
- **Base URL:** `https://api.peopledatalabs.com/v5`

#### Tags

- Person Data
- Search
- Contact Discovery
- Web Intelligence

#### Properties

- [Documentation](https://docs.peopledatalabs.com/docs/person-endpoints)
- [API Reference](https://docs.peopledatalabs.com/docs/reference-person-search-api)
- [OpenAPI](openapi/peopledatalabs-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/peopledatalabs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/peopledatalabs.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### People Data Labs Company Enrichment API

Resolves a company from an identifier such as a website domain, LinkedIn URL, or name and returns firmographic reference data - industry, employee count, headquarters location, founding year, ticker, tags, and social profiles - via `GET /company/enrich`. Ideal for domain-to-company lookups, CRM enrichment, and account intelligence.

- **Human URL:** [https://docs.peopledatalabs.com/docs/company-enrichment-api](https://docs.peopledatalabs.com/docs/company-enrichment-api)
- **Base URL:** `https://api.peopledatalabs.com/v5`

#### Tags

- Company Data
- Firmographics
- Data Enrichment
- Reference Data

#### Properties

- [Documentation](https://docs.peopledatalabs.com/docs/company-enrichment-api)
- [API Reference](https://docs.peopledatalabs.com/docs/company-endpoints)
- [OpenAPI](openapi/peopledatalabs-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/peopledatalabs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/peopledatalabs.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### People Data Labs Company Search API

Finds all companies matching Elasticsearch or SQL criteria across firmographic fields such as industry, size, location, tags, and founding year via `GET` and `POST /company/search`. Powers target-account list building, market mapping, and total-addressable-market analysis over the company dataset.

- **Human URL:** [https://docs.peopledatalabs.com/docs/company-endpoints](https://docs.peopledatalabs.com/docs/company-endpoints)
- **Base URL:** `https://api.peopledatalabs.com/v5`

#### Tags

- Company Data
- Search
- Firmographics
- B2B Data

#### Properties

- [Documentation](https://docs.peopledatalabs.com/docs/company-endpoints)
- [API Reference](https://docs.peopledatalabs.com/docs/reference-company-search-api)
- [OpenAPI](openapi/peopledatalabs-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/peopledatalabs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/peopledatalabs.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### People Data Labs Cleaner and Enrichment Utilities API

Standardizes and enriches reference data with the Cleaner APIs - company (`/company/clean`), school (`/school/clean`), and location (`/location/clean`) - plus job title enrichment (`/job_title/enrich`), skill enrichment (`/skill/enrich`), IP enrichment (`/ip/enrich`), and typeahead suggestions via `/autocomplete`. Used to normalize messy inputs before enrichment and to build data-quality and web-intelligence pipelines.

- **Human URL:** [https://docs.peopledatalabs.com/docs/cleaner-apis](https://docs.peopledatalabs.com/docs/cleaner-apis)
- **Base URL:** `https://api.peopledatalabs.com/v5`

#### Tags

- Reference Data
- Data Cleaning
- Autocomplete
- Web Intelligence

#### Properties

- [Documentation](https://docs.peopledatalabs.com/docs/cleaner-apis)
- [API Reference](https://docs.peopledatalabs.com/docs/cleaner-apis-reference)
- [OpenAPI](openapi/peopledatalabs-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/peopledatalabs.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/peopledatalabs.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [Authentication](authentication/peopledatalabs-authentication.yml)
- [GitHub Organization](https://github.com/peopledatalabs)
- [LinkedIn](https://www.linkedin.com/company/people-data-labs)
- [Website](https://www.peopledatalabs.com)
- [Documentation](https://docs.peopledatalabs.com)
- [Plans](plans/peopledatalabs-plans-pricing.yml)
- [Rate Limits](rate-limits/peopledatalabs-rate-limits.yml)
- [Fin Ops](finops/peopledatalabs-finops.yml)
- [Blog](https://www.peopledatalabs.com/blog)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
