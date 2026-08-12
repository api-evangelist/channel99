---
name: channel99-resolve-company-by-ip
description: >-
  Resolve an IP address to an identified company (domain, name, sector, region, revenue range)
  using the Channel99 IP API, and reconcile that company against Channel99 audiences. Use when a
  user wants to deanonymize inbound traffic, enrich a server log, or check whether an address
  belongs to a target account — and needs to know first whether their contract even includes it.
api: Channel99 Pulsar Reporting API
base_url: https://pulsar.channel99.com
operations:
  - postAuthToken
  - getCompanyByIp
  - getCompanies
  - getAudiences
generated: '2026-08-12'
method: generated
source: openapi/channel99-pulsar-openapi.json + https://support.channel99.com/hc/en-us/articles/49766041989787-Channel99-Reporting-API-Developer-Guide
---

# Resolve an IP address to a company

## Check the entitlement first

The Channel99 IP API is **separately permissioned**. Channel99's developer guide states:
"Access to separately permissioned services, including the Channel99 IP API, is not enabled
unless explicitly granted." A working token for `/events/*` and `/dimensions/*` says nothing
about this route — a 403 here is an entitlement answer, not a credential bug. Confirm access
before building anything on it.

## Steps

### 1. Token — `postAuthToken`

`POST /auth/token` with `{client_id, client_secret}`.

### 2. Resolve — `getCompanyByIp`

```
GET https://pulsar.channel99.com/ip/{ipAddress}
Authorization: Bearer <access_token>
x-client-id: <client_id>
```

Returns an `IpCompanyRecord`:
`ip_address, company_domain, company_name, region_name, rev_range_name, sector_name`.

Note the shape difference from the rest of the API: this record returns resolved **names**
(`region_name`, `sector_name`, `rev_range_name`), whereas the `Company` dimension record returns
**ids** (`region_id`, `sector_id`, `revenue_range_id`). Do not join the two on those fields.

### 3. Tie it back to an account — `getCompanies`, `getAudiences`

Match `company_domain` against `/dimensions/companies` to obtain the `company_id`, then check
membership by pulling `/dimensions/audiences` and looking for that company in the
`audience_id_list` of its recent visit or impression events.

## Rules

- **Company level only.** The resolution answers "which organization", never "which person".
  Channel99 states it does not track at user level and uses no cookies in its pixel. Do not
  present a resolution as identifying an individual, and do not merge it with personal data
  fetched elsewhere.
- **Unresolved is a normal answer.** Channel99's own reporting has a dedicated
  `count_unresolved_visits` bucket for addresses its identification network cannot resolve.
  Treat a miss as data, not as an error to retry.
- **Rate limits are different here.** `/ip/*` sits on a WAF elevated tier and the application
  burst limit does not apply — but the ceiling is not published. Pace conservatively and honour
  `Retry-After` on 429.
- **One address per call.** There is no batch resolution operation. For bulk enrichment, use the
  event endpoints (`getVisits`, `getImpressions`), which already carry `company_id`, instead of
  looping this route.
- **Consumer/ISP ranges.** B2B IP resolution is strongest on corporate egress and weakest on
  residential and mobile ranges. A missing resolution is far more likely than a wrong one, but
  do not treat a single resolution as proof of intent from that account.

## Errors

| Status | Meaning |
|---|---|
| 401 `err:pulsar.core.missing-header` | `x-client-id` not sent |
| 401 `err:pulsar.auth.invalid-credentials` | Token expired or wrong credentials |
| 403 | No entitlement to the IP API, or token/header client mismatch |
| 404 `err:pulsar.core.not-found` | Route or address not found |
| 429 | Honour `Retry-After` |
