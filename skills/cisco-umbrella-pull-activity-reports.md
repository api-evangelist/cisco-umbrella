---
name: cisco-umbrella-pull-activity-reports
description: Pull DNS, proxy, firewall and intrusion activity plus top-N and summary aggregations out of the Cisco Umbrella Reporting API.
generated: '2026-08-19'
method: generated
source: openapi/cisco-umbrella-reports-reporting-openapi.yml + https://developer.cisco.com/docs/cloud-security/umbrella-api-reference-reporting-overview/
api: cisco-umbrella:reports-reporting
base_url: https://api.umbrella.com/reports/v2
scopes:
  - reports.granularEvents:read
  - reports.aggregations:read
  - reports.utilities:read
  - reports.summariesByRule:read
operations:
  - getActivities
  - getActivityDns
  - getActivityProxy
  - getActivityFirewall
  - getActivityIntrusion
  - getSummary
  - getSummaryByType
  - getTopDestinations
  - getTopIdentities
  - getTopCategories
  - getTopThreats
  - getTotalRequests
  - getRequestsByTimerange
  - getIdentities
  - getCategories
---

# Pull Cisco Umbrella activity and reports

The Reporting API is the largest surface in the Umbrella API — 62 operations — and splits cleanly into three shapes: granular events, top-N aggregations, and summaries.

## Steps

1. Get a bearer token — see `cisco-umbrella-authenticate`.
2. **Granular events.** `getActivities` — `GET /activity` — for everything, or the typed variants `getActivityDns`, `getActivityProxy`, `getActivityFirewall`, `getActivityIntrusion`, `getActivityIP`, `getActivityAmpRetrospective` when you only want one traffic type. These need `reports.granularEvents:read`.
3. **Aggregations.** `getTopDestinations`, `getTopIdentities`, `getTopCategories`, `getTopThreats`, `getTopThreatTypes`, `getTopUrls`, `getTopIps` — each with a `/{type}` variant that filters to one traffic type. These need `reports.aggregations:read`.
4. **Summaries and totals.** `getSummary`, `getSummaryByType`, `getSummariesByCategory`, `getSummariesByDestination`, `getTotalRequests`, `getRequestsByTimerange`, `getRequestsByHour`, `getBandwidthByHour`.
5. **Lookup tables.** `getIdentities`, `getCategories`, `getThreatTypes`, `getThreatNames` resolve the ids that appear in the event and aggregation payloads. `postIdentities` resolves identities in bulk. These need `reports.utilities:read`.
6. MSPs pull across managed customers with the `/providers/*` operations (`getProviderRequestsByOrganization`, `getDeploymentStatusByOrganization`, ...), which need `reports.customers:read`.

## Rules that bite

- **The Reporting rate limit is per organization, not per key** — 5 requests per second across every key you hold. Splitting work over more API keys does not buy more throughput here, unlike the deployments and policies scopes where limits are per key.
- Paginate with `limit` and `offset`. `offset` is the entry point into the collection.
- Every operation is a `GET` except `postIdentities`, so the missing idempotency contract mostly does not bite in this scope.
- `429` is a bare status with no `Retry-After`. Wait one second.
