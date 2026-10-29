# CI HUB

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

CI HUB (CI HUB GmbH, Berlin) builds the connector layer between digital asset repositories and the applications creative and marketing teams actually work in. Its Connector plugs Adobe Creative Cloud, Microsoft 365, Google Workspace, Figma, Sketch, Canva, WordPress, SharePoint, Salesforce and Asana into more than 60 DAM, MAM, PIM, CMS, stock and cloud-storage systems through one uniform panel, so an asset is placed, relinked and version-checked without leaving the design or document. For developers CI HUB publishes two SDKs at developer.ci-hub.com. The Access SDK is an HTTP API under /api/v1 that lets a partner platform embed that same connectivity: a partner backend signs an RS256 JWT, exchanges it for a CI HUB session, connects an end user to a DAM provider, then browses, searches and reads assets over one contract regardless of which DAM is behind it. The Integration SDK runs the other direction, letting a DAM, PIM or storage vendor translate its own API into CI HUB's handler format and appear in every CI HUB connector. CI HUB also operates a hosted MCP server (mcp.ci-hub.com, marketed as the Bright AI Connector) that exposes governed, permission-mapped DAM search and asset operations to ChatGPT, Claude, Gemini, Microsoft Copilot and any other MCP-compatible client.

**Areas:** Company · Digital Asset Management · Content Management · Product Information Management · Integration · Connectors · Creative Tools · Marketing · Brand Management · MCP · Cloud Storage · Germany

## CI HUB on APIs.io

Everything profiled for CI HUB is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/ci-hub/](https://apis.io/providers/ci-hub/)** — or go straight to one of the APIs or properties below.

### APIs (5)

- [CI HUB Integration SDK](https://apis.io/apis/ci-hub/integration-sdk/) — The other direction of the CI HUB platform. A DAM, MAM, PIM, CMS, stock or cloud-storage vendor writes an integration that translates its own proprietary…
- [CI HUB MCP Server (Bright AI Connector)](https://apis.io/apis/ci-hub/mcp/) — A hosted, remote Model Context Protocol server that gives an AI client governed access to the DAM systems a user has connected through CI HUB. It exposes…
- [CI HUB Assets API](https://apis.io/apis/ci-hub/ci-hub-assets-api/) — The Assets API from CI HUB — 4 operation(s) for assets.
- [CI HUB Auth API](https://apis.io/apis/ci-hub/ci-hub-auth-api/) — The Auth API from CI HUB — 6 operation(s) for auth.
- [CI HUB System API](https://apis.io/apis/ci-hub/ci-hub-system-api/) — The System API from CI HUB — 1 operation(s) for system.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [CI HUB MCP Server](https://apis.io/servers/ci-hub/ci-hub-mcp-server/)

#### Collections (1)

Postman and Open Collection request collections.

- [CI HUB Access SDK API](https://apis.io/collections/ci-hub/open-ci-hub-access/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Ci Hub Access Overlay](https://apis.io/overlays/ci-hub/ci-hub-access-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Ci Hub Authentication](https://apis.io/security/ci-hub/ci-hub-authentication/)
- [Ci Hub Domain Security](https://apis.io/security/ci-hub/ci-hub-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Ci Hub Scopes](https://apis.io/scopes/ci-hub/ci-hub-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Ci Hub Plans Pricing](https://apis.io/plans/ci-hub/ci-hub-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Ci Hub Rate Limits](https://apis.io/rate-limits/ci-hub/ci-hub-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://ci-hub.com/)
- [Documentation](https://developer.ci-hub.com/docs)
- [Getting started](https://developer.ci-hub.com/access/getting-started)
- [API reference](https://developer.ci-hub.com/access)
- [Pricing](https://ci-hub.com/pricing)
- [Sign up](https://ci-hub.com/start-free-trial)
- [Support](https://ci-hub.com/support)
- [Blog](https://ci-hub.com/blog)
- [Terms of service](https://ci-hub.com/legal/terms)
- [Privacy policy](https://ci-hub.com/legal/privacy-policy)
- [GitHub](https://github.com/CI-HUB-GmbH)

## API Evangelist rating

**Kin Score 61.5** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 52.7 |
| Contract governance | 18.2 |
| Operational transparency | 39.5 |
| Developer ergonomics | 80.4 |
| Access clarity | 76.3 |

**Agent readiness 51.2** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 34.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/ci-hub/](https://apis.io/providers/ci-hub/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for CI HUB: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/ci-hub/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
