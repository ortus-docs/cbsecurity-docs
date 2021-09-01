---
description: 2020-FEB-12
---

# What's New With 2.2.0

## Features

* `Feature` : Migrated from the jwt to the `jwtcfml` \([https://forgebox.io/view/jwt-cfml](https://forgebox.io/view/jwt-cfml)\) library to expand encoding/decoding capabilities to support `RS` and `ES` algorithms:
  * HS256
  * HS384
  * HS512
  * RS256
  * RS384
  * RS512
  * ES256
  * ES384
  * ES512
* `Feature` : Added a new convenience method on the JWT Service: `isTokenInStorage( token )` to verify if a token still exists in the token storage
* `Feature` : If no jwt secret is given in the settings, we will dynamically generate one that will last for the duration of the application scope.
* `Feature` : New setting for `jwt` struct: `issuer`, you can now set the issuer of tokens string or if not set, then cbSecurity will use the home page URI as the issuer of authority string.
* `Feature` : All tokens will be validated that the same `iss` \(Issuer\) has granted the token

## Improvements

* `Improve` : Ability to have defaults for all JWT settings instead of always typing them in the configs
* `Improve` : More formattting goodness!

## Bugs

* `Bug` : Invalidation of tokens was not happening due to not using the actual key for the storage

