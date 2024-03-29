---
description: January 2023
---

# What's New With 3.0.0

<figure><img src="../../.gitbook/assets/CBSecurity-M-darkbg.png" alt=""><figcaption><p>v3.x Release</p></figcaption></figure>

### Compatibility

* Dropped Adobe ColdFusion 2016
* New **`JwtAuthValidator`** instead of mixing concerns with the `JwtService`. You will have to update your configuration to use this `validator` instead of the `JwtService`
* All settings have changed. They are not single-level anymore. They are now grouped by functionality. Please see the [Configuration](../../getting-started/configuration/) area for the new approach.

### Added

* New ability for the firewall to log all action events to a database table.
* If enabled, a new visualizer can visualize all settings and firewall events via the log table.
* New Basic Auth validator and basic auth user credentials storage system. This will allow you to secure apps where no database interaction is needed or required.
* New global and rule action: `block` and the firewall will block the request with a 401 Unauthorized page.
* New event `cbSecurity_onFirewallBlock` announced whenever the firewall blocks a request into the system with a 403.
* `DBTokenStorage` now rotates using the async scheduler and not direct usage anymore.
* Ability to set the `cbcsrf` module settings into the `cbsecurity` settings as `csrf`.
* We now default the user service class and the auth token rotation events according to the user authentication service (cbauth, etc.); no need to duplicate work.
* New rule-based IP security. You can add a `allowedIPs` key into any rule and add which IP Addresses are allowed into the match. By default, it matches all IPs.
* New rule-based HTTP method security. You can add a `httpMethods` key into any rule and add which HTTP methods are allowed into the match. By default, it matches all HTTP Verbs.
* New `securityHeaders` configuration to allow a developer to protect their apps from common exploits: XSS, HSTS, Content Type Options, host header validation, IP validation, clickjacking, non-SSL redirection, and much more.
* The security firewall now stores the authenticated user according to the `prcUserVariable` on authenticated calls via `preProcess()` no matter the validator used
* Dynamic Custom Claims: You can pass a function/closure as the value for a custom claim, and it will be evaluated at runtime, passing in the current claims before being encoded
* Allow passing in custom refresh token claims to `attempt()` and `fromUser()` and `refreshToken()` : `refreshCustomClaims`
* Added `TokenInvalidException` and `TokenExpiredException` to the `refreshToken` endpoint

### Fixed

* Disable lastAccessTimeouts for JWT CacheTokenStorage BOX-128
* Fix spelling of property `datasource` on `queryExecute` that was causing a read issue.
