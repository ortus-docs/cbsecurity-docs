---
description: June 14, 2023
---

# What's New With 3.4.0

### Added

* Official Adobe 2023 Support
* Gitflows for testing all engines and all versions of ColdBox
* Added `transientCache=false` to auth `User` to avoid any issues when doing security operations
* Added population control for auth `User` for extra security

### Fixed

* `User` auth was not serializing the `id` of the user in the mementifier config
