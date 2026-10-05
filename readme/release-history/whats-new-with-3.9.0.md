---
description: Unreleased
---

# What's new With 3.9.0

#### Added

* **Route middleware**: secure routes and groups right where they are declared using ColdBox route-scoped middleware (`route().middleware()`), with no firewall rules needed. New WireBox IDs: `Authenticated@cbsecurity`, `Authorized@cbsecurity`, `JwtAuth@cbsecurity` and `BasicAuth@cbsecurity`. Permissions, roles and the permission `mode` (`any`, `all`, `none`) are declared in the route `meta()`. See [Route Middleware](../../usage/route-middleware.md).
* Custom middleware by extending `cbsecurity.models.middleware.Guard`.
* `Security` interceptor `validateAccess()` and `processInvalidAccess()` public methods, used by the middleware and available to custom integrations.

#### Changed

* Handler and action annotation security now shares `processInvalidAccess()` with route middleware. The behavior is unchanged.

#### Requirements

* Route middleware requires ColdBox 8.2+.
* Group-level `meta` and route middleware in integration tests (`execute()`, `get()`, etc.) require ColdBox 8.3+.
