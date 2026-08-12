---
name: channel99-vendor-impression-analysis
description: >-
  Compare advertising vendors in Channel99 by pulling ad impression events and the ad hierarchy
  from the Pulsar Reporting API, then aggregating exposure, click-through and audience reach per
  vendor and campaign. Use when a user asks which vendors or campaigns actually reached their
  target accounts, wants to find wasted ad spend, or needs view-through exposure evidence.
api: Channel99 Pulsar Reporting API
base_url: https://pulsar.channel99.com
operations:
  - postAuthToken
  - getImpressions
  - getVendors
  - getAdCampaigns
  - getAdCampaignGroups
  - getAdGroups
  - getAds
  - getAdUnits
  - getAdAccounts
  - getAudiences
generated: '2026-08-12'
method: generated
source: openapi/channel99-pulsar-openapi.json + https://support.channel99.com/hc/en-us/articles/49766041989787-Channel99-Reporting-API-Developer-Guide
---

# Analyse vendor and campaign ad exposure

Channel99's impression pixel records which *companies* were exposed to an ad, not which people.
This skill turns those raw exposure facts into a vendor comparison.

## Prerequisites

- M2M `client_id` / `client_secret` for the Channel99 instance.
- The impression pixel must actually be deployed in the vendor's creative — impressions only
  exist for vendors that implemented it (LinkedIn, G2, Google Ads, Microsoft, Facebook, Reddit,
  TikTok, X, StackAdapt, GAM, RollWorks/AdRoll, 6sense, Demandbase are the documented ones).

## Steps

### 1. Token — `postAuthToken`

`POST /auth/token` with `{client_id, client_secret}` → `{access_token, expires_in: 3600}`.
Send `Authorization: Bearer <token>` **and** `x-client-id: <client_id>` on everything after this.

### 2. Load the ad hierarchy

```
GET /dimensions/ad-accounts          -> getAdAccounts
GET /dimensions/ad-campaign-groups   -> getAdCampaignGroups
GET /dimensions/ad-campaigns         -> getAdCampaigns
GET /dimensions/ad-groups            -> getAdGroups
GET /dimensions/ads                  -> getAds
GET /dimensions/ad-units             -> getAdUnits
GET /dimensions/vendors              -> getVendors
GET /dimensions/audiences            -> getAudiences
```

Every impression carries the full chain of ids; these lookups turn them into names. Note that
`Audience`, `AdCampaign`, `AdGroup`, `Ad` and `AdUnit` all carry `is_deleted` — filter it with
`filter[is_deleted][eq]=false` unless you deliberately want historical objects.

### 3. Page the impressions — `getImpressions`

```
GET /events/impressions
      ?filter[event_date][gte]=2026-01-01
      &filter[event_date][lte]=2026-01-31
      &filter[vendor_id][in]=<id1>,<id2>
      &sort[event_date]=asc
      &limit=1000
```

Loop on `cursor=<nextCursor>` until `nextCursor` is `null`.

Each `Impression` record is:
`imp_event_id, event_date, channel_id, vendor_id, tag_id, company_id, audience_id_list,
ad_account_id, ad_campaign_group_id, ad_campaign_id, ad_group_id, ad_id, ad_detail_id,
ad_site, ad_unit_id, has_click`.

### 4. Aggregate

Per `vendor_id` (then per `ad_campaign_id`):

- **Impressions** — count of records.
- **Companies reached** — distinct `company_id`.
- **Target-audience reach** — distinct `company_id` whose `audience_id_list` contains the
  audience you are measuring against.
- **Click-through share** — records where `has_click` is true, over total.
- **View-through candidates** — companies with impressions but no click; join to
  `getVisits` on `company_id` within the window to see who later showed up on the site. This is
  the mechanism behind Channel99's direct-traffic reallocation.

## Rules

- **Spend, clicks and CPM are not in this API.** The Pulsar surface carries exposure and
  identity facts only. Spend, impressions-from-the-ad-platform and cost efficiency live in the
  Channel99 UI and the Snowflake data share. Do not compute a return-on-spend number from these
  endpoints and present it as Channel99's — it will not reconcile.
- **Impression ≠ person.** Every record resolves to a company domain. Channel99 is explicit that
  no contact-level data exists on this surface. Never present an impression as an individual.
- **A vendor with zero impressions is ambiguous.** It means either the vendor served nothing, or
  the pixel was never installed in their creative. Check the pixel deployment before calling a
  vendor ineffective.
- **Rate limits**: burst 100 requests / 10s per client on `/events/*`; WAF 60 rps per
  `x-client-id`. Honour `Retry-After` on 429; there are no remaining-budget headers.
- **Bots.** Visit records classify bot traffic separately; keep bot and unresolved traffic out of
  reach and efficiency numerators.

## Errors

Same catalogue as every Pulsar operation — `err:pulsar.core.missing-header` (401, missing
`x-client-id`), `err:pulsar.request.invalid-limit` (400, `limit` > 1000),
`err:pulsar.core.not-found` (404), 403 on token/header mismatch, 429 with `Retry-After`.
See `errors/channel99-problem-types.yml`.
