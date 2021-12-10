---
description: 2021-DEC-10
---

# What's New With 2.15.0

### 🚀 Added

* Pass custom claims from `refreshToken()` method when refreshing tokens

In the JWTService the `refreshToken( struct customClaims = {} )` now has a `customClaims` argument which you can use to seed the refresh token with custom claims.

* Pass in the current JWT payload in to `getJWTCustomClaims( payload )` method

This was done to help authors have the exact payload that was used for the execution call. This affects the `IJwtSubject` interface.

* The auto refresh token features now will auto refresh not only on expired tokens, but on invalid and missing tokens as well. Thanks to @elpete

### 🐛 Fixed

* Timeout in token storage is now the token timeout
