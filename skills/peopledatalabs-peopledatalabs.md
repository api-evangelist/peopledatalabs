---
name: Peopledatalabs
description: Use when enriching person or company profiles with real-time data, searching for people or companies matching specific criteria, identifying individuals from partial information, or building lead lists and talent pipelines. Reach for this skill when you need to look up professional profiles, verify contact information, find candidates, or segment audiences using comprehensive B2B data.
metadata:
    mintlify-proj: peopledatalabs
    version: "1.0"
---

# People Data Labs API

## Product summary

People Data Labs (PDL) is a B2B data API platform that enriches and searches person and company profiles. Use it to look up detailed professional information about individuals (email, phone, work history, education, social profiles) and companies (funding, employee count, industry, location), or to search for people and companies matching specific criteria. The primary API endpoint is `https://api.peopledatalabs.com/v5/` with endpoints for person enrichment, person search, company enrichment, company search, and supporting APIs. Authenticate using an API key in the URL, header (`X-Api-Key`), or via SDKs (Python, JavaScript, Ruby, Go, Rust). Access the dashboard at https://dashboard.peopledatalabs.com to manage API keys and monitor usage.

## When to use

Reach for this skill when:
- **Enriching profiles**: You have a person's name, email, LinkedIn URL, or phone and need their full professional profile (work history, education, contact info, social profiles)
- **Searching for people**: You need to find individuals matching criteria (job title, company, location, skills, salary range)
- **Enriching companies**: You have a company name or website and need details (funding, employee count, industry, location, revenue)
- **Searching for companies**: You need to find companies matching filters (industry, size, location, funding stage)
- **Identifying individuals**: You have partial info (name + location, or email) and need to verify or find the person
- **Building lead lists**: You need to generate lists of prospects matching specific job titles, companies, or locations
- **Bulk operations**: You need to enrich or search for 2-100 records in a single request
- **Verifying data**: You need to check if contact information (email, phone) is current or find alternative contact methods

## Quick reference

### API Endpoints

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/v5/person/enrich` | GET/POST | Look up one person by name, email, LinkedIn URL, phone, or ID |
| `/v5/person/bulk` | POST | Enrich 1-100 people in one request |
| `/v5/person/search` | POST | Find people matching Elasticsearch query or field filters |
| `/v5/person/identify` | POST | Find up to 20 people matching broad criteria (returns multiple matches) |
| `/v5/company/enrich` | GET/POST | Look up one company by name, website, or LinkedIn URL |
| `/v5/company/bulk` | POST | Enrich 1-100 companies in one request |
| `/v5/company/search` | POST | Find companies matching Elasticsearch query or field filters |

### Authentication Methods

```python
# SDK (Python)
from peopledatalabs import PDLPY
client = PDLPY(api_key="YOUR_API_KEY")

# Header
headers = {"X-Api-Key": "YOUR_API_KEY"}

# URL parameter
url = "https://api.peopledatalabs.com/v5/person/enrich?api_key=YOUR_API_KEY"
```

### Common Input Parameters (Person Enrichment)

| Parameter | Type | Example | Notes |
|-----------|------|---------|-------|
| `profile` | string/array | `linkedin.com/in/seanthorne` | LinkedIn, Twitter, GitHub URLs |
| `email` | string/array | `john@example.com` | Work or personal email |
| `phone` | string/array | `+1-555-234-1234` | Must include country code |
| `name` | string | `John Smith` | Full name (first + last minimum) |
| `first_name` | string | `John` | First name only |
| `last_name` | string | `Smith` | Last name only |
| `company` | string/array | `Google` | Current or past employer |
| `school` | string/array | `Stanford` | University or college name |
| `location` | string | `San Francisco, CA` | City, state, or country |
| `pdl_id` | string | `qEnOZ5Oh0poWnQ1luFBfVw_0000` | PDL persistent ID (overrides other params) |
| `data_include` | string | `full_name,emails.address` | Comma-separated fields to return; use `-field` to exclude |
| `required` | string | `emails AND (phone OR profiles)` | Boolean expression for required fields in response |
| `min_likelihood` | integer | `7` | Minimum match confidence (1-10) |

### Response Structure

```json
{
  "status": 200,
  "likelihood": 10,
  "data": {
    "id": "qEnOZ5Oh0poWnQ1luFBfVw_0000",
    "full_name": "john smith",
    "emails": [{"address": "john@example.com", "type": "work"}],
    "phone_numbers": ["+1-555-234-1234"],
    "work_email": "john@example.com",
    "job_title": "Software Engineer",
    "job_company_name": "Google",
    "education": [...],
    "experience": [...],
    "profiles": [...]
  }
}
```

### Rate Limits & Credits

| Plan | Requests/Minute | Monthly Limit | Notes |
|------|-----------------|---------------|-------|
| Free | 100 | 10,000 | Enrichment API only |
| Pro | 1,000 | Varies | Contact support for limits |
| Enterprise | Up to 5,000 | Custom | Negotiable with team |

Check remaining credits in response headers: `x-ratelimit-remaining.minute`, `x-totallimit-remaining`

## Decision guidance

### When to use Enrichment vs Search

| Use Case | API | Why |
|----------|-----|-----|
| You have a specific person's email/LinkedIn and need their full profile | Enrichment | One-to-one match, returns single best result |
| You need to find all people matching criteria (job title, company, location) | Search | Returns multiple results, you control filtering |
| You have partial info and want to verify it's correct | Enrichment | Returns likelihood score (1-10) for match confidence |
| You need to find 100+ people matching criteria | Search | Enrichment limited to bulk of 100; search can paginate |
| You want to enrich a known list of 50 people at once | Bulk Enrichment | Faster than 50 individual calls, same credit cost |

### When to use Person Identify vs Person Search

| Use Case | API | Why |
|----------|-----|-----|
| You have a name and location, want to find the person | Identify | Returns up to 20 ranked matches, one credit per call |
| You need to find all people in a company with a specific title | Search | Returns all matches, you control result size |
| You want broad matching on a single attribute (name only) | Identify | Simpler, returns ranked results |
| You need complex filtering (multiple fields, date ranges, OR logic) | Search | Supports Elasticsearch queries for advanced logic |

### When to use Field Filters vs Elasticsearch Query

| Approach | Use When | Example |
|----------|----------|---------|
| Field Filters | Simple filtering by documented fields, AND logic only | `company_website=google.com&location=San Francisco` |
| Elasticsearch Query | Need OR logic, must_not, exists, complex matching | `{"bool": {"must": [{"match": {"title": "engineer"}}]}}` |

## Workflow

### Enrich a single person profile

1. **Gather input data**: Collect at least one identifier (email, LinkedIn URL, phone, or name + location)
2. **Choose authentication**: Use SDK for simplicity, or raw HTTP with header/URL auth
3. **Build request**: Add parameters for the person (name, email, company, etc.)
4. **Add filters** (optional): Use `data_include` to request only needed fields, `required` to ensure response has certain data
5. **Execute request**: Call `/v5/person/enrich` with GET or POST
6. **Check status**: Verify `status: 200` (match found) or `404` (no match)
7. **Inspect likelihood**: If status 200, check `likelihood` score (1-10) to assess match confidence
8. **Extract data**: Use fields from `data` object; null values indicate missing data
9. **Monitor credits**: Check response headers for remaining credits

### Bulk enrich 2-100 people

1. **Prepare request array**: Build array of up to 100 request objects, each with `params` and optional `metadata`
2. **Add metadata** (optional): Include `user_id` or other tracking info in each request to match responses
3. **Set global filters** (optional): Add `required`, `data_include`, `min_likelihood` at top level (applies to all unless overridden)
4. **POST to bulk endpoint**: Send to `/v5/person/bulk` with `Content-Type: application/json`
5. **Iterate responses**: Responses return in same order as requests; check each `status` individually
6. **Track failures**: Responses with status != 200 are not charged; use metadata to identify which records failed
7. **Retry failures**: Resubmit failed records in a new bulk request

### Search for people matching criteria

1. **Decide query mode**: Use Field Filters for simple searches, Elasticsearch for complex logic
2. **Build field filters** (simple): Add parameters like `company_website`, `job_title`, `location`, `salary_min`
3. **Or build Elasticsearch query** (advanced): Write JSON query using Elasticsearch DSL v7.7 syntax
4. **Set pagination**: Use `size` parameter to limit results (default varies); use `scroll_token` for large datasets
5. **POST to search endpoint**: Send to `/v5/person/search`
6. **Check response**: Verify `status: 200`; `data` array contains matching profiles
7. **Monitor credits**: Each result in `data` array costs 1 credit; use `size` to control spend
8. **Paginate if needed**: Use `scroll_token` from response to fetch next batch

### Identify a person from partial info

1. **Provide broad criteria**: Name, email, phone, company, school, or location (one or more)
2. **POST to identify endpoint**: Send to `/v5/person/identify`
3. **Receive ranked results**: Response includes up to 20 profiles sorted by match score
4. **Check status**: `status: 200` means matches found; `404` means no matches
5. **Note credit usage**: One credit per call, regardless of number of results returned
6. **Select best match**: Use match scores and data to pick the correct person

## Common gotchas

- **Phone numbers must include country code**: `+1-555-234-1234` works; `555-234-1234` does not
- **Minimum inputs not met**: Requests fail if you don't provide enough data. Minimum is: `profile` OR `email` OR `phone` OR `email_hash` OR `lid` OR `(name AND (location OR company OR school))`
- **Confusing Bulk Enrichment with Search**: Bulk Enrichment is for known people you want to enrich; Search is for finding unknown people matching criteria
- **Forgetting to check individual status codes in bulk responses**: Each response in a bulk array has its own status; don't assume all succeeded
- **Using Field Filters and query together**: If you provide both, `query` takes precedence and Field Filters are ignored
- **Hitting 1MB response size limit**: Large searches (80-100 records) can exceed limit; use `data_include` to reduce fields or request fewer records
- **Not tracking metadata in bulk requests**: Without metadata, you can't easily match responses back to your input records
- **Assuming null fields mean no data**: Null values indicate the field is not in your data bundle; check your subscription level
- **Exceeding rate limits silently**: Monitor `x-ratelimit-remaining` headers; requests return `429` when limit hit
- **Invalid API key not caught early**: Always test authentication before bulk operations; `401` errors stop the request

## Verification checklist

Before submitting work with PDL API:

- [ ] API key is valid and has not expired (test with a simple request first)
- [ ] Minimum input requirements met for the endpoint (e.g., name + location, or email, or LinkedIn URL)
- [ ] Response status code checked (200 = match, 404 = no match, 4xx/5xx = error)
- [ ] For bulk requests: each response in array has individual status checked
- [ ] Likelihood score reviewed (if applicable) to assess match confidence
- [ ] `data_include` parameter used to request only needed fields (reduces response size)
- [ ] `required` parameter set if certain fields must be present in response
- [ ] Rate limit headers checked to ensure credits remain
- [ ] Metadata included in bulk requests for tracking responses back to inputs
- [ ] Error handling in place for 401 (auth), 404 (not found), 429 (rate limit), 402 (out of credits)
- [ ] Response size under 1MB (use gzip or reduce fields if needed)
- [ ] Phone numbers formatted with country code if using phone parameter

## Resources

- **Comprehensive page navigation**: https://docs.peopledatalabs.com/llms.txt
- **API Dashboard**: https://dashboard.peopledatalabs.com (manage keys, view usage, test endpoints)
- **Person Enrichment API Reference**: https://docs.peopledatalabs.com/docs/reference-person-enrichment-api
- **Person Search API Reference**: https://docs.peopledatalabs.com/docs/reference-person-search-api
- **Company Enrichment API Reference**: https://docs.peopledatalabs.com/docs/reference-company-enrichment-api

---

> For additional documentation and navigation, see: https://docs.peopledatalabs.com/llms.txt