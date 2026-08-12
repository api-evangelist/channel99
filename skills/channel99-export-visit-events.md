---
name: channel99-export-visit-events
description: >-
  Export a complete, deduplicated page of Channel99 website visit events for a date range from
  the Pulsar Reporting API, paging with cursors until exhausted. Use when a user asks which
  companies visited the site, wants visit data in a warehouse or BI tool, or needs visit counts
  reconciled against the Channel99 UI.
api: Channel99 Pulsar Reporting API
base_url: https://pulsar.channel99.com
operations:
  - postAuthToken
  - getVisits
  - getCompanies
  - getChannels
  - getVendors
generated: '2026-08-12'
method: generated
source: openapi/channel99-pulsar-openapi.json + https://support.channel99.com/hc/en-us/articles/49766041989787-Channel99-Reporting-API-Developer-Guide
---

# Export Channel99 visit events

Channel99 resolves anonymous website traffic to identified companies. This skill pulls the raw
visit facts for a date range and joins them to the human-readable dimension names.

## Prerequisites

- An M2M `client_id` and `client_secret` issued by Channel99 for the instance you are reporting
  on. There is no self-service key page — Channel99 issues these.
- Credentials are bound to one Channel99 instance. A token can only read that tenant's data.
- Never put the secret or the token in logs, tickets, screenshots, source control or
  client-side code.

## Steps

### 1. Mint an access token — `postAuthToken`

```
POST https://pulsar.channel99.com/auth/token
Content-Type: application/json

{"client_id": "<client_id>", "client_secret": "<client_secret>"}
```

The response is `{access_token, token_type: "bearer", expires_in: 3600}`. Cache it for its
stated lifetime only; there is no refresh token, so re-POST the same credentials on expiry.
This endpoint is limited to **20 requests per minute per `client_id`** — mint once per job, not
once per page.

### 2. Cache the dimensions first — `getChannels`, `getVendors`, `getCompanies`

```
GET https://pulsar.channel99.com/dimensions/channels
GET https://pulsar.channel99.com/dimensions/vendors
GET https://pulsar.channel99.com/dimensions/companies
Authorization: Bearer <access_token>
x-client-id: <client_id>
```

Visit records carry only ids (`channel_id`, `vendor_id`, `company_id`). Fetch these lookups once
and join locally — the developer guide explicitly recommends caching them rather than
re-requesting per page.

### 3. Page the visits — `getVisits`

```
GET https://pulsar.channel99.com/events/visits
      ?filter[event_date][gte]=2026-01-01
      &filter[event_date][lte]=2026-01-31
      &sort[event_date]=asc
      &limit=1000
Authorization: Bearer <access_token>
x-client-id: <client_id>
```

- Filters use `filter[<field>][<op>]=<value>`; multiple filters are ANDed. Date fields accept
  `eq, ne, gt, gte, lt, lte, isNull, isNotNull`.
- `limit` defaults to 200, maximum 1000. Above 1000 you get HTTP 400 `err:pulsar.request.invalid-limit`.
- Sort with `sort[event_date]=asc|desc` — `event_date` is the only sortable field on this
  operation, and ascending order makes a resumable export deterministic.

Then loop:

```
GET /events/visits?...&cursor=<nextCursor>
```

Stop when `nextCursor` is `null`. Always replay the exact opaque `nextCursor` from the previous
response — a mangled cursor returns HTTP 400.

### 4. Shape the output

Each `data[]` record is a flat `Visit`:
`visit_event_id, event_date, channel_id, vendor_id, tag_id, company_id, audience_id_list,
ad_account_id, ad_campaign_group_id, ad_campaign_id, ad_group_id, ad_id, ad_detail_id,
current_page, c99_user_cookie_id`.

`current_page` is the landing page of the visit. For every page in the visit, pull
`getPageviews` and join on `visit_event_id`.

## Rules

- **Both credentials, every request.** `Authorization: Bearer <token>` *and*
  `x-client-id: <client_id>`. Missing the header is HTTP 401 `err:pulsar.core.missing-header`;
  a header that does not match the token's `client_id` claim is HTTP 403.
- **Back off on 429.** `/events/*` allows a burst of 100 requests per 10 seconds per client, and
  a WAF tier of 60 requests/second per `x-client-id`. On 429, sleep for the `Retry-After` value
  (seconds) and retry. There are no `RateLimit-*` budget headers, so you cannot see exhaustion
  coming — pace deliberately.
- **Retry the failed page, not the whole export.** Persist the last successful `nextCursor` plus
  the date range and filters used, so a broken run resumes instead of restarting.
- **Filter by id, not by display name**, unless the reference explicitly supports name filtering.
- **Do not treat visit counts as web-analytics sessions.** Channel99 classifies visits into
  target-audience, other resolved, bot and unresolved. Reconcile against the Channel99 UI using
  the *same* date range, timezone, audience filter and bot handling before reporting a number.
- **Expect lag.** Visit identification, ad-platform imports and CRM imports all settle on their
  own schedules. Recent data may be incomplete; Channel99 publishes no numeric freshness SLA.

## Errors

| Status | Code | Do |
|---|---|---|
| 400 | `err:pulsar.request.invalid-limit` | Use `limit` between 1 and 1000 |
| 400 | — | Invalid cursor, filter or sort — re-check the `filter[field][op]` syntax |
| 401 | `err:pulsar.core.missing-header` | Send `x-client-id` |
| 401 | `err:pulsar.auth.invalid-credentials` | Re-mint the token |
| 403 | — | Token and `x-client-id` disagree, or no access to this resource |
| 429 | — | Honour `Retry-After`, back off exponentially |
| 500 | — | Retry later; contact support with endpoint, params, date range and request time |
