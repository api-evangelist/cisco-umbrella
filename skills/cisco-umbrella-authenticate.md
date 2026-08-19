---
name: cisco-umbrella-authenticate
description: Exchange a Cisco Umbrella API key ID and secret for an OAuth 2.0 access token, and keep it fresh for the one hour it lives.
generated: '2026-08-19'
method: generated
source: openapi/cisco-umbrella-auth-token-openapi.yml + https://developer.cisco.com/docs/cloud-security/umbrella-api-authentication/
api: cisco-umbrella:auth-token
base_url: https://api.umbrella.com/auth/v2
operations:
  - createAuthToken
---

# Authenticate against the Cisco Umbrella API

Every Umbrella API call outside `auth/v2` needs a bearer token. There is exactly one way to get one.

## Steps

1. Get an Umbrella API key from the Umbrella dashboard. You need Full Admin privileges to create one, and you choose the OAuth 2.0 scopes at creation time — the key's scopes are fixed, you cannot widen them per request. See `scopes/cisco-umbrella-scopes.yml` for the 61 documented scopes.
2. Call `createAuthToken` — `POST https://api.umbrella.com/auth/v2/token` — with HTTP Basic authentication, where the username is the API key ID and the password is the API key secret. The security scheme in the spec is `http` / `basic`; this is the only Umbrella endpoint that is not bearer-authorized.
3. Read `access_token` and `expires_in` from the response. **The token lives one hour.** Cache it and refresh on expiry rather than minting one per call — the token endpoint is itself rate limited to 20 requests per minute per API key.
4. Send `Authorization: Bearer <access_token>` on every subsequent request.

## Failure modes

- `401` — the Authorization header is missing, or the key/secret pair is wrong. Cisco's own troubleshooting guide also flags an expired API key here: keys have an expiration date you set.
- `403` — the key is valid but does not carry the scope the endpoint needs, or the Umbrella package does not license that API.
- `429` on the token endpoint — you are minting tokens too often. Cache.

Errors come back as `{"error": "...", "message": "...", "statusCode": 401}` with media type `application/json`. This is a vendor envelope, not RFC 9457 `application/problem+json`.

## What not to assume

There is no refresh token and no OIDC discovery document. `/.well-known/openid-configuration` and `/.well-known/oauth-authorization-server` both 404 on `api.umbrella.com`; the token URL is only published in prose and in the OpenAPI `securitySchemes`.
