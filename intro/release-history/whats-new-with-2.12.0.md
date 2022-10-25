---
description: 2021-MAR-29
---

# What's New With 2.12.0

#### Added

* More and more apps will need real ip's from request, so expose it via the `CBSecurity` model service as : `getRealIp()`

#### Fixed

* When using `getHTTPREquestData()` send `false` so we DON'T retrieve the http body when we just need the headers
* More updates to `getRealIp()` when dealing with lists
