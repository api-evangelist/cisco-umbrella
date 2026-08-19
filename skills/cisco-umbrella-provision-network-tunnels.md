---
name: cisco-umbrella-provision-network-tunnels
description: Stand up and monitor IPsec network tunnels from a customer edge into Umbrella's Secure Internet Gateway, including datacenter selection, credential rotation and tunnel state.
generated: '2026-08-19'
method: generated
source: openapi/cisco-umbrella-deployments-network-tunnels-openapi.yml + https://developer.cisco.com/docs/cloud-security/umbrella-api-reference-network-tunnels-overview/
api: cisco-umbrella:deployments-network-tunnels
base_url: https://api.umbrella.com/deployments/v2
scopes:
  - deployments.tunnels:read
  - deployments.tunnels:write
  - deployments.datacenters:read
operations:
  - getDatacenters
  - listTunnels
  - addTunnel
  - getTunnel
  - updateTunnel
  - deleteTunnel
  - updateTunnelCredentials
  - getTunnelPolicies
  - getOrgTunnelState
  - getTunnelState
  - getTunnelErrorEvents
---

# Provision Umbrella network tunnels

Network tunnels are how traffic from a branch, datacenter or cloud VPC reaches Umbrella's Secure Internet Gateway. This is the deployment flow with the highest blast radius in the Umbrella API — a bad tunnel edit takes a site offline.

## Steps

1. Get a bearer token — see `cisco-umbrella-authenticate`. The key needs `deployments.tunnels:write`.
2. `getDatacenters` — `GET /datacenters` — to choose the Umbrella head-end the tunnel will terminate against.
3. `addTunnel` — `POST /tunnels` — to create the tunnel. The response carries the tunnel id and the credentials the customer edge device authenticates with.
4. `getTunnelPolicies` — `GET /tunnels/{id}/policies` — to confirm which policy the tunnel's traffic will be evaluated against before you cut traffic over.
5. Watch it come up: `getTunnelState` — `GET /tunnels/{id}/state` — for one tunnel, `getOrgTunnelState` — `GET /tunnelsState` — for the whole organization.
6. Debug with `getTunnelErrorEvents` — `GET /tunnels/{id}/events` — and, for a specific source IP, `getTunnelGlobalErrorEvents`.
7. Rotate credentials with `updateTunnelCredentials` — `POST /tunnels/{id}/keys`. This is a two-sided change: the edge device must be updated in the same window or the tunnel drops.

## Rules that bite

- **The Network Tunnels API carves out its own rate limit**: 3,000 requests/minute per API key, far above the 5 req/sec that applies to the rest of the `deployments` scope. Do not size a bulk provisioning job against the wrong number in either direction.
- **No idempotency key.** `addTunnel` is a create with no dedupe. A timed-out `POST /tunnels` may have succeeded — call `listTunnels` and match on name before retrying, or you will end up with duplicate tunnels holding live credentials.
- `updateTunnel` is a `PUT`, not a `PATCH`: send the full object or you will clear fields.
- Pagination for the deployments scope is `page` + `limit`, default 200 per page.
