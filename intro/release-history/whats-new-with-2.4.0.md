---
description: 2020-APR-02
---

# What's New With 2.4.0

This release adds the inclusion of the Cross Site Request Forgery module into cbsecurity: `cbcsrf`.  You can find all the details about this module here: [https://github.com/coldbox-modules/cbcsrf](https://github.com/coldbox-modules/cbcsrf).  Below are the major features of this module:

### Features

* Ability to generate security tokens based on your session
* Automatic token rotation when leveraging `cbauth` login and logout operations
* Ability to on-demand rotate all security tokens for specific users
* Leverages `cbStorages` to store your tokens in CacheBox, which can be easily distributed and clustered
* Ability to create multiple tokens via unique reference `keys`
* Auto-verification interceptor that will verify all non-GET operations to ensure a security token is passed via `rc` or headers
* Auto-sensing of integration testing so the verifier can allow testing calls
* Token automatic rotation on specific time periods for enhance security
* Helpers to automatically generate hidden fields for the token
* Automatic generation endpoint that can be used for Ajax applications to request tokens for users
