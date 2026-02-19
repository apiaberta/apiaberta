# API Contract

This document defines the contract between the **gateway** and **services**, and between the **gateway** and **external developers**.

---

## Gateway → External developers

### Authentication

All authenticated requests must include:

```
X-API-Key: ak_your_key_here
```

The gateway returns these headers on every response:

```
X-RateLimit-Tier: free
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1740837600
```

### Rate limits

| Tier | Requests/min | Requests/day | Cost |
|------|-------------|-------------|------|
| Free | 60 | 1,000 | Free |
| Pro | 600 | 10,000 | TBD |
| Admin | 6,000 | 100,000 | Internal |

### Base URL

```
https://api.apiaberta.pt/v1/
```

### Standard error codes

| Code | Meaning |
|------|---------|
| 400 | Bad request - invalid parameters |
| 401 | Missing or invalid API key |
| 404 | Resource not found |
| 429 | Rate limit exceeded |
| 502 | Downstream service unavailable |
| 503 | Gateway temporarily unavailable |

---

## Gateway → Services (internal)

The gateway forwards requests with these additional headers:

```
x-developer-id:   64f3a1b2c3d4e5f6a7b8c9d0   (MongoDB ObjectId)
x-developer-tier: free | pro | admin
```

The `X-API-Key` header is STRIPPED before forwarding to services.

Services should NOT implement their own authentication - trust the gateway.

---

## Service → Gateway registration

To register a new service, a record is added to `src/config.js` in the gateway:

```js
{
  prefix: '/my-service',       // URL path on the gateway
  target: process.env.MY_SERVICE_URL  // Internal URL of the service
}
```

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
