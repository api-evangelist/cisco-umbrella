---
name: cisco-umbrella-investigate-a-domain
description: Enrich a domain, IP or file hash with Cisco Umbrella Investigate threat intelligence — categorization, risk score, passive DNS, WHOIS and related samples.
generated: '2026-08-19'
method: generated
source: openapi/cisco-umbrella-investigate-investigate-openapi.yml + https://developer.cisco.com/docs/cloud-security/umbrella-api-reference-investigate-overview/
api: cisco-umbrella:investigate-investigate
base_url: https://api.umbrella.com/investigate/v2
scopes:
  - investigate.investigate:read
  - investigate.bulk:read
operations:
  - getDomainsCategorization
  - postDomainsCategorization
  - getDomainsRiskScore
  - getSecurityName
  - getPassiveDNSName
  - getPassiveDNSIp
  - getWhois
  - getWhoisHistory
  - getSubDomains
  - getSamplesDestination
  - getSampleHash
  - getTimeline
---

# Investigate a domain with Cisco Umbrella

Umbrella Investigate is the threat-intelligence half of the Umbrella API. It answers "what do you know about this domain, IP, or hash", from the same resolver telemetry that powers DNS-layer blocking.

## Steps

1. Get a bearer token — see `cisco-umbrella-authenticate`. The key needs `investigate.investigate:read`.
2. Start with categorization: `getDomainsCategorization` — `GET /domains/categorization/{domain}` — for content and security categories plus a status flag.
3. Score it: `getDomainsRiskScore` — `GET /domains/risk-score/{domain}` — and `getSecurityName` — `GET /security/name/{domain}` — for the model-level security features behind the score.
4. Pivot on infrastructure: `getPassiveDNSName` (`GET /pdns/name/{domain}`) and `getPassiveDNSIp` (`GET /pdns/ip/{ip}`) for passive DNS; `getWhois` and `getWhoisHistory` for registration; `getSubDomains` for observed subdomains.
5. Pivot on malware: `getSamplesDestination` (`GET /samples/{destination}`) for samples associated with the destination, then `getSampleHash` and `getSampleHashArtifacts` / `getSampleHashConnections` / `getSampleHashBehaviors` for one sample.
6. For a corpus rather than one domain, use `postDomainsCategorization` — `POST /domains/categorization` — which accepts **up to 1,000 domains per request**.

## Rate limits are tier-dependent, and this is the part that breaks integrations

Investigate has four access levels — Integration, Tier 1, Tier 2, Tier 3 — and the tier sets the limit:

- Passive DNS / WHOIS / samples endpoints: 3 req/sec on Integration and Tier 1, 12 req/sec on Tier 2, and 48 req/sec on Tier 3 for the samples/WHOIS group.
- `POST /domains/categorization` is **not available below Tier 2**; at Tier 2 and 3 it is 150 req/sec.
- `GET /search/{expression}` is 18 req/min, dropping to **3 req/min** when the expression is prefixed with `.*`, because a wildcard prefix scans the whole collection.
- The Integration level is additionally capped at 2,000 requests per day.

Limits are shared across all of the organization's API keys. Exhaustion is HTTP 429 with no headers; back off one second.

## Pagination

`limit` and `offset` on the pdns, whois-emails and samples endpoints; `limit` only on `getWhoisHistory`, `getWhoisNameservers` and `getTopmillion`. **`getTopmillion` returns one million records if you do not set a limit** — always set one.
