---
name: Authenticate with the Price Lab API
description: Obtain and refresh a JWT bearer token, then load the authenticated user's account data.
api: openapi/price-lab-openapi.yml
operations: [login, refreshToken, getUserData]
generated: '2026-07-20'
method: generated
---

# Authenticate with the Price Lab API

The Price Lab API is served from `https://backend.pricelab.com.pe/api` and secured with JWT bearer tokens.

## Steps

1. **Log in** — `POST /login/` with your account credentials. This operation is public (no token required) and returns a JWT access token (and refresh token).
2. **Send the token** — on every subsequent request, set the header `Authorization: Bearer <access_token>`.
3. **Load account context** — call `getUserData` (`GET /users/get_data/`) to retrieve the authenticated user's data and available scope of stores/catalog.
4. **Refresh when expired** — when a request returns `401`, call `refreshToken` (`POST /token/refresh/`) with your refresh token to obtain a new access token, then retry.

## Rules

- A missing or expired token returns `401 Unauthorized` on every non-login operation.
- Do not re-run `login` on each request; reuse the access token until it expires, then refresh.
- See `authentication/price-lab-authentication.yml` and `conventions/price-lab-conventions.yml` for the full auth model.
