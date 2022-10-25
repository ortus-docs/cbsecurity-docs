---
description: 2021-MAR-10
---

# What's New With 2.11.x

### Added

* Add a `secureSameUser` method to throw when passed a different user #29 ([https://github.com/coldbox-modules/cbsecurity/pull/29](https://github.com/coldbox-modules/cbsecurity/pull/29))

## \[2.11.1] => 2021-MAR-10

### Fixed

* Fix `getRealIP()` to only return originating user's source IP, if the forwarded ip is a list
