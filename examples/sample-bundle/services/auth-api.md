---
type: Service
title: Auth API
description: Issues and verifies short-lived JWTs for every Storefront service.
resource: https://github.com/acme/storefront/tree/main/services/auth
tags: [auth, security, platform]
timestamp: "2026-06-14T10:00:00Z"
---

# Overview

Stateless HTTP service that issues signed JWTs and verifies them for the rest of
the platform. It is the trust root for [Orders API](/services/orders-api.md) and
[Payments API](/services/payments-api.md): both reject any request whose token
this service did not sign. Knowledge here is derived from the service source and
its README — a worked example of the [OKF adoption decision](/decisions/use-okf.md).

# Endpoints

| Method | Path      | Description                                  |
|--------|-----------|----------------------------------------------|
| `POST` | `/token`  | Exchange credentials for a short-lived JWT.  |
| `POST` | `/verify` | Validate a JWT and return its claims.        |
| `POST` | `/revoke` | Add a token's `jti` to the deny list.        |

# Claims

```json
{ "sub": "user_42", "scope": ["orders:write", "payments:read"], "exp": 1718360000 }
```

Tokens live 15 minutes; clients refresh against `/token`. Signing keys rotate
weekly and are pinned by `kid` in the JWT header.

# Citations

[1] [Service README](https://github.com/acme/storefront/tree/main/services/auth)
