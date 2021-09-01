---
description: 2020-NOV-09
---

# What's New With 2.8.0

#### Added

* `parseToken( token )` now accepts a token of your choice to work with in the request or it will continue to discover it if not passed.
* Added new JWT Service method: `invalidateAll()` which invalidates all tokens in the token storage
* Added the new event: `cbSecurity_onJWTInvalidateAllTokens` that fires once all tokens in the storage are cleared
* Added storage of the authenticated user into the `prc` scope when using `attempt()` to be consistent with API calls

#### Fixed

* Spelling corrections on the readme
* Added full var scoping for `cbsecurity` in JWTService calls

