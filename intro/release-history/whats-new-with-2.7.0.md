---
description: 2020-SEP-14
---

# What's New With 2.7.0

#### Added

* Contributed module rules are now **pre-pended** instead of appended. \(@wpdebruin\)

#### Fixed

* Not loading rules by source file detection due to invalid setting check
* Don't trigger ColdBox's invalid event looping protection. It also auto-senses between ColdBox 6 and 5 \(@homestar9\)
* Fixed token scopes according to JWT spec, it is called `scope` and it is a **space** separated list. This doesn't change the User interface for it. \(@wpdebruin\)
* Update token storages so no token rejection anymore when storage is not enabled. \(@wpdebruin\)

