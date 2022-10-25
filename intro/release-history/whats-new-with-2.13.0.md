---
description: 2021-SEP-
---

# What's New With 2.13.0

### Added

* Adobe 2021 Support
* Migration to GitHub Actions from Travis CI
* Refresh tokens support
* Refresh token endpoint `/cbsecurity/refreshToken` for secure refresh token generation
* Manual refresh token method on the `JwtService` : `refreshToken( token )`
* Auto refresh token header interceptions for JWT validators
* Detect on `authenticate()` if the payload is empty and throw the appropriate exceptions
* Added ability for the `authenticate( payload )` to receive a payload to authenticate
* Added ability to recreate the token storage using a `force` argument `getTokenStorage( force = false )`
* Ability for the `parseToken()` to choose to store and authenticate or just parse

### Fixed

* Unique `jti` could have collisions if tokens created at the same time, add randomness to it
* `TokenExpirationException` not relayed from the base jwt library
* If `variables.settings.jwt.tokenStorage.enabled` is disabled all invalidations failed, make sure if the storage is disabled to not throw storage exceptions.

