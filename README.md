# Cisco Umbrella

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

Cisco Umbrella, built on the OpenDNS platform Cisco acquired in 2015 and now sold within Cisco Secure
Access, is Cisco's cloud-delivered security service: DNS-layer security, secure web gateway,
cloud-delivered firewall, CASB (Cisco Cloudlock) and remote browser isolation.

## Ownership

Part of the Cisco family (acquired 2015).

## Contract status

**Published — 26 first-party OpenAPI documents, 256 operations.** Harvested 2026-08-19 from Cisco's own
docs CDN (`pubhub.devnetcloud.com`), which serves
[developer.cisco.com/docs/cloud-security/](https://developer.cisco.com/docs/cloud-security/). Every
document names `servers[] https://api.umbrella.com/{basePath}` and
`info.contact "Cloud Security Developer Community"`.

Pristine downloads are in `openapi/_original/`; the working copies in `openapi/` are byte-identical
except for an appended `x-provenance` / `x-evidence` block recording where each came from.

> **Correction to an earlier round.** This repository previously recorded
> `x-contract-status: none` on the basis that `https://api.umbrella.com/openapi.json` returns a real
> 404. That probe was correct and the conclusion was wrong: Cisco does not serve the spec from the API
> host. The specs are listed in the docs navigation config at
> `https://pubhub.devnetcloud.com/media/cloud-security-apis-in-eft/docs/umbrella-config.json` and
> downloadable from `.../docs/reference/**.yaml`.

## API surface

| Scope | Base | Documents | Operations |
|---|---|---|---|
| `auth` | `https://api.umbrella.com/auth/v2` | 1 | 1 |
| `admin` | `https://api.umbrella.com/admin/v2` | 6 | 50 |
| `deployments` | `https://api.umbrella.com/deployments/v2` | 11 | 60 |
| `investigate` | `https://api.umbrella.com/investigate/v2` | 1 | 28 |
| `policies` | `https://api.umbrella.com/policies/v2` | 2 | 14 |
| `reports` | `https://api.umbrella.com/reports/v2` | 4 | 81 |
| Cloudlock (CASB) | `https://api.cloudlock.com/api/v2` | 1 | 22 |

Authorization is OAuth 2.0 client credentials against `POST https://api.umbrella.com/auth/v2/token`,
with 61 documented scopes across the five non-auth scope groups. Access tokens live one hour.

## Not published by Cisco for Umbrella

Recorded as honest absences, not as unchecked fields:

- **No MCP server.** `CiscoDevNet/secure-access-mcp-community` is a Cisco-published MCP server, but it
  targets `https://api.sse.cisco.com` (Cisco Secure Access), a sibling product on a different API host.
  It is recorded in `mcp/` and deliberately **not** credited to Umbrella.
- **No A2A agent card** at `/.well-known/agent-card.json` or `/.well-known/agent.json` on any host.
- **No `/.well-known/` surface on `api.umbrella.com`** — no api-catalog, no OIDC/OAuth server metadata,
  no `llms.txt`. All real 404s with JSON bodies.
- **No idempotency contract.** No idempotency key or header is documented anywhere.
- **No rate-limit response headers.** Exhaustion is a bare HTTP 429.
- **No webhooks, events or AsyncAPI surface.**
- **No installable SDK in any registry.** The "Umbrella API Client" is a Python source listing printed
  inside the documentation.
- **No published prices.** Every package is quote-only.

## Verified links

- [Developer portal / API reference](https://developer.cisco.com/docs/cloud-security/)
- [Getting started](https://developer.cisco.com/docs/cloud-security/umbrella-api-getting-started/)
- [Authentication](https://developer.cisco.com/docs/cloud-security/umbrella-api-authentication/)
- [OAuth 2.0 scopes](https://developer.cisco.com/docs/cloud-security/umbrella-api-oauth-scopes/)
- [Rate limits](https://developer.cisco.com/docs/cloud-security/umbrella-api-rate-limits/)
- [Changelog](https://developer.cisco.com/docs/cloud-security/umbrella-api-changelog/)
- [Status page](https://status.umbrella.com/)
- [Product documentation](https://docs.umbrella.com/)
- [Pricing / packages](https://umbrella.cisco.com/products/packages)
- [Postman examples](https://github.com/CiscoDevNet/cloud-security/tree/master/Umbrella/PostmanExamples)
- [Cisco PSIRT security.txt](https://www.cisco.com/.well-known/security.txt)
- [Parent company](https://apis.io/providers/cisco/)

All URLs above returned HTTP 200 when probed on 2026-08-19.
