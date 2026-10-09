---
description: Unreleased
---

# What's new With 3.9.0

#### Added

* **Route middleware**: secure routes and groups right where they are declared using ColdBox route-scoped middleware (`route().middleware()`), with no firewall rules needed. New WireBox IDs: `Authenticated@cbsecurity`, `Authorized@cbsecurity`, `JwtAuth@cbsecurity` and `BasicAuth@cbsecurity`. Permissions, roles and the permission `mode` (`any`, `all`, `none`) are declared in the route `meta()`. See [Route Middleware](../../usage/route-middleware.md).
* **More route middleware**, configured in the route `meta()` with defaults in the new `middleware` and `signedUrls` module settings:
  * [`Throttle@cbsecurity`](../../usage/route-middleware/throttle.md) rate limits with named or inline limiters on any CacheBox cache (default `default`), plus the `RateLimiter@cbsecurity` model for use anywhere.
  * [`ApiKey@cbsecurity`](../../usage/route-middleware/api-key.md) reads the `x-api-key` header or the `apiKey` request key, both configurable, and can use a validator service.
  * [`AllowedIPs@cbsecurity` and `DenyIPs@cbsecurity`](../../usage/route-middleware/ip-filtering.md) filter by IPv4, IPv6 and CIDR, with `trustedProxies` support.
  * [`EnsureHttps@cbsecurity`](../../usage/route-middleware/ensure-https.md), [`VerifyCsrf@cbsecurity`](../../usage/route-middleware/verify-csrf.md) and [`Honeypot@cbsecurity`](../../usage/route-middleware/honeypot.md).
* **Signed URLs**: [`Signed@cbsecurity`](../../usage/route-middleware/signed-urls.md) middleware, the `UrlSigner@cbsecurity` model and the `signedRoute()`, `signedUrl()` and `hasValidSignature()` helpers for tamper-proof, expiring links.
* Custom middleware by extending `cbsecurity.models.middleware.Guard`.
* `Security` interceptor `validateAccess()` and `processInvalidAccess()` public methods, used by the middleware and available to custom integrations.

#### Changed

* Handler and action annotation security now shares `processInvalidAccess()` with route middleware. The behavior is unchanged.

#### Fixed

* Settings you set on the `cbcsrf` module were overwritten by the cbsecurity `csrf` defaults, even when you never set `cbsecurity.csrf`. Now only the keys you explicitly set in `cbsecurity.csrf` are applied, and they win over the `cbcsrf` settings. See [Which Settings Win](../../usage/cross-site-request-forgery-cbcsrf.md#which-settings-win).

#### Requirements

* Route middleware requires ColdBox 8.2+.
* Group-level `meta` and route middleware in integration tests (`execute()`, `get()`, etc.) require ColdBox 8.3+.
