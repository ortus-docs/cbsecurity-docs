---
description: 2020-DEC-11
---

# What's New With 2.9.0

#### Fixed

* Fixes a typo in the `cbSecurity_onInvalidAuthorization` interception point declaration. Previously, the typo would prevent ColdBox from allowing the correctly-typed interception point from ever triggering an interception listener.
* The `userValidator()` method has been changed to `roleValidator()`, but the error message was forgotten! So the developer is told they need a `userValidator()` method... because the `userValidator` method is no longer supported. :/

#### Added

* The `isLoggedIn()` method now makes sure that a jwt is in place and valid, before determining if you are logged in or not.
* Migrated all automated tests to `focal` and `mysql8` in preparation for latest updates
* Add support for JSON/XML/model rules source when loading rules from modules.  Each module can now load rules not only inline but from the documented external sources.
* Ensure non-configured `rules` default to empty array
