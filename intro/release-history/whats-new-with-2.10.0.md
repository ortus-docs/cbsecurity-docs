---
description: 2021-FEB-12
---

# What's New With 2.10.0

#### Added

* Moved the registration of the validator from the `configure()` to the `afterAspectsLoad()` interception point to allow for modules to declare the validator if needed.
* Moved handler bean to `afterAspectsLoad()` to allow for module based invalid events to work.
