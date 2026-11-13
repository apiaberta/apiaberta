# API Contract

This document defines the contract between the **gateway** and **services**, and between the **gateway** and **external developers**.

---

## Gateway → External developers

### Authentication

Data endpoints (`/v1/fuel`, `/v1/ipma`, `/v1/geo`, …) are **public**: no API key is required. An API key raises your rate limit. Send it in the `X-API-Key` header:

```
X-API-Key: ak_your_key_here
```

Account routes (`/v1/auth/*`) use a `Authorization: Bearer <jwt>` token from `POST /v1/auth/login`. `/v1/webhooks/*` and `/v1/admin/*` accept either an `X-API-Key` or a token. Keys are created in the developer portal at [app.apiaberta.pt](https://app.apiaberta.pt).

### Rate limits

Enforced per minute — per client IP without a key, per API key with one:

| Access | Requests/min |
|--------|--------------|
| No API key | 30 |
| With API key | 300 |

Every response carries:

```
X-RateLimit-Limit: 30
X-RateLimit-Remaining: 26
X-RateLimit-Reset: 40
```

`X-RateLimit-Reset` is the number of seconds until the window resets. When the limit is exceeded the gateway answers `429`.

Accounts also have a tier (`free`, `pro`, `admin`). The tier currently controls access to admin routes; it does not change the request limit above.

### Caching

Successful `GET` responses are cached in the gateway, per URL, with a time-to-live per service (for example 20 s for `/v1/carris`, 600 s for `/v1/fuel`, 3600 s for `/v1/ine`). Responses carry `X-Cache: HIT | MISS`, `X-Cache-Remaining` and `Cache-Control`.

### Base URL

```
https://api.apiaberta.pt/v1/
```

### Standard error codes

| Code | Meaning |
|------|---------|
| 400 | Bad request - invalid parameters |
| 401 | Missing or invalid API key / token |
| 404 | Resource not found |
| 429 | Rate limit exceeded |
| 502 | Downstream service unavailable |
| 503 | Gateway temporarily unavailable |

---

## Gateway → Services (internal)

The gateway proxies `GET /v1/<service>/<path>` to the service as `GET /<service>/<path>`:

- the leading `/v1` is **stripped**, the service prefix is **kept**
- the query string is forwarded unchanged
- `GET /health` at the service root is probed by the gateway for `GET /v1/status`

So a service must serve its routes under `/<service>/...` and `/health` at the root — not under `/v1`.

When the request was authenticated (a valid API key or token was sent on a route that requires one), the gateway adds:

```
x-developer-id:   64f3a1b2c3d4e5f6a7b8c9d0   (MongoDB ObjectId)
x-developer-tier: free | pro | admin
```

Data routes are public, so on most requests these headers are **absent**. Services must not depend on them.

The `X-API-Key` header is STRIPPED before forwarding to services.

Services should NOT implement their own authentication - trust the gateway.

---

## Service → Gateway registration

To register a new service, the gateway needs four changes (the gateway tests check that they stay in sync):

1. `src/config.js` — add a record to `services`:

   ```js
   {
     name:   'My Service',
     prefix: '/my-service',      // becomes /v1/my-service on the gateway
     target: process.env.MY_SERVICE_URL || 'http://localhost:30xx'  // internal URL
   }
   ```

2. `src/plugins/authenticate.js` — add `'/v1/my-service'` to `PUBLIC_ROUTES` if the data should be public
3. `src/plugins/cache.js` — add a cache TTL for `/my-service`
4. `.env.example` — document `MY_SERVICE_URL`

Routes exposed by the service become available as:

```
https://api.apiaberta.pt/v1/my-service/...
```

---

## Versioning policy

- Current version: **v1**
- URL prefix: `/v1/`
- Breaking changes require a new version with 6 months overlap
- Services and the gateway version independently
- The gateway maintains compatibility with all active service versions
