---
name: cisco-umbrella-manage-destination-lists
description: Create, inspect and edit Umbrella destination lists (the allow/block lists policies are built from), including the paginated destinations inside them.
generated: '2026-08-19'
method: generated
source: openapi/cisco-umbrella-policies-destination-lists-openapi.yml + https://developer.cisco.com/docs/cloud-security/umbrella-api-reference-destination-lists-overview/
api: cisco-umbrella:policies-destination-lists
base_url: https://api.umbrella.com/policies/v2
scopes:
  - policies.destinationLists:read
  - policies.destinationLists:write
  - policies.destinations:read
  - policies.destinations:write
operations:
  - getDestinationLists
  - createDestinationList
  - getDestinationList
  - updateDestinationLists
  - deleteDestinationList
  - getDestinations
  - createDestinations
  - deleteDestinations
---

# Manage Cisco Umbrella destination lists

Destination lists are the allow/block collections that Umbrella policies reference. A list holds domains, URLs and IPs; policies point at lists, not at individual destinations.

## Steps

1. Get a bearer token — see `cisco-umbrella-authenticate`.
2. `getDestinationLists` — `GET /destinationlists` — to enumerate the organization's lists. Paginate with `page` and `limit`; the default page size is 200.
3. `createDestinationList` — `POST /destinationlists` — to create a list. Choose `allow` or `block` access at creation.
4. `getDestinations` — `GET /destinationlists/{destinationListId}/destinations` — to read what is in a list. **This endpoint caps the batch at 100 records**, so paginate rather than assuming one call returns everything.
5. `createDestinations` — `POST /destinationlists/{destinationListId}/destinations` — to add entries.
6. `deleteDestinations` — `DELETE /destinationlists/{destinationListId}/destinations/remove` — to remove entries. Note the trailing `/remove` segment; this is not a plain DELETE on the collection.
7. `updateDestinationLists` (PATCH) renames a list; `deleteDestinationList` removes it.

## Rules that bite

- **No idempotency.** Cisco publishes no idempotency key for the Umbrella API. If `createDestinations` times out you cannot safely blind-retry it — read the list back with `getDestinations` and reconcile.
- **Rate limits** for the Destination Lists API are 2,000 requests/minute and 6,000 requests/hour per API key. Exhaustion is a bare HTTP 429 with no `Retry-After` header; wait one second and retry.
- `409 Conflict` here means you exceeded the limit of a list, or tried to delete a list that a policy is still using. Detach the policy first.
- Write operations need `policies.destinationLists:write` / `policies.destinations:write`. A `403` means the key was minted without them, and the key's scopes cannot be widened after creation.
