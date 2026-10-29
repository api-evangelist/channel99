# Channel99

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Channel99 is a B2B marketing performance and attribution platform founded by Demandbase creator Chris Golec. It uses AI and first-party account identification to unify cross-channel marketing results, verify B2B ad delivery, reveal the "dark funnel" and view-through traffic, and benchmark vendor and channel performance against the industry, then recommend budget and campaign changes that grow pipeline while reducing wasted spend. Marketing and RevOps teams export unified performance data into data warehouses (Snowflake), CDPs, and BI tools, and activate intent-based audiences. Developers reach the same data through the Pulsar Reporting API, a documented OAuth-issued M2M REST surface of visit, pageview, impression and intent facts with cursor pagination, and through a first-party read-only MCP server that connects Channel99 marketing intelligence to ChatGPT and Claude. Backed by Norwest Venture Partners, Jackson Square Ventures, Industry Ventures, Ridge Ventures, Geek Ventures, and Marin-Sonoma Impact Ventures.

**Areas:** Company · Marketing · Analytics · Attribution · B2B · Advertising · Marketing Technology · Artificial Intelligence · Account Based Marketing · Reporting · MCP · Agent Ready

## Channel99 on APIs.io

Everything profiled for Channel99 is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/channel99/](https://apis.io/providers/channel99/)** — or go straight to one of the APIs or properties below.

### APIs (18)

- [Channel99 MCP Server](https://apis.io/apis/channel99/channel99-mcp-server/) — First-party remote Model Context Protocol server that exposes Channel99 marketing intelligence - visits, pixel impressions, vendor and channel scores,…
- [Channel99 Ad Accounts API](https://apis.io/apis/channel99/channel99-ad-accounts-api/) — Ad account dimension records — `GET /dimensions/ad-accounts`
- [Channel99 Ad Campaign Groups API](https://apis.io/apis/channel99/channel99-ad-campaign-groups-api/) — Ad campaign group dimension records — `GET /dimensions/ad-campaign-groups`
- [Channel99 Ad Campaigns API](https://apis.io/apis/channel99/channel99-ad-campaigns-api/) — Ad campaign dimension records — `GET /dimensions/ad-campaigns`
- [Channel99 Ad Groups API](https://apis.io/apis/channel99/channel99-ad-groups-api/) — Ad group dimension records — `GET /dimensions/ad-groups`
- [Channel99 Ad Units API](https://apis.io/apis/channel99/channel99-ad-units-api/) — Ad unit dimension records — `GET /dimensions/ad-units`
- [Channel99 Ads API](https://apis.io/apis/channel99/channel99-ads-api/) — Ad dimension records — `GET /dimensions/ads`
- [Channel99 Audiences API](https://apis.io/apis/channel99/channel99-audiences-api/) — Audience dimension records — `GET /dimensions/audiences`
- [Channel99 Auth API](https://apis.io/apis/channel99/channel99-auth-api/) — Token issuance — `POST /auth/token`
- [Channel99 Channels API](https://apis.io/apis/channel99/channel99-channels-api/) — Channel dimension records — `GET /dimensions/channels`
- [Channel99 Companies API](https://apis.io/apis/channel99/channel99-companies-api/) — Company dimension records — `GET /dimensions/companies`
- [Channel99 Health API](https://apis.io/apis/channel99/channel99-health-api/) — Service health check
- [Channel99 Impressions API](https://apis.io/apis/channel99/channel99-impressions-api/) — Ad impression events — `GET /events/impressions`
- [Channel99 Intents API](https://apis.io/apis/channel99/channel99-intents-api/) — Buyer intent signal events — `GET /events/intents`
- [Channel99 IP Lookup API](https://apis.io/apis/channel99/channel99-ip-lookup-api/) — Company information lookup by IP address — `GET /ip/{ipAddress}`
- [Channel99 Pageviews API](https://apis.io/apis/channel99/channel99-pageviews-api/) — Website pageview events — `GET /events/pageviews`
- [Channel99 Vendors API](https://apis.io/apis/channel99/channel99-vendors-api/) — Vendor dimension records — `GET /dimensions/vendors`
- [Channel99 Visits API](https://apis.io/apis/channel99/channel99-visits-api/) — Website visit events — `GET /events/visits`

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Channel99 MCP Server](https://apis.io/servers/channel99/channel99-mcp-server/)

#### Collections (1)

Postman and Open Collection request collections.

- [Pulsar API](https://apis.io/collections/channel99/open-channel99-pulsar/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Channel99 Pulsar Overlay](https://apis.io/overlays/channel99/channel99-pulsar-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Channel99 Authentication](https://apis.io/security/channel99/channel99-authentication/)
- [Channel99 Domain Security](https://apis.io/security/channel99/channel99-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Channel99 Scopes](https://apis.io/scopes/channel99/channel99-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Channel99 Plans Pricing](https://apis.io/plans/channel99/channel99-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Channel99 Rate Limits](https://apis.io/rate-limits/channel99/channel99-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://channel99.com)
- [Documentation](https://support.channel99.com/hc/en-us)
- [Getting started](https://www.channel99.com/articles/getting-started)
- [API reference](https://pulsar.channel99.com/docs/#/)
- [Sign up](https://app.channel99.com/sign-up)
- [Support](https://support.channel99.com/hc/en-us)
- [Blog](https://www.channel99.com/resources)
- [Terms of service](https://www.channel99.com/terms-of-service)
- [Privacy policy](https://www.channel99.com/company/privacy)
- [Status](https://channel99.freshstatus.io/)

## API Evangelist rating

**Kin Score 44.1** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 71.7 |
| Contract quality | 43.7 |
| Contract governance | 4.5 |
| Operational transparency | 63.2 |
| Developer ergonomics | 49.4 |
| Access clarity | 27.6 |

**Agent readiness 34.4** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | documented (partial credit) |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | verified |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | na |

**Regulatory alignment 31.8** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/channel99/](https://apis.io/providers/channel99/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Channel99: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/channel99/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
